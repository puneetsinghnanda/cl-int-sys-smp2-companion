---
schema: secure-review.v1
run_id: run_01M79NZC0GJYR61WB0MNSTYDHQ
step: secrev
attempt: 1
skill: secure-code-review@1.1.0
inputs:
  - path: artifacts/HMS-13/run_01M79NZC0GJYR61WB0MNSTYDHQ/impl.md
    sha256: 0f2c4f20475bbde55247932470829b031ff1e2cdc92e598958cdae21bc853691
  - path: context/conventions.md
    sha256: dc53d94ec34eb193eeb06fdd0db19eb86a24c29f7d3c434e7c212d61038ca852
  - path: context/architecture.md
    sha256: 9faab093d10933a46f6473b25c80728f8745e597100e650cd2537bbafb2b595a
pack_sha:
  sample-system: 7681f87749883d71abb4011d118366ca1242e1cc
claims:
  - id: C1
    text: "The detail read loads the doctor with a non-null assertion: `doctors.get(detail.doctor.id))!`."
    cites: ["sample-system:api/src/services/appointmentService.ts#L60"]
  - id: C2
    text: "Only the doctor's status is copied into the response: `doctorStatus: doctor.status`."
    cites: ["sample-system:api/src/services/appointmentService.ts#L68"]
  - id: C3
    text: "An unknown appointment id still raises `notFound` before any doctor lookup."
    cites: ["sample-system:api/src/services/appointmentService.ts#L57"]
  - id: C4
    text: "The doctor id used for the lookups comes from the stored appointment (`detail.doctor.id`), not from the request."
    cites: ["sample-system:api/src/services/appointmentService.ts#L60"]
  - id: C5
    text: "The notices are fixed texts in `role=\"status\"` paragraphs, with no request data inside."
    cites: ["sample-system:web/src/pages/AppointmentDetailPage.tsx#L73"]
  - id: C6
    text: "Routes declare a permission through `requirePermission`; in the MVP every permission is granted."
    cites: ["context/architecture.md#identity"]
  - id: C7
    text: "Logs carry `method, route and error message only`."
    cites: ["context/conventions.md#logging-and-data"]
  - id: C8
    text: "API errors use the JSON error shape, with `unknown ids 404`."
    cites: ["context/conventions.md#errors"]
  - id: C9
    text: "The service change is confined to `AppointmentService.get`."
    cites: ["artifacts/HMS-13/run_01M79NZC0GJYR61WB0MNSTYDHQ/impl.md#claim:C1"]
trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9]
known: [SF-1]
assumed: []
unknown: []
questions: []
scans:
  - kind: sast
    report_sha256: de13e84e40714871c29c0389f97a466bb120b64abb42a00ac219ae7ee2b49345
  - kind: secrets
    report_sha256: 5a1cea4400bf5f18180b6d2eff35db1be83f6e03aa19603fd628c0484b8d90a3
findings:
  - id: SF-1
    severity: minor
    text: "Non-null assertion on the doctor lookup turns a missing doctor into an unhandled error; raise an explicit, generic error instead."
    cites: ["sample-system:api/src/services/appointmentService.ts#L60"]
---
# Secure code review: HMS-13

## Summary
Overall risk is low. The change adds two non-personal fields to existing appointment responses and two fixed-text notices to one page.
- The service change is confined to `AppointmentService.get` (C9). The change's file list holds no new route, dependency or lock file.
- The existing routes keep their permission declarations (C6).
- Only the doctor's status leaves the doctor record (C2).
- The doctor lookups are keyed by the stored appointment's doctor id, not by request input (C4).
- An unknown appointment id still returns 404 before any new code runs (C3, C8).
- The page renders only fixed strings (C5).
- No logging was added (C7).

The SAST and secrets scans from the verifier are attached by hash.

The most important finding is minor. SF-1: the doctor lookup relies on a non-null assertion rather than an explicit error.

## Findings
- SF-1 (minor), sample-system:api/src/services/appointmentService.ts#L60: the non-null assertion on the doctor lookup (C1) would turn a broken invariant into an unhandled error on every detail read and action response for that appointment. Raise an explicit, generic error instead. The existing reschedule code uses the same pattern, so this does not widen current behaviour.

## Known
- SF-1: read directly from the changed line (C1). Doctors with appointments are not deletable today, so the risk is defensive only.

## Assumed
None.

## Unknown
None.

## Open questions
None.
