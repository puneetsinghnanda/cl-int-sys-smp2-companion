---
schema: design.v1
run_id: run_01M89M2JW0X6H7Q7W6EJQ53R4B
step: design
attempt: 1
skill: design@1.10.0
inputs:
  - path: artifacts/HMS-34/run_01M89M2JW0X6H7Q7W6EJQ53R4B/split.md
    sha256: ffdc205028446dc36d5b0a750fdfe01f2de2f7347891a727faae856bcb6899c4
  - path: artifacts/HMS-34/run_01M89M2JW0X6H7Q7W6EJQ53R4B/req.md
    sha256: a8bbff4d030d3399dda032db1075102c75ad2b3cc130090a3aa204af163da300
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
    text: "The system has no background jobs yet: `No background jobs exist yet`."
    cites: ["context/architecture.md#open-points"]
  - id: C2
    text: "The appointment status CHECK lists `'SCHEDULED', 'COMPLETED', 'CANCELLED', 'NO_SHOW'` and has no constraint name."
    cites: ["sample-system:db/migrations/20261016T090300__create_appointments.sql#L10"]
  - id: C3
    text: "The doctor overlap constraint applies only `WHERE (status = 'SCHEDULED')`, so a Stale appointment does not block its slot."
    cites: ["sample-system:db/migrations/20261016T090300__create_appointments.sql#L21"]
  - id: C4
    text: "The history event CHECK lists `'BOOKED', 'RESCHEDULED', 'CANCELLED', 'COMPLETED', 'NO_SHOW'` and has no constraint name."
    cites: ["sample-system:db/migrations/20261016T090300__create_appointments.sql#L35"]
  - id: C5
    text: "The API builds its services into `app.locals.ctx`, from which the job can take the appointment service."
    cites: ["sample-system:api/src/app.ts#L20"]
  - id: C6
    text: "The badge class is built from the status as `badge-${status.toLowerCase()}`, so STALE needs a .badge-stale rule."
    cites: ["sample-system:web/src/components/StatusBadge.tsx#L6"]
  - id: C7
    text: "The default CHECK names are `appointment_status_check` and appointment_event_event_check, confirmed in the IDE (implement must confirm)."
    cites: ["runs/run_01M89M2JW0X6H7Q7W6EJQ53R4B/ide-answers.md#a-10"]
  - id: C8
    text: "The list's status filter options come from `Object.entries(STATUS_LABEL)`, so a new label also adds the filter option."
    cites: ["sample-system:web/src/pages/AppointmentsPage.tsx#L34"]
  - id: C9
    text: "The 24h cutoff can use the existing constant `DAY_MS`."
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/lib/time.ts:DAY_MS"]
  - id: C10
    text: "Every appointment change records its actor in `updated_by`."
    cites: ["context/generated/db-schema.json#column:appointment.updated_by"]
  - id: C11
    text: "Every status action goes through a private scheduled() guard in `AppointmentService`."
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/services/appointmentService.ts:AppointmentService"]
  - id: C12
    text: "The web status labels live in `STATUS_LABEL` in web/src/lib/format.ts."
    cites: ["sample-system:web/src/lib/format.ts#L36"]
  - id: C13
    text: "The appointment page labels history events through `EVENT_LABEL`."
    cites: ["sample-system:web/src/pages/AppointmentDetailPage.tsx#L13"]
  - id: C14
    text: "The badge rules live in web/src/styles.css, for example `.badge-scheduled`."
    cites: ["sample-system:web/src/styles.css#L76"]
trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10, REQ-11, REQ-12, REQ-13, REQ-14, REQ-15, REQ-16]
known: [CO-2, CO-3, CO-4, CO-5, CO-6, CO-7, CO-8, CO-9, CO-10, CO-11, CO-12, EP-1, EP-2, EP-3, EP-4, EP-5, EP-6]
assumed: [CO-1]
unknown: []
questions: []
components:
  - id: CO-1
    name: Migration adding STALE to both status CHECKs
    module: module:sample-system:db
    change: new
    cites: ["sample-system:db/migrations/20261016T090300__create_appointments.sql#L10", "sample-system:db/migrations/20261016T090300__create_appointments.sql#L35", "runs/run_01M89M2JW0X6H7Q7W6EJQ53R4B/ide-answers.md#a-10"]
  - id: CO-2
    name: API AppointmentStatus and APPOINTMENT_STATUSES gain STALE
    module: module:sample-system:api
    change: changed
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/types.ts:AppointmentStatus"]
  - id: CO-3
    name: AppointmentRepo gains markStale; move() sets status SCHEDULED
    module: module:sample-system:api
    change: changed
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/repositories/appointmentRepo.ts:AppointmentRepo", "runs/run_01M89M2JW0X6H7Q7W6EJQ53R4B/ide-answers.md#a-06"]
  - id: CO-4
    name: AppointmentService gains markStale; scheduled() accepts STALE
    module: module:sample-system:api
    change: changed
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/services/appointmentService.ts:AppointmentService", "runs/run_01M89M2JW0X6H7Q7W6EJQ53R4B/ide-answers.md#a-03"]
  - id: CO-5
    name: Stale job api/src/jobs/staleJob.ts, hourly, actor system
    module: module:sample-system:api
    change: new
    cites: ["context/architecture.md#open-points", "runs/run_01M89M2JW0X6H7Q7W6EJQ53R4B/ide-answers.md#a-01", "runs/run_01M89M2JW0X6H7Q7W6EJQ53R4B/ide-answers.md#a-08", "runs/run_01M89M2JW0X6H7Q7W6EJQ53R4B/ide-answers.md#a-09"]
  - id: CO-6
    name: server.ts starts the job after listen and stops it in shutdown
    module: module:sample-system:api
    change: changed
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/server.ts:shutdown", "sample-system:api/src/app.ts#L20"]
  - id: CO-7
    name: Web AppointmentStatus gains STALE
    module: module:sample-system:web
    change: changed
    cites: ["context/generated/module-map.json#sym:sample-system:web/src/api/types.ts:AppointmentStatus"]
  - id: CO-8
    name: STATUS_LABEL gains STALE 'Stale'
    module: module:sample-system:web
    change: changed
    cites: ["sample-system:web/src/lib/format.ts#L36", "sample-system:web/src/pages/AppointmentsPage.tsx#L34", "runs/run_01M89M2JW0X6H7Q7W6EJQ53R4B/ide-answers.md#a-04"]
  - id: CO-9
    name: AppointmentDetailPage shows actions and a history label for STALE
    module: module:sample-system:web
    change: changed
    cites: ["context/generated/module-map.json#sym:sample-system:web/src/pages/AppointmentDetailPage.tsx:AppointmentDetailPage", "context/generated/module-map.json#sym:sample-system:web/src/pages/AppointmentDetailPage.tsx:Reschedule", "runs/run_01M89M2JW0X6H7Q7W6EJQ53R4B/ide-answers.md#a-03"]
  - id: CO-10
    name: web/src/styles.css .badge-stale rule
    module: module:sample-system:web
    change: changed
    cites: ["sample-system:web/src/styles.css#L76", "sample-system:web/src/components/StatusBadge.tsx#L6"]
  - id: CO-11
    name: StatusBadge, AppointmentTable and the dashboard, unchanged
    module: module:sample-system:web
    change: existing
    cites: ["context/generated/module-map.json#sym:sample-system:web/src/components/StatusBadge.tsx:StatusBadge", "runs/run_01M89M2JW0X6H7Q7W6EJQ53R4B/ide-answers.md#a-04"]
  - id: CO-12
    name: Glossary entry for Stale, made in the context pack by its owner
    module: module:sample-system:.
    change: existing
    cites: ["runs/run_01M89M2JW0X6H7Q7W6EJQ53R4B/ide-answers.md#a-11"]
endpoints:
  - id: EP-1
    method: GET
    path: /api/appointments
    change: changed
    fields:
      - {name: status, type: string, nullable: false, format: "one of SCHEDULED, COMPLETED, CANCELLED, NO_SHOW, STALE; the status query filter accepts STALE", example: "STALE", status: known}
    cites: ["context/generated/api-inventory.json#http:GET:/api/appointments", "context/generated/db-schema.json#column:appointment.status"]
  - id: EP-2
    method: GET
    path: /api/appointments/:id
    change: changed
    fields:
      - {name: status, type: string, nullable: false, format: "one of SCHEDULED, COMPLETED, CANCELLED, NO_SHOW, STALE", example: "STALE", status: known}
      - {name: "history[].event", type: string, nullable: false, format: "one of BOOKED, RESCHEDULED, CANCELLED, COMPLETED, NO_SHOW, STALE", example: "STALE", status: known}
      - {name: "history[].actor", type: string, nullable: false, format: "'system' for an event the job writes", example: "system", status: known}
    cites: ["context/generated/api-inventory.json#http:GET:/api/appointments/:id", "context/generated/db-schema.json#column:appointment_event.event", "runs/run_01M89M2JW0X6H7Q7W6EJQ53R4B/ide-answers.md#a-09"]
  - id: EP-3
    method: POST
    path: /api/appointments/:id/complete
    change: changed
    fields:
      - {name: status, type: string, nullable: false, format: "request accepted when the current status is SCHEDULED or STALE; response status COMPLETED", example: "COMPLETED", status: known}
    cites: ["context/generated/api-inventory.json#http:POST:/api/appointments/:id/complete", "runs/run_01M89M2JW0X6H7Q7W6EJQ53R4B/ide-answers.md#a-03"]
  - id: EP-4
    method: POST
    path: /api/appointments/:id/no-show
    change: changed
    fields:
      - {name: status, type: string, nullable: false, format: "request accepted when the current status is SCHEDULED or STALE; response status NO_SHOW", example: "NO_SHOW", status: known}
    cites: ["context/generated/api-inventory.json#http:POST:/api/appointments/:id/no-show", "runs/run_01M89M2JW0X6H7Q7W6EJQ53R4B/ide-answers.md#a-03"]
  - id: EP-5
    method: POST
    path: /api/appointments/:id/cancel
    change: changed
    fields:
      - {name: status, type: string, nullable: false, format: "request accepted when the current status is SCHEDULED or STALE; response status CANCELLED", example: "CANCELLED", status: known}
    cites: ["context/generated/api-inventory.json#http:POST:/api/appointments/:id/cancel", "runs/run_01M89M2JW0X6H7Q7W6EJQ53R4B/ide-answers.md#a-03"]
  - id: EP-6
    method: POST
    path: /api/appointments/:id/reschedule
    change: changed
    fields:
      - {name: status, type: string, nullable: false, format: "request accepted when the current status is SCHEDULED or STALE; response status SCHEDULED", example: "SCHEDULED", status: known}
    cites: ["context/generated/api-inventory.json#http:POST:/api/appointments/:id/reschedule", "runs/run_01M89M2JW0X6H7Q7W6EJQ53R4B/ide-answers.md#a-06"]
answers:
  - id: A-08
    kind: question
    about: Q-01
    answer: "Every hour"
    decides: [CO-5]
  - id: A-09
    kind: question
    about: Q-02
    answer: "system (proposed default)"
    decides: [CO-5, EP-2]
  - id: A-10
    kind: assumption
    about: C7
    answer: "Yes, confirm"
    decides: [C7, CO-1]
  - id: A-11
    kind: question
    about: Q-03
    answer: "Context pack, glossary owner (proposed default)"
    decides: [CO-12]
---
# Design: HMS-34

## Overview
Stale becomes a fifth appointment status, STALE, stored in the existing status column. A new
migration (CO-1) widens the status CHECK (C2) and the history event CHECK (C4). The system has no
background jobs yet (C1), so the first one, CO-5, runs inside the API process on a timer every hour
(A-08); no new infrastructure is needed. Trade-off: an in-process timer is simple, but with more than
one API instance each runs the job; the UPDATE only touches rows still SCHEDULED, so repeats are
harmless. The job takes the appointment service from `app.locals.ctx` (C5), computes the cutoff as
now minus `DAY_MS` (C9) and, in one transaction, sets every Scheduled appointment that started at or
before the cutoff to STALE with actor system (A-09, C10) and writes one STALE history event each.

Staff actions still work on a Stale appointment (A-03): the guard `private async scheduled` (C11)
accepts STALE, so complete, no-show, cancel and reschedule work (EP-3 to EP-6). A reschedule sets the
status back to SCHEDULED (A-06) in AppointmentRepo.move(). Trade-off: a Stale appointment no longer
holds its slot, because the overlap constraints apply only to Scheduled rows (C3); that is wanted,
since it is in the past.

The web app gains the STALE type and the label 'Stale' (C12). The label also adds the list's filter
option (C8). The badge class comes from the status (C6), so a .badge-stale rule is added beside the
other badge rules (C14). The appointment page shows its actions for STALE and labels the history
event (C13). The dashboard and the shared table are unchanged (CO-11). The glossary entry is made in
the context pack by its owner (A-11, CO-12).

## Components
- CO-1 Migration adding STALE to both status CHECKs (new, db): drops and re-adds the unnamed CHECKs by their default names (C7).
- CO-2 API AppointmentStatus and APPOINTMENT_STATUSES gain STALE (changed, api).
- CO-3 AppointmentRepo gains markStale(cutoff, actor); move() sets status SCHEDULED (changed, api).
- CO-4 AppointmentService gains markStale(actor); scheduled() accepts STALE (changed, api).
- CO-5 Stale job api/src/jobs/staleJob.ts, hourly, actor system (new, api).
- CO-6 server.ts starts the job after listen and stops it in shutdown (changed, api).
- CO-7 Web AppointmentStatus gains STALE (changed, web).
- CO-8 STATUS_LABEL gains STALE 'Stale' (changed, web): badge label and list filter option.
- CO-9 AppointmentDetailPage shows actions and a history label for STALE (changed, web); the reschedule picker starts on today for a past appointment.
- CO-10 web/src/styles.css .badge-stale rule (changed, web).
- CO-11 StatusBadge, AppointmentTable and the dashboard, unchanged (existing, web).
- CO-12 Glossary entry for Stale, made in the context pack by its owner (existing, outside this diff).
- EP-1 GET /api/appointments (changed): status string, one of SCHEDULED, COMPLETED, CANCELLED, NO_SHOW, STALE; the status query filter accepts STALE; example "STALE".
- EP-2 GET /api/appointments/:id (changed): status string as EP-1; history[].event string, one of BOOKED, RESCHEDULED, CANCELLED, COMPLETED, NO_SHOW, STALE; history[].actor 'system' for an event the job writes.
- EP-3 POST /api/appointments/:id/complete (changed): status, request accepted when the current status is SCHEDULED or STALE; response status COMPLETED.
- EP-4 POST /api/appointments/:id/no-show (changed): status, request accepted when the current status is SCHEDULED or STALE; response status NO_SHOW.
- EP-5 POST /api/appointments/:id/cancel (changed): status, request accepted when the current status is SCHEDULED or STALE; response status CANCELLED.
- EP-6 POST /api/appointments/:id/reschedule (changed): status, request accepted when the current status is SCHEDULED or STALE; response status SCHEDULED.

Stories served: S-1 by CO-1, CO-3 to CO-6; S-2 by CO-3 (only rows still SCHEDULED change); S-3 by CO-4, CO-9, EP-3 to EP-5; S-4 by CO-3, CO-4, CO-9, EP-6; S-5 by CO-7, CO-8, CO-10, CO-11, EP-1, EP-2; S-6 by CO-12; S-7 by CO-8 and CO-11.

## Known
- CO-2 to CO-11: each names a file or type proven by a module-map fact or a product code line.
- CO-12: decided in the IDE (A-11).
- EP-1 to EP-6: existing routes in the api inventory; their fields follow the status column and A-03, A-06, A-09.

## Assumed
- CO-1: the unnamed CHECKs take the Postgres default names appointment_status_check and appointment_event_event_check (C7, confirmed in A-10); implement must confirm.

## Unknown
None.

## Open questions
None open.

Answered in the IDE:
- A-08 (Q-01, how often the job runs): "Every hour"; decides CO-5.
- A-09 (Q-02, the job's actor): "system (proposed default)"; decides CO-5 and EP-2.
- A-10 (C7, default CHECK names): "Yes, confirm"; decides C7 and CO-1.
- A-11 (Q-03, where the glossary entry is made): "Context pack, glossary owner (proposed default)"; decides CO-12.
