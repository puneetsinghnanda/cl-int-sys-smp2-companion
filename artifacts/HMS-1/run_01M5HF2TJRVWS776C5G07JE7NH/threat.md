---
schema: threat-model.v1
run_id: run_01M5HF2TJRVWS776C5G07JE7NH
step: threat
attempt: 1
skill: threat-model@1.0.1
inputs:
  - path: artifacts/HMS-1/run_01M5HF2TJRVWS776C5G07JE7NH/design.md
    sha256: e617b8de9ea36ed77674d61d42f57c860fd9cb915d3618cb6e24b49929506873
  - path: context/architecture.md
    sha256: 9faab093d10933a46f6473b25c80728f8745e597100e650cd2537bbafb2b595a
  - path: context/data-model.md
    sha256: 2386ae151d54a73cdbe4fd9e0b69f8e8539c2870f5e52c12e5ed0191cfa95e55
  - path: context/generated/api-inventory.json
    sha256: 8dfdbde2e1d64359cb29b8f710a2a467d5e19b383d90def7c856e7738d4ca104
pack_sha:
  sample-system: 415369398bdad3122ac084a00766a01a3034f900
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
    text: "The index `appointment_patient_start_idx` supports per-patient lookups by start time."
    cites: ["context/generated/db-schema.json#index:appointment_patient_start_idx"]
  - id: C13
    text: "The API has a time zone check `isValidTimeZone`."
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/lib/time.ts:isValidTimeZone"]
  - id: C14
    text: "Every API request passes through `identify`, which sets the actor."
    cites: ["context/architecture.md#identity"]
trace: []
known: [T-1, T-2, T-3, T-5, T-6, T-7, T-9, T-11, T-12]
assumed: [T-4, T-8, T-10]
unknown:
  - {id: U-1, blocking: false}
questions:
  - id: Q-01
    blocking: false
    ask: tech lead
    text: "Is the deployed app reachable only by hospital staff (for example behind a network boundary), given that the MVP grants every request the staff actor?"
    why: "This ticket adds each patient's next appointment time and doctor to the list, so anyone who can reach GET /api/patients sees more personal data."
    evidence: ["context/architecture.md#identity", "context/generated/api-inventory.json#http:GET:/api/patients"]
    options: ["Staff-only network access", "Publicly reachable"]
    default: "Assume staff-only access until sign-in replaces identify and requirePermission."
threats:
  - id: T-1
    component: patientsRouter
    stride: spoofing
    threat: "A caller who is not hospital staff calls GET /api/patients and is treated as staff, because the MVP sets every request to the staff actor."
    mitigation: "Existing control: every request passes through identify and routes call requirePermission (C1, C2, C14); sign-in replaces these two functions later. No new control in this ticket; exposure depends on Q-01."
  - id: T-2
    component: patientsRouter
    stride: elevation_of_privilege
    threat: "The widened list gives every caller the next appointment of every patient, with no narrower permission than the existing list."
    mitigation: "Existing control: the route keeps its current requirePermission check (C2). New control for implement: keep the same permission on GET /api/patients and do not add the new data to any unauthenticated route."
  - id: T-3
    component: PatientService
    stride: information_disclosure
    threat: "Mapping the widened query result onto list items copies medical history fields from the patient row into the list response."
    mitigation: "Existing rule: medical history leaves the database only for the patient's own page (C3, C4). New control for implement: build list items from an explicit field list, and add a test that GET /api/patients items contain no medical history fields."
  - id: T-4
    component: PatientRepo
    stride: tampering
    threat: "The extended list SQL combines search input with the new appointment subquery, so unsafely built SQL could let search text change the query."
    mitigation: "Existing control: SQL is kept in repositories (C6), assumed to use bound parameters. New control for implement: pass all search and paging values as bound parameters; never concatenate them into the SQL text."
  - id: T-5
    component: PatientRepo
    stride: denial_of_service
    threat: "A per-patient next-appointment lookup makes every patient list page slow under load."
    mitigation: "Existing control: the lookup can use the index appointment_patient_start_idx (C12), and the list stays one page of the existing size. New control for implement: a single query per page (no per-patient calls), checked by a test or query plan."
  - id: T-6
    component: PatientRepo
    stride: information_disclosure
    threat: "The widened SELECT on the patient table pulls medical history columns into application memory or logs."
    mitigation: "Existing rule: medical history leaves the database only for the patient's own page (C4). New control for implement: select only the columns the list needs; never use a wildcard select on the patient table."
  - id: T-7
    component: AppointmentRepo
    stride: tampering
    threat: "The ticket changes appointment code paths and so weakens how appointments are written."
    mitigation: "Existing control: AppointmentRepo is not changed or used by this design; appointment changes stay recorded in appointment_event (C5, C6). No new control."
  - id: T-8
    component: api client (client.ts)
    stride: information_disclosure
    threat: "The client stores the widened list response somewhere it outlives the page, such as a persistent browser store."
    mitigation: "Existing control: the web app is served by the API from web/dist on the same origin (C7); assumed to hold responses in memory only. New control for implement: do not persist the patient list response to browser storage."
  - id: T-9
    component: PatientsPage
    stride: tampering
    threat: "A doctor name containing markup is rendered as HTML in the new column and runs in a staff member's browser."
    mitigation: "New control for implement: render doctor_name and the formatted time as plain text only, with no raw HTML insertion; add a test with a name containing markup characters."
  - id: T-10
    component: dateTime formatter
    stride: tampering
    threat: "A malformed time zone value from settings makes the formatter fail or show a wrong time."
    mitigation: "Existing control: the API has a time zone check isValidTimeZone (C13), assumed to guard the configured zone. New control for implement: fall back to the hospital zone default when the settings value is missing or invalid."
  - id: T-11
    component: useSettings
    stride: information_disclosure
    threat: "Extending settings to carry the time zone exposes other configuration through GET /api/settings."
    mitigation: "Existing route GET /api/settings is unchanged by the design (C9). New control for implement: if the zone is added to settings, add only the zone value, never other configuration or secrets."
  - id: T-12
    component: AppointmentDetailPage
    stride: elevation_of_privilege
    threat: "Changing the appointment id in the /appointments/:id link opens another patient's appointment."
    mitigation: "Existing behaviour: in the MVP every staff request may read any appointment through GET /api/appointments/:id (C1, C10, C11); the link adds no new access. No new control; covered by Q-01 until sign-in exists."
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

The threats cover the design's components for every requirement it traces; requirement ids are not repeated here because this step's inputs do not include the intent.

## Threats
- T-1 (patientsRouter, spoofing): non-staff caller treated as staff in the MVP; existing identify and requirePermission; depends on Q-01.
- T-2 (patientsRouter, elevation_of_privilege): widened list visible to every caller; keep the existing permission (new control: no unauthenticated route gets the data).
- T-3 (PatientService, information_disclosure): medical history copied into list items; new control: explicit field list and a test.
- T-4 (PatientRepo, tampering): search text altering the extended SQL; new control: bound parameters only.
- T-5 (PatientRepo, denial_of_service): slow per-patient lookups; existing index plus new control: one query per page.
- T-6 (PatientRepo, information_disclosure): medical history columns selected; new control: no wildcard select.
- T-7 (AppointmentRepo, tampering): weakened appointment writes; not changed, appointment_event still records changes.
- T-8 (api client (client.ts), information_disclosure): response persisted in the browser; new control: memory only.
- T-9 (PatientsPage, tampering): markup in a doctor name rendered as HTML; new control: plain-text rendering and a test.
- T-10 (dateTime formatter, tampering): malformed zone breaks display; existing isValidTimeZone plus new fallback control.
- T-11 (useSettings, information_disclosure): settings exposing more configuration; new control: add only the zone value.
- T-12 (AppointmentDetailPage, elevation_of_privilege): changed id opens another appointment; no new access in the MVP; covered by Q-01.

## Known
- T-1, T-2: the MVP actor and permission model is stated in the architecture (C1, C2, C14).
- T-3, T-6: the rule that medical history leaves the database only for the patient's own page is in the data model (C3, C4).
- T-5: the index used by the lookup is in the generated schema (C12).
- T-7: AppointmentRepo is untouched and appointment changes are recorded in appointment_event (C5, C6).
- T-9: the new column renders doctor names that come from stored data; the control is new and testable.
- T-11: GET /api/settings exists and is unchanged by the design (C9).
- T-12: any staff request can already read any appointment in the MVP (C1, C10, C11).

## Assumed
- T-4: repositories are assumed to use bound parameters. Basis: SQL is kept in repositories (C6); the implement step must confirm.
- T-8: the web client is assumed to keep responses in memory only. Basis: same-origin web app served from web/dist (C7).
- T-10: isValidTimeZone is assumed to guard the configured zone. Basis: the check exists in the API (C13).

## Unknown
- U-1 (non-blocking): whether the deployment is reachable only by staff, which decides how much T-1, T-2 and T-12 matter (Q-01).

## Open questions
- Q-01 (non-blocking, tech lead): is the deployed app reachable only by hospital staff? Default: assume staff-only access until sign-in replaces identify and requirePermission.
