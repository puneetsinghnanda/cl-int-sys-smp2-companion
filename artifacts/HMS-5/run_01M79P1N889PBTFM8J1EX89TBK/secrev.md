---
schema: secure-review.v1
run_id: run_01M79P1N889PBTFM8J1EX89TBK
step: secrev
attempt: 2
skill: secure-code-review@1.2.0
inputs:
  - {path: artifacts/HMS-5/run_01M79P1N889PBTFM8J1EX89TBK/impl.md, sha256: 72eafcaf6291816266d51f848b24dd9000dc0c3aab1c974d8f12b91e8fe0e1a4}
  - {path: context/conventions.md, sha256: dc53d94ec34eb193eeb06fdd0db19eb86a24c29f7d3c434e7c212d61038ca852}
  - {path: context/architecture.md, sha256: 9faab093d10933a46f6473b25c80728f8745e597100e650cd2537bbafb2b595a}
pack_sha: {sample-system: 7681f87749883d71abb4011d118366ca1242e1cc}
claims:
  - {id: C1, text: "Every API request passes through `identify`, and routes call requirePermission.", cites: ["context/architecture.md#identity"]}
  - {id: C2, text: "In the MVP every request is the actor `staff` and every permission is granted.", cites: ["context/architecture.md#identity"]}
  - {id: C3, text: "SQL belongs in `api/src/repositories`, and rules in api/src/services.", cites: ["context/conventions.md#code-layout"]}
  - {id: C4, text: "Unknown ids return `404`, and invalid input returns 400 with one message per field.", cites: ["context/conventions.md#errors"]}
  - {id: C5, text: "Logs carry method, route and error message only, `never request bodies`, contact details or medical data.", cites: ["context/conventions.md#logging-and-data"]}
  - {id: C6, text: "The change adds the field `visitSummary { completed, noShow }` to the patient detail.", cites: ["artifacts/HMS-5/run_01M79P1N889PBTFM8J1EX89TBK/impl.md#claim:C7"]}
trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9]
known: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9]
assumed: []
unknown: []
questions: []
resolves:
  - {id: F-1, how: "C6 now cites the change artifact's claim C7 instead of the design step's IDE answer."}
scans:
  - {kind: sast, report_sha256: 56ff2da3c6133c0042a0f6e33cdd0763135eba58ce64ed389e6d5f23e73acc62}
  - {kind: secrets, report_sha256: 5a1cea4400bf5f18180b6d2eff35db1be83f6e03aa19603fd628c0484b8d90a3}
findings: []
---
# Secure code review: HMS-5

## Summary
The change carries low risk, and I found no security weakness. The SAST and secrets reports from the verifier are attached by hash; I did not restate their results.

The change does three things. It adds one count query to the existing patient repository, two integer fields to the patient detail response (C6), and a read-only block on the patient page. It adds no route, no permission, no dependency and no log line.

Retry fix for F-1: C6 now cites the change artifact's claim C7 instead of the design step's IDE answer.

## Findings
None. What I checked in the diff:
- **Injection:** `PatientRepo.visitSummary` runs one query with fixed status literals. The patient id is bound as `$1`, never concatenated into the SQL (sample-system:api/src/repositories/patientRepo.ts#L221-L229). The query sits in the repository layer, as the conventions require (C3).
- **Authorisation and trust boundaries:**
  - The field rides on the existing GET, POST and PATCH patient routes. These already pass through `identify` and `requirePermission` (C1).
  - No new route or permission is added, so the change adds no new exposure.
  - In the MVP every request is the `staff` actor (C2). That is an existing, accepted design point, not something this change introduces.
- **Error leakage:** for an unknown patient, `PatientService.get` throws `notFound` before the count runs (sample-system:api/src/services/patientService.ts#L41-L42). So the existing 404 path is unchanged (C4), and no count is computed for an id that does not exist.
- **Data exposure:**
  - The response gains only two integers, `completed` and `noShow`.
  - No dates, doctor names or medical data are added.
  - The appointments list and medical history the page already returns are unchanged.
- **Output encoding:** the web block renders the numbers as React text children (sample-system:web/src/pages/PatientDetailPage.tsx#L76-L81). There is no raw HTML injection and no URL building.
- **Logging:** no log statements were added, which matches the logging rule (C5).
- **Dependencies:** no package or lock file changed.

## Known
- REQ-1 to REQ-9: the changed code that serves these requirements was read at head and checked against the items above. None of it adds a weakness.

## Assumed
None.

## Unknown
None.

## Open questions
None.
