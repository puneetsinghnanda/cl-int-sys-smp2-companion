---
schema: secure-review.v1
run_id: run_01M820V4J0VSJHKNZZ5VK0BT2Y
step: secrev
attempt: 1
skill: secure-code-review@1.3.0
inputs:
  - path: artifacts/HMS-31/run_01M820V4J0VSJHKNZZ5VK0BT2Y/impl.md
    sha256: 51c1d75b880628f2f9a209c6df2fbc6b037b82aa389b6a1880dc9b76cd6b1bbe
  - path: context/conventions.md
    sha256: dc53d94ec34eb193eeb06fdd0db19eb86a24c29f7d3c434e7c212d61038ca852
  - path: context/architecture.md
    sha256: 9faab093d10933a46f6473b25c80728f8745e597100e650cd2537bbafb2b595a
pack_sha:
  sample-system: 7681f87749883d71abb4011d118366ca1242e1cc
claims:
  - id: C1
    text: "The new doctor query is parameterised; the doctor id is bound as `$1`, never concatenated into the SQL."
    cites: ["sample-system:api/src/repositories/appointmentRepo.ts#L121"]
  - id: C2
    text: "The new query selects only specialization, department and `d.fee_paise`; it selects no phone or email."
    cites: ["sample-system:api/src/repositories/appointmentRepo.ts#L116-L117"]
  - id: C3
    text: "Every API request passes through `identify`, and routes declare their permission."
    cites: ["context/architecture.md#identity"]
  - id: C4
    text: "The existing detail route keeps its `appointments:read` permission; no route was added or changed."
    cites: ["sample-system:api/src/routes/appointments.ts#L28"]
  - id: C5
    text: "Logs never carry request bodies, `contact details` or medical history; the change adds no logging."
    cites: ["context/conventions.md#logging-and-data"]
  - id: C6
    text: "The web app talks to the API only through `web/src/api/client.ts`; the change adds no new client call."
    cites: ["context/architecture.md#components"]
  - id: C7
    text: "The detail result is taken with a non-null assertion, `rows[0]!`."
    cites: ["sample-system:api/src/repositories/appointmentRepo.ts#L124"]
trace: [REQ-9, REQ-10, REQ-11]
known: [SF-1]
assumed: []
unknown:
  - {id: U-1, blocking: false}
questions: []
scans:
  - {kind: sast, report_sha256: 462885f0ff301ad61320314980b408e6aedbf3003750aa4bf643869c5b531b00}
  - {kind: secrets, report_sha256: 5a1cea4400bf5f18180b6d2eff35db1be83f6e03aa19603fd628c0484b8d90a3}
findings:
  - id: SF-1
    severity: minor
    text: "The doctor lookup uses a non-null assertion. If the doctor row were ever missing, the request fails with an untyped TypeError instead of a clear error. Fix direction: check the row and throw the existing notFound error."
    cites: ["sample-system:api/src/repositories/appointmentRepo.ts#L124"]
---
# Secure code review: HMS-31

## Summary
The change adds no route, no permission, no dependency, no logging and no new user input. The overall
risk is low. The new SQL is parameterised (C1) and selects only the doctor's specialization,
department and fee, never phone, email or patient data (C2). So the response stays inside the data
rules (REQ-9, REQ-10). The detail route keeps its existing `appointments:read` guard (C3, C4). The web
page renders the new values as React text nodes and uses no raw HTML, so names are escaped. The SAST
and secrets scans from the verifier are attached by hash. The only finding is a minor robustness point
(SF-1).

## Findings
- SF-1 (minor): the doctor lookup's result uses a non-null assertion (`rows[0]!`). A missing doctor row would throw an untyped TypeError rather than a clear 404. The foreign key makes this unlikely, but the fix direction is to check the row and throw `notFound` — sample-system:api/src/repositories/appointmentRepo.ts#L124.

## Known
- SF-1: read directly from the changed line (C7).

## Assumed
None.

## Unknown
- U-1 (non-blocking): the glossary has no entry for Specialization, Department or Consultation fee. This is already open as runs/run_01M820V4J0VSJHKNZZ5VK0BT2Y/open-questions.md#q-02 and has no security impact.

## Open questions
None new. runs/run_01M820V4J0VSJHKNZZ5VK0BT2Y/open-questions.md#q-02 stays open (non-blocking, tech lead).
