---
schema: design.v1
run_id: run_01M897G118QMGZ7FAT3WCXETBZ
step: design
attempt: 1
skill: design@1.10.0
inputs:
  - path: artifacts/HMS-34/run_01M897G118QMGZ7FAT3WCXETBZ/split.md
    sha256: cb0b1c68ceef02d6fb6550506b35cd99814cb26aded4e14a0ff8670de9a0a06a
  - path: artifacts/HMS-34/run_01M897G118QMGZ7FAT3WCXETBZ/req.md
    sha256: ffba5da30ea3e68697ff8bc3701d510d9c7482c75dc4f24d256402210175ae13
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
    text: "`No background jobs exist yet`, so the Stale job is the first one and runs in the API process."
    cites: ["context/architecture.md#open-points"]
  - id: C2
    text: "The appointment status column allows only `CHECK (status IN ('SCHEDULED', 'COMPLETED', 'CANCELLED', 'NO_SHOW'))`."
    cites: ["sample-system:db/migrations/20261016T090300__create_appointments.sql#L10"]
  - id: C3
    text: "The history event column allows only `CHECK (event IN ('BOOKED', 'RESCHEDULED', 'CANCELLED', 'COMPLETED', 'NO_SHOW'))`."
    cites: ["sample-system:db/migrations/20261016T090300__create_appointments.sql#L35"]
  - id: C4
    text: "createApp builds the services into `app.locals.ctx`, so server.ts reads them from there after createApp returns."
    cites: ["sample-system:api/src/app.ts#L20"]
  - id: C5
    text: "The status badge takes its label from `STATUS_LABEL` in web/src/lib/format.ts."
    cites: ["sample-system:web/src/components/StatusBadge.tsx#L2"]
  - id: C6
    text: "The badge's CSS class is `badge-${status.toLowerCase()}`, so Stale needs a .badge-stale rule."
    cites: ["sample-system:web/src/components/StatusBadge.tsx#L6"]
  - id: C7
    text: "The new migration drops the unnamed CHECKs by the Postgres default names `appointment_status_check` and appointment_event_event_check."
    cites: ["sample-system:db/migrations/20261016T090300__create_appointments.sql#L10", "runs/run_01M897G118QMGZ7FAT3WCXETBZ/ide-answers.md#a-10"]
  - id: C8
    text: "The appointment list's status filter lists one option per `Object.entries(STATUS_LABEL)` entry."
    cites: ["sample-system:web/src/pages/AppointmentsPage.tsx#L34"]
  - id: C9
    text: "The 24h cutoff uses the existing constant `DAY_MS`."
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/lib/time.ts:DAY_MS"]
  - id: C10
    text: "The double-booking exclusions apply only `WHERE (status = 'SCHEDULED')`, so a Stale appointment frees its slot."
    cites: ["sample-system:db/migrations/20261016T090300__create_appointments.sql#L21"]
  - id: C11
    text: "Each appointment change records an actor in `updated_by`."
    cites: ["context/generated/db-schema.json#column:appointment.updated_by"]
  - id: C12
    text: "The Stale job runs `Every hour`."
    cites: ["runs/run_01M897G118QMGZ7FAT3WCXETBZ/ide-answers.md#a-08"]
  - id: C13
    text: "The Stale job records the actor `system`."
    cites: ["runs/run_01M897G118QMGZ7FAT3WCXETBZ/ide-answers.md#a-09"]
trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10, REQ-11, REQ-12, REQ-13, REQ-14, REQ-15, REQ-16]
known: [S-1, S-2, S-3, S-4, S-5, S-6, S-7]
assumed: [CO-1]
unknown: []
questions: []
components:
  - id: CO-1
    name: Migration adding STALE to both status CHECKs
    module: module:sample-system:db
    change: new
    cites: ["sample-system:db/migrations/20261016T090300__create_appointments.sql#L10", "sample-system:db/migrations/20261016T090300__create_appointments.sql#L35", "runs/run_01M897G118QMGZ7FAT3WCXETBZ/ide-answers.md#a-10"]
  - id: CO-2
    name: API AppointmentStatus gains STALE
    module: module:sample-system:api
    change: changed
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/types.ts:AppointmentStatus"]
  - id: CO-3
    name: AppointmentRepo markStale; move() sets SCHEDULED
    module: module:sample-system:api
    change: changed
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/repositories/appointmentRepo.ts:AppointmentRepo"]
  - id: CO-4
    name: AppointmentService markStale; actions accept STALE
    module: module:sample-system:api
    change: changed
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/services/appointmentService.ts:AppointmentService", "runs/run_01M897G118QMGZ7FAT3WCXETBZ/ide-answers.md#a-09"]
  - id: CO-5
    name: Stale job api/src/jobs/staleJob.ts, hourly, actor system
    module: module:sample-system:api
    change: new
    cites: ["context/architecture.md#open-points", "runs/run_01M897G118QMGZ7FAT3WCXETBZ/ide-answers.md#a-08", "runs/run_01M897G118QMGZ7FAT3WCXETBZ/ide-answers.md#a-09"]
  - id: CO-6
    name: server.ts starts the job from app.locals.ctx, stops it on shutdown
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
    cites: ["sample-system:web/src/components/StatusBadge.tsx#L2", "sample-system:web/src/pages/AppointmentsPage.tsx#L34"]
  - id: CO-9
    name: AppointmentDetailPage actions and history label for STALE
    module: module:sample-system:web
    change: changed
    cites: ["context/generated/module-map.json#sym:sample-system:web/src/pages/AppointmentDetailPage.tsx:AppointmentDetailPage"]
  - id: CO-10
    name: styles.css .badge-stale rule
    module: module:sample-system:web
    change: changed
    cites: ["sample-system:web/src/components/StatusBadge.tsx#L6"]
  - id: CO-11
    name: StatusBadge, AppointmentTable and patient and doctor pages
    module: module:sample-system:web
    change: existing
    cites: ["context/generated/module-map.json#sym:sample-system:web/src/components/StatusBadge.tsx:StatusBadge"]
  - id: CO-12
    name: Glossary entry for Stale in the context pack, outside the product diff
    module: module:sample-system:.
    change: existing
    cites: ["runs/run_01M897G118QMGZ7FAT3WCXETBZ/ide-answers.md#a-11"]
endpoints:
  - id: EP-1
    method: GET
    path: /api/appointments
    change: changed
    fields:
      - {name: status, type: string, nullable: false, format: "one of SCHEDULED, COMPLETED, CANCELLED, NO_SHOW, STALE", example: "STALE", status: known}
    cites: ["context/generated/api-inventory.json#http:GET:/api/appointments"]
  - id: EP-2
    method: GET
    path: /api/appointments/:id
    change: changed
    fields:
      - {name: status, type: string, nullable: false, format: "one of SCHEDULED, COMPLETED, CANCELLED, NO_SHOW, STALE", example: "STALE", status: known}
      - {name: "history[].event", type: string, nullable: false, format: "one of BOOKED, RESCHEDULED, CANCELLED, COMPLETED, NO_SHOW, STALE", example: "STALE", status: known}
    cites: ["context/generated/api-inventory.json#http:GET:/api/appointments/:id"]
  - id: EP-3
    method: POST
    path: /api/appointments/:id/complete
    change: changed
    fields:
      - {name: status, type: string, nullable: false, format: "request accepted when the current status is SCHEDULED or STALE; response status COMPLETED", example: "COMPLETED", status: known}
    cites: ["context/generated/api-inventory.json#http:POST:/api/appointments/:id/complete"]
  - id: EP-4
    method: POST
    path: /api/appointments/:id/no-show
    change: changed
    fields:
      - {name: status, type: string, nullable: false, format: "request accepted when the current status is SCHEDULED or STALE; response status NO_SHOW", example: "NO_SHOW", status: known}
    cites: ["context/generated/api-inventory.json#http:POST:/api/appointments/:id/no-show"]
  - id: EP-5
    method: POST
    path: /api/appointments/:id/cancel
    change: changed
    fields:
      - {name: status, type: string, nullable: false, format: "request accepted when the current status is SCHEDULED or STALE; response status CANCELLED", example: "CANCELLED", status: known}
    cites: ["context/generated/api-inventory.json#http:POST:/api/appointments/:id/cancel"]
  - id: EP-6
    method: POST
    path: /api/appointments/:id/reschedule
    change: changed
    fields:
      - {name: status, type: string, nullable: false, format: "request accepted when the current status is SCHEDULED or STALE; response status SCHEDULED", example: "SCHEDULED", status: known}
    cites: ["context/generated/api-inventory.json#http:POST:/api/appointments/:id/reschedule"]
answers:
  - id: A-08
    kind: question
    about: Q-01
    answer: "Every hour"
    decides: [CO-5, C12]
  - id: A-09
    kind: question
    about: Q-02
    answer: "system (proposed default)"
    decides: [CO-4, CO-5, C13]
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
Stale becomes a fifth appointment status. A new migration adds STALE to the status CHECK (C2) and to the
history event CHECK (C3), dropping them by their default names (C7). An hourly job (C12) inside the API
process, the system's first background job (C1), marks every appointment still SCHEDULED whose start time
is at least `DAY_MS` (C9) in the past as STALE, writes one STALE history event per appointment, and records
the actor system (C11, C13), all in one transaction. server.ts starts the job after listen, taking the
services from app.locals.ctx (C4), and stops it on shutdown. The service's "must be Scheduled" guard for
complete, no-show, cancel and reschedule also accepts STALE, and a reschedule sets the status back to
SCHEDULED. In the web app the new STATUS_LABEL entry feeds the badge (C5) and the list's status filter (C8);
a .badge-stale rule styles it (C6).

Trade-offs:
- Timer in the API process rather than a separate worker: no new infrastructure (C1); with several API
  instances the job would run on each, which is harmless because the UPDATE only touches SCHEDULED rows.
- Hourly runs (C12): an appointment shows Stale up to about an hour after its 24h mark.
- The double-booking exclusions apply only to SCHEDULED (C10), so a Stale appointment frees its slot;
  rescheduling it back to SCHEDULED is checked against the exclusions again, as a normal move is.

## Components
- CO-1 Migration adding STALE to both status CHECKs (new, db): drops and re-adds the status and event CHECKs with STALE (C2, C3, C7).
- CO-2 API AppointmentStatus gains STALE (changed, api/src/types.ts), so list filtering accepts STALE.
- CO-3 AppointmentRepo markStale; move() sets SCHEDULED (changed): markStale(cutoff, actor) updates SCHEDULED rows with starts_at at or before the cutoff and returns their ids; move() also sets status SCHEDULED.
- CO-4 AppointmentService markStale; actions accept STALE (changed): markStale(actor) uses cutoff = clock() minus DAY_MS (C9) and writes the STALE events in one transaction; the Scheduled guard accepts STALE.
- CO-5 Stale job api/src/jobs/staleJob.ts, hourly, actor system (new): runs once at startup and every hour (C12) with actor system (C13); logs only the error message; returns a stop function.
- CO-6 server.ts starts the job from app.locals.ctx, stops it on shutdown (changed, C4).
- CO-7 Web AppointmentStatus gains STALE (changed, web/src/api/types.ts).
- CO-8 STATUS_LABEL gains STALE 'Stale' (changed, web/src/lib/format.ts): feeds the badge (C5) and the status filter (C8).
- CO-9 AppointmentDetailPage actions and history label for STALE (changed): shows the actions for STALE, starts the reschedule picker on today for a past appointment, labels the event "Marked stale".
- CO-10 styles.css .badge-stale rule (changed, C6).
- CO-11 StatusBadge, AppointmentTable and patient and doctor pages (existing): show the label with no change; the doctor page keeps listing only upcoming Scheduled appointments.
- CO-12 Glossary entry for Stale in the context pack, outside the product diff (existing in the product repo): the glossary owner adds the entry to context/glossary.md; no product file changes.
- EP-1 GET /api/appointments (changed): status may be STALE; the status filter accepts STALE. Field status: string, not null, one of SCHEDULED, COMPLETED, CANCELLED, NO_SHOW, STALE, example "STALE", known.
- EP-2 GET /api/appointments/:id (changed): status as EP-1; history[].event: string, not null, one of BOOKED, RESCHEDULED, CANCELLED, COMPLETED, NO_SHOW, STALE, example "STALE", known.
- EP-3 POST /api/appointments/:id/complete (changed): status: string, not null, accepted when the current status is SCHEDULED or STALE; response status COMPLETED, example "COMPLETED", known.
- EP-4 POST /api/appointments/:id/no-show (changed): status: string, not null, accepted when the current status is SCHEDULED or STALE; response status NO_SHOW, example "NO_SHOW", known.
- EP-5 POST /api/appointments/:id/cancel (changed): status: string, not null, accepted when the current status is SCHEDULED or STALE; response status CANCELLED, example "CANCELLED", known.
- EP-6 POST /api/appointments/:id/reschedule (changed): status: string, not null, accepted when the current status is SCHEDULED or STALE; response status SCHEDULED, example "SCHEDULED", known.

Story map: S-1 by CO-1 to CO-6; S-2 by CO-3 and CO-4 (only SCHEDULED rows are updated); S-3 by CO-4, CO-9, EP-3 to EP-5; S-4 by CO-3, CO-4, CO-9, EP-6; S-5 by CO-7, CO-8, CO-10, EP-1, EP-2 (dashboard unchanged); S-6 by CO-12; S-7 by CO-8 and CO-11.

## Known
- S-1 to S-7: every story is served by the components in the story map above.

## Assumed
- CO-1: the unnamed CHECKs carry the Postgres default names appointment_status_check and appointment_event_event_check (C7). The user confirmed it; implement must confirm, and the verifier's migrate run proves it.

## Unknown
None.

## Open questions
None open.

Answered in the IDE:
- A-08 (Q-01, how often the job runs): "Every hour"; decides CO-5, C12.
- A-09 (Q-02, actor the job records): "system (proposed default)"; decides CO-4, CO-5, C13.
- A-10 (C7, default CHECK constraint names): "Yes, confirm"; decides C7, CO-1.
- A-11 (Q-03, where the glossary entry is made): "Context pack, glossary owner (proposed default)"; decides CO-12.
