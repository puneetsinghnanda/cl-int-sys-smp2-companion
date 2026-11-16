---
schema: impact.v1
run_id: run_01M7HH0DM88V7JHMVZPSYX5BWE
step: impact
attempt: 3
skill: impact-analysis@1.3.0
inputs:
  - path: artifacts/HMS-14/run_01M7HH0DM88V7JHMVZPSYX5BWE/req.md
    sha256: 7f7f379c0fac62230826a6d88863ac42c7c9dd2da60c5db6afedef2d3e5ebcdb
  - path: context/generated/module-map.json
    sha256: 4158c8fcd239c22cb8f1029dd11f61d8839867ff8088bd9ab76197e2db96db4b
  - path: context/generated/api-inventory.json
    sha256: ba024ab1b4b4151d26ce1afa77bd2dab1cec645ea737ce37f98e0e8674f795e9
  - path: context/generated/db-schema.json
    sha256: e2a9d2990f6618ea5d55994a16c6264b0186b851cc3216014b812c4a4b98908a
  - path: context/architecture.md
    sha256: 9faab093d10933a46f6473b25c80728f8745e597100e650cd2537bbafb2b595a
  - path: context/data-model.md
    sha256: 2386ae151d54a73cdbe4fd9e0b69f8e8539c2870f5e52c12e5ed0191cfa95e55
pack_sha:
  sample-system: 7681f87749883d71abb4011d118366ca1242e1cc
claims:
  - id: C1
    text: "The appointment page is rendered by `web/src/pages/AppointmentDetailPage.tsx`."
    cites: ["context/generated/module-map.json#sym:sample-system:web/src/pages/AppointmentDetailPage.tsx:AppointmentDetailPage"]
  - id: C2
    text: "The book appointment page is rendered by `web/src/pages/BookAppointmentPage.tsx`."
    cites: ["context/generated/module-map.json#sym:sample-system:web/src/pages/BookAppointmentPage.tsx:BookAppointmentPage"]
  - id: C3
    text: "The appointment page is the UI route `/appointments/:id`, declared in web/src/App.tsx; the route-to-page mapping is inferred from names, since the route fact cites only App.tsx."
    cites: ["context/generated/api-inventory.json#ui:/appointments/:id"]
  - id: C4
    text: "The book appointment page is the UI route `/appointments/new`, declared in web/src/App.tsx; the route-to-page mapping is inferred from names, since the route fact cites only App.tsx."
    cites: ["context/generated/api-inventory.json#ui:/appointments/new"]
  - id: C5
    text: "The appointment detail route GET /api/appointments/:id is served from `api/src/routes/appointments.ts`."
    cites: ["context/generated/api-inventory.json#http:GET:/api/appointments/:id"]
  - id: C6
    text: "The appointment table already has a reason column, `column:appointment.reason`, so no migration is needed for the reason for visit."
    cites: ["context/generated/db-schema.json#table:appointment"]
  - id: C7
    text: "The doctor table already has a status column, `column:doctor.status`."
    cites: ["context/generated/db-schema.json#table:doctor"]
  - id: C8
    text: "The patient table already has a status column, `column:patient.status`."
    cites: ["context/generated/db-schema.json#table:patient"]
  - id: C9
    text: "The web app talks to the API only through `web/src/api/client.ts`."
    cites: ["context/architecture.md#components"]
  - id: C10
    text: "Overlapping Scheduled appointments are forbidden by database `exclusion constraints`, turned into 409 responses."
    cites: ["context/architecture.md#double-booking"]
  - id: C11
    text: "\"Today\" uses the hospital time zone setting `HOSPITAL_TZ`."
    cites: ["context/architecture.md#time-and-slots"]
  - id: C12
    text: "`No background jobs` exist, so no scheduled job is affected."
    cites: ["context/architecture.md#open-points"]
  - id: C13
    text: "Medical history leaves the database only for the `patient's own page`."
    cites: ["context/data-model.md#patients"]
  - id: C14
    text: "The appointment page gets doctor and patient status from an extended appointment detail response: `Extend appointment GET (proposed default)`."
    cites: ["context/generated/api-inventory.json#http:GET:/api/appointments/:id", "runs/run_01M7HH0DM88V7JHMVZPSYX5BWE/ide-answers.md#a-04"]
  - id: C15
    text: "Free slots for booking come from the doctors router `api/src/routes/doctors.ts`."
    cites: ["context/generated/api-inventory.json#http:GET:/api/doctors/:id/slots"]
  - id: C16
    text: "Every appointment change is recorded in `appointment_event`."
    cites: ["context/data-model.md#appointments"]
  - id: C17
    text: "The API detail row shape `DetailRow` in appointmentRepo changes to carry doctor and patient status."
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/repositories/appointmentRepo.ts:DetailRow", "runs/run_01M7HH0DM88V7JHMVZPSYX5BWE/ide-answers.md#a-04"]
  - id: C18
    text: "The service that builds the detail response, `AppointmentService`, is in the change path for the extended detail."
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/services/appointmentService.ts:AppointmentService"]
  - id: C19
    text: "The web contract type `AppointmentDetail` in web/src/api/types.ts changes to carry doctor and patient status."
    cites: ["context/generated/module-map.json#sym:sample-system:web/src/api/types.ts:AppointmentDetail", "runs/run_01M7HH0DM88V7JHMVZPSYX5BWE/ide-answers.md#a-04"]
  - id: C20
    text: "The cancel, reschedule, complete and no-show routes are treated as possible readers of the detail shape to regression-check: `Yes, include actions (proposed default)`."
    cites: ["runs/run_01M7HH0DM88V7JHMVZPSYX5BWE/ide-answers.md#a-05", "context/generated/api-inventory.json#http:POST:/api/appointments/:id/cancel"]
  - id: C21
    text: "The web app reads settings through the hook `useSettings`."
    cites: ["context/generated/module-map.json#sym:sample-system:web/src/hooks/useSettings.tsx:useSettings"]
  - id: C22
    text: "Settings are served by the route `/api/settings`."
    cites: ["context/generated/api-inventory.json#http:GET:/api/settings"]
  - id: C23
    text: "The API's own contract type in `api/src/types.ts` (AppointmentDetail) changes to carry doctor and patient status."
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/types.ts:AppointmentDetail", "runs/run_01M7HH0DM88V7JHMVZPSYX5BWE/ide-answers.md#a-04"]
trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10, REQ-11, REQ-12, REQ-13, REQ-14, REQ-15, REQ-16, REQ-17, REQ-18, REQ-19]
known: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10, REQ-11, REQ-12, REQ-13, REQ-14, REQ-15, REQ-16, REQ-17, REQ-18, REQ-19]
assumed: []
unknown: []
questions: []
modules:
  - id: module:sample-system:web
    change: changed
    cites: ["context/generated/module-map.json#module:sample-system:web", "context/generated/module-map.json#sym:sample-system:web/src/pages/AppointmentDetailPage.tsx:AppointmentDetailPage", "context/generated/module-map.json#sym:sample-system:web/src/pages/BookAppointmentPage.tsx:BookAppointmentPage", "context/generated/module-map.json#sym:sample-system:web/src/api/types.ts:AppointmentDetail"]
  - id: module:sample-system:api
    change: changed
    cites: ["context/generated/module-map.json#module:sample-system:api", "context/generated/module-map.json#sym:sample-system:api/src/repositories/appointmentRepo.ts:DetailRow", "context/generated/module-map.json#sym:sample-system:api/src/types.ts:AppointmentDetail", "context/generated/api-inventory.json#http:GET:/api/appointments/:id", "runs/run_01M7HH0DM88V7JHMVZPSYX5BWE/ide-answers.md#a-04"]
  - id: module:sample-system:db
    change: existing
    cites: ["context/generated/module-map.json#module:sample-system:db", "context/generated/db-schema.json#table:appointment"]
apis:
  - id: http:GET:/api/appointments/:id
    change: changed
    cites: ["context/generated/api-inventory.json#http:GET:/api/appointments/:id", "runs/run_01M7HH0DM88V7JHMVZPSYX5BWE/ide-answers.md#a-04"]
  - id: http:POST:/api/appointments/:id/cancel
    change: existing
    cites: ["context/generated/api-inventory.json#http:POST:/api/appointments/:id/cancel", "runs/run_01M7HH0DM88V7JHMVZPSYX5BWE/ide-answers.md#a-05"]
  - id: http:POST:/api/appointments/:id/reschedule
    change: existing
    cites: ["context/generated/api-inventory.json#http:POST:/api/appointments/:id/reschedule", "runs/run_01M7HH0DM88V7JHMVZPSYX5BWE/ide-answers.md#a-05"]
  - id: http:POST:/api/appointments/:id/complete
    change: existing
    cites: ["context/generated/api-inventory.json#http:POST:/api/appointments/:id/complete", "runs/run_01M7HH0DM88V7JHMVZPSYX5BWE/ide-answers.md#a-05"]
  - id: http:POST:/api/appointments/:id/no-show
    change: existing
    cites: ["context/generated/api-inventory.json#http:POST:/api/appointments/:id/no-show", "runs/run_01M7HH0DM88V7JHMVZPSYX5BWE/ide-answers.md#a-05"]
  - id: http:POST:/api/appointments
    change: existing
    cites: ["context/generated/api-inventory.json#http:POST:/api/appointments", "artifacts/HMS-14/run_01M7HH0DM88V7JHMVZPSYX5BWE/req.md#item:REQ-12"]
  - id: http:GET:/api/doctors/:id/slots
    change: existing
    cites: ["context/generated/api-inventory.json#http:GET:/api/doctors/:id/slots", "artifacts/HMS-14/run_01M7HH0DM88V7JHMVZPSYX5BWE/req.md#item:REQ-12"]
  - id: http:GET:/api/settings
    change: existing
    cites: ["context/generated/api-inventory.json#http:GET:/api/settings", "artifacts/HMS-14/run_01M7HH0DM88V7JHMVZPSYX5BWE/req.md#item:REQ-7"]
tables:
  - id: table:appointment
    change: existing
    cites: ["context/generated/db-schema.json#table:appointment", "artifacts/HMS-14/run_01M7HH0DM88V7JHMVZPSYX5BWE/req.md#item:REQ-14"]
  - id: table:appointment_event
    change: existing
    cites: ["context/generated/db-schema.json#table:appointment_event", "artifacts/HMS-14/run_01M7HH0DM88V7JHMVZPSYX5BWE/req.md#item:REQ-14"]
  - id: table:doctor
    change: existing
    cites: ["context/generated/db-schema.json#table:doctor"]
  - id: table:patient
    change: existing
    cites: ["context/generated/db-schema.json#table:patient"]
answers:
  - id: A-04
    kind: question
    about: Q-01
    answer: "Extend appointment GET (proposed default)"
    decides: [http:GET:/api/appointments/:id, module:sample-system:api, C14, C17, C19, C23]
  - id: A-05
    kind: question
    about: Q-02
    answer: "Yes, include actions (proposed default)"
    decides: [http:POST:/api/appointments/:id/cancel, http:POST:/api/appointments/:id/reschedule, http:POST:/api/appointments/:id/complete, http:POST:/api/appointments/:id/no-show, C20]
---
# Impact analysis: HMS-14

## Summary
HMS-14 changes the web app's appointment page (`AppointmentDetailPage`, route `/appointments/:id`) to show a "Book again" button for Cancelled and No-show appointments, disabled when the doctor or patient is Inactive, and the book appointment page (`BookAppointmentPage`, route `/appointments/new`) to take a reason for visit when opened from it. Per A-04, `GET /api/appointments/:id` is extended to return the doctor's and patient's status. That changes the API detail row `DetailRow` (built through `AppointmentService`), the API contract type `AppointmentDetail` in api/src/types.ts and the web contract type `AppointmentDetail` in web/src/api/types.ts (C17, C18, C23, C19), and the appointment page never loads the full patient record (C13). No table or migration changes: the reason, doctor status and patient status columns already exist (C6, C7, C8). Main risks: per A-05, the cancel, reschedule, complete and no-show routes may return the same detail shape and need a regression check (C20); the book page's existing address handling must keep working (REQ-17).

## Impacted modules
- module:sample-system:web — changed: AppointmentDetailPage gains the "Book again" button and its disabled states; BookAppointmentPage fills in the reason for visit from its address; the web `AppointmentDetail` type gains doctor and patient status (C1, C2, C19).
- module:sample-system:api — changed: `DetailRow` in appointmentRepo, the API contract type `AppointmentDetail` in api/src/types.ts, and the detail response built by `AppointmentService` add doctor and patient status (A-04, C17, C18, C23).
- module:sample-system:db — existing: no migration; the columns needed already exist (C6, C7, C8).
- http:GET:/api/appointments/:id — changed: response adds doctor and patient status (C5, C14).
- http:POST:/api/appointments/:id/cancel — existing: possible reader of the detail shape; regression-check (A-05).
- http:POST:/api/appointments/:id/reschedule — existing: possible reader of the detail shape; regression-check (A-05).
- http:POST:/api/appointments/:id/complete — existing: possible reader of the detail shape; regression-check (A-05).
- http:POST:/api/appointments/:id/no-show — existing: possible reader of the detail shape; regression-check (A-05).
- http:POST:/api/appointments — existing: booking rules unchanged (REQ-12).
- http:GET:/api/doctors/:id/slots — existing: free slots unchanged (REQ-12, C15).
- http:GET:/api/settings — existing: read by the web app through `useSettings` for the hospital time zone behind "today" (REQ-7, C21, C22).
- table:appointment — existing: read for doctor, patient and reason; the original row is not changed (REQ-14).
- table:appointment_event — existing: the original's history is not changed; the new booking records its own events as today (C16).
- table:doctor — existing: status read (C7).
- table:patient — existing: status read (C8).

## Known
- REQ-1 to REQ-3, REQ-9 to REQ-11: appointment page logic in module:sample-system:web, with statuses from the changed http:GET:/api/appointments/:id via `DetailRow`, the API `AppointmentDetail` and the web `AppointmentDetail` (C17, C23, C19).
- REQ-4 to REQ-8, REQ-15, REQ-16, REQ-18, REQ-19: book appointment page in module:sample-system:web; "today" follows the hospital time zone (C11), which the web side reads from http:GET:/api/settings via `useSettings` (C21, C22); only the appointment's own reason is copied (C6).
- REQ-12, REQ-13: existing http:POST:/api/appointments and http:GET:/api/doctors/:id/slots, guarded by exclusion constraints (C10).
- REQ-14: table:appointment and table:appointment_event are read, not changed, for the original (C16).
- REQ-17: regression scope is the two pages in module:sample-system:web, the changed detail response, and the four action routes that may return it (C20).
- No scheduled job is affected (C12).
- Page-to-route mapping (C3, C4) is inferred from names: the UI route facts point only at web/src/App.tsx.

## Assumed
None

## Unknown
None

## Open questions
None open.

Answered in the IDE:
- A-04 (Q-01, answered by the user in the IDE): where the appointment page gets doctor and patient status → "Extend appointment GET (proposed default)". Decides http:GET:/api/appointments/:id (changed), module:sample-system:api (changed), C14, C17, C19 and C23. No mismatch with the facts.
- A-05 (Q-02, answered by the user in the IDE): whether the action routes count as possible readers of the detail shape → "Yes, include actions (proposed default)". Decides http:POST:/api/appointments/:id/cancel, /reschedule, /complete and /no-show (existing, regression-check) and C20. No mismatch; the pack has no call facts either way.
