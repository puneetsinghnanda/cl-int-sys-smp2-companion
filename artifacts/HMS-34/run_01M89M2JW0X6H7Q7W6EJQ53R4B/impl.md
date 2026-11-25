---
schema: change.v1
run_id: run_01M89M2JW0X6H7Q7W6EJQ53R4B
step: impl
attempt: 1
skill: implement@1.4.0
inputs:
  - path: artifacts/HMS-34/run_01M89M2JW0X6H7Q7W6EJQ53R4B/design.md
    sha256: a59526ca2cc3b301ca9003ed980419374e57a89a462f280a68e7bfaa649c7960
  - path: artifacts/HMS-34/run_01M89M2JW0X6H7Q7W6EJQ53R4B/split.md
    sha256: ffdc205028446dc36d5b0a750fdfe01f2de2f7347891a727faae856bcb6899c4
  - path: context/conventions.md
    sha256: dc53d94ec34eb193eeb06fdd0db19eb86a24c29f7d3c434e7c212d61038ca852
  - path: context/generated/module-map.json
    sha256: 4158c8fcd239c22cb8f1029dd11f61d8839867ff8088bd9ab76197e2db96db4b
pack_sha:
  sample-system: 7681f87749883d71abb4011d118366ca1242e1cc
claims:
  - id: C1
    text: "The new `Migration adding STALE to both status CHECKs` is built as CO-1 asks."
    cites: ["artifacts/HMS-34/run_01M89M2JW0X6H7Q7W6EJQ53R4B/design.md#item:CO-1"]
  - id: C2
    text: "The API's `AppointmentStatus` gains STALE, as CO-2 asks."
    cites: ["artifacts/HMS-34/run_01M89M2JW0X6H7Q7W6EJQ53R4B/design.md#item:CO-2"]
  - id: C3
    text: "AppointmentRepo gains `markStale` and move() sets SCHEDULED, as CO-3 asks."
    cites: ["artifacts/HMS-34/run_01M89M2JW0X6H7Q7W6EJQ53R4B/design.md#item:CO-3"]
  - id: C4
    text: "AppointmentService gains `markStale` and its actions accept STALE, as CO-4 asks."
    cites: ["artifacts/HMS-34/run_01M89M2JW0X6H7Q7W6EJQ53R4B/design.md#item:CO-4"]
  - id: C5
    text: "The new job `api/src/jobs/staleJob.ts` runs hourly with actor system, as CO-5 asks."
    cites: ["artifacts/HMS-34/run_01M89M2JW0X6H7Q7W6EJQ53R4B/design.md#item:CO-5"]
  - id: C6
    text: "`server.ts` starts the job after listen and stops it in shutdown, as CO-6 asks."
    cites: ["artifacts/HMS-34/run_01M89M2JW0X6H7Q7W6EJQ53R4B/design.md#item:CO-6"]
  - id: C7
    text: "The web app's `AppointmentStatus` gains STALE, as CO-7 asks."
    cites: ["artifacts/HMS-34/run_01M89M2JW0X6H7Q7W6EJQ53R4B/design.md#item:CO-7"]
  - id: C8
    text: "`STATUS_LABEL` gains STALE 'Stale', as CO-8 asks."
    cites: ["artifacts/HMS-34/run_01M89M2JW0X6H7Q7W6EJQ53R4B/design.md#item:CO-8"]
  - id: C9
    text: "`AppointmentDetailPage` shows the actions and a history label for STALE, as CO-9 asks."
    cites: ["artifacts/HMS-34/run_01M89M2JW0X6H7Q7W6EJQ53R4B/design.md#item:CO-9"]
  - id: C10
    text: "`web/src/styles.css` gains a .badge-stale rule, as CO-10 asks."
    cites: ["artifacts/HMS-34/run_01M89M2JW0X6H7Q7W6EJQ53R4B/design.md#item:CO-10"]
  - id: C11
    text: "The glossary entry is made in the `context pack` by its owner and is not part of this diff (CO-12)."
    cites: ["artifacts/HMS-34/run_01M89M2JW0X6H7Q7W6EJQ53R4B/design.md#item:CO-12"]
  - id: C12
    text: "The cutoff uses the existing constant `DAY_MS = 24 * 60 * MINUTE` from lib/time."
    cites: ["sample-system:api/src/lib/time.ts#L9"]
  - id: C13
    text: "The status CHECK is an unnamed column constraint, `CHECK (status IN ('SCHEDULED', 'COMPLETED', 'CANCELLED', 'NO_SHOW'))`, so Postgres names it appointment_status_check."
    cites: ["sample-system:db/migrations/20261016T090300__create_appointments.sql#L10"]
  - id: C14
    text: "The event CHECK is an unnamed column constraint, `CHECK (event IN ('BOOKED', 'RESCHEDULED', 'CANCELLED', 'COMPLETED', 'NO_SHOW'))`, so Postgres names it appointment_event_event_check."
    cites: ["sample-system:db/migrations/20261016T090300__create_appointments.sql#L35"]
  - id: C15
    text: "The new migration drops `appointment_status_check` by that name."
    cites: ["sample-system:db/migrations/20261125T120000__add_stale_status.sql#L3"]
trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10, REQ-11, REQ-12, REQ-13, REQ-14, REQ-16]
known: [S-1, S-2, S-3, S-4, S-5, S-7, CO-1]
assumed: []
unknown: []
questions: []
code:
  - repo: sample-system
    base_sha: 7681f87749883d71abb4011d118366ca1242e1cc
    head_sha: 1552e170b5788809a98b32ff3b5d5e0d838fdb39
    diff_sha256: 186c32f02e537ac0c0f4722be856c950e9f1b11c0a51e181f4d109fc1236f6eb
    files:
      - api/src/jobs/staleJob.ts
      - api/src/repositories/appointmentRepo.ts
      - api/src/server.ts
      - api/src/services/appointmentService.ts
      - api/src/types.ts
      - db/migrations/20261125T120000__add_stale_status.sql
      - web/src/api/types.ts
      - web/src/lib/format.ts
      - web/src/pages/AppointmentDetailPage.tsx
      - web/src/styles.css
summary: "Adds the STALE appointment status: DB CHECKs, API and web types and labels, an hourly in-process job that marks Scheduled appointments 24h past their start as Stale with actor 'system', and staff actions that still work on Stale (reschedule returns it to Scheduled)."
---
# Change: HMS-34

## Summary
The change builds design CO-1 to CO-10 (C1 to C10) and nothing more.
- A new migration adds STALE to both CHECK constraints.
- The API and web types and the labels gain STALE.
- An hourly job in the API process marks appointments that are still Scheduled at least 24h after their start time as Stale. It uses `DAY_MS` for the 24h (C12) and actor 'system', and writes one STALE history event per appointment in one transaction.
- The status check in AppointmentService.scheduled() now accepts STALE, so complete, no-show, cancel and reschedule work on a Stale appointment. A reschedule sets the status back to SCHEDULED in AppointmentRepo.move().
- The web appointment page shows its actions for Stale, starts the reschedule picker on today for a past appointment, and labels the event "Marked stale".

CO-11 needs no change. CO-12, the glossary entry for S-6/REQ-15, is made in the context pack and is outside this diff (C11). No tests were added: the design names no regression tests, and broad tests are the unit-test step's job.

## Changes
- db/migrations/20261125T120000__add_stale_status.sql (new): CO-1, adds STALE to the appointment status CHECK and the appointment_event event CHECK.
- api/src/types.ts: CO-2, adds STALE to AppointmentStatus and APPOINTMENT_STATUSES.
- api/src/repositories/appointmentRepo.ts: CO-3, adds markStale(cutoff, actor); move() also sets status SCHEDULED.
- api/src/services/appointmentService.ts: CO-4, adds markStale(actor) with cutoff = clock() minus DAY_MS; scheduled() accepts STALE.
- api/src/jobs/staleJob.ts (new): CO-5, startStaleJob runs at startup and every 60 minutes with actor 'system', logs only the error message, and returns a stop function.
- api/src/server.ts: CO-6, starts the job after listen from app.locals.ctx and stops it in shutdown.
- web/src/api/types.ts: CO-7, adds STALE to AppointmentStatus.
- web/src/lib/format.ts: CO-8, adds STALE: 'Stale' to STATUS_LABEL (badge label and list filter option).
- web/src/pages/AppointmentDetailPage.tsx: CO-9, actions for STALE, picker starts on today for a past appointment, and EVENT_LABEL STALE: 'Marked stale'.
- web/src/styles.css: CO-10, .badge-stale uses the warning colours.

## Known
- S-1: the job and the migration (CO-1, CO-3 to CO-6).
- S-2: the job's UPDATE only touches rows still SCHEDULED (CO-3).
- S-3: scheduled() accepts STALE, so complete, no-show and cancel work (CO-4, CO-9).
- S-4: reschedule from STALE sets SCHEDULED (CO-3, CO-4, CO-9).
- S-5: the STATUS_LABEL entry feeds the badge and the list filter; the dashboard is unchanged (CO-7, CO-8, CO-10).
- S-7: the patient page table shows the new label through STATUS_LABEL; the doctor page query is unchanged (CO-8).
- CO-1: confirmed. Both CHECKs are unnamed column constraints (C13, C14), which Postgres names table_column_check, and the migration drops them by those names (C15).

## Assumed
None.

## Unknown
None.

## Open questions
None.
