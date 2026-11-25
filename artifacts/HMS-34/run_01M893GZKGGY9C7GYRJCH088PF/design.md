---
schema: design.v1
run_id: run_01M893GZKGGY9C7GYRJCH088PF
step: design
attempt: 2
skill: design@1.10.0
inputs:
  - path: artifacts/HMS-34/run_01M893GZKGGY9C7GYRJCH088PF/split.md
    sha256: a7155fb4e16dcf12bf1ec55dc77f49a05fccbeef9c9084fb822e43d519892151
  - path: artifacts/HMS-34/run_01M893GZKGGY9C7GYRJCH088PF/req.md
    sha256: 36441e0509699b22a67bdb9f3ca7a1f511be79db2162ba7c24652d3871e33a4f
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
resolves:
  - id: F-1
    how: "server.ts reads the context from app.locals.ctx and passes its appointments service and clock to startStaleJob."
  - id: F-2
    how: "CO-12 serves S-6: the glossary owner adds Stale to context/glossary.md when the pack is refreshed after merge."
claims:
  - id: C1
    text: "The API declares appointment statuses as `AppointmentStatus` in api/src/types.ts."
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/types.ts:AppointmentStatus"]
  - id: C2
    text: "The appointment list validates its status filter against `APPOINTMENT_STATUSES`."
    cites: ["sample-system:api/src/services/appointmentService.ts#L35"]
  - id: C3
    text: "Reschedule, cancel, complete and no-show all load the row through the private method `scheduled(repo: AppointmentRepo, id: number)`, which refuses any status other than SCHEDULED."
    cites: ["sample-system:api/src/services/appointmentService.ts#L133"]
  - id: C4
    text: "Reschedule changes only the times via `move(id: number, startsAt: Date, endsAt: Date, actor: string)`; it does not set the status."
    cites: ["sample-system:api/src/repositories/appointmentRepo.ts#L155"]
  - id: C5
    text: "Closing sets the status through `close(id: number, status: Exclude<AppointmentStatus, 'SCHEDULED'>`."
    cites: ["sample-system:api/src/repositories/appointmentRepo.ts#L162"]
  - id: C6
    text: "The database allows only `CHECK (status IN ('SCHEDULED', 'COMPLETED', 'CANCELLED', 'NO_SHOW'))` on appointment."
    cites: ["sample-system:db/migrations/20261016T090300__create_appointments.sql#L10"]
  - id: C7
    text: "The history table allows only `CHECK (event IN ('BOOKED', 'RESCHEDULED', 'CANCELLED', 'COMPLETED', 'NO_SHOW'))`."
    cites: ["sample-system:db/migrations/20261016T090300__create_appointments.sql#L35"]
  - id: C8
    text: "The overlap constraints apply only `WHERE (status = 'SCHEDULED')`, so a Stale row leaves them and a rescheduled one re-enters them."
    cites: ["sample-system:db/migrations/20261016T090300__create_appointments.sql#L21"]
  - id: C9
    text: "The two unnamed CHECK constraints carry the Postgres default names `appointment_status_check` and appointment_event_event_check; implement must confirm."
    cites: ["sample-system:db/migrations/20261016T090300__create_appointments.sql#L10", "runs/run_01M893GZKGGY9C7GYRJCH088PF/ide-answers.md#a-10"]
  - id: C10
    text: "The web app maps each status to its label in `STATUS_LABEL`."
    cites: ["sample-system:web/src/lib/format.ts#L36"]
  - id: C11
    text: "The appointment list's status filter builds its options from `Object.entries(STATUS_LABEL)`."
    cites: ["sample-system:web/src/pages/AppointmentsPage.tsx#L34"]
  - id: C12
    text: "The appointment page shows its actions only when `a.status === 'SCHEDULED'`."
    cites: ["sample-system:web/src/pages/AppointmentDetailPage.tsx#L44"]
  - id: C13
    text: "The appointment page labels history events with `EVENT_LABEL`."
    cites: ["sample-system:web/src/pages/AppointmentDetailPage.tsx#L13"]
  - id: C14
    text: "The API process starts and shuts down in `api/src/server.ts`."
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/server.ts:shutdown"]
  - id: C15
    text: "Services take an injectable clock, `clock: () => Date = () => new Date()`, so tests can fix now."
    cites: ["sample-system:api/src/services/index.ts#L26"]
  - id: C16
    text: "`No background jobs exist yet`; this is the first."
    cites: ["context/architecture.md#open-points"]
  - id: C17
    text: "The web app declares `AppointmentStatus` in web/src/api/types.ts."
    cites: ["context/generated/module-map.json#sym:sample-system:web/src/api/types.ts:AppointmentStatus"]
  - id: C18
    text: "The status badge uses the CSS class `badge-${status.toLowerCase()}`."
    cites: ["sample-system:web/src/components/StatusBadge.tsx#L6"]
  - id: C19
    text: "SQL for appointments lives in `AppointmentRepo`."
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/repositories/appointmentRepo.ts:AppointmentRepo"]
  - id: C20
    text: "The appointment rules live in `AppointmentService`."
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/services/appointmentService.ts:AppointmentService"]
  - id: C21
    text: "An appointment becomes Stale `24h` after its start time if still Scheduled, marked by an automatic job."
    cites: ["artifacts/HMS-34/run_01M893GZKGGY9C7GYRJCH088PF/req.md#item:REQ-2"]
  - id: C22
    text: "createApp builds the services and clock into `app.locals.ctx = createContext(db, config, clock)`, which server.ts can read from the returned app."
    cites: ["sample-system:api/src/app.ts#L20"]
  - id: C23
    text: "server.ts gets only the app back: `const app = createApp(pool, config);`."
    cites: ["sample-system:api/src/server.ts#L7"]
  - id: C24
    text: "The app runs as one Render web service on the `plan: free`, which sleeps when idle."
    cites: ["sample-system:render.yaml#L9"]
  - id: C25
    text: "The reschedule picker starts on the appointment's own day, `useState(todayIn(timeZone, new Date(appointment.startsAt)))`, which is in the past for a Stale appointment."
    cites: ["sample-system:web/src/pages/AppointmentDetailPage.tsx#L106"]
  - id: C26
    text: "The picker's input has a minimum of today, `min={todayIn(timeZone)}`."
    cites: ["sample-system:web/src/pages/AppointmentDetailPage.tsx#L113"]
  - id: C27
    text: "AppointmentService also has a private method named `guard` that maps overlap errors to 409; it is unrelated to the status check."
    cites: ["sample-system:api/src/services/appointmentService.ts#L91"]
trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10, REQ-11, REQ-12, REQ-13, REQ-14, REQ-15]
known: [S-1, S-2, S-3, S-4, S-5, S-6, S-7]
assumed: [CO-1]
unknown: []
questions: []
components:
  - id: CO-1
    name: "Migration __add_stale_status.sql: STALE in status and event CHECKs"
    module: module:sample-system:db
    change: new
    cites: ["sample-system:db/migrations/20261016T090300__create_appointments.sql#L10", "sample-system:db/migrations/20261016T090300__create_appointments.sql#L35", "runs/run_01M893GZKGGY9C7GYRJCH088PF/ide-answers.md#a-10"]
  - id: CO-2
    name: "api/src/types.ts AppointmentStatus: add STALE"
    module: module:sample-system:api
    change: changed
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/types.ts:AppointmentStatus", "sample-system:api/src/services/appointmentService.ts#L35"]
  - id: CO-3
    name: "AppointmentRepo: new markStale; move sets SCHEDULED"
    module: module:sample-system:api
    change: changed
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/repositories/appointmentRepo.ts:AppointmentRepo", "sample-system:api/src/repositories/appointmentRepo.ts#L155"]
  - id: CO-4
    name: "AppointmentService: scheduled() accepts STALE; new markStale"
    module: module:sample-system:api
    change: changed
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/services/appointmentService.ts:AppointmentService", "sample-system:api/src/services/appointmentService.ts#L133"]
  - id: CO-5
    name: "Stale job api/src/jobs/staleJob.ts (hourly, actor system)"
    module: module:sample-system:api
    change: new
    cites: ["artifacts/HMS-34/run_01M893GZKGGY9C7GYRJCH088PF/req.md#item:REQ-3", "runs/run_01M893GZKGGY9C7GYRJCH088PF/ide-answers.md#a-08", "runs/run_01M893GZKGGY9C7GYRJCH088PF/ide-answers.md#a-09"]
  - id: CO-6
    name: "api/src/server.ts: start the job from app.locals.ctx; stop it"
    module: module:sample-system:api
    change: changed
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/server.ts:shutdown", "sample-system:api/src/app.ts#L20"]
  - id: CO-7
    name: "web/src/api/types.ts AppointmentStatus: add STALE"
    module: module:sample-system:web
    change: changed
    cites: ["context/generated/module-map.json#sym:sample-system:web/src/api/types.ts:AppointmentStatus"]
  - id: CO-8
    name: "web/src/lib/format.ts STATUS_LABEL: add STALE 'Stale'"
    module: module:sample-system:web
    change: changed
    cites: ["sample-system:web/src/lib/format.ts#L36", "sample-system:web/src/pages/AppointmentsPage.tsx#L34"]
  - id: CO-9
    name: "AppointmentDetailPage: actions and picker for STALE; event label"
    module: module:sample-system:web
    change: changed
    cites: ["sample-system:web/src/pages/AppointmentDetailPage.tsx#L44", "sample-system:web/src/pages/AppointmentDetailPage.tsx#L13", "sample-system:web/src/pages/AppointmentDetailPage.tsx#L106"]
  - id: CO-10
    name: "web/src/styles.css: new .badge-stale rule"
    module: module:sample-system:web
    change: changed
    cites: ["sample-system:web/src/components/StatusBadge.tsx#L6"]
  - id: CO-11
    name: "StatusBadge and AppointmentTable: unchanged"
    module: module:sample-system:web
    change: existing
    cites: ["context/generated/module-map.json#sym:sample-system:web/src/components/StatusBadge.tsx:StatusBadge"]
  - id: CO-12
    name: "context/glossary.md: Stale entry, by the glossary owner"
    module: module:companion:context/glossary.md
    change: new
    cites: ["artifacts/HMS-34/run_01M893GZKGGY9C7GYRJCH088PF/req.md#item:REQ-14", "runs/run_01M893GZKGGY9C7GYRJCH088PF/ide-answers.md#a-11"]
endpoints:
  - id: EP-1
    method: GET
    path: /api/appointments
    change: changed
    fields:
      - {name: "status (query)", type: string, nullable: true, format: "one of SCHEDULED, COMPLETED, CANCELLED, NO_SHOW, STALE", example: "STALE", status: known}
      - {name: "items[].status", type: string, nullable: false, format: "one of SCHEDULED, COMPLETED, CANCELLED, NO_SHOW, STALE", example: "STALE", status: known}
    cites: ["context/generated/api-inventory.json#http:GET:/api/appointments", "sample-system:api/src/services/appointmentService.ts#L35"]
  - id: EP-2
    method: GET
    path: /api/appointments/:id
    change: changed
    fields:
      - {name: status, type: string, nullable: false, format: "one of SCHEDULED, COMPLETED, CANCELLED, NO_SHOW, STALE", example: "STALE", status: known}
      - {name: "history[].event", type: string, nullable: false, format: "one of BOOKED, RESCHEDULED, CANCELLED, COMPLETED, NO_SHOW, STALE", example: "STALE", status: known}
      - {name: "history[].actor", type: string, nullable: false, format: "'system' for the Stale job; 'staff' otherwise", example: "system", status: known}
    cites: ["context/generated/api-inventory.json#http:GET:/api/appointments/:id", "sample-system:db/migrations/20261016T090300__create_appointments.sql#L35", "runs/run_01M893GZKGGY9C7GYRJCH088PF/ide-answers.md#a-09"]
  - id: EP-3
    method: POST
    path: /api/appointments/:id/complete
    change: changed
    fields:
      - {name: status, type: string, nullable: false, format: "COMPLETED; now also accepted from STALE", example: "COMPLETED", status: known}
    cites: ["context/generated/api-inventory.json#http:POST:/api/appointments/:id/complete"]
  - id: EP-4
    method: POST
    path: /api/appointments/:id/no-show
    change: changed
    fields:
      - {name: status, type: string, nullable: false, format: "NO_SHOW; now also accepted from STALE", example: "NO_SHOW", status: known}
    cites: ["context/generated/api-inventory.json#http:POST:/api/appointments/:id/no-show"]
  - id: EP-5
    method: POST
    path: /api/appointments/:id/cancel
    change: changed
    fields:
      - {name: status, type: string, nullable: false, format: "CANCELLED; now also accepted from STALE", example: "CANCELLED", status: known}
    cites: ["context/generated/api-inventory.json#http:POST:/api/appointments/:id/cancel"]
  - id: EP-6
    method: POST
    path: /api/appointments/:id/reschedule
    change: changed
    fields:
      - {name: status, type: string, nullable: false, format: "SCHEDULED after a reschedule, also from STALE", example: "SCHEDULED", status: known}
    cites: ["context/generated/api-inventory.json#http:POST:/api/appointments/:id/reschedule", "sample-system:api/src/repositories/appointmentRepo.ts#L155"]
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
    about: C9
    answer: "Yes, confirm"
    decides: [C9, CO-1]
  - id: A-11
    kind: question
    about: Q-03
    answer: "Context pack, glossary owner (proposed default)"
    decides: [CO-12]
---
# Design: HMS-34

Resolves F-1: server.ts reads the context from app.locals.ctx and passes its appointments service and clock to startStaleJob.
Resolves F-2: CO-12 serves S-6: the glossary owner adds Stale to context/glossary.md when the pack is refreshed after merge.

## Overview
A new status value `STALE` is added at three layers: the database CHECK (CO-1), the API type (CO-2) and
the web type and labels (CO-7, CO-8). A new hourly job in the API process (CO-5, CO-6) marks every
Scheduled appointment whose start time is at least 24h in the past as STALE, with actor `system`, and
writes one STALE history event for each (CO-3, CO-4). Today the private method scheduled() refuses
every status but SCHEDULED (C3); it is widened to SCHEDULED or STALE. The separate private method guard()
(C27) is left unchanged. Staff can then complete, no-show, cancel or reschedule a Stale appointment, and
a reschedule sets it back to SCHEDULED (C4).
Trade-offs:
- An in-process timer adds no infrastructure, but it runs only while the API process is up. The app is one Render web service on the free plan (C24), which sleeps when idle, so the run at startup is what catches up after a sleep. The UPDATE only touches rows still SCHEDULED, so a repeat run is harmless.
- Running hourly (A-08) means an appointment can take up to an hour past its 24h to show as Stale, in exchange for fewer queries.
- scheduled() is widened in one place, not per action, so all four actions behave the same on Stale.
- Stale rows fall outside the overlap constraints (C8), but they are past, so no booking can clash with them. A rescheduled one re-enters the constraints and is checked like any other reschedule.

## Components
- CO-1 (new): migration db/migrations/<UTC timestamp>__add_stale_status.sql. It drops and re-adds the appointment status CHECK and the appointment_event event CHECK (C6, C7) with STALE added, by the names in C9.
- CO-2 (changed): `AppointmentStatus` and `APPOINTMENT_STATUSES` in api/src/types.ts gain STALE, so the list filter accepts it (C1, C2).
- CO-3 (changed): `AppointmentRepo` gains markStale(cutoff, actor), which sets SCHEDULED rows with starts_at <= cutoff to STALE and returns their ids. move() also sets status = 'SCHEDULED' (C4, C19).
- CO-4 (changed): in `AppointmentService`, the private method scheduled() (C3) accepts SCHEDULED or STALE, so reschedule, cancel, complete and no-show work on a Stale appointment. The private method guard() (C27) is unchanged. A new public markStale(actor) computes cutoff = clock() minus 24h, runs the repo update and writes one STALE history event per row, in one transaction (C20).
- CO-5 (new): api/src/jobs/staleJob.ts. startStaleJob(appointments) calls appointments.markStale('system') once at startup and then every hour, and returns a stop function (C15, C21).
- CO-6 (changed): api/src/server.ts gets only the app from createApp (C23). After listen it reads `app.locals.ctx` (C22) and calls startStaleJob(ctx.services.appointments); the service already holds the context's clock. shutdown() calls the stop function before server.close.
- CO-7 (changed): web `AppointmentStatus` gains STALE (C17).
- CO-8 (changed): `STATUS_LABEL` gains STALE: 'Stale', which feeds the badge label and the list's status filter option (C10, C11).
- CO-9 (changed): AppointmentDetailPage shows its actions for STALE as well as SCHEDULED (C12), and EVENT_LABEL gains STALE: 'Marked stale' (C13). For a Stale appointment the reschedule picker starts on today, not the past start day (C25, C26).
- CO-10 (changed): styles.css gets .badge-stale, using the warning colours, because the badge class comes from the status (C18).
- CO-11 (existing): StatusBadge and AppointmentTable need no change; they show the new label through STATUS_LABEL.
- CO-12 (new): context/glossary.md in the context pack (this companion repo), not the product repo. Its owner adds a Stale entry matching REQ-2 when the pack is refreshed after merge (A-11). It is not part of the impl diff.
- EP-1 GET /api/appointments (changed): `status` query and `items[].status` may be STALE, for example "STALE".
- EP-2 GET /api/appointments/:id (changed): `status` may be STALE; `history[].event` is one of BOOKED, RESCHEDULED, CANCELLED, COMPLETED, NO_SHOW, STALE; `history[].actor` is "system" for the job's event.
- EP-3 POST /api/appointments/:id/complete (changed): accepts a STALE appointment; returns status COMPLETED.
- EP-4 POST /api/appointments/:id/no-show (changed): accepts a STALE appointment; returns status NO_SHOW.
- EP-5 POST /api/appointments/:id/cancel (changed): accepts a STALE appointment; returns status CANCELLED.
- EP-6 POST /api/appointments/:id/reschedule (changed): accepts a STALE appointment; returns status SCHEDULED.

Story map: S-1 by CO-1 to CO-6; S-2 by CO-4 (the job only touches rows still SCHEDULED); S-3 by CO-4, CO-9, EP-3 to EP-5; S-4 by CO-3, CO-4, CO-9, EP-6; S-5 by CO-2, CO-7, CO-8, CO-10, EP-1, EP-2; S-6 by CO-12; S-7 by CO-8 and CO-11 (the patient page shows every status; the doctor page's upcoming list stays SCHEDULED only).

## Known
- S-1 to S-7: every story is served by the components above, built on the cited facts.

## Assumed
- CO-1: relies on C9, that the unnamed CHECK constraints carry the Postgres default names `appointment_status_check` and appointment_event_event_check. The basis is the unnamed CHECK at the migration's L10 and L35. The user confirmed this (A-10); implement must confirm against the database.

## Unknown
None.

## Open questions
None open.

Answered in the IDE:
- A-08 (Q-01, how often the job runs): "Every hour"; decides CO-5.
- A-09 (Q-02, actor recorded by the job): "system (proposed default)"; decides CO-5, EP-2.
- A-10 (C9, default constraint names): "Yes, confirm"; decides C9, CO-1.
- A-11 (Q-03, where the glossary entry is made): "Context pack, glossary owner (proposed default)"; decides CO-12.
