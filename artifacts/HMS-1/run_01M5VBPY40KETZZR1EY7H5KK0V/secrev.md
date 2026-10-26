---
schema: secure-review.v1
run_id: run_01M5VBPY40KETZZR1EY7H5KK0V
step: secrev
attempt: 1
skill: secure-code-review@1.0.1
inputs:
  - path: artifacts/HMS-1/run_01M5VBPY40KETZZR1EY7H5KK0V/impl.md
    sha256: 9d94f1a05213257a0f4efc2a50ad1445847172044216294dbcd203326c1ca9dd
  - path: context/conventions.md
    sha256: dc53d94ec34eb193eeb06fdd0db19eb86a24c29f7d3c434e7c212d61038ca852
  - path: context/architecture.md
    sha256: 9faab093d10933a46f6473b25c80728f8745e597100e650cd2537bbafb2b595a
pack_sha:
  sample-system: 415369398bdad3122ac084a00766a01a3034f900
claims:
  - id: C1
    text: "In the MVP every request is the actor `staff` and every permission is granted."
    cites: ["context/architecture.md#identity"]
  - id: C2
    text: "Logs never carry request bodies, contact details or `medical history`."
    cites: ["context/conventions.md#logging-and-data"]
  - id: C3
    text: "API errors are JSON with one message per invalid field (`ValidationError`)."
    cites: ["context/conventions.md#errors"]
  - id: C4
    text: "The change adds `next_appointment` to each patient list item."
    cites: ["artifacts/HMS-1/run_01M5VBPY40KETZZR1EY7H5KK0V/impl.md#L66"]
trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10, REQ-11, REQ-12, REQ-13]
known: [SF-1, SF-2]
assumed: []
unknown:
  - {id: U-1, blocking: false}
questions:
  - id: Q-01
    blocking: false
    ask: tech lead
    text: "Before this column is released, will GET /api/patients be limited to signed-in staff, or is the deployment reachable only by staff?"
    why: "In the MVP every request is treated as staff (C1), so the list now discloses each patient's next appointment and doctor to anyone who can reach the API."
    evidence: ["context/architecture.md#identity"]
    options: ["Staff-only network", "Add sign-in before release", "Accept for the MVP"]
    default: "Accept for the MVP as for the existing list, and track it with the sign-in work."
scans:
  - {kind: sast, report_sha256: 91d9eb6293bcaa093175c0e6efce77ad1c81bbf87d97f7478d6abc37fc4a7c03}
  - {kind: secrets, report_sha256: 5a1cea4400bf5f18180b6d2eff35db1be83f6e03aa19603fd628c0484b8d90a3}
findings:
  - id: SF-1
    severity: minor
    text: "The patient list now returns each patient's next appointment time and doctor name to every caller; in the MVP there is no access control on the route, so the exposure grows with this change. Fix direction: keep it behind the same permission as the list and restrict once sign-in exists (Q-01)."
    cites: ["sample-system:api/src/repositories/patientRepo.ts#L123", "context/architecture.md#identity"]
  - id: SF-2
    severity: minor
    text: "The page silently falls back to a fixed default zone when settings carry an invalid zone, which hides a misconfiguration from staff. Fix direction: keep the fallback for display but log or surface the invalid setting through the existing error path, without personal data."
    cites: ["sample-system:web/src/pages/PatientsPage.tsx#L15"]
---
# Secure code review: HMS-1

## Summary
Overall risk is low. The SAST scan reports four hits, all in files this change does not touch (a CI workflow, `api/src/lib/codes.ts` and `api/src/middleware/errorHandler.ts`); the secrets scan reports none. Reading the diff: the new list query keeps every value, including the current instant, as a bound parameter (`sample-system:api/src/repositories/patientRepo.ts#L129`), selects only the existing list columns plus the appointment id, start and doctor name, so no medical history is read or logged (C2), and the page renders the doctor name and time as plain text through React, so no markup is injected. The most important finding is SF-1: the list now discloses each patient's next appointment and doctor to any caller, because the MVP grants every request (C1).

## Findings
- SF-1 (minor): next appointment and doctor now returned to every caller of the list, with no access control in the MVP — `sample-system:api/src/repositories/patientRepo.ts#L123`.
- SF-2 (minor): an invalid zone setting is silently replaced by the default zone on the page — `sample-system:web/src/pages/PatientsPage.tsx#L15`.

## Known
- SF-1: the MVP identity model grants every request (C1) and the change adds `next_appointment` to every list item (C4).
- SF-2: the fallback is in the changed page code; the existing error format is JSON with field messages (C3), so a settings problem has a place to be reported.

## Assumed
- None.

## Unknown
- U-1 (non-blocking): whether access to the list is limited before release (Q-01).

## Open questions
- Q-01 (non-blocking, tech lead): will GET /api/patients be limited to signed-in staff or a staff-only network before release? Default: accept for the MVP and track with the sign-in work.
