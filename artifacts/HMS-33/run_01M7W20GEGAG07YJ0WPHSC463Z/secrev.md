---
schema: secure-review.v1
run_id: run_01M7W20GEGAG07YJ0WPHSC463Z
step: secrev
attempt: 1
skill: secure-code-review@1.3.0
inputs:
  - path: artifacts/HMS-33/run_01M7W20GEGAG07YJ0WPHSC463Z/impl.md
    sha256: 9a068691d7737d9adcc8c4515e667f65c6f20e24684d5b883e0c09c28c213df2
  - path: context/conventions.md
    sha256: dc53d94ec34eb193eeb06fdd0db19eb86a24c29f7d3c434e7c212d61038ca852
  - path: context/architecture.md
    sha256: 9faab093d10933a46f6473b25c80728f8745e597100e650cd2537bbafb2b595a
pack_sha:
  sample-system: 7681f87749883d71abb4011d118366ca1242e1cc
claims:
  - id: C1
    text: "In the MVP every request is the actor `staff`, so the change adds no new trust boundary."
    cites: ["context/architecture.md#identity"]
  - id: C2
    text: "The list route is still guarded by `appointments:read`; no route was added."
    cites: ["sample-system:api/src/routes/appointments.ts#L12"]
  - id: C3
    text: "The reason comes from the existing query column through `cancellation_reason`; no new SQL or user input reaches the database."
    cites: ["sample-system:api/src/repositories/appointmentRepo.ts#L83"]
  - id: C4
    text: "The cell is built by `reasonCell` and rendered as React text, which React escapes; no raw HTML is written."
    cites: ["sample-system:web/src/components/AppointmentTable.tsx#L60"]
  - id: C5
    text: "Logs never carry request bodies, `contact details` or medical history; the change adds no logging."
    cites: ["context/conventions.md#logging-and-data"]
  - id: C6
    text: "The user decided to `Show it as is` in the list."
    cites: ["runs/run_01M7W20GEGAG07YJ0WPHSC463Z/ide-answers.md#a-01"]
  - id: C7
    text: "The field is added for the `List endpoint only`, so other responses carry no more data."
    cites: ["runs/run_01M7W20GEGAG07YJ0WPHSC463Z/ide-answers.md#a-02"]
trace: [REQ-3, REQ-14, REQ-15, REQ-17]
known: [SF-1]
assumed: []
unknown: []
questions: []
scans:
  - kind: sast
    report_sha256: 04825aa05c7f54a3ba5302ec541a357a826b216b0e77d8b8544829ff046af101
  - kind: secrets
    report_sha256: 5a1cea4400bf5f18180b6d2eff35db1be83f6e03aa19603fd628c0484b8d90a3
findings:
  - id: SF-1
    severity: minor
    text: "The free-text cancellation reason, which staff could fill with health details, is now sent in every list page and shown in the list; accepted by the user in A-01, it remains a data-exposure risk. Fix direction: a hint on the cancel form, or a later review of reasons."
    cites: ["sample-system:api/src/repositories/appointmentRepo.ts#L83", "runs/run_01M7W20GEGAG07YJ0WPHSC463Z/ide-answers.md#a-01"]
---
# Secure code review: HMS-33

## Summary
Overall risk is low. The change adds one field to an existing, permission-guarded list route (C2) and one display column. It adds no route, no trust boundary (C1), no new SQL or input path (C3), no logging (C5) and no dependency; the field goes only to the list endpoint (C7). The reason is rendered as escaped React text, never as HTML (C4), and the cut to 60 characters works on code points, so it cannot split a character into an invalid one.

The most important finding is SF-1 (minor): the reason is free text, so health details typed into it now appear in the list. The user decided in A-01 to show it as entered (C6), so this is recorded as an accepted risk rather than a defect.

Both verifier scans from the implement step are attached unchanged (SAST and secrets, by report hash).

## Findings
- SF-1 (minor): the free-text reason is now returned on every list page and shown there; accepted in A-01. Fix direction: a hint on the cancel form not to enter medical details, or a later review of stored reasons. Cites: sample-system:api/src/repositories/appointmentRepo.ts#L83, runs/run_01M7W20GEGAG07YJ0WPHSC463Z/ide-answers.md#a-01.

## Known
- SF-1: seen directly in the changed line that adds `cancellationReason` to each list item, and in the user's answer A-01.

## Assumed
None.

## Unknown
None.

## Open questions
None.
