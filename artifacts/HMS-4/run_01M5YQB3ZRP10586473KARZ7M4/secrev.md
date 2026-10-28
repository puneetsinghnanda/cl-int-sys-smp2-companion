---
schema: secure-review.v1
run_id: run_01M5YQB3ZRP10586473KARZ7M4
step: secrev
attempt: 1
skill: secure-code-review@1.0.1
inputs:
  - path: artifacts/HMS-4/run_01M5YQB3ZRP10586473KARZ7M4/impl.md
    sha256: 0f9e83d1d5682f454a07a45622414cba47d2b81c15471f261b4b09b016bbbdc1
  - path: context/conventions.md
    sha256: dc53d94ec34eb193eeb06fdd0db19eb86a24c29f7d3c434e7c212d61038ca852
  - path: context/architecture.md
    sha256: 9faab093d10933a46f6473b25c80728f8745e597100e650cd2537bbafb2b595a
pack_sha:
  sample-system: bd98ff08b83f8762c5e5c073ba5fadef88169b33
claims:
  - id: C1
    text: "Routes parse input and declare a permission; `repositories` hold the SQL."
    cites: ["context/architecture.md#components"]
  - id: C2
    text: "In the MVP every request is the actor `staff` and every permission is granted."
    cites: ["context/architecture.md#identity"]
  - id: C3
    text: "Logs carry method, route and error message only, never request bodies, contact details or `medical` history."
    cites: ["context/conventions.md#logging-and-data"]
  - id: C4
    text: "Lists answer items, page, page_size and total, `20 per page`."
    cites: ["context/conventions.md#lists"]
trace: [REQ-2, REQ-3, REQ-5, REQ-6, REQ-7, REQ-9, REQ-13]
known: [REQ-2, REQ-3, REQ-5, REQ-6, REQ-7, REQ-9, REQ-13]
assumed: []
unknown: [U-1]
unknowns:
  - {id: U-1, blocking: false, text: "The SAST scan reports 4 results in files this change does not touch (CI workflow action tags, a non-literal RegExp in the codes helper, a format string in the error handler). They predate HMS-4 and are outside its diff.", question: Q-01}
questions:
  - id: Q-01
    blocking: false
    ask: tech lead
    text: "Should the 4 pre-existing SAST results outside the HMS-4 diff be triaged in a separate ticket?"
    why: "They show in every run's SAST report and are not caused or fixed by this change."
    evidence: ["context/conventions.md#logging-and-data"]
    options: ["Triage them in a separate ticket", "Accept them as known"]
    default: "Triage them in a separate ticket"
scans:
  - {kind: sast, report_sha256: 9c5970a416969fcab7363825f928aa2461291beee490e9a1ca9f83ff4ce3549c}
  - {kind: secrets, report_sha256: 5a1cea4400bf5f18180b6d2eff35db1be83f6e03aa19603fd628c0484b8d90a3}
findings:
  - id: SF-1
    severity: minor
    text: "The new last-visit value is readable by any caller of GET /api/patients because the MVP grants every permission; the route still declares patients:read, so it will be covered once sign-in replaces the MVP identity."
    cites: ["sample-system:api/src/services/patientService.ts#L38", "sample-system:api/src/routes/patients.ts#L12"]
---
# Secure code review: HMS-4

## Summary
Overall risk is low. The change adds one read-only, derived value to the patient list, and no finding is major or worse. The most important point is SF-1: the value inherits the MVP identity, where every permission is granted (C2). The route itself is unchanged and still declares its permission (C1), so nothing new is exposed beyond what the list already shows today.

What was checked in the diff:
- **Injection:** the new subquery in PatientRepo (sample-system:api/src/repositories/patientRepo.ts#L131-L132) uses only the literal status COMPLETED and the outer row's id. No request value is concatenated into it; the existing search filter keeps its bound parameters.
- **Data exposure:** the list's column list is unchanged, so no medical history or extra contact detail is selected. The service drops the raw instant and sends only a day string (sample-system:api/src/services/patientService.ts#L36-L38).
- **Output encoding:** the page renders the value as a React text node, with "-" for null (sample-system:web/src/pages/PatientsPage.tsx#L76). There is no raw HTML insertion.
- **Logging:** nothing new is logged (C3).
- **Resource use:** the per-row lookup runs once for each row of a page that stays capped at 20 (C4).
- **Dependencies:** no package or lock file changed.

Scans: the secrets scan has no results. The SAST scan has 4 results, all in files outside this diff (U-1).

## Findings
- SF-1 (minor): the last-visit value is visible to any caller while the MVP grants every permission; it is covered by the existing patients:read declaration once sign-in lands. Cites sample-system:api/src/services/patientService.ts#L38 and sample-system:api/src/routes/patients.ts#L12.

## Known
- REQ-2, REQ-5, REQ-6, REQ-7: the status filter is a literal in SQL, not request input.
- REQ-3: the time zone comes from server configuration, not from the request.
- REQ-9: page size stays capped, which bounds the per-row lookup.
- REQ-13: no medical-history column is selected or sent.

## Assumed
- None.

## Unknown
- U-1 (non-blocking): 4 pre-existing SAST results sit outside the HMS-4 diff (Q-01).

## Open questions
- Q-01 (non-blocking, tech lead): triage the 4 pre-existing SAST results in a separate ticket? Default: yes, separately.
