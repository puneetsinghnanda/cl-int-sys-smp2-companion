---
schema: change.v1
run_id: run_01M8E1NVV8GFP3FDJ5XQMWQ952
step: impl
attempt: 2
skill: implement@1.4.0
inputs:
  - path: artifacts/HMS-34/run_01M8E1NVV8GFP3FDJ5XQMWQ952/design.md
    sha256: 49e40dd8e05c2f3d0fe0da3f188d2de68b5fc4abce7931c55896c0850ac3d324
  - path: artifacts/HMS-34/run_01M8E1NVV8GFP3FDJ5XQMWQ952/split.md
    sha256: 75a7d123b0e9c6c2abede2df9d1cedfa4c8cb1c4885b7f3d7d41dbe935908603
  - path: context/conventions.md
    sha256: dc53d94ec34eb193eeb06fdd0db19eb86a24c29f7d3c434e7c212d61038ca852
  - path: context/generated/module-map.json
    sha256: 4158c8fcd239c22cb8f1029dd11f61d8839867ff8088bd9ab76197e2db96db4b
pack_sha:
  sample-system: 7681f87749883d71abb4011d118366ca1242e1cc
code:
  - repo: sample-system
    base_sha: 7681f87749883d71abb4011d118366ca1242e1cc
    head_sha: 1b5065f1c766d39c6eecfea1447963a2afbdeab2
    diff_sha256: b66072d6e3389ff512fe31a6edb4454005d61540129e77b75697396cc344ba20
    files:
      - api/src/repositories/appointmentRepo.ts
      - api/src/server.ts
      - api/src/services/appointmentService.ts
      - api/src/types.ts
      - db/migrations/20261127T090000__appointment_stale.sql
      - web/src/api/types.ts
      - web/src/lib/format.ts
      - web/src/pages/AppointmentDetailPage.tsx
claims:
  - id: C1
    text: "The tech lead approved adding the migration: `add the migration as designed`."
    cites: ["runs/run_01M8E1NVV8GFP3FDJ5XQMWQ952/open-questions.md#q-01"]
  - id: C2
    text: "Database changes are new SQL files named `YYYYMMDDTHHMMSS__what.sql`."
    cites: ["context/conventions.md#database-changes"]
  - id: C3
    text: "The status type `AppointmentStatus` is declared in api/src/types.ts."
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/types.ts:AppointmentStatus"]
  - id: C4
    text: "Appointment SQL lives in `AppointmentRepo`."
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/repositories/appointmentRepo.ts:AppointmentRepo"]
  - id: C5
    text: "Appointment rules live in `AppointmentService`."
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/services/appointmentService.ts:AppointmentService"]
  - id: C6
    text: "The one-day constant `DAY_MS` is in api/src/lib/time.ts."
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/lib/time.ts:DAY_MS"]
  - id: C7
    text: "The API process stops through `shutdown` in api/src/server.ts."
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/server.ts:shutdown"]
  - id: C8
    text: "The web status type `AppointmentStatus` is in web/src/api/types.ts."
    cites: ["context/generated/module-map.json#sym:sample-system:web/src/api/types.ts:AppointmentStatus"]
  - id: C9
    text: "The web status labels are in `web/src/lib/format.ts`."
    cites: ["artifacts/HMS-34/run_01M8E1NVV8GFP3FDJ5XQMWQ952/design.md#claim:C9"]
  - id: C10
    text: "Appointment actions are rendered by `AppointmentDetailPage`."
    cites: ["context/generated/module-map.json#sym:sample-system:web/src/pages/AppointmentDetailPage.tsx:AppointmentDetailPage"]
  - id: C11
    text: "The job runs `Every hour`."
    cites: ["runs/run_01M8E1NVV8GFP3FDJ5XQMWQ952/ide-answers.md#a-08"]
  - id: C12
    text: "The job's changes are recorded with actor `system`."
    cites: ["runs/run_01M8E1NVV8GFP3FDJ5XQMWQ952/ide-answers.md#a-04"]
  - id: C13
    text: "Logs carry method, route and `error message` only."
    cites: ["context/conventions.md#logging-and-data"]
trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-10, REQ-11, REQ-12, REQ-13]
known: [CO-1, CO-2, CO-3, CO-4, CO-5, CO-6, CO-7, CO-8]
assumed: [U-2]
unknown: []
questions: []
summary: "Adds the STALE appointment status (migration, API and web types, label), an hourly in-process job that marks Scheduled appointments Stale 24h after their start with a STALE event by actor system, and lets staff complete, no-show, cancel or reschedule a Stale appointment (reschedule makes it Scheduled again)."
---
# Change: HMS-34

## Summary
The tech lead approved the migration (C1), so this attempt builds the whole design:
- CO-2: a new migration replaces the `appointment.status` and `appointment_event.event` CHECKs so both allow `STALE`. The overlap constraints still cover SCHEDULED rows only.
- CO-1, CO-6: `STALE` is added to the API and web `AppointmentStatus` types and to the API's status list, so the list filter accepts it (EP-1).
- CO-3: `AppointmentRepo.markStale(cutoff, actor)` updates SCHEDULED rows with `starts_at <= cutoff` to STALE and returns their ids. `move()` now also sets the status to SCHEDULED, so a rescheduled Stale appointment is Scheduled again (REQ-13).
- CO-4: `AppointmentService.markStale(actor)` takes the cutoff from the injected clock minus `DAY_MS`, then marks the rows and writes one `STALE` event each in one transaction. The open-state guard now accepts SCHEDULED or STALE, so complete, no-show, cancel and reschedule work on Stale appointments (EP-3 to EP-6).
- CO-5: `api/src/server.ts` runs the job once at listen and then every hour with actor `system`. It logs only the error message on failure and clears the timer in `shutdown`.
- CO-7, CO-8: the web label `Stale`. The detail page keeps its actions for STALE and labels the STALE history event "Marked stale".

CO-9 (glossary) is a companion-context edit for the tech lead; no product file serves it, so REQ-9 is not in this change's trace.

## Changes
- db/migrations/20261127T090000__appointment_stale.sql: CO-2.
- api/src/types.ts: CO-1.
- api/src/repositories/appointmentRepo.ts: CO-3.
- api/src/services/appointmentService.ts: CO-4.
- api/src/server.ts: CO-5.
- web/src/api/types.ts: CO-6.
- web/src/lib/format.ts: CO-7.
- web/src/pages/AppointmentDetailPage.tsx: CO-8.
- No tests were added: the design asks for no regression tests; the unit-test step covers S-1 to S-4.

## Known
- CO-1 to CO-8: each built where the module map, design claims and answers place it (C2 to C12).

## Assumed
- U-2: the inline CHECKs created in `20261016T090300__create_appointments.sql` have Postgres's default names `appointment_status_check` and `appointment_event_event_check`. Basis: Postgres names an unnamed column CHECK `<table>_<column>_check`. If a name differs, the migration fails at deploy, and the verifier's migration run would show it.

## Unknown
None.

## Open questions
None. The migration question was answered by the tech lead (C1).
