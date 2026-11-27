---
schema: secure-review.v1
run_id: run_01M8E1NVV8GFP3FDJ5XQMWQ952
step: secrev
attempt: 1
skill: secure-code-review@1.4.0
inputs:
  - path: artifacts/HMS-34/run_01M8E1NVV8GFP3FDJ5XQMWQ952/impl.md
    sha256: d16f96994ae53c5ec527f12300adf0d58bbbf03ef87b7976c08cea69203ec162
  - path: context/conventions.md
    sha256: dc53d94ec34eb193eeb06fdd0db19eb86a24c29f7d3c434e7c212d61038ca852
  - path: context/architecture.md
    sha256: 9faab093d10933a46f6473b25c80728f8745e597100e650cd2537bbafb2b595a
pack_sha:
  sample-system: 7681f87749883d71abb4011d118366ca1242e1cc
claims:
  - id: C1
    text: "The new Stale UPDATE binds its values as parameters (`starts_at <= $1`), with no string-built SQL."
    cites: ["sample-system:api/src/repositories/appointmentRepo.ts#L168"]
  - id: C2
    text: "The job's failure log writes only the error message (`err.message`), no request data or row contents."
    cites: ["sample-system:api/src/server.ts#L19"]
  - id: C3
    text: "Logs may carry method, route and `error message` only."
    cites: ["context/conventions.md#logging-and-data"]
  - id: C4
    text: "The job's actor is a fixed constant `STALE_JOB_ACTOR = 'system'`, not taken from any input."
    cites: ["sample-system:api/src/server.ts#L10"]
  - id: C5
    text: "Every API request still passes through `identify`; the change adds no route."
    cites: ["context/architecture.md#identity"]
  - id: C6
    text: "The open-state guard still locks the row and rejects closed appointments with a `conflict`."
    cites: ["sample-system:api/src/services/appointmentService.ts#L154"]
  - id: C7
    text: "The Stale marking and its events run in one transaction via `inTransaction`."
    cites: ["sample-system:api/src/services/appointmentService.ts#L138"]
trace: [REQ-2, REQ-5, REQ-6, REQ-8, REQ-10, REQ-11, REQ-12, REQ-13]
known: [SF-1]
assumed: []
unknown: []
questions: []
scans:
  - kind: sast
    report_sha256: 26553a964da3b3e925b7011fa25b22ee3d3359c52ee4cc53a07988cf643f721d
  - kind: secrets
    report_sha256: 5a1cea4400bf5f18180b6d2eff35db1be83f6e03aa19603fd628c0484b8d90a3
findings:
  - id: SF-1
    severity: minor
    text: "markStale updates every overdue Scheduled row in one unbounded UPDATE inside one transaction; the first run after deploy (or after downtime) may lock many rows at once and block staff actions. Fix direction: process in bounded batches, for example by id with a LIMIT, each in its own transaction."
    cites: ["sample-system:api/src/repositories/appointmentRepo.ts#L167"]
---
# Secure code review: HMS-34

## Summary
Overall risk is low. The change adds no route and no new input; the hourly job runs inside the
API process with a fixed actor `system` (C4, C5). Its SQL is parameterised (C1), and it logs only
the error message, as the logging convention requires (C2, C3). The staff actions on Stale
appointments go through the existing row-locking guard, which still rejects closed appointments
(C6). The SAST and secrets scans from the verifier are attached by hash. The only finding is SF-1,
a minor availability point: the first run's single unbounded UPDATE.

## Findings
- SF-1 (minor): one unbounded UPDATE in one transaction may lock many rows on the first run; process in bounded batches. Cite: sample-system:api/src/repositories/appointmentRepo.ts#L167.

## Known
- SF-1: read from the changed repository code; the UPDATE has no LIMIT and runs inside the transaction opened in the service (C7).

## Assumed
None.

## Unknown
None.

## Open questions
None.
