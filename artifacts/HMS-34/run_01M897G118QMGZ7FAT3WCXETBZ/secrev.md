---
schema: secure-review.v1
run_id: run_01M897G118QMGZ7FAT3WCXETBZ
step: secrev
attempt: 1
skill: secure-code-review@1.4.0
inputs:
  - path: artifacts/HMS-34/run_01M897G118QMGZ7FAT3WCXETBZ/impl.md
    sha256: cb0d95bc7f6acc19c5972731cefe7bd1eeddb4d512ad74a15273a44f7ba4ec27
  - path: context/conventions.md
    sha256: dc53d94ec34eb193eeb06fdd0db19eb86a24c29f7d3c434e7c212d61038ca852
  - path: context/architecture.md
    sha256: 9faab093d10933a46f6473b25c80728f8745e597100e650cd2537bbafb2b595a
pack_sha:
  sample-system: 7681f87749883d71abb4011d118366ca1242e1cc
claims:
  - id: C1
    text: "The new repository update binds the cutoff and actor as parameters: `WHERE status = 'SCHEDULED' AND starts_at <= $1`."
    cites: ["sample-system:api/src/repositories/appointmentRepo.ts#L168"]
  - id: C2
    text: "The changed move() keeps every value parameterised, with a literal `status = 'SCHEDULED'`."
    cites: ["sample-system:api/src/repositories/appointmentRepo.ts#L158"]
  - id: C3
    text: "The job logs only the error message, `err.message`, with no request body or personal data."
    cites: ["sample-system:api/src/jobs/staleJob.ts#L12"]
  - id: C4
    text: "Logs carry `method, route and error message only`."
    cites: ["context/conventions.md#logging-and-data"]
  - id: C5
    text: "The write routes the widened status check serves still call `requirePermission('appointments:write')`; the change adds no route."
    cites: ["sample-system:api/src/routes/appointments.ts#L36"]
  - id: C6
    text: "In the MVP `every permission is granted`, so the change does not alter who may act."
    cites: ["context/architecture.md#identity"]
  - id: C7
    text: "The widened guard still refuses Completed, Cancelled and No-show: `current.status !== 'SCHEDULED' && current.status !== 'STALE'`."
    cites: ["sample-system:api/src/services/appointmentService.ts#L147"]
  - id: C8
    text: "The job writes as the fixed actor `system`, not as a request actor."
    cites: ["runs/run_01M897G118QMGZ7FAT3WCXETBZ/ide-answers.md#a-09"]
trace: [REQ-2, REQ-3, REQ-7, REQ-8, REQ-9, REQ-10]
known: [REQ-2, REQ-3, REQ-7, REQ-8, REQ-9, REQ-10]
assumed: []
unknown: []
questions: []
scans:
  - kind: sast
    report_sha256: 56307c55cdd702b9ca48d1116ddb8b41322d8a3fb3ff3d7672e8ab8edf57e8dc
  - kind: secrets
    report_sha256: 5a1cea4400bf5f18180b6d2eff35db1be83f6e03aa19603fd628c0484b8d90a3
findings: []
---
# Secure code review: HMS-34

## Summary
Overall risk is low. The change adds no route and no dependency, keeps every SQL value parameterised
(C1, C2) and logs only the error message from the job (C3, C4). The widened status check opens Stale
appointments to the same write routes and permissions that Scheduled ones already use (C5, C6), and
still refuses closed ones (C7). The job writes as the fixed actor system (C8). The SAST and secrets
scans from the implement step's verifier are attached by hash. This review found nothing to fix.

## Findings
None.

## Known
- REQ-2, REQ-3: the job's update is parameterised and logs no personal data (C1, C3, C4, C8).
- REQ-7, REQ-8, REQ-9, REQ-10: the actions on Stale go through the existing permission-checked write routes (C5, C6, C7); move() stays parameterised (C2).

## Assumed
None.

## Unknown
None.

## Open questions
None.
