---
schema: design.v1
run_id: run_01M8E1NVV8GFP3FDJ5XQMWQ952
step: design
attempt: 1
skill: design@1.10.0
inputs:
  - path: artifacts/HMS-34/run_01M8E1NVV8GFP3FDJ5XQMWQ952/split.md
    sha256: 75a7d123b0e9c6c2abede2df9d1cedfa4c8cb1c4885b7f3d7d41dbe935908603
  - path: artifacts/HMS-34/run_01M8E1NVV8GFP3FDJ5XQMWQ952/req.md
    sha256: 47389008525cbbf3efa944cfba39e9e05ef3931e2e9a26a9acf835126f435647
  - path: context/intent-baseline.md
    sha256: 9bfd821aeb2fb9524863e346707e049e4fcfb949a6e55b90223a79194946a215
  - path: context/architecture.md
    sha256: 9faab093d10933a46f6473b25c80728f8745e597100e650cd2537bbafb2b595a
  - path: context/generated/module-map.json
    sha256: 4158c8fcd239c22cb8f1029dd11f61d8839867ff8088bd9ab76197e2db96db4b
  - path: context/generated/api-inventory.json
    sha256: ba024ab1b4b4151d26ce1afa77bd2dab1cec645ea737ce37f98e0e8674f795e9
pack_sha:
  sample-system: 7681f87749883d71abb4011d118366ca1242e1cc
claims:
  - id: C1
    text: "The API's appointment status type `AppointmentStatus` is declared in api/src/types.ts."
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/types.ts:AppointmentStatus"]
  - id: C2
    text: "Appointment SQL lives in `AppointmentRepo`."
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/repositories/appointmentRepo.ts:AppointmentRepo"]
  - id: C3
    text: "Appointment rules and transactions live in `AppointmentService`."
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/services/appointmentService.ts:AppointmentService"]
  - id: C4
    text: "The API process stops through `shutdown` in api/src/server.ts."
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/server.ts:shutdown"]
  - id: C5
    text: "The web app mirrors the status type as `AppointmentStatus` in web/src/api/types.ts."
    cites: ["context/generated/module-map.json#sym:sample-system:web/src/api/types.ts:AppointmentStatus"]
  - id: C6
    text: "Reschedule, cancel, complete and no-show actions are rendered by `AppointmentDetailPage`."
    cites: ["context/generated/module-map.json#sym:sample-system:web/src/pages/AppointmentDetailPage.tsx:AppointmentDetailPage"]
  - id: C7
    text: "Statuses are shown with the `StatusBadge` component."
    cites: ["context/generated/module-map.json#sym:sample-system:web/src/components/StatusBadge.tsx:StatusBadge"]
  - id: C8
    text: "Appointment rows are held in the `appointment` table."
    cites: ["context/generated/db-schema.json#table:appointment"]
  - id: C9
    text: "The web status labels STATUS_LABEL are in `web/src/lib/format.ts`."
    cites: ["runs/run_01M8E1NVV8GFP3FDJ5XQMWQ952/ide-answers.md#a-10", "context/generated/module-map.json#sym:sample-system:web/src/lib/format.ts:dateTime"]
  - id: C10
    text: "Appointment history is held in the `appointment_event` table."
    cites: ["context/generated/db-schema.json#table:appointment_event"]
  - id: C11
    text: "No `background jobs` exist yet."
    cites: ["context/architecture.md#open-points"]
  - id: C12
    text: "Database changes are new SQL files named `YYYYMMDDTHHMMSS__what.sql`."
    cites: ["context/conventions.md#database-changes"]
  - id: C13
    text: "Database `exclusion constraints` forbid overlapping Scheduled appointments, and the service turns violations into 409."
    cites: ["context/architecture.md#double-booking"]
  - id: C14
    text: "Services get an injectable clock from `createContext`, so tests can fix now."
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/services/index.ts:createContext"]
  - id: C15
    text: "All appointment endpoints are routed in `api/src/routes/appointments.ts`."
    cites: ["context/generated/api-inventory.json#http:GET:/api/appointments", "context/generated/api-inventory.json#http:POST:/api/appointments/:id/reschedule"]
  - id: C16
    text: "A one-day constant `DAY_MS` exists in api/src/lib/time.ts."
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/lib/time.ts:DAY_MS"]
  - id: C17
    text: "The new status and history event value is `STALE for both`."
    cites: ["runs/run_01M8E1NVV8GFP3FDJ5XQMWQ952/ide-answers.md#a-07"]
  - id: C18
    text: "The job runs `Every hour`."
    cites: ["runs/run_01M8E1NVV8GFP3FDJ5XQMWQ952/ide-answers.md#a-08"]
  - id: C19
    text: "The job is a `Timer inside the API process`."
    cites: ["runs/run_01M8E1NVV8GFP3FDJ5XQMWQ952/ide-answers.md#a-09"]
  - id: C20
    text: "The Stale change is recorded with actor `system`."
    cites: ["runs/run_01M8E1NVV8GFP3FDJ5XQMWQ952/ide-answers.md#a-04"]
trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10, REQ-11, REQ-12, REQ-13]
known: [S-1, S-2, S-3, S-4, S-5, CO-1, CO-2, CO-3, CO-4, CO-5, CO-6, CO-7, CO-8, CO-9, EP-1, EP-2, EP-3, EP-4, EP-5, EP-6]
assumed: []
unknown: []
questions: []
components:
  - id: CO-1
    name: "AppointmentStatus type (API): add STALE"
    module: module:sample-system:api
    change: changed
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/types.ts:AppointmentStatus", "runs/run_01M8E1NVV8GFP3FDJ5XQMWQ952/ide-answers.md#a-07"]
  - id: CO-2
    name: "Migration <UTC timestamp>__appointment_stale.sql: allow STALE in appointment.status and appointment_event.event"
    module: module:sample-system:db
    change: new
    cites: ["context/conventions.md#database-changes", "context/generated/db-schema.json#table:appointment", "context/generated/db-schema.json#table:appointment_event", "runs/run_01M8E1NVV8GFP3FDJ5XQMWQ952/ide-answers.md#a-07"]
  - id: CO-3
    name: "AppointmentRepo: new markStale(cutoff, actor); move() also sets status SCHEDULED"
    module: module:sample-system:api
    change: changed
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/repositories/appointmentRepo.ts:AppointmentRepo", "runs/run_01M8E1NVV8GFP3FDJ5XQMWQ952/ide-answers.md#a-06"]
  - id: CO-4
    name: "AppointmentService: open-state guard accepts SCHEDULED or STALE; new markStale() using clock and DAY_MS"
    module: module:sample-system:api
    change: changed
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/services/appointmentService.ts:AppointmentService", "context/generated/module-map.json#sym:sample-system:api/src/lib/time.ts:DAY_MS", "runs/run_01M8E1NVV8GFP3FDJ5XQMWQ952/ide-answers.md#a-01", "runs/run_01M8E1NVV8GFP3FDJ5XQMWQ952/ide-answers.md#a-03"]
  - id: CO-5
    name: "API server: hourly Stale timer started at listen, cleared in shutdown"
    module: module:sample-system:api
    change: changed
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/server.ts:shutdown", "runs/run_01M8E1NVV8GFP3FDJ5XQMWQ952/ide-answers.md#a-02", "runs/run_01M8E1NVV8GFP3FDJ5XQMWQ952/ide-answers.md#a-08", "runs/run_01M8E1NVV8GFP3FDJ5XQMWQ952/ide-answers.md#a-09"]
  - id: CO-6
    name: "AppointmentStatus type (web): add STALE"
    module: module:sample-system:web
    change: changed
    cites: ["context/generated/module-map.json#sym:sample-system:web/src/api/types.ts:AppointmentStatus", "runs/run_01M8E1NVV8GFP3FDJ5XQMWQ952/ide-answers.md#a-07"]
  - id: CO-7
    name: "STATUS_LABEL in web/src/lib/format.ts: label Stale (shown by StatusBadge)"
    module: module:sample-system:web
    change: changed
    cites: ["runs/run_01M8E1NVV8GFP3FDJ5XQMWQ952/ide-answers.md#a-10", "context/generated/module-map.json#sym:sample-system:web/src/components/StatusBadge.tsx:StatusBadge"]
  - id: CO-8
    name: "AppointmentDetailPage: show reschedule, cancel, complete and no-show for STALE too; history label for the STALE event"
    module: module:sample-system:web
    change: changed
    cites: ["context/generated/module-map.json#sym:sample-system:web/src/pages/AppointmentDetailPage.tsx:AppointmentDetailPage", "runs/run_01M8E1NVV8GFP3FDJ5XQMWQ952/ide-answers.md#a-03"]
  - id: CO-9
    name: "Glossary entry Stale in the companion's context/glossary.md (tech lead edits the context section)"
    module: module:sample-system:.
    change: new
    cites: ["runs/run_01M8E1NVV8GFP3FDJ5XQMWQ952/ide-answers.md#a-05"]
endpoints:
  - id: EP-1
    method: GET
    path: /api/appointments
    change: changed
    fields:
      - {name: "status (query)", type: string, nullable: true, format: "one of SCHEDULED, COMPLETED, CANCELLED, NO_SHOW, STALE", example: "STALE", status: known}
      - {name: "items[].status", type: string, nullable: false, format: "one of SCHEDULED, COMPLETED, CANCELLED, NO_SHOW, STALE", example: "STALE", status: known}
    cites: ["context/generated/api-inventory.json#http:GET:/api/appointments", "runs/run_01M8E1NVV8GFP3FDJ5XQMWQ952/ide-answers.md#a-07"]
  - id: EP-2
    method: GET
    path: /api/appointments/:id
    change: changed
    fields:
      - {name: status, type: string, nullable: false, format: "one of SCHEDULED, COMPLETED, CANCELLED, NO_SHOW, STALE", example: "STALE", status: known}
      - {name: "history[].event", type: string, nullable: false, format: "adds STALE", example: "STALE", status: known}
      - {name: "history[].actor", type: string, nullable: false, format: "system for the STALE event", example: "system", status: known}
      - {name: "history[].at", type: string, nullable: false, format: "ISO 8601 UTC instant (unchanged)", example: "2026-03-10T10:01:00.000Z", status: known}
    cites: ["context/generated/api-inventory.json#http:GET:/api/appointments/:id", "runs/run_01M8E1NVV8GFP3FDJ5XQMWQ952/ide-answers.md#a-07", "runs/run_01M8E1NVV8GFP3FDJ5XQMWQ952/ide-answers.md#a-04"]
  - id: EP-3
    method: POST
    path: /api/appointments/:id/complete
    change: changed
    fields:
      - {name: status, type: string, nullable: false, format: "COMPLETED; now also accepted from STALE", example: "COMPLETED", status: known}
    cites: ["context/generated/api-inventory.json#http:POST:/api/appointments/:id/complete", "runs/run_01M8E1NVV8GFP3FDJ5XQMWQ952/ide-answers.md#a-03"]
  - id: EP-4
    method: POST
    path: /api/appointments/:id/no-show
    change: changed
    fields:
      - {name: status, type: string, nullable: false, format: "NO_SHOW; now also accepted from STALE", example: "NO_SHOW", status: known}
    cites: ["context/generated/api-inventory.json#http:POST:/api/appointments/:id/no-show", "runs/run_01M8E1NVV8GFP3FDJ5XQMWQ952/ide-answers.md#a-03"]
  - id: EP-5
    method: POST
    path: /api/appointments/:id/cancel
    change: changed
    fields:
      - {name: status, type: string, nullable: false, format: "CANCELLED; now also accepted from STALE", example: "CANCELLED", status: known}
    cites: ["context/generated/api-inventory.json#http:POST:/api/appointments/:id/cancel", "runs/run_01M8E1NVV8GFP3FDJ5XQMWQ952/ide-answers.md#a-03"]
  - id: EP-6
    method: POST
    path: /api/appointments/:id/reschedule
    change: changed
    fields:
      - {name: status, type: string, nullable: false, format: "SCHEDULED after rescheduling, also from STALE", example: "SCHEDULED", status: known}
    cites: ["context/generated/api-inventory.json#http:POST:/api/appointments/:id/reschedule", "runs/run_01M8E1NVV8GFP3FDJ5XQMWQ952/ide-answers.md#a-03", "runs/run_01M8E1NVV8GFP3FDJ5XQMWQ952/ide-answers.md#a-06"]
answers:
  - id: A-07
    kind: question
    about: Q-01
    answer: "STALE for both (proposed default)"
    decides: [CO-1, CO-2, CO-6, EP-1, EP-2, C17]
  - id: A-08
    kind: question
    about: Q-02
    answer: "Every hour"
    decides: [CO-5, C18]
  - id: A-09
    kind: question
    about: Q-03
    answer: "Timer inside the API process (proposed default)"
    decides: [CO-5, C19]
  - id: A-10
    kind: assumption
    about: C9
    answer: "Yes, confirm"
    decides: [C9, CO-7]
---
# Design: HMS-34

## Overview
A new appointment status `STALE` (A-07) is added to the database CHECKs, the API and web types, and
the web labels. `AppointmentService.markStale()` moves every SCHEDULED appointment whose start is at
or before now minus `DAY_MS` (24h after start time, A-01) to STALE in one UPDATE, and writes one
`STALE` appointment_event per row with actor `system` (A-04), in one transaction. A timer inside
the API process (A-09) calls it every hour (A-08), starting when the server listens and stopping in
`shutdown`. STALE stays an open state (A-03): the service guard that today allows only SCHEDULED
also allows STALE, so complete, no-show, cancel and reschedule work on it, and reschedule sets the
status back to SCHEDULED (A-06).

Trade-offs:
- In-process timer: no new infrastructure, but with more than one API instance each runs the job; the UPDATE's `WHERE status = 'SCHEDULED'` keeps it idempotent, so a double run marks nothing twice.
- Hourly run: few queries, but an appointment may show as Scheduled for up to an hour after its 24h window ends; tests call `markStale()` with a fixed clock, so the interval does not affect the ACs.
- Bulk UPDATE … RETURNING then event inserts: one statement per run instead of a row-by-row loop; a staff action at the same moment either locks the row first (then it is no longer SCHEDULED) or waits.
- Rescheduling a STALE row back to SCHEDULED re-enters the overlap `exclusion constraints`, so a clash still gives 409 as today.
- Interval is a constant in the server, not an env variable: simpler, but changing it needs a deploy.

## Components
- CO-1 (changed) `AppointmentStatus` in api/src/types.ts: add `STALE`; the status list used by the list filter gains it too.
- CO-2 (new) migration `<UTC timestamp>__appointment_stale.sql` in db/migrations: replace the `appointment.status` CHECK and the `appointment_event.event` CHECK so both allow `STALE`. The overlap constraints stay `WHERE status = 'SCHEDULED'`.
- CO-3 (changed) `AppointmentRepo`: new `markStale(cutoff, actor)` updating SCHEDULED rows with `starts_at <= cutoff` to STALE and returning their ids; `move()` also sets `status = 'SCHEDULED'`.
- CO-4 (changed) `AppointmentService`: the open-state guard accepts SCHEDULED or STALE; new `markStale()` computes the cutoff from the injected clock minus `DAY_MS` and writes the `STALE` events with actor `system`.
- CO-5 (changed) api/src/server.ts: start an hourly timer after `listen` that calls `markStale()`, log a failure without request data, and clear the timer in `shutdown`.
- CO-6 (changed) `AppointmentStatus` in web/src/api/types.ts: add `STALE`.
- CO-7 (changed) STATUS_LABEL in web/src/lib/format.ts: label `Stale`, shown by `StatusBadge`.
- CO-8 (changed) `AppointmentDetailPage`: the action buttons show for SCHEDULED or STALE; the history list labels the `STALE` event.
- CO-9 (new) a `Stale` entry in the companion's context/glossary.md, added by the tech lead (the context owner), not by the product change.
- EP-1 (changed) GET /api/appointments: query `status` accepts `STALE`; `items[].status` may be `STALE`.
- EP-2 (changed) GET /api/appointments/:id: `status` may be `STALE`; `history[].event` may be `STALE` with `history[].actor` = `system`; `history[].at` stays an ISO 8601 UTC instant.
- EP-3 (changed) POST /api/appointments/:id/complete: also accepted from STALE; returns `COMPLETED`.
- EP-4 (changed) POST /api/appointments/:id/no-show: also accepted from STALE; returns `NO_SHOW`.
- EP-5 (changed) POST /api/appointments/:id/cancel: also accepted from STALE; returns `CANCELLED` with the reason.
- EP-6 (changed) POST /api/appointments/:id/reschedule: also accepted from STALE; returns `SCHEDULED` at the new time.

Story map: S-1 → CO-1, CO-2, CO-3, CO-4, CO-5, EP-2. S-2 → CO-3, CO-4 (the UPDATE only touches SCHEDULED rows past the cutoff). S-3 → CO-4, CO-7, CO-8, EP-3, EP-4, EP-5. S-4 → CO-3, CO-4, CO-8, EP-6. S-5 → CO-9.

## Known
- S-1 to S-5: each story is served by the components in the story map.
- CO-1, CO-6: the status types exist in the module map; the value is from A-07.
- CO-2: new SQL file per the migration convention; tables from the db schema.
- CO-3, CO-4: existing repo and service symbols; rules from A-01, A-03, A-04, A-06.
- CO-5: `shutdown` exists in api/src/server.ts; interval and placement from A-08 and A-09.
- CO-7: location confirmed in A-10.
- CO-8: the page symbol exists in the module map.
- CO-9: decided by A-05.
- EP-1 to EP-6: all exist in the api inventory, so they are changed, not new.

## Assumed
None.

## Unknown
None.

## Open questions
None open.

Answered in the IDE:
- A-07 (Q-01): value for the new status and its history event → STALE for both. Decides CO-1, CO-2, CO-6, EP-1, EP-2, C17.
- A-08 (Q-02): how often the job runs → Every hour. Decides CO-5, C18.
- A-09 (Q-03): where the job runs → Timer inside the API process. Decides CO-5, C19.
- A-10 (C9): STATUS_LABEL is in web/src/lib/format.ts → Yes, confirm. Decides C9, CO-7.
