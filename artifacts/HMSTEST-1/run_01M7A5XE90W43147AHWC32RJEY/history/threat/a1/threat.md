---
schema: threat-model.v1
run_id: run_01M7A5XE90W43147AHWC32RJEY
step: threat
attempt: 1
skill: threat-model@1.1.0
inputs:
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
    text: "In the MVP every request is the actor `staff` and every permission is granted."
    cites: ["context/architecture.md#identity"]
  - id: C2
    text: "Routes declare a permission by calling `requirePermission`."
    cites: ["context/architecture.md#identity"]
  - id: C3
    text: "Every API request passes through `identify`, which sets the actor."
    cites: ["context/architecture.md#identity"]
  - id: C4
    text: "Sign-in will replace identify and requirePermission only; the architecture describes no sign-in today (`Sign-in replaces`)."
    cites: ["context/architecture.md#identity"]
  - id: C5
    text: "Routes `parse input` and declare a permission; services hold the rules and transactions; repositories hold the SQL."
    cites: ["context/architecture.md#components"]
  - id: C6
    text: "The web app talks to the API only through `web/src/api/client.ts`."
    cites: ["context/architecture.md#components"]
  - id: C7
    text: "Database `exclusion constraints` forbid overlapping Scheduled appointments per doctor and per patient."
    cites: ["context/architecture.md#double-booking"]
  - id: C8
    text: "The service turns overlap violations into `409` responses."
    cites: ["context/architecture.md#double-booking"]
  - id: C9
    text: "Availability, slots and today use the hospital time zone `HOSPITAL_TZ`."
    cites: ["context/architecture.md#time-and-slots"]
  - id: C10
    text: "The API also serves the built web app from `web/dist`."
    cites: ["context/architecture.md#deployment-shape"]
  - id: C11
    text: "The health check is `/api/health`."
    cites: ["context/architecture.md#deployment-shape"]
  - id: C12
    text: "A patient row holds identity, contact details and the basic `medical history`."
    cites: ["context/data-model.md#patients"]
  - id: C13
    text: "Medical history leaves the database only for the patient's `own page`."
    cites: ["context/data-model.md#patients"]
  - id: C14
    text: "`appointment_event` records every appointment change with time and actor."
    cites: ["context/data-model.md#appointments"]
  - id: C15
    text: "Every table carries `created_by` and updated_by from the request actor."
    cites: ["context/data-model.md#data-rules"]
  - id: C16
    text: "Free slots are served by the route `/api/doctors/:id/slots`."
    cites: ["context/generated/api-inventory.json#http:GET:/api/doctors/:id/slots"]
  - id: C17
    text: "Appointments are booked through the route `/api/appointments`."
    cites: ["context/generated/api-inventory.json#http:POST:/api/appointments"]
  - id: C18
    text: "Appointments are cancelled through the route `/api/appointments/:id/cancel`."
    cites: ["context/generated/api-inventory.json#http:POST:/api/appointments/:id/cancel"]
  - id: C19
    text: "A single patient is served by the route `/api/patients/:id`."
    cites: ["context/generated/api-inventory.json#http:GET:/api/patients/:id"]
  - id: C20
    text: "Settings are served by the route `/api/settings`."
    cites: ["context/generated/api-inventory.json#http:GET:/api/settings"]
  - id: C21
    text: "`No background jobs` exist yet."
    cites: ["context/architecture.md#open-points"]
trace: []
known: [T-1, T-2, T-3, T-5, T-7, T-8, T-10, T-11]
assumed: [T-4, T-6, T-9, T-12, T-13, T-14]
unknown:
  - {id: U-1, blocking: false}
  - {id: U-2, blocking: false}
questions:
  - id: Q-01
    blocking: false
    ask: product owner
    text: "The ticket says patients sign in, search free slots and book one, but the architecture has no sign-in and treats every request as the staff actor. Should this review model the current staff-only booking flow, or a planned patient sign-in?"
    why: "Patient self-service booking would need per-patient ownership checks on slots, bookings and patient pages that do not exist today, which changes most threats below."
    evidence: ["context/architecture.md#identity", "context/generated/api-inventory.json#http:POST:/api/appointments", "artifacts/HMSTEST-1/run_01M7A5XE90W43147AHWC32RJEY/ticket.md#L11"]
    options: ["Model the current staff-only flow", "Model a planned patient sign-in"]
    default: "Model the current staff-only flow; list patient sign-in threats as new controls a later design must cover."
  - id: Q-02
    blocking: false
    ask: tech lead
    text: "Is the deployed app reachable only by hospital staff (for example behind a network boundary), given that the MVP grants every request the staff actor?"
    why: "If the app is publicly reachable, anyone can read patient records and book, cancel or change appointments."
    evidence: ["context/architecture.md#identity", "context/architecture.md#deployment-shape"]
    options: ["Staff-only network access", "Publicly reachable"]
    default: "Assume staff-only access until sign-in replaces identify and requirePermission."
threats:
  - id: T-1
    component: identify / requirePermission
    stride: spoofing
    threat: "Any caller who can reach the API is treated as hospital staff, because identify sets every request to the staff actor."
    mitigation: "Existing control: every request passes through identify and routes call requirePermission (C1, C2, C3); sign-in replaces these two functions later (C4). New control the implement step must build before patients sign in: real authentication in identify. Exposure today depends on Q-02."
  - id: T-2
    component: identify / requirePermission
    stride: elevation_of_privilege
    threat: "Every permission is granted, so any caller can use every booking, cancellation and patient-record operation."
    mitigation: "Existing control: routes already declare a permission through requirePermission (C2), so the hook for checks exists. New control the implement step must build: grant permissions by role, and for patient sign-in limit a patient to their own records and appointments (Q-01)."
  - id: T-3
    component: database
    stride: repudiation
    threat: "Every change is recorded with the same staff actor, so the audit trail cannot show which person booked, rescheduled or cancelled an appointment."
    mitigation: "Existing control: appointment_event records every change with time and actor (C14) and every table carries created_by and updated_by (C15). New control the implement step must build: a distinct actor per signed-in user once sign-in exists (C4)."
  - id: T-4
    component: routes
    stride: tampering
    threat: "Malformed or out-of-range booking input (doctor, patient, start time) reaches the service and creates an appointment outside a valid slot."
    mitigation: "Existing control: routes parse input (C5) and slots must end inside an availability block in the hospital time zone (C9), assumed to be re-checked when booking. New control the implement step must build if this is not so: validate on POST /api/appointments that the start is a generated slot of that doctor (C17)."
  - id: T-5
    component: routes
    stride: information_disclosure
    threat: "Changing the patient id in a request returns another patient's record, including medical history, through the patient page route."
    mitigation: "Existing rule: medical history leaves the database only for the patient's own page (C12, C13, C19); in the MVP every staff caller may read any patient (C1). New control the implement step must build for patient sign-in: an ownership check on the patient and appointment routes (Q-01)."
  - id: T-6
    component: routes
    stride: denial_of_service
    threat: "Repeated slot searches or bookings from one caller overload the API, or hold many slots so other patients cannot book."
    mitigation: "No cited rate-limit control exists. New control the implement step must build: request rate limits on /api/doctors/:id/slots and /api/appointments (C16, C17), and a limit on open Scheduled bookings per patient."
  - id: T-7
    component: services
    stride: tampering
    threat: "Two concurrent bookings for the same doctor or patient both succeed and create a double booking."
    mitigation: "Existing control: database exclusion constraints forbid overlapping Scheduled appointments per doctor and per patient (C7), and the service turns violations into 409 responses (C8). No new control."
  - id: T-8
    component: services
    stride: elevation_of_privilege
    threat: "A caller cancels, completes or marks no-show on an appointment that is not theirs."
    mitigation: "Existing control: services hold the rules (C5); in the MVP every staff caller may change any appointment (C1, C18). New control the implement step must build for patient sign-in: a patient may cancel only their own Scheduled appointments, and only staff may complete or mark no-show."
  - id: T-9
    component: repositories
    stride: tampering
    threat: "Search or filter text is built into SQL unsafely and changes the query on appointment or patient tables."
    mitigation: "Existing control: SQL is kept in repositories (C5), assumed to use bound parameters. New control the implement step must build if any query is not parameterised: pass every input value as a bound parameter."
  - id: T-10
    component: database
    stride: tampering
    threat: "An application bug writes an overlapping Scheduled appointment directly, bypassing the service."
    mitigation: "Existing control: the database, not the service, is the final guard through exclusion constraints (C7). No new control."
  - id: T-11
    component: database
    stride: information_disclosure
    threat: "Medical history is read into list or appointment responses where it is not needed."
    mitigation: "Existing rule: medical history leaves the database only for the patient's own page (C12, C13). New control the implement step must build: a test that appointment and slot responses carry no medical history fields."
  - id: T-12
    component: web app (web/src/api/client.ts)
    stride: information_disclosure
    threat: "Patient or appointment responses are kept in persistent browser storage and outlive the session on a shared hospital computer."
    mitigation: "Existing control: the web app talks to the API only through web/src/api/client.ts (C6), assumed to keep responses in memory only. New control the implement step must build if not: do not persist patient or appointment data in browser storage."
  - id: T-13
    component: web app (web/src/api/client.ts)
    stride: tampering
    threat: "Stored text such as a patient name, doctor name or cancellation reason is rendered as HTML and runs script in a staff browser."
    mitigation: "Existing control: the web app is served by the API from web/dist on the same origin (C10), assumed to render values as plain text. New control the implement step must build: no raw HTML rendering of stored values, and a content security policy on the served app."
  - id: T-14
    component: Render deployment
    stride: information_disclosure
    threat: "The health or settings routes expose configuration or secrets beyond what the web app needs."
    mitigation: "Existing routes: /api/health (C11) and /api/settings (C20), assumed to return only status and display settings such as HOSPITAL_TZ (C9). New control the implement step must build if not: return only non-secret values from these routes."
  - id: T-15
    component: Render deployment
    stride: denial_of_service
    threat: "A failed migration or seed at start-up stops the API, and with it the web app, so no booking is possible."
    mitigation: "Existing control: the health check /api/health (C11) lets the platform detect a failed start; no background jobs depend on the API (C21). No new control in this review."
---
# Threat model: HMSTEST-1

## Components
No design is an input to this security-review run, so the components come from the architecture section (C5, C6, C10).
- identify / requirePermission: sets the actor of every request and checks the permission each route declares; today every request is staff and every permission is granted (C1, C2, C3).
- routes: parse input for slot search (/api/doctors/:id/slots), booking (/api/appointments), appointment changes and patient pages (C5, C16, C17, C18, C19).
- services: hold the booking rules and transactions, and turn overlap violations into 409 responses (C5, C8).
- repositories: hold the SQL for patients, doctors and appointments (C5).
- database: holds patients with medical history, doctors, availability, appointments and appointment_event, with exclusion constraints against overlaps (C7, C12, C14, C15).
- web app (web/src/api/client.ts): the browser app, which talks to the API only through client.ts and is served from web/dist (C6, C10).
- Render deployment: runs migrate, seed-if-empty and the API, with the health check /api/health (C11).

## Threats
- T-1 (identify / requirePermission, spoofing): any caller is treated as staff. Existing identify and requirePermission; new control: real authentication before patients sign in. Depends on Q-02.
- T-2 (identify / requirePermission, elevation_of_privilege): every permission is granted. New control: role-based grants and patient-own-record limits.
- T-3 (database, repudiation): all changes are recorded as the same staff actor. Existing appointment_event and created_by; new control: a distinct actor per user.
- T-4 (routes, tampering): invalid booking input creates an appointment outside a slot. Existing input parsing; new control: check that the start is a generated slot.
- T-5 (routes, information_disclosure): a changed patient id shows another patient's record. Existing own-page rule; new control: ownership checks for patient sign-in.
- T-6 (routes, denial_of_service): slot search and booking floods, or slot hoarding. New control: rate limits and a cap on open bookings.
- T-7 (services, tampering): concurrent double booking. Existing exclusion constraints and 409 responses.
- T-8 (services, elevation_of_privilege): changing someone else's appointment. New control: patients cancel only their own appointments; only staff complete or mark no-show.
- T-9 (repositories, tampering): unsafe SQL building. Bound parameters assumed; new control if any query is not parameterised.
- T-10 (database, tampering): an overlap written past the service. Existing exclusion constraints.
- T-11 (database, information_disclosure): medical history leaking into other responses. Existing own-page rule; new control: a test.
- T-12 (web app (web/src/api/client.ts), information_disclosure): data persisted in the browser. New control: memory only.
- T-13 (web app (web/src/api/client.ts), tampering): stored text rendered as HTML. New control: plain-text rendering and a content security policy.
- T-14 (Render deployment, information_disclosure): health or settings routes exposing configuration. New control: non-secret values only.
- T-15 (Render deployment, denial_of_service): a failed migrate or seed stops the API. Existing health check.

## Known
- T-1, T-2: the MVP actor and permission model is stated in the architecture (C1, C2, C3, C4).
- T-3: appointment_event and created_by record the actor of every change (C14, C15).
- T-5: the own-page rule for medical history and the patient route are cited (C13, C19).
- T-7, T-10: the exclusion constraints and the 409 mapping are stated in the architecture (C7, C8).
- T-8: the cancel route exists and every staff caller may use it in the MVP (C1, C18).
- T-11: the rule that medical history leaves the database only for the patient's own page is in the data model (C12, C13).

## Assumed
- T-4: the booking route is assumed to re-check that the start is a valid slot. Basis: routes parse input and slots must end inside a block (C5, C9).
- T-6: no rate limit is assumed to exist. Basis: none is described in the architecture or the route inventory (C16, C17).
- T-9: repositories are assumed to use bound parameters. Basis: SQL is kept in repositories (C5).
- T-12: the web client is assumed to keep responses in memory only. Basis: all calls go through web/src/api/client.ts (C6).
- T-13: the web app is assumed to render stored values as plain text. Basis: same-origin app served from web/dist (C10).
- T-14: the health and settings routes are assumed to return only status and display settings. Basis: the routes exist (C11, C20).

## Unknown
- U-1 (non-blocking): the ticket describes patients signing in, but the architecture has no sign-in (C4). Which flow should be modelled? See Q-01.
- U-2 (non-blocking): whether the deployment is reachable only by staff, which decides how much T-1, T-2, T-5 and T-8 matter. See Q-02.

## Open questions
- Q-01 (non-blocking, product owner): should this review model the current staff-only booking flow, or a planned patient sign-in? Default: model the current staff-only flow, and list patient sign-in threats as new controls a later design must cover.
- Q-02 (non-blocking, tech lead): is the deployed app reachable only by hospital staff? Default: assume staff-only access until sign-in replaces identify and requirePermission.
