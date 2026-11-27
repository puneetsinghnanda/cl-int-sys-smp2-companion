---
schema: change.v1
run_id: run_01M8E7Q99RKS5X0BN20RNDJM8C
step: impl
attempt: 2
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
    head_sha: b8528a20749f339298cce9055c0f9954ba39e625
    diff_sha256: 8c4b0e773b7624e178615d1bbcc3364947b128b545dfa321fe2926a6c9634fba
    files:
      - api/src/repositories/appointmentRepo.ts
      - api/src/server.ts
      - api/src/services/appointmentService.ts
      - api/src/types.ts
      - db/migrations/20261127T104500__add_stale_status.sql
      - web/src/api/types.ts
      - web/src/lib/format.ts
      - web/src/pages/AppointmentDetailPage.tsx
claims:
  - id: C1
    text: "The change limits mark `**/migrations/**` as a sensitive path."
    cites: ["tsl.yaml#L37"]
  - id: C2
    text: "The tech lead approved the migration: `Yes, add the migration as designed`."
    cites: ["runs/run_01M8E7Q99RKS5X0BN20RNDJM8C/open-questions.md#q-01"]
  - id: C3
    text: "The original table restricts status with `CHECK (status IN ('SCHEDULED', 'COMPLETED', 'CANCELLED', 'NO_SHOW'))`, which the new migration widens with STALE."
    cites: ["sample-system:db/migrations/20261016T090300__create_appointments.sql#L10"]
  - id: C4
    text: "The original event column is restricted with `event IN ('BOOKED', 'RESCHEDULED', 'CANCELLED', 'COMPLETED', 'NO_SHOW')`, which the new migration widens with STALE."
    cites: ["sample-system:db/migrations/20261016T090300__create_appointments.sql#L35"]
  - id: C5
    text: "Migrations are new SQL files named `YYYYMMDDTHHMMSS__what.sql`."
    cites: ["context/conventions.md#database-changes"]
  - id: C6
    text: "The companion glossary is updated by the tech lead: `Tech lead edits it when the PR is merged`."
    cites: ["runs/run_01M8E7Q99RKS5X0BN20RNDJM8C/open-questions.md#q-02"]
  - id: C7
    text: "The sweep runs as an `In-process timer`."
    cites: ["runs/run_01M8E7Q99RKS5X0BN20RNDJM8C/ide-answers.md#a-08"]
  - id: C8
    text: "The sweep runs `Every hour`."
    cites: ["runs/run_01M8E7Q99RKS5X0BN20RNDJM8C/ide-answers.md#a-09"]
  - id: C9
    text: "The Stale change is recorded with `Event, actor system`."
    cites: ["runs/run_01M8E7Q99RKS5X0BN20RNDJM8C/ide-answers.md#a-03"]
  - id: C10
    text: "Logs carry method, route and `error message only`."
    cites: ["context/conventions.md#logging-and-data"]
trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10, REQ-11, REQ-12]
known: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10, REQ-11, REQ-12]
assumed: [ASM-1]
unknown: []
questions: []
summary: "Adds the STALE status end to end: a migration widening the status and event CHECKs, AppointmentRepo/AppointmentService.markStale (actor system), an hourly in-process sweep started in server.ts, staff close/reschedule of Stale appointments, and Stale in the web list, filter, badge and history. The companion glossary is left to the tech lead at merge."
---
# Change: HMS-34

## Summary
Attempt 2 adds the migration CO-2, which the tech lead approved in Q-01 (C2). It sits under the sensitive path `**/migrations/**` (C1). Everything else is unchanged from attempt 1:
- **CO-2:** a new migration, `20261127T104500__add_stale_status.sql` (named per C5), drops and re-adds the appointment status CHECK and the appointment_event event CHECK, each with STALE added (C3, C4).
- **CO-1:** the API status type and `APPOINTMENT_STATUSES` gain STALE.
- **CO-3:** `AppointmentRepo.markStale(cutoff, actor)` moves Scheduled rows that started at or before the cutoff to STALE, with one `STALE` event each. `move()` sets the status back to SCHEDULED.
- **CO-4:** `AppointmentService.markStale()` uses the cutoff `clock() - DAY_MS` and the actor `system` (C9) in one transaction. The `scheduled()` guard accepts SCHEDULED or STALE.
- **CO-5:** `api/src/server.ts` runs the sweep once after listen, then every hour (C7, C8), and clears the timer on shutdown. Failures log the error message only (C10).
- **CO-6, CO-7, CO-8:** the web types gain STALE and the labels gain `Stale`, which feeds the badge and the filter. The appointment page shows its actions and the `Marked stale` history label for Stale appointments.
- **CO-11:** the glossary is not edited here. The tech lead updates the companion's context/glossary.md at merge, as Q-02 decided (C6).

## Changes
- api/src/types.ts — CO-1: STALE in `AppointmentStatus` and `APPOINTMENT_STATUSES`.
- api/src/repositories/appointmentRepo.ts — CO-3: new `markStale()`; `move()` sets status SCHEDULED.
- api/src/services/appointmentService.ts — CO-4: new `markStale()`, `SYSTEM_ACTOR`, and the guard accepts STALE.
- api/src/server.ts — CO-5: hourly Stale sweep timer, run once at start and cleared on shutdown.
- db/migrations/20261127T104500__add_stale_status.sql — CO-2: STALE in the status and event CHECKs.
- web/src/api/types.ts — CO-6: STALE in the web `AppointmentStatus`.
- web/src/lib/format.ts — CO-7: `STALE: 'Stale'` in STATUS_LABEL.
- web/src/pages/AppointmentDetailPage.tsx — CO-8: actions for Stale, and the `Marked stale` history label.
- No tests added: the design asks for no regression tests; the unit-test step covers them.

## Known
- REQ-1: STALE added to the types and the database checks (CO-1, CO-2, CO-6).
- REQ-2: markStale marks rows still SCHEDULED at or before now minus DAY_MS; the migration lets STALE be stored (CO-2, CO-3, CO-4).
- REQ-3, REQ-4: only rows still SCHEDULED are touched, and a reschedule moves the start into the future (CO-3).
- REQ-5: markStale updates only `status = 'SCHEDULED'` rows (CO-3).
- REQ-6: the guard accepts STALE for cancel, complete and no-show (CO-4, CO-8).
- REQ-7: the guard accepts STALE for reschedule, and `move()` sets SCHEDULED (CO-3, CO-4).
- REQ-8: one `STALE` appointment_event row per marked appointment, allowed by the widened event CHECK (CO-2, CO-3).
- REQ-9: the event actor is `system` (C9).
- REQ-10, REQ-11: Stale is labelled in the badge and offered in the list filter (CO-7).
- REQ-12: the tech lead edits context/glossary.md when the PR is merged (C6).

## Assumed
- ASM-1: the inline CHECKs have Postgres's default names, `appointment_status_check` and `appointment_event_event_check` (Postgres names an inline column CHECK `<table>_<column>_check`). Basis: the create migration declares both CHECKs without names (C3, C4). Implement cannot run the database, so the verifier's migration run confirms it.
- Defaults: the constant names `SYSTEM_ACTOR` and `STALE_SWEEP_MS` follow the neighbouring constant style. Trade-off: they are local to their files rather than shared.

## Unknown
None.

## Open questions
None open. Answered before this attempt: Q-01 (approve the migration: yes), Q-02 (glossary: the tech lead edits it at merge), Q-03 (U-1: same as Q-01).
