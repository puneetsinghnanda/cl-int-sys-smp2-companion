---
schema: design.v1
run_id: run_01M8E7Q99RKS5X0BN20RNDJM8C
step: design
attempt: 1
skill: design@1.10.0
inputs:
  - path: artifacts/HMS-34/run_01M8E7Q99RKS5X0BN20RNDJM8C/split.md
    sha256: de964b65dc63706622cc2da9b738e4978b15ad686429c50dcbb5d068f9d8336e
  - path: artifacts/HMS-34/run_01M8E7Q99RKS5X0BN20RNDJM8C/req.md
    sha256: f5e5e578aa75507ceb63ff21361a3ad8250648de451470e0318990ce245cbd09
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
    text: "The API status type `AppointmentStatus` is declared in api/src/types.ts."
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/types.ts:AppointmentStatus"]
  - id: C2
    text: "The API lists valid statuses in `APPOINTMENT_STATUSES`, which the list endpoint uses to validate the status filter."
    cites: ["sample-system:api/src/types.ts#L5", "sample-system:api/src/services/appointmentService.ts#L35"]
  - id: C3
    text: "The appointment table restricts status with `CHECK (status IN ('SCHEDULED', 'COMPLETED', 'CANCELLED', 'NO_SHOW'))`."
    cites: ["sample-system:db/migrations/20261016T090300__create_appointments.sql#L10"]
  - id: C4
    text: "appointment_event restricts its event column with `event IN ('BOOKED', 'RESCHEDULED', 'CANCELLED', 'COMPLETED', 'NO_SHOW')`."
    cites: ["sample-system:db/migrations/20261016T090300__create_appointments.sql#L35"]
  - id: C5
    text: "The overlap exclusion constraints apply only `WHERE (status = 'SCHEDULED')`."
    cites: ["sample-system:db/migrations/20261016T090300__create_appointments.sql#L21"]
  - id: C6
    text: "The service guard rejects any change when `current.status !== 'SCHEDULED'`."
    cites: ["sample-system:api/src/services/appointmentService.ts#L136"]
  - id: C7
    text: "Rescheduling moves the row with `UPDATE appointment SET starts_at = $2, ends_at = $3` and does not touch status."
    cites: ["sample-system:api/src/repositories/appointmentRepo.ts#L157"]
  - id: C8
    text: "`DAY_MS` (24 hours in milliseconds) is exported from api/src/lib/time.ts."
    cites: ["sample-system:api/src/lib/time.ts#L9"]
  - id: C9
    text: "`MINUTE` (one minute in milliseconds) is exported from api/src/lib/time.ts."
    cites: ["sample-system:api/src/lib/time.ts#L8"]
  - id: C10
    text: "The API process stops in `function shutdown()` in api/src/server.ts."
    cites: ["sample-system:api/src/server.ts#L13"]
  - id: C11
    text: "Web status labels come from `STATUS_LABEL` in web/src/lib/format.ts."
    cites: ["sample-system:web/src/lib/format.ts#L36"]
  - id: C12
    text: "The appointment list's status filter is built from `Object.entries(STATUS_LABEL)`."
    cites: ["sample-system:web/src/pages/AppointmentsPage.tsx#L34"]
  - id: C13
    text: "The status badge shows `STATUS_LABEL[status]` for appointment statuses."
    cites: ["sample-system:web/src/components/StatusBadge.tsx#L5"]
  - id: C14
    text: "The appointment page shows its actions only when `a.status === 'SCHEDULED'`."
    cites: ["sample-system:web/src/pages/AppointmentDetailPage.tsx#L44"]
  - id: C15
    text: "History event labels come from `EVENT_LABEL` in the appointment page."
    cites: ["sample-system:web/src/pages/AppointmentDetailPage.tsx#L13"]
  - id: C16
    text: "The web status type `AppointmentStatus` mirrors the API one in web/src/api/types.ts."
    cites: ["sample-system:web/src/api/types.ts#L3"]
  - id: C17
    text: "Database changes are new SQL files only, named `YYYYMMDDTHHMMSS__what.sql`."
    cites: ["context/conventions.md#database-changes"]
  - id: C18
    text: "`No background jobs exist yet`; the Stale sweep is the first one."
    cites: ["context/architecture.md#open-points"]
  - id: C19
    text: "The Stale sweep runs as an `In-process timer` in the API process."
    cites: ["runs/run_01M8E7Q99RKS5X0BN20RNDJM8C/ide-answers.md#a-08"]
  - id: C20
    text: "The timer runs the sweep `Every hour`."
    cites: ["runs/run_01M8E7Q99RKS5X0BN20RNDJM8C/ide-answers.md#a-09"]
  - id: C21
    text: "Assumed: the Stale glossary entry goes in the product's docs/PRD.md next to the appointment states, because the user chose to `Add entry`; implement must confirm."
    cites: ["artifacts/HMS-34/run_01M8E7Q99RKS5X0BN20RNDJM8C/req.md#item:REQ-12"]
  - id: C22
    text: "AppointmentRepo writes history rows with `INSERT INTO appointment_event`, including the actor."
    cites: ["sample-system:api/src/repositories/appointmentRepo.ts#L173"]
trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10, REQ-11, REQ-12]
known: [CO-1, CO-2, CO-3, CO-4, CO-5, CO-6, CO-7, CO-8, CO-9, CO-10, EP-1, EP-2, EP-3, EP-4, EP-5, EP-6]
assumed: [CO-11]
unknown: []
questions: []
components:
  - id: CO-1
    name: "API status type and list: add STALE to AppointmentStatus and APPOINTMENT_STATUSES (api/src/types.ts)"
    module: module:sample-system:api
    change: changed
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/types.ts:AppointmentStatus", "sample-system:api/src/types.ts#L5"]
  - id: CO-2
    name: "Migration adding STALE to the status and event CHECKs"
    module: module:sample-system:db
    change: new
    cites: ["sample-system:db/migrations/20261016T090300__create_appointments.sql#L10", "sample-system:db/migrations/20261016T090300__create_appointments.sql#L35", "context/conventions.md#database-changes"]
  - id: CO-3
    name: "AppointmentRepo: markStale() and move() back to SCHEDULED"
    module: module:sample-system:api
    change: changed
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/repositories/appointmentRepo.ts:AppointmentRepo", "sample-system:api/src/repositories/appointmentRepo.ts#L157", "sample-system:api/src/repositories/appointmentRepo.ts#L170"]
  - id: CO-4
    name: "AppointmentService: markStale() and guard accepting STALE"
    module: module:sample-system:api
    change: changed
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/services/appointmentService.ts:AppointmentService", "sample-system:api/src/services/appointmentService.ts#L136", "sample-system:api/src/lib/time.ts#L9", "runs/run_01M8E7Q99RKS5X0BN20RNDJM8C/ide-answers.md#a-08"]
  - id: CO-5
    name: "Hourly Stale sweep timer in api/src/server.ts"
    module: module:sample-system:api
    change: changed
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/server.ts:shutdown", "sample-system:api/src/lib/time.ts#L8", "runs/run_01M8E7Q99RKS5X0BN20RNDJM8C/ide-answers.md#a-08", "runs/run_01M8E7Q99RKS5X0BN20RNDJM8C/ide-answers.md#a-09"]
  - id: CO-6
    name: "Web status type: add STALE to AppointmentStatus (web/src/api/types.ts)"
    module: module:sample-system:web
    change: changed
    cites: ["context/generated/module-map.json#sym:sample-system:web/src/api/types.ts:AppointmentStatus"]
  - id: CO-7
    name: "STATUS_LABEL: add STALE: 'Stale' (web/src/lib/format.ts); this gives the badge label and the list filter option"
    module: module:sample-system:web
    change: changed
    cites: ["sample-system:web/src/lib/format.ts#L36"]
  - id: CO-8
    name: "AppointmentDetailPage: actions and history label for STALE"
    module: module:sample-system:web
    change: changed
    cites: ["context/generated/module-map.json#sym:sample-system:web/src/pages/AppointmentDetailPage.tsx:AppointmentDetailPage", "sample-system:web/src/pages/AppointmentDetailPage.tsx#L44", "sample-system:web/src/pages/AppointmentDetailPage.tsx#L13"]
  - id: CO-9
    name: "AppointmentsPage status filter (picks up Stale from STATUS_LABEL)"
    module: module:sample-system:web
    change: existing
    cites: ["context/generated/module-map.json#sym:sample-system:web/src/pages/AppointmentsPage.tsx:AppointmentsPage", "sample-system:web/src/pages/AppointmentsPage.tsx#L34"]
  - id: CO-10
    name: "StatusBadge (shows Stale from STATUS_LABEL)"
    module: module:sample-system:web
    change: existing
    cites: ["context/generated/module-map.json#sym:sample-system:web/src/components/StatusBadge.tsx:StatusBadge", "sample-system:web/src/components/StatusBadge.tsx#L5"]
  - id: CO-11
    name: "Glossary entry for Stale in docs/PRD.md (assumed location; implement must confirm)"
    module: module:sample-system:.
    change: changed
    cites: ["artifacts/HMS-34/run_01M8E7Q99RKS5X0BN20RNDJM8C/req.md#item:REQ-12", "runs/run_01M8E7Q99RKS5X0BN20RNDJM8C/ide-answers.md#a-05"]
endpoints:
  - id: EP-1
    method: GET
    path: /api/appointments
    change: changed
    fields:
      - {name: "status (query)", type: string enum, nullable: true, format: "one of SCHEDULED, COMPLETED, CANCELLED, NO_SHOW, STALE", example: "STALE", status: known}
      - {name: "items[].status", type: string enum, nullable: false, format: "one of SCHEDULED, COMPLETED, CANCELLED, NO_SHOW, STALE", example: "STALE", status: known}
    cites: ["context/generated/api-inventory.json#http:GET:/api/appointments", "sample-system:api/src/services/appointmentService.ts#L35"]
  - id: EP-2
    method: GET
    path: /api/appointments/:id
    change: changed
    fields:
      - {name: status, type: string enum, nullable: false, format: "one of SCHEDULED, COMPLETED, CANCELLED, NO_SHOW, STALE", example: "STALE", status: known}
      - {name: "history[].event", type: string, nullable: false, format: "one of BOOKED, RESCHEDULED, CANCELLED, COMPLETED, NO_SHOW, STALE", example: "STALE", status: known}
      - {name: "history[].actor", type: string, nullable: false, format: "system for the STALE event", example: "system", status: known}
      - {name: "history[].at", type: string, nullable: false, format: "ISO 8601 UTC instant (toISOString)", example: "2026-12-02T04:30:00.000Z", status: known}
    cites: ["context/generated/api-inventory.json#http:GET:/api/appointments/:id", "sample-system:api/src/repositories/appointmentRepo.ts#L107", "runs/run_01M8E7Q99RKS5X0BN20RNDJM8C/ide-answers.md#a-03"]
  - id: EP-3
    method: POST
    path: /api/appointments/:id/reschedule
    change: changed
    fields:
      - {name: status, type: string enum, nullable: false, format: "SCHEDULED after rescheduling a SCHEDULED or STALE appointment", example: "SCHEDULED", status: known}
    cites: ["context/generated/api-inventory.json#http:POST:/api/appointments/:id/reschedule", "runs/run_01M8E7Q99RKS5X0BN20RNDJM8C/ide-answers.md#a-06"]
  - id: EP-4
    method: POST
    path: /api/appointments/:id/cancel
    change: changed
    fields:
      - {name: status, type: string enum, nullable: false, format: "CANCELLED; now also accepted from STALE", example: "CANCELLED", status: known}
    cites: ["context/generated/api-inventory.json#http:POST:/api/appointments/:id/cancel", "runs/run_01M8E7Q99RKS5X0BN20RNDJM8C/ide-answers.md#a-07"]
  - id: EP-5
    method: POST
    path: /api/appointments/:id/complete
    change: changed
    fields:
      - {name: status, type: string enum, nullable: false, format: "COMPLETED; now also accepted from STALE", example: "COMPLETED", status: known}
    cites: ["context/generated/api-inventory.json#http:POST:/api/appointments/:id/complete", "runs/run_01M8E7Q99RKS5X0BN20RNDJM8C/ide-answers.md#a-02"]
  - id: EP-6
    method: POST
    path: /api/appointments/:id/no-show
    change: changed
    fields:
      - {name: status, type: string enum, nullable: false, format: "NO_SHOW; now also accepted from STALE", example: "NO_SHOW", status: known}
    cites: ["context/generated/api-inventory.json#http:POST:/api/appointments/:id/no-show", "runs/run_01M8E7Q99RKS5X0BN20RNDJM8C/ide-answers.md#a-02"]
answers:
  - id: A-08
    kind: question
    about: Q-01
    answer: "In-process timer"
    decides: [CO-4, CO-5, C19]
    mismatch:
      text: "The architecture says no background jobs exist yet; the user decided the Stale sweep runs as the first in-process timer job."
      cites: ["context/architecture.md#open-points"]
  - id: A-09
    kind: question
    about: Q-02
    answer: "Every hour"
    decides: [CO-5, C20]
    mismatch:
      text: "Story S-1 AC1 expects the state to be Stale when 24h after start is reached; with a sweep every hour, the stored state can lag that mark until the next run. Tests call markStale() at the AC's instant."
      cites: ["artifacts/HMS-34/run_01M8E7Q99RKS5X0BN20RNDJM8C/split.md#item:S-1"]
---
# Design: HMS-34

## Overview
A new status `STALE` is added end to end: the API types (CO-1), a new migration that widens the status
and event checks (CO-2), the repository and service (CO-3, CO-4), and the web types, labels and
appointment page (CO-6 to CO-8). An in-process timer in the API server (CO-5) calls
`AppointmentService.markStale()` once at start and then every hour. markStale sets every SCHEDULED
appointment whose start is at or before now minus `DAY_MS` to STALE, and writes one `STALE` history
event per row with actor `system`, in one transaction.

Staff can still close or reschedule a Stale appointment. The `scheduled()` guard accepts SCHEDULED or
STALE, and `move()` sets the status back to SCHEDULED. Because the overlap constraints cover only
SCHEDULED rows (C5), a rescheduled appointment counts for double-booking again.

Trade-offs:
- Timer in `server.ts` and not in `createApp`: tests that build the app never start a timer, at the cost of the sweep being started only by the real process.
- Running once at start: rows are fresh right after each deploy, at the cost of one extra UPDATE per start.
- Hourly runs (A-09): few writes, but an appointment can look Scheduled for up to an hour after the 24h mark.
- Stale rows stay out of the overlap constraints: past rows never block new bookings, and a reschedule must pass the constraint again.
- No environment setting for the interval: there is no extra name to invent, but changing the interval needs a code change.

## Components
- CO-1 (changed) API status type and `APPOINTMENT_STATUSES` gain STALE (api/src/types.ts).
- CO-2 (new) Migration `<UTC timestamp>__add_stale_status.sql` in db/migrations. It drops and re-adds the appointment status CHECK and the appointment_event event CHECK, each with STALE added. Implement picks the timestamp when it writes the file.
- CO-3 (changed) AppointmentRepo: new `markStale(cutoff, actor)` (UPDATE … SET status = 'STALE', updated_by = actor WHERE status = 'SCHEDULED' AND starts_at <= cutoff RETURNING id, then one `event(id, 'STALE', actor)` per id). `move()` also sets `status = 'SCHEDULED'`.
- CO-4 (changed) AppointmentService: new `markStale()` using `clock() - DAY_MS` and actor `system` in one transaction. The `scheduled()` guard accepts SCHEDULED or STALE.
- CO-5 (changed) api/src/server.ts: after listen, calls markStale() once, then `setInterval` every hour (`60 * MINUTE`). Errors are logged with the message only. `shutdown()` clears the timer.
- CO-6 (changed) web AppointmentStatus gains STALE.
- CO-7 (changed) STATUS_LABEL gains `STALE: 'Stale'`.
- CO-8 (changed) AppointmentDetailPage shows the actions for SCHEDULED or STALE, and EVENT_LABEL gains `STALE: 'Marked stale'`.
- CO-9 (existing) AppointmentsPage status filter lists Stale through STATUS_LABEL.
- CO-10 (existing) StatusBadge shows Stale through STATUS_LABEL.
- CO-11 (changed, assumed) Glossary entry for Stale in docs/PRD.md.
- EP-1 (changed) GET /api/appointments: query `status` accepts STALE, for example `?status=STALE`; `items[].status` may be STALE.
- EP-2 (changed) GET /api/appointments/:id: `status` may be STALE. `history[].event` may be STALE with `history[].actor` = system, and `history[].at` is an ISO 8601 UTC instant.
- EP-3 (changed) POST /api/appointments/:id/reschedule: accepted from STALE; the result `status` is SCHEDULED.
- EP-4 (changed) POST /api/appointments/:id/cancel: accepted from STALE; the result `status` is CANCELLED.
- EP-5 (changed) POST /api/appointments/:id/complete: accepted from STALE; the result `status` is COMPLETED.
- EP-6 (changed) POST /api/appointments/:id/no-show: accepted from STALE; the result `status` is NO_SHOW.

Stories served: S-1 by CO-1 to CO-5. S-2 by CO-4 (Completed rows are no longer SCHEDULED, and rescheduled rows start in the future). S-3 by CO-3, CO-4 and EP-2. S-4 by CO-4, CO-8 and EP-4 to EP-6. S-5 by CO-3, CO-4, EP-3 and the overlap constraint (C5). S-6 by CO-6, CO-7, CO-9, CO-10, EP-1 and EP-2. S-7 by CO-11.

## Known
- CO-1 to CO-10, EP-1 to EP-6: each file, type and line is cited from the module map, the api inventory or the product code at the pinned SHA. Timer behaviour comes from A-08 and A-09.

## Assumed
- CO-11 and C21: the Stale glossary entry goes in docs/PRD.md next to the appointment states. Basis: REQ-12 asks for a glossary entry, and the product's own docs live in docs/PRD.md. Implement must confirm.

## Unknown
None.

## Open questions
None open.

Answered in the IDE:
- A-08 (Q-01): how should the API mark appointments Stale? — "In-process timer". Decides CO-4, CO-5, C19. Mismatch: the architecture says no background jobs exist yet; this is the first.
- A-09 (Q-02): how often should the timer run? — "Every hour". Decides CO-5, C20. Mismatch: story S-1 AC1 expects Stale at the 24h mark; the stored state can lag until the next hourly run. Tests call markStale() at the AC's instant.
