---
schema: impact.v1
run_id: run_01M5HF2TJRVWS776C5G07JE7NH
step: impact
attempt: 2
skill: impact-analysis@1.1.1
inputs:
  - path: artifacts/HMS-1/run_01M5HF2TJRVWS776C5G07JE7NH/req.md
    sha256: b8f74a55a75818106966c7b4ca51081d509c09577bfc68cee5cea3f9ec42d7a4
  - path: context/generated/module-map.json
    sha256: 31d9ca15805fc1b6488e8da89fdb0ee7714adbcdfda176d69581ab8f3d6482da
  - path: context/generated/api-inventory.json
    sha256: 8dfdbde2e1d64359cb29b8f710a2a467d5e19b383d90def7c856e7738d4ca104
  - path: context/generated/db-schema.json
    sha256: 43f6d599b6cbbd10eeb62bfbc1f7b947dea565e519d28b267856a8570f6de2dc
  - path: context/architecture.md
    sha256: 9faab093d10933a46f6473b25c80728f8745e597100e650cd2537bbafb2b595a
  - path: context/data-model.md
    sha256: 2386ae151d54a73cdbe4fd9e0b69f8e8539c2870f5e52c12e5ed0191cfa95e55
pack_sha:
  sample-system: 415369398bdad3122ac084a00766a01a3034f900
resolves:
  - {id: F-1, how: "Cited the UI route ui:/appointments/:id from App.tsx as the link target (C18), moved REQ-8 to Known, and dropped U-2 and Q-02."}
  - {id: F-2, how: "Listed patientsRouter (C19) and PatientService (C20) with PatientRepo as the API symbols the widened patient list passes through."}
  - {id: F-3, how: "Listed the web client api symbol in client.ts (C21) as a changed caller of GET /api/patients, alongside PatientsPage."}
  - {id: F-4, how: "Based REQ-13 on glossary#inactive (C23) instead of data-model#doctors, and noted that DELETE /api/doctors/:id (C22) may affect it."}
claims:
  - id: C1
    text: "The patient list is served by the route `/api/patients`."
    cites: ["context/generated/api-inventory.json#http:GET:/api/patients"]
  - id: C2
    text: "A single appointment is served by the route `/api/appointments/:id`."
    cites: ["context/generated/api-inventory.json#http:GET:/api/appointments/:id"]
  - id: C3
    text: "The web patient list is the page `PatientsPage`."
    cites: ["context/generated/module-map.json#sym:sample-system:web/src/pages/PatientsPage.tsx:PatientsPage"]
  - id: C4
    text: "An appointment page already exists as `AppointmentDetailPage`."
    cites: ["context/generated/module-map.json#sym:sample-system:web/src/pages/AppointmentDetailPage.tsx:AppointmentDetailPage"]
  - id: C5
    text: "Patient rows are read through `PatientRepo`."
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/repositories/patientRepo.ts:PatientRepo"]
  - id: C6
    text: "Appointment rows are read through `AppointmentRepo`."
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/repositories/appointmentRepo.ts:AppointmentRepo"]
  - id: C7
    text: "The appointment table has a `status` column."
    cites: ["context/generated/db-schema.json#column:appointment.status"]
  - id: C8
    text: "The appointment table has a `starts_at` column."
    cites: ["context/generated/db-schema.json#column:appointment.starts_at"]
  - id: C9
    text: "The appointment table has a `doctor_id` column."
    cites: ["context/generated/db-schema.json#column:appointment.doctor_id"]
  - id: C10
    text: "An index `appointment_patient_start_idx` already exists on the appointment table."
    cites: ["context/generated/db-schema.json#index:appointment_patient_start_idx"]
  - id: C11
    text: "Instants are stored in `UTC` and shown in the hospital time zone."
    cites: ["context/architecture.md#time-and-slots"]
  - id: C12
    text: "The hospital time zone setting is `HOSPITAL_TZ`."
    cites: ["context/architecture.md#time-and-slots"]
  - id: C13
    text: "The web app calls the API only through `client.ts`."
    cites: ["context/architecture.md#components"]
  - id: C14
    text: "No `background jobs` exist yet."
    cites: ["context/architecture.md#open-points"]
  - id: C15
    text: "Medical history leaves the database only for the patient's `own page`."
    cites: ["context/data-model.md#patients"]
  - id: C16
    text: "Appointment status values include `SCHEDULED`."
    cites: ["context/data-model.md#appointments"]
  - id: C17
    text: "The web app has a date-time formatter `dateTime`."
    cites: ["context/generated/module-map.json#sym:sample-system:web/src/lib/format.ts:dateTime"]
  - id: C18
    text: "The web app routes `/appointments/:id` to the appointment page."
    cites: ["context/generated/api-inventory.json#ui:/appointments/:id"]
  - id: C19
    text: "Patient routes are defined in `patientsRouter`."
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/routes/patients.ts:patientsRouter"]
  - id: C20
    text: "Patient rules live in `PatientService`."
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/services/patientService.ts:PatientService"]
  - id: C21
    text: "The web client's API calls go through the `api` symbol in client.ts."
    cites: ["context/generated/module-map.json#sym:sample-system:web/src/api/client.ts:api"]
  - id: C22
    text: "Doctors can be deleted through the route `/api/doctors/:id`."
    cites: ["context/generated/api-inventory.json#http:DELETE:/api/doctors/:id"]
  - id: C23
    text: "An Inactive doctor is kept for `history` but not offered for new use."
    cites: ["context/glossary.md#inactive"]
  - id: C24
    text: "The patient table has the index `patient_name_idx`."
    cites: ["context/generated/db-schema.json#index:patient_name_idx"]
  - id: C25
    text: "Paging input is parsed in the API by `parsePage`."
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/lib/codes.ts:parsePage"]
  - id: C26
    text: "The web patient list is routed at `/patients`."
    cites: ["context/generated/api-inventory.json#ui:/patients"]
trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10, REQ-11, REQ-12, REQ-13]
known: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-12]
assumed: [REQ-10, REQ-11, REQ-13]
unknown:
  - {id: U-1, blocking: false}
  - {id: U-3, blocking: false}
questions:
  - id: Q-01
    blocking: false
    ask: tech lead
    text: "Are the patient list's search and sort done by GET /api/patients on the server, or in PatientsPage in the browser?"
    why: "Adding next-appointment data to the list must not change search results or order (REQ-10, REQ-11); where they run decides what to regression-test."
    evidence: ["context/generated/api-inventory.json#http:GET:/api/patients", "context/generated/db-schema.json#index:patient_name_idx", "context/generated/module-map.json#sym:sample-system:api/src/lib/codes.ts:parsePage"]
    options: ["Server side in GET /api/patients", "Client side in PatientsPage"]
    default: "Assume server side and regression-test GET /api/patients search and sort."
  - id: Q-03
    blocking: false
    ask: product owner
    text: "Is the existing web dateTime formatter the agreed format for the Next appointment column?"
    why: "The intent leaves the display format open (intent U-1, its Q-01); reusing dateTime keeps the change inside the web module."
    evidence: ["context/generated/module-map.json#sym:sample-system:web/src/lib/format.ts:dateTime", "artifacts/HMS-1/run_01M5HF2TJRVWS776C5G07JE7NH/req.md#L124"]
    options: ["Reuse dateTime", "Add a new formatter"]
    default: "Reuse dateTime with the hospital time zone."
modules:
  - id: module:sample-system:api
    change: changed
    cites: ["context/generated/module-map.json#module:sample-system:api", "context/generated/module-map.json#sym:sample-system:api/src/routes/patients.ts:patientsRouter", "context/generated/module-map.json#sym:sample-system:api/src/services/patientService.ts:PatientService", "context/generated/module-map.json#sym:sample-system:api/src/repositories/patientRepo.ts:PatientRepo"]
  - id: module:sample-system:web
    change: changed
    cites: ["context/generated/module-map.json#module:sample-system:web", "context/generated/module-map.json#sym:sample-system:web/src/pages/PatientsPage.tsx:PatientsPage", "context/generated/module-map.json#sym:sample-system:web/src/api/client.ts:api"]
  - id: module:sample-system:db
    change: existing
    cites: ["context/generated/module-map.json#module:sample-system:db", "context/generated/db-schema.json#index:appointment_patient_start_idx"]
apis:
  - id: http:GET:/api/patients
    change: changed
    cites: ["context/generated/api-inventory.json#http:GET:/api/patients"]
  - id: http:GET:/api/appointments/:id
    change: existing
    cites: ["context/generated/api-inventory.json#http:GET:/api/appointments/:id", "context/generated/api-inventory.json#ui:/appointments/:id"]
  - id: http:DELETE:/api/doctors/:id
    change: existing
    cites: ["context/generated/api-inventory.json#http:DELETE:/api/doctors/:id"]
tables:
  - id: table:patient
    change: existing
    cites: ["context/generated/db-schema.json#table:patient", "context/data-model.md#patients"]
  - id: table:appointment
    change: existing
    cites: ["context/generated/db-schema.json#table:appointment", "context/generated/db-schema.json#column:appointment.status"]
  - id: table:doctor
    change: existing
    cites: ["context/generated/db-schema.json#table:doctor", "context/generated/db-schema.json#column:appointment.doctor_id"]
---
# Impact analysis: HMS-1

## Summary
HMS-1 changes the patient list in the API and the web app. In the API, `GET /api/patients` gains each patient's next Scheduled appointment (start and doctor); the widened response passes through `patientsRouter`, `PatientService` and `PatientRepo`. In the web app, the `api` client in client.ts returns the new fields and `PatientsPage` (routed at `/patients`) shows them in a new column linking to the existing appointment page at `/appointments/:id`. No table, migration or background job changes: the data already exists in `appointment`, `patient` and `doctor`, and `appointment_patient_start_idx` already covers a per-patient lookup by start time. The main risks are slowing down or reordering the patient list (search, sort and the 20-per-page paging must stay the same), leaking medical history if the list query is widened carelessly, and doctor deletion through `DELETE /api/doctors/:id` leaving an appointment without a doctor name.

## Impacted modules
- module:sample-system:api — changed: `patientsRouter`, `PatientService` and `PatientRepo` carry the next appointment in the patient list query and response.
- module:sample-system:web — changed: the `api` client in client.ts returns the new fields, and `PatientsPage` adds the "Next appointment" column and link.
- module:sample-system:db — existing: no migration; `appointment_patient_start_idx` is reused.
- http:GET:/api/patients — changed: each list item gains next-appointment start time and doctor name.
- http:GET:/api/appointments/:id — existing: unchanged; it backs the appointment page at the UI route `/appointments/:id` that the column links to.
- http:DELETE:/api/doctors/:id — existing: unchanged, but deleting a doctor may remove the name REQ-13 relies on.
- table:patient — existing: read only; medical history columns must stay out of the list.
- table:appointment — existing: read for `status`, `starts_at` and `doctor_id`.
- table:doctor — existing: read for the doctor's name.

## Known
- REQ-1, REQ-3, REQ-5: the column and its start time and doctor name are rendered in `PatientsPage` from data added to GET /api/patients and returned by the web `api` client (C1, C3, C21).
- REQ-2, REQ-6, REQ-7: picking the earliest Scheduled appointment after now uses `appointment.status` and `appointment.starts_at`, supported by `appointment_patient_start_idx` (C7, C8, C10, C16).
- REQ-4: instants are stored in UTC and shown in the hospital time zone, `HOSPITAL_TZ` (C11, C12).
- REQ-8: the appointment page `AppointmentDetailPage` is routed at `/appointments/:id` and backed by GET /api/appointments/:id, so it is the link target (C2, C4, C18).
- REQ-9: paging is part of GET /api/patients, which is changed, so 20 per page is a regression check on that route (C1, C25).
- REQ-12: medical history leaves the database only for the patient's own page, so the widened list query in `PatientService` and `PatientRepo` must not select it (C15, C19, C20).

## Assumed
- REQ-10, REQ-11: search and sort are assumed to run in GET /api/patients, so they are regression checks on that route. Basis: routes parse input and services hold the rules (architecture#components), the patient table has `patient_name_idx` (C24), and paging is parsed in the API by `parsePage` (C25). Confirm with Q-01.
- REQ-13: an Inactive doctor's name is still shown. Basis: an Inactive doctor is kept for history (glossary#inactive, C23). Doctors can still be deleted through `DELETE /api/doctors/:id` (C22), so a deleted doctor may leave no name to show; design should cover that case.

## Unknown
- U-1 (non-blocking): whether search and sort run on the server or in the browser (Q-01).
- U-3 (non-blocking): whether the existing `dateTime` formatter is the agreed display format (Q-03).

## Open questions
- Q-01 (non-blocking, tech lead): are search and sort server side in GET /api/patients? Default: assume server side and regression-test that route.
- Q-03 (non-blocking, product owner): reuse the web `dateTime` formatter for the column? Default: reuse it with the hospital time zone.
