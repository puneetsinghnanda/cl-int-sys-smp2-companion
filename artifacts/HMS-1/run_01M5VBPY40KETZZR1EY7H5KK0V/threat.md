---
schema: threat-model.v1
run_id: run_01M5VBPY40KETZZR1EY7H5KK0V
step: threat
attempt: 2
skill: threat-model@1.0.1
inputs:
  - path: artifacts/HMS-1/run_01M5VBPY40KETZZR1EY7H5KK0V/design.md
    sha256: b35d0758b7a70010b1e5bf16befa8a356a6c9c2b09beb353e093f34384cad393
  - path: context/architecture.md
    sha256: 9faab093d10933a46f6473b25c80728f8745e597100e650cd2537bbafb2b595a
  - path: context/data-model.md
    sha256: 2386ae151d54a73cdbe4fd9e0b69f8e8539c2870f5e52c12e5ed0191cfa95e55
  - path: context/generated/api-inventory.json
    sha256: 8dfdbde2e1d64359cb29b8f710a2a467d5e19b383d90def7c856e7738d4ca104
pack_sha:
  sample-system: 415369398bdad3122ac084a00766a01a3034f900
resolves:
  - {id: F-1, how: "T-10 no longer assumes server-side validation of the zone; where the zone comes from is unknown U-3 (Q-03, linked to design Q-05), and T-10 relies only on the new client-side fallback."}
  - {id: F-2, how: "Q-01 cites the Render deployment shape (C15), treats the app as possibly public, and asks the tech lead to decide on access before release instead of assuming staff-only access."}
claims:
  - id: C1
    text: "In the MVP every request is the actor `staff` and every permission is granted."
    cites: ["context/architecture.md#identity"]
  - id: C2
    text: "Routes declare a permission by calling `requirePermission`."
    cites: ["context/architecture.md#identity"]
  - id: C3
    text: "A patient row holds identity, contact details and the basic `medical history`."
    cites: ["context/data-model.md#patients"]
  - id: C4
    text: "Medical history leaves the database only for the patient's `own page`."
    cites: ["context/data-model.md#patients"]
  - id: C5
    text: "`appointment_event` records every appointment change with time and actor."
    cites: ["context/data-model.md#appointments"]
  - id: C6
    text: "Repositories hold the `SQL`; services hold the rules."
    cites: ["context/architecture.md#components"]
  - id: C7
    text: "The API also serves the built web app from `web/dist`."
    cites: ["context/architecture.md#deployment-shape"]
  - id: C8
    text: "The patient list is served by the route `/api/patients`."
    cites: ["context/generated/api-inventory.json#http:GET:/api/patients"]
  - id: C9
    text: "Settings are served by the route `/api/settings`."
    cites: ["context/generated/api-inventory.json#http:GET:/api/settings"]
  - id: C10
    text: "The appointment page is routed at `/appointments/:id`."
    cites: ["context/generated/api-inventory.json#ui:/appointments/:id"]
  - id: C11
    text: "A single appointment is served by the route `/api/appointments/:id`."
    cites: ["context/generated/api-inventory.json#http:GET:/api/appointments/:id"]
  - id: C12
    text: "A doctor has one `specialization` and one department."
    cites: ["context/data-model.md#doctors"]
  - id: C13
    text: "Every API request passes through `identify`, which sets the actor."
    cites: ["context/architecture.md#identity"]
  - id: C14
    text: "Sign-in replaces the two identity functions `only`."
    cites: ["context/architecture.md#identity"]
  - id: C15
    text: "The system is deployed on `Render`, which builds the workspaces and runs the API."
    cites: ["context/architecture.md#deployment-shape"]
  - id: C16
    text: "The index `appointment_patient_start_idx` supports per-patient lookups by start time."
    cites: ["context/generated/db-schema.json#index:appointment_patient_start_idx"]
  - id: C17
    text: "Logs never carry `medical history`."
    cites: ["context/conventions.md#logging-and-data"]
  - id: C18
    text: "The list item addition carries only id, start and `doctor_name`."
    cites: ["artifacts/HMS-1/run_01M5VBPY40KETZZR1EY7H5KK0V/design.md#L177"]
trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10, REQ-11, REQ-12, REQ-13]
known: [T-1, T-2, T-3, T-5, T-6, T-7, T-9, T-10, T-12, T-13]
assumed: [T-4, T-8, T-11]
unknown:
  - {id: U-1, blocking: false}
  - {id: U-2, blocking: false}
  - {id: U-3, blocking: false}
questions:
  - id: Q-01
    blocking: false
    ask: tech lead
    text: "The app is deployed on Render and the API serves the web app, and the MVP grants every request the staff actor. Who can reach it today, and must access control be in place before the next-appointment column is released?"
    why: "In the MVP there is no effective access control on GET /api/patients, so if the deployment is public anyone can read each patient's next appointment and doctor."
    evidence: ["context/architecture.md#deployment-shape", "context/architecture.md#identity", "context/generated/api-inventory.json#http:GET:/api/patients"]
    options: ["Reachable by staff only", "Publicly reachable; add access control before release", "Publicly reachable; accept for the MVP"]
    default: "Treat the deployment as possibly public. The ticket adds appointment data to a list that already shows patient identity and contact data with no access control, so it does not change the trust boundary; record the risk and have the tech lead decide on access before release."
  - id: Q-02
    blocking: false
    ask: product owner
    text: "Is it acceptable for the patient list to show each patient's next doctor, given that a doctor's specialization can hint at a patient's condition?"
    why: "Medical history is kept out of the list, but the next doctor shown to every list viewer may still imply sensitive information."
    evidence: ["context/data-model.md#doctors", "context/data-model.md#patients"]
    options: ["Show the doctor's name", "Show only the date and time"]
    default: "Show the doctor's name, as the ticket asks, and note the inference risk."
  - id: Q-03
    blocking: false
    ask: tech lead
    text: "Does GET /api/settings carry the hospital time zone to the web, and is that value checked before it is stored?"
    why: "T-10 and T-11 depend on where the display zone comes from; the design asks the same in its Q-05."
    evidence: ["context/generated/api-inventory.json#http:GET:/api/settings", "artifacts/HMS-1/run_01M5VBPY40KETZZR1EY7H5KK0V/design.md#L148"]
    options: ["Yes, settings carry a checked zone", "Settings carry it unchecked", "Settings do not carry it"]
    default: "Rely on the client-side fallback in T-10 whichever answer applies."
threats:
  - id: T-1
    component: patientsRouter
    stride: spoofing
    threat: "A caller who is not hospital staff calls GET /api/patients and is treated as staff, because the MVP sets every request to the staff actor."
    mitigation: "No effective control exists in the MVP: identify sets every request to staff and requirePermission grants everything (C1, C2, C13). The deployment on Render may be publicly reachable (C15), so the only real mitigation is an access decision before release (Q-01); sign-in later replaces these two functions (C14). No new control in this ticket."
  - id: T-2
    component: patientsRouter
    stride: elevation_of_privilege
    threat: "The widened list gives every caller the next appointment of every patient, with no narrower permission than the existing list."
    mitigation: "No effective permission check exists in the MVP (C1). New control for implement: keep the same requirePermission call on GET /api/patients so sign-in can later restrict it, and add the data to no other route."
  - id: T-3
    component: PatientService
    stride: information_disclosure
    threat: "Mapping the widened query result onto list items copies medical history fields from the patient row into the list response."
    mitigation: "Existing rule: medical history leaves the database only for the patient's own page (C3, C4). New control for implement: build list items from an explicit field list, and add a test that GET /api/patients items contain no medical history fields."
  - id: T-4
    component: PatientRepo
    stride: tampering
    threat: "The extended list SQL combines search input with the new appointment subquery, so unsafely built SQL could let search text change the query."
    mitigation: "Existing control: SQL is kept in repositories (C6), assumed to use bound parameters. New control for implement: pass all search, paging and time values as bound parameters; never concatenate them into the SQL text."
  - id: T-5
    component: PatientRepo
    stride: denial_of_service
    threat: "A per-patient next-appointment lookup makes every patient list page slow under load."
    mitigation: "Existing control: the lookup can use the index appointment_patient_start_idx (C16). New control for implement: a single query per page (no per-patient calls), checked by a test."
  - id: T-6
    component: PatientRepo
    stride: information_disclosure
    threat: "The widened SELECT on the patient table pulls medical history columns into application memory or logs."
    mitigation: "Existing rules: medical history leaves the database only for the patient's own page (C4) and logs never carry medical history (C17). New control for implement: select only the columns the list needs; never use a wildcard select on the patient table."
  - id: T-7
    component: AppointmentRepo
    stride: tampering
    threat: "Code that reads appointments for the list could be tempted to reuse or change AppointmentRepo write paths, weakening how appointment changes are recorded."
    mitigation: "Existing control: the design keeps AppointmentRepo unchanged and does the lookup as a read-only join in PatientRepo; appointment changes stay recorded in appointment_event (C5, C6). New control for implement: no edits to AppointmentRepo in this change."
  - id: T-8
    component: api client (client.ts)
    stride: information_disclosure
    threat: "The client stores the widened list response somewhere it outlives the page, such as a persistent browser store."
    mitigation: "Existing control: the web app is served by the API from web/dist on the same origin (C7); assumed to hold responses in memory only. New control for implement: do not persist the patient list response to browser storage."
  - id: T-9
    component: PatientsPage
    stride: tampering
    threat: "A doctor name containing markup is rendered as HTML in the new column and runs in a staff member's browser."
    mitigation: "New control for implement: render doctor_name and the formatted time as plain text only, with no raw HTML insertion."
  - id: T-10
    component: dateTime formatter
    stride: tampering
    threat: "A missing or malformed time zone value makes the formatter fail or show a wrong time."
    mitigation: "New control for implement: when the zone from settings is missing or not a valid zone, fall back to the hospital zone default instead of failing. Where the zone comes from is asked in Q-03."
  - id: T-11
    component: useSettings
    stride: tampering
    threat: "The page trusts whatever zone value GET /api/settings returns, so a wrong value in settings shows every next appointment at the wrong time."
    mitigation: "Existing route GET /api/settings is unchanged (C9), assumed to return the configured hospital zone (Q-03). New control for implement: use the zone only for display, with the fallback from T-10."
  - id: T-12
    component: AppointmentDetailPage
    stride: elevation_of_privilege
    threat: "Changing the appointment id in the /appointments/:id link opens another patient's appointment."
    mitigation: "Existing behaviour: in the MVP every staff request may read any appointment through GET /api/appointments/:id (C1, C10, C11); the link adds no new access. No new control; covered by Q-01 until sign-in exists."
  - id: T-13
    component: PatientsPage
    stride: information_disclosure
    threat: "Showing each patient's next doctor on the list can reveal sensitive information by inference, because a doctor's specialization can hint at a patient's condition, even though medical history is excluded."
    mitigation: "Existing design control: the list item addition carries only id, start and doctor name (C18), so the specialization and department (C12) are never sent. New control for implement: keep the column to the doctor's name only. Acceptance of the name itself is asked in Q-02."
---
# Threat model: HMS-1

## Components
- patientsRouter: GET /api/patients route; handles search and paging input and returns patient list items, now with each patient's next appointment.
- PatientService: builds list items, including next_appointment (id, start, doctor name); must keep medical history out.
- PatientRepo: the patient list SQL, extended with the next Scheduled appointment and doctor name; reads the patient table, which holds identity, contact details and medical history.
- AppointmentRepo: appointment SQL; not changed or used by this design.
- api client (client.ts): web client that receives the widened list response.
- PatientsPage: renders the new column (time, doctor name, link, "None").
- dateTime formatter: formats the UTC start in the hospital time zone.
- useSettings: supplies settings, including the hospital time zone, to the page.
- AppointmentDetailPage: the existing appointment page the column links to.

## Threats
Highest risk first:
- T-1 (patientsRouter, spoofing): no effective access control in the MVP and a possibly public Render deployment; access decision before release (Q-01).
- T-3 (PatientService, information_disclosure): medical history copied into list items; new control: explicit field list and a test.
- T-6 (PatientRepo, information_disclosure): medical history columns selected; existing logging rule plus new control: no wildcard select.
- T-2 (patientsRouter, elevation_of_privilege): widened list visible to every caller; keep the existing permission call so sign-in can restrict it.
- T-13 (PatientsPage, information_disclosure): the next doctor can hint at a condition; only the name is sent and shown (Q-02).
- T-4 (PatientRepo, tampering): search text altering the extended SQL; new control: bound parameters only.
- T-9 (PatientsPage, tampering): markup in a doctor name rendered as HTML; new control: plain-text rendering.
- T-5 (PatientRepo, denial_of_service): slow per-patient lookups; existing index plus one query per page.
- T-12 (AppointmentDetailPage, elevation_of_privilege): changed id opens another appointment; no new access in the MVP; covered by Q-01.
- T-8 (api client (client.ts), information_disclosure): response persisted in the browser; new control: memory only.
- T-10 (dateTime formatter, tampering): missing or malformed zone breaks display; new fallback control.
- T-11 (useSettings, tampering): a wrong zone from settings shows wrong times; display only, with the T-10 fallback.
- T-7 (AppointmentRepo, tampering): appointment write paths changed by mistake; new control: no edits to AppointmentRepo.

## Known
- T-1, T-2: the MVP actor and permission model and the Render deployment are stated in the architecture (C1, C2, C13, C14, C15).
- T-3, T-6: medical history leaves the database only for the patient's own page, and logs never carry it (C3, C4, C17).
- T-5: the lookup can use an existing index (C16).
- T-7: appointment changes are recorded in appointment_event and the design leaves AppointmentRepo unchanged (C5, C6).
- T-9: the new column renders doctor names that come from stored data; the control is new and testable.
- T-10: the control is a new, testable client-side fallback that does not depend on where the zone comes from.
- T-12: any staff request can already read any appointment in the MVP (C1, C10, C11).
- T-13: doctors have a specialization (C12), and the design sends only the doctor's name (C18).

## Assumed
- T-4: repositories are assumed to use bound parameters. Basis: SQL is kept in repositories (C6); the implement step must confirm.
- T-8: the web client is assumed to keep responses in memory only. Basis: same-origin web app served from web/dist (C7).
- T-11: GET /api/settings is assumed to return the configured hospital zone (C9); checked by Q-03.

## Unknown
- U-1 (non-blocking): who can reach the Render deployment, which decides how much T-1, T-2 and T-12 matter (Q-01).
- U-2 (non-blocking): whether showing the next doctor is acceptable given the inference risk in T-13 (Q-02).
- U-3 (non-blocking): whether settings carry the hospital zone and whether it is checked when stored (Q-03).

## Open questions
- Q-01 (non-blocking, tech lead): who can reach the Render deployment, and is access control needed before release? Default: treat it as possibly public and decide on access before release.
- Q-02 (non-blocking, product owner): is showing each patient's next doctor acceptable? Default: show the name, as the ticket asks.
- Q-03 (non-blocking, tech lead): do settings carry a checked hospital zone? Default: rely on the client-side fallback.
