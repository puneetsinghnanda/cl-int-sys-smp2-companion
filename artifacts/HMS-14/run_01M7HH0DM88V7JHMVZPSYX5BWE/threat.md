---
schema: threat-model.v1
run_id: run_01M7HH0DM88V7JHMVZPSYX5BWE
step: threat
attempt: 1
skill: threat-model@1.1.0
inputs:
  - path: artifacts/HMS-14/run_01M7HH0DM88V7JHMVZPSYX5BWE/design.md
    sha256: 4623a1e818ebe4e3423442e8697a9912d8940c0b19ebe9de19fb8b428bc6a035
  - path: context/architecture.md
    sha256: 9faab093d10933a46f6473b25c80728f8745e597100e650cd2537bbafb2b595a
  - path: context/data-model.md
    sha256: 2386ae151d54a73cdbe4fd9e0b69f8e8539c2870f5e52c12e5ed0191cfa95e55
  - path: context/generated/api-inventory.json
    sha256: ba024ab1b4b4151d26ce1afa77bd2dab1cec645ea737ce37f98e0e8674f795e9
pack_sha:
  sample-system: 7681f87749883d71abb4011d118366ca1242e1cc
claims:
  - id: C1
    text: "Every API request passes through `identify`, and in the MVP every request is the actor staff with every permission granted."
    cites: ["context/architecture.md#identity"]
  - id: C2
    text: "Routes call `requirePermission`, the hook sign-in will later replace."
    cites: ["context/architecture.md#identity"]
  - id: C3
    text: "Every appointment change is recorded in `appointment_event` with time and actor."
    cites: ["context/data-model.md#appointments"]
  - id: C4
    text: "Medical history leaves the database only for the `patient's own page`."
    cites: ["context/data-model.md#patients"]
  - id: C5
    text: "Overlapping Scheduled appointments are forbidden by database `exclusion constraints` and surface as 409."
    cites: ["context/architecture.md#double-booking"]
  - id: C6
    text: "SQL lives in repositories, rules in services: `repositories hold the SQL`."
    cites: ["context/architecture.md#components"]
  - id: C7
    text: "The reason for visit is handed to the book page in router `Navigation state (proposed default)`, not the URL."
    cites: ["runs/run_01M7HH0DM88V7JHMVZPSYX5BWE/ide-answers.md#a-09"]
  - id: C8
    text: "The detail response adds only `doctor_status/patient_status (proposed default)`, no patient record fields."
    cites: ["runs/run_01M7HH0DM88V7JHMVZPSYX5BWE/ide-answers.md#a-06"]
  - id: C9
    text: "The reason for visit keeps its `300-character` limit."
    cites: ["artifacts/HMS-14/run_01M7HH0DM88V7JHMVZPSYX5BWE/req.md#item:REQ-16"]
  - id: C10
    text: "Booking from Book again follows the existing rules unchanged, including `APT-2` free slots."
    cites: ["artifacts/HMS-14/run_01M7HH0DM88V7JHMVZPSYX5BWE/req.md#item:REQ-12"]
  - id: C11
    text: "Booking is done by the existing route in `api/src/routes/appointments.ts`."
    cites: ["context/generated/api-inventory.json#http:POST:/api/appointments"]
  - id: C12
    text: "An Inactive doctor or patient is `not offered for new use`."
    cites: ["context/glossary.md#inactive"]
  - id: C13
    text: "Invalid input is answered 400 with per-field messages via `ValidationError`."
    cites: ["context/conventions.md#errors"]
  - id: C14
    text: "Logs never carry request bodies or medical history: `Logs carry method, route and error message only`."
    cites: ["context/conventions.md#logging-and-data"]
trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10, REQ-11, REQ-12, REQ-13, REQ-14, REQ-15, REQ-16, REQ-17, REQ-18, REQ-19]
known: [T-1, T-2, T-3, T-4, T-5, T-7, T-8, T-9, T-10, T-11, T-12, T-13]
assumed: [T-6]
unknown: []
questions: []
threats:
  - id: T-1
    component: AppointmentDetailPage — Book again button and reasons
    stride: elevation_of_privilege
    threat: "The disabled state for an Inactive doctor or patient is only a UI control; a direct booking request could still try to book an Inactive doctor or patient."
    mitigation: "The server-side booking path stays the authority: POST /api/appointments applies the existing rule that a booking needs an Active doctor and patient (C10, C11, C12). Implement must confirm the route refuses Inactive doctors and patients, and add a test if none exists (see T-6)."
  - id: T-2
    component: AppointmentDetailPage — Book again button and reasons
    stride: information_disclosure
    threat: "Loading the full patient record to decide the disabled state would bring medical history onto the appointment page."
    mitigation: "Existing control kept by design: the page reads only doctor_status and patient_status from the detail response, never GET /api/patients/:id (C4, C8)."
  - id: T-3
    component: AppointmentDetailPage — Book again button and reasons
    stride: spoofing
    threat: "There is no sign-in, so anyone who can reach the shared URL can use Book again as staff."
    mitigation: "Existing accepted MVP state: every request passes through identify as the staff actor, and requirePermission guards routes so sign-in can be added later (C1, C2). No new exposure; Book again uses the same routes as manual booking."
  - id: T-4
    component: BookAppointmentPage — reason from navigation state
    stride: information_disclosure
    threat: "A free-text reason for visit put in the URL would land in browser history and server or proxy access logs."
    mitigation: "Design control: the reason travels in router navigation state, not the URL (C7); request bodies are never logged (C14)."
  - id: T-5
    component: BookAppointmentPage — reason from navigation state
    stride: tampering
    threat: "Navigation state is client-controlled, so the prefilled reason could be altered or oversized before submit."
    mitigation: "Existing control: the reason is user-editable by design and the booking request is validated server-side as today, including the 300-character limit, with 400 ValidationError on bad input (C9, C13)."
  - id: T-6
    component: BookAppointmentPage — reason from navigation state
    stride: elevation_of_privilege
    threat: "Doctor and patient ids in the address could be changed to an Inactive doctor or patient, bypassing the disabled button."
    mitigation: "Assumed existing control: POST /api/appointments enforces Active doctor and patient (C12). Implement must confirm this check exists server-side and keep it; if it does not, it must add it in the service layer (new control)."
  - id: T-7
    component: BookAppointmentPage — reason from navigation state
    stride: tampering
    threat: "A prefilled reason rendered into the page could inject markup if it were inserted as HTML."
    mitigation: "New control for implement: render and bind the reason only as plain text in the form field, never as HTML."
  - id: T-8
    component: web AppointmentDetail type — two status fields
    stride: tampering
    threat: "A wrong or missing status literal could make the page treat an Inactive record as Active and offer Book again."
    mitigation: "New control for implement: compare against the RecordStatus literals confirmed in code, treat any value other than the Active literal as inactive, and cover both statuses in tests. Server-side enforcement (T-1) remains the backstop."
  - id: T-9
    component: API AppointmentDetail type — two status fields
    stride: information_disclosure
    threat: "Extending the detail contract could pull in more doctor or patient fields than needed."
    mitigation: "Design control: only doctor_status and patient_status are added (C8); no patient record or medical history fields (C4)."
  - id: T-10
    component: appointmentRepo DetailRow and detail query
    stride: tampering
    threat: "Changing the detail query could introduce SQL built from request input."
    mitigation: "Existing control: SQL stays in repositories (C6). New control for implement: the added joins and columns use the existing parameterised query for the appointment id, with no string-built SQL."
  - id: T-11
    component: appointmentRepo DetailRow and detail query
    stride: denial_of_service
    threat: "Extra joins on every detail read and on the action routes that may return it add query cost."
    mitigation: "The joins are by primary key to doctor and patient, one row each; implement must reuse the existing doctor and patient joins rather than adding separate queries."
  - id: T-12
    component: AppointmentService — detail pass-through
    stride: repudiation
    threat: "A booking made via Book again might not be attributable."
    mitigation: "Existing control: the new appointment goes through the normal booking path, which records events with time and actor in appointment_event (C3); the original appointment is not written."
  - id: T-13
    component: todayIn helper — hospital-local today
    stride: tampering
    threat: "A client clock or time zone skew could choose the wrong default date."
    mitigation: "Existing control: the date is only a default; free slots and booking are validated server-side by the existing rules, including APT-2 (C10) and exclusion constraints (C5)."
---
# Threat model: HMS-14

## Components
- AppointmentDetailPage — Book again button and reasons: reads appointment status, doctor_status and patient_status; shows or disables Book again; hands doctor, patient and reason for visit to the book page.
- BookAppointmentPage — reason from navigation state: receives doctor and patient ids in the address and the reason for visit in navigation state; submits a booking.
- web AppointmentDetail type — two status fields: client contract for the detail response.
- API AppointmentDetail type — two status fields: server contract for the detail response.
- appointmentRepo DetailRow and detail query: reads appointment, doctor.status and patient.status.
- AppointmentService — detail pass-through: passes the detail through; the booking path is unchanged.
- todayIn helper — hospital-local today: computes the default booking date.

## Threats
- T-1 (AppointmentDetailPage, elevation_of_privilege): the disabled state is UI-only; the server booking path enforces Active doctor and patient (implement confirms).
- T-2 (AppointmentDetailPage, information_disclosure): no patient record loaded; only the two statuses are used.
- T-3 (AppointmentDetailPage, spoofing): no sign-in in the MVP; identify plus requirePermission is the existing seam; no new exposure.
- T-4 (BookAppointmentPage, information_disclosure): the reason stays out of the URL via navigation state; bodies are never logged.
- T-5 (BookAppointmentPage, tampering): client-controlled reason; server-side validation including the 300-character limit.
- T-6 (BookAppointmentPage, elevation_of_privilege): edited ids could target an Inactive record; server must refuse it (assumed, implement confirms or adds).
- T-7 (BookAppointmentPage, tampering): render the reason as text only (new control).
- T-8 (web AppointmentDetail type, tampering): wrong literal could offer Book again; compare against the confirmed Active literal and test both.
- T-9 (API AppointmentDetail type, information_disclosure): only the two status fields are added.
- T-10 (appointmentRepo DetailRow and detail query, tampering): parameterised SQL only (new control for the changed query).
- T-11 (appointmentRepo DetailRow and detail query, denial_of_service): primary-key joins only; reuse the existing joins.
- T-12 (AppointmentService, repudiation): appointment_event records actor and time for the new booking.
- T-13 (todayIn helper, tampering): the date is only a default; the server validates slots.

## Known
- T-1, T-3, T-12: rest on the identity seam and the appointment_event history (C1, C2, C3), plus the server booking route (C11).
- T-2, T-9: rest on the data-protection rule and the decided contract (C4, C8).
- T-4: rests on the navigation-state decision and the logging rule (C7, C14).
- T-5, T-13: rest on existing server-side validation and booking rules (C9, C10, C13, C5).
- T-7, T-8, T-10, T-11: new controls the implement step must build or follow, scoped by the design (C6).

## Assumed
- T-6: assumes POST /api/appointments already refuses an Inactive doctor or patient, because the ticket says a booking needs an Active doctor and patient and Inactive records are not offered for new use (C12). Implement must confirm this in code and add the check in the service layer if it is missing.

## Unknown
None

## Open questions
None
