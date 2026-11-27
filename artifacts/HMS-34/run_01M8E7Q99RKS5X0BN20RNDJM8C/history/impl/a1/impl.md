---
schema: change.v1
run_id: run_01M8E7Q99RKS5X0BN20RNDJM8C
step: impl
attempt: 1
skill: implement@1.4.0
inputs:
  - path: artifacts/HMS-34/run_01M8E7Q99RKS5X0BN20RNDJM8C/design.md
    sha256: cd22c633358d52a27d9055d4efad6c44d657cc425c641ae2ca35993468701d8a
  - path: artifacts/HMS-34/run_01M8E7Q99RKS5X0BN20RNDJM8C/split.md
    sha256: de964b65dc63706622cc2da9b738e4978b15ad686429c50dcbb5d068f9d8336e
  - path: context/conventions.md
    sha256: dc53d94ec34eb193eeb06fdd0db19eb86a24c29f7d3c434e7c212d61038ca852
  - path: context/generated/module-map.json
    sha256: 4158c8fcd239c22cb8f1029dd11f61d8839867ff8088bd9ab76197e2db96db4b
pack_sha:
  sample-system: 7681f87749883d71abb4011d118366ca1242e1cc
code:
  - repo: sample-system
    base_sha: 7681f87749883d71abb4011d118366ca1242e1cc
    head_sha: 2ce51c7d0e93cc5322f981bc579ff5ee0302404c
    diff_sha256: 6c31d0ae9d09975a59a1144f30bd104fe39a3d64a141de98a43369f55d2d53ef
    files:
      - api/src/repositories/appointmentRepo.ts
      - api/src/server.ts
      - api/src/services/appointmentService.ts
      - api/src/types.ts
      - web/src/api/types.ts
      - web/src/lib/format.ts
      - web/src/pages/AppointmentDetailPage.tsx
claims:
  - id: C1
    text: "The change limits mark `**/migrations/**` as a sensitive path."
    cites: ["tsl.yaml#L37"]
  - id: C2
    text: "The design needs a new migration that adds STALE to the status and event checks (`Migration adding STALE to the status and event CHECKs`)."
    cites: ["artifacts/HMS-34/run_01M8E7Q99RKS5X0BN20RNDJM8C/design.md#item:CO-2"]
  - id: C3
    text: "Without that migration, the appointment table still restricts status with `CHECK (status IN ('SCHEDULED', 'COMPLETED', 'CANCELLED', 'NO_SHOW'))`, so the sweep's UPDATE to STALE would fail at run time."
    cites: ["sample-system:db/migrations/20261016T090300__create_appointments.sql#L10"]
  - id: C4
    text: "The glossary change lives in the companion repo (`New Stale glossary entry in companion context/glossary.md`), outside the product repo this step edits."
    cites: ["artifacts/HMS-34/run_01M8E7Q99RKS5X0BN20RNDJM8C/design.md#item:CO-11"]
  - id: C5
    text: "The sweep runs as an `In-process timer`."
    cites: ["runs/run_01M8E7Q99RKS5X0BN20RNDJM8C/ide-answers.md#a-08"]
  - id: C6
    text: "The sweep runs `Every hour`."
    cites: ["runs/run_01M8E7Q99RKS5X0BN20RNDJM8C/ide-answers.md#a-09"]
  - id: C7
    text: "The Stale change is recorded with `Event, actor system`."
    cites: ["runs/run_01M8E7Q99RKS5X0BN20RNDJM8C/ide-answers.md#a-03"]
  - id: C8
    text: "Logs carry method, route and `error message only`."
    cites: ["context/conventions.md#logging-and-data"]
trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10, REQ-11, REQ-12]
known: [REQ-1, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-9, REQ-10, REQ-11]
assumed: []
unknown: [{id: U-1, blocking: true}, {id: U-2, blocking: false}]
questions:
  - id: Q-01
    blocking: true
    ask: tech lead
    text: "May this run add the migration db/migrations/<UTC timestamp>__add_stale_status.sql (sensitive path **/migrations/**) that drops and re-adds the appointment status CHECK and the appointment_event event CHECK with STALE added?"
    why: "Without it, markStale() fails on the status CHECK and the STALE event CHECK, so REQ-2 and REQ-8 cannot work in a real database."
    evidence: ["tsl.yaml#L37", "artifacts/HMS-34/run_01M8E7Q99RKS5X0BN20RNDJM8C/design.md#item:CO-2", "sample-system:db/migrations/20261016T090300__create_appointments.sql#L10", "sample-system:db/migrations/20261016T090300__create_appointments.sql#L35"]
    options: ["Yes, add the migration as designed", "No, stop the run"]
    default: "Yes, add the migration as designed"
  - id: Q-02
    blocking: false
    ask: tech lead
    text: "Who updates the companion's context/glossary.md (new Stale entry, updated Scheduled entry) for REQ-12, since this step edits only the product repo?"
    why: "The design places the glossary change in the companion repo (CO-11), which the implement step may not edit."
    evidence: ["artifacts/HMS-34/run_01M8E7Q99RKS5X0BN20RNDJM8C/design.md#item:CO-11"]
    options: ["Tech lead edits it when the PR is merged", "Next context refresh updates it"]
    default: "Tech lead edits it when the PR is merged"
summary: "Adds the STALE status to the API and web types, an hourly in-process Stale sweep (AppointmentService.markStale via AppointmentRepo.markStale, actor system), lets staff close or reschedule Stale appointments, and shows Stale in the list, filter, badge and history. The migration (sensitive path) and the companion glossary are held for the tech lead."
---
# Change: HMS-34

## Summary
This change builds every component of the design except two:
- **CO-2 (the migration)** is under the sensitive path `**/migrations/**` (C1), so Q-01 asks the tech lead first. Until it lands, the database CHECK still rejects STALE (C3).
- **CO-11 (the glossary)** lives in the companion repo, which this step does not edit (C4). Q-02 asks who updates it.

Built:
- **CO-1:** the API status type and `APPOINTMENT_STATUSES` gain STALE, so the list filter accepts `?status=STALE`.
- **CO-3:** `AppointmentRepo.markStale(cutoff, actor)` sets Scheduled rows that started at or before the cutoff to STALE and writes one `STALE` event per row. `move()` sets the status back to SCHEDULED.
- **CO-4:** `AppointmentService.markStale()` uses the cutoff `clock() - DAY_MS` and the actor `system` (C7) in one transaction. The `scheduled()` guard accepts SCHEDULED or STALE, so reschedule, cancel, complete and no-show work on Stale appointments.
- **CO-5:** `api/src/server.ts` runs the sweep once after listen, then every hour (C5, C6), and clears the timer in `shutdown()`. Failures log the error message only (C8).
- **CO-6, CO-7, CO-8:** the web status type gains STALE, and `STATUS_LABEL` gains `STALE: 'Stale'`, which feeds the badge and the list filter. The appointment page shows its actions for Stale appointments and labels the history event "Marked stale".

## Changes
- api/src/types.ts — CO-1: STALE in `AppointmentStatus` and `APPOINTMENT_STATUSES`.
- api/src/repositories/appointmentRepo.ts — CO-3: new `markStale()`; `move()` sets status SCHEDULED.
- api/src/services/appointmentService.ts — CO-4: new `markStale()`, `SYSTEM_ACTOR`, and the guard accepts STALE.
- api/src/server.ts — CO-5: hourly Stale sweep timer, run once at start and cleared on shutdown.
- web/src/api/types.ts — CO-6: STALE in the web `AppointmentStatus`.
- web/src/lib/format.ts — CO-7: `STALE: 'Stale'` in STATUS_LABEL.
- web/src/pages/AppointmentDetailPage.tsx — CO-8: actions for Stale, and the `Marked stale` history label.
- No tests added: the design asks for no regression tests; the unit-test step covers them.

## Known
- REQ-1: STALE added to both status types (CO-1, CO-6).
- REQ-3, REQ-4: markStale only touches rows still SCHEDULED, and a reschedule moves the start into the future (CO-3).
- REQ-5: markStale updates only `status = 'SCHEDULED'` rows (CO-3).
- REQ-6: the guard accepts STALE for cancel, complete and no-show (CO-4, CO-8).
- REQ-7: the guard accepts STALE for reschedule, and `move()` sets SCHEDULED (CO-3, CO-4).
- REQ-9: the event actor is `system` (C7).
- REQ-10, REQ-11: Stale is labelled in the badge and offered in the list filter (CO-7).

## Assumed
None. Defaults taken: the constant names `SYSTEM_ACTOR` and `STALE_SWEEP_MS` follow the neighbouring constant style. Trade-off: they are local to their files rather than shared.

## Unknown
- U-1 (blocking): REQ-2 and REQ-8 need the migration CO-2 on a sensitive path; see Q-01.
- U-2 (non-blocking): REQ-12 needs the companion glossary edit CO-11; see Q-02.

## Open questions
- Q-01 (blocking, tech lead, for U-1): may this run add `db/migrations/<UTC timestamp>__add_stale_status.sql`, which widens the status and event CHECKs with STALE? Default: yes, as designed.
- Q-02 (non-blocking, tech lead, for U-2): who updates the companion's context/glossary.md? Default: the tech lead, when the PR is merged.
