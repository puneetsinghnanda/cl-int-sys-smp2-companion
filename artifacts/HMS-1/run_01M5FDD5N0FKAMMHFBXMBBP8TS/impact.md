---
schema: impact.v1
run_id: run_01M5FDD5N0FKAMMHFBXMBBP8TS
step: impact
attempt: 1
skill: impact-analysis@1.1.1
inputs:
  - path: artifacts/HMS-1/run_01M5FDD5N0FKAMMHFBXMBBP8TS/req.md
    sha256: 1ed073519e20e57dc1df2f58ae0923916091d9ef5f8df4c7ba350901e572bc05
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
claims:
  - id: C1
    text: "The patient list API is GET /api/patients, declared in api/src/routes/patients.ts."
    cites: ["context/generated/api-inventory.json#http:GET:/api/patients"]
  - id: C2
    text: "An appointment page already exists: UI route /appointments/:id, rendered by AppointmentDetailPage, backed by GET /api/appointments/:id."
    cites: ["context/generated/api-inventory.json#ui:/appointments/:id", "context/generated/module-map.json#sym:sample-system:web/src/pages/AppointmentDetailPage.tsx:AppointmentDetailPage", "context/generated/api-inventory.json#http:GET:/api/appointments/:id"]
  - id: C3
    text: "The patient list UI is route /patients, rendered by PatientsPage."
    cites: ["context/generated/api-inventory.json#ui:/patients", "context/generated/module-map.json#sym:sample-system:web/src/pages/PatientsPage.tsx:PatientsPage"]
  - id: C4
    text: "The web app talks to the API only through web/src/api/client.ts; each resource has its own router under /api."
    cites: ["context/architecture.md#components"]
  - id: C5
    text: "Instants are stored in UTC and local display uses the hospital time zone (HOSPITAL_TZ, default Asia/Kolkata)."
    cites: ["context/architecture.md#time-and-slots"]
  - id: C6
    text: "An appointment links one doctor and one patient for [starts_at, ends_at) in UTC, with status SCHEDULED, COMPLETED, CANCELLED or NO_SHOW."
    cites: ["context/data-model.md#appointments"]
  - id: C7
    text: "The patient row holds the medical history, and medical history leaves the database only for the patient's own page."
    cites: ["context/data-model.md#patients"]
  - id: C8
    text: "An index appointment_patient_start_idx exists on table appointment over (patient_id, starts_at)."
    cites: ["context/generated/db-schema.json#index:appointment_patient_start_idx"]
  - id: C9
    text: "Doctors and patients each have a status column."
    cites: ["context/generated/db-schema.json#column:doctor.status", "context/generated/db-schema.json#column:patient.status"]
  - id: C10
    text: "No background jobs exist yet."
    cites: ["context/architecture.md#open-points"]
trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10, REQ-11, REQ-12]
known: [REQ-1, REQ-7, REQ-8, REQ-9, REQ-10, REQ-11]
assumed: [REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-12]
unknown: [U-1, U-2, U-3]
unknowns:
  - id: U-1
    text: "The cited facts do not show which other web pages call GET /api/patients besides the patient list."
    question: Q-01
  - id: U-2
    text: "Doctors and patients have a status column (C9), but the intent does not say whether an Inactive doctor or patient changes what the column shows."
    question: Q-02
  - id: U-3
    text: "The display format of the start date and time is still open (intent Q-01)."
    question: Q-03
questions:
  - id: Q-01
    blocking: false
    ask: tech lead
    text: "Which other pages call GET /api/patients, and may its rows gain a next-appointment field?"
    why: "If the list response changes shape, every caller of the route sees it; an added field is safe only if callers ignore unknown fields."
    evidence: ["context/generated/api-inventory.json#http:GET:/api/patients", "context/architecture.md#components"]
    options: ["Add the field to GET /api/patients rows", "Serve the next appointments from a separate route"]
    default: "Add the field to the existing rows; design confirms callers are unaffected"
  - id: Q-02
    blocking: false
    ask: product owner
    text: "Should an upcoming Scheduled appointment with an Inactive doctor, or for an Inactive patient, still be shown in the Next appointment column?"
    why: "The selection rule uses status Scheduled and start time only, but status columns exist on doctor and patient."
    evidence: ["artifacts/HMS-1/run_01M5FDD5N0FKAMMHFBXMBBP8TS/req.md#L53", "context/generated/db-schema.json#column:doctor.status", "context/generated/db-schema.json#column:patient.status"]
    options: ["Show it", "Hide it"]
    default: "Show it, because REQ-2 selects by Scheduled status and start time only"
  - id: Q-03
    blocking: false
    ask: product owner
    text: "In what format should the next appointment's start date and time be shown?"
    why: "The web module renders the value; the time zone is fixed but the format is not."
    evidence: ["artifacts/HMS-1/run_01M5FDD5N0FKAMMHFBXMBBP8TS/req.md#L145", "context/architecture.md#time-and-slots"]
    options: ["Reuse the app's existing date-time format", "A new format chosen by the product owner"]
    default: "Reuse the app's existing date-time format, shown in Asia/Kolkata"
modules:
  - id: module:sample-system:api
    change: changed
    cites: ["context/generated/module-map.json#module:sample-system:api", "context/generated/module-map.json#sym:sample-system:api/src/repositories/patientRepo.ts:PatientRepo", "context/architecture.md#components"]
  - id: module:sample-system:web
    change: changed
    cites: ["context/generated/module-map.json#module:sample-system:web", "context/generated/module-map.json#sym:sample-system:web/src/pages/PatientsPage.tsx:PatientsPage"]
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
tables:
  - id: table:patient
    change: existing
    cites: ["context/generated/db-schema.json#table:patient", "context/data-model.md#patients"]
  - id: table:appointment
    change: existing
    cites: ["context/generated/db-schema.json#table:appointment", "context/data-model.md#appointments"]
  - id: table:doctor
    change: existing
    cites: ["context/generated/db-schema.json#table:doctor"]
---
# Impact analysis: HMS-1

## Summary
HMS-1 changes the patient list end to end: the web patient list page (PatientsPage, route /patients) gains a column, and the API behind it (GET /api/patients) must supply each patient's next Scheduled appointment with its start time and doctor name (C1, C3, C4). The data already exists: tables patient, appointment and doctor are only read, and an index on appointment (patient_id, starts_at) already supports a per-patient lookup by start time (C8), so no table or migration change is expected. The link target exists: UI route /appointments/:id with AppointmentDetailPage (C2), which answers the intent's question about the appointment page. The main risks are changing the shape of GET /api/patients for any other caller (U-1), keeping medical history out of the list query (C7), and converting UTC start times to the hospital time zone for display (C5). There are no background jobs to update (C10).

## Impacted modules
- module:sample-system:api, **changed**: the patient list service and repository (PatientRepo) must return each patient's next upcoming Scheduled appointment, start time and doctor name.
- module:sample-system:web, **changed**: PatientsPage shows the new "Next appointment" column, "None", and the link to /appointments/:id.
- module:sample-system:db, **existing**: no new table or migration expected; the existing index appointment_patient_start_idx covers the lookup.
- http:GET:/api/patients, **changed** (assumed, see Assumed): each row gains the next-appointment data; paging (20 per page), search and sort stay as they are.
- http:GET:/api/appointments/:id, **existing**: backs the appointment page the new link opens.
- table:patient, **existing**: read for the list; medical history columns must stay out of the list query.
- table:appointment, **existing**: read for the earliest Scheduled appointment starting after now; Cancelled, Completed and No-show rows are excluded.
- table:doctor, **existing**: read for the doctor's name.

## Known
- REQ-1: the new column belongs on PatientsPage at /patients, in module:sample-system:web (C3).
- REQ-7: the link target exists: UI route /appointments/:id with AppointmentDetailPage and http:GET:/api/appointments/:id (C2).
- REQ-8: paging of the patient list stays in http:GET:/api/patients, which already exists (C1).
- REQ-9: search on the patient list stays in http:GET:/api/patients (C1).
- REQ-10: sort order of the patient list stays in http:GET:/api/patients (C1).
- REQ-11: medical history lives on table:patient and leaves the database only for the patient's own page, so the list query must not select it (C7).

## Assumed
- REQ-2: the earliest Scheduled appointment after now is read from table:appointment through module:sample-system:api, assumed to be returned by http:GET:/api/patients. Basis: it is the only patient list route (C1) and the web app reaches data only through the API (C4); the index on (patient_id, starts_at) supports the lookup (C8).
- REQ-3: the start time is stored in UTC and converted to the hospital time zone for display (C5); it reaches the page through the same assumed route.
- REQ-4: the doctor's name is read from table:doctor and returned through the same assumed route (C6).
- REQ-5: Cancelled, Completed and No-show are filtered on the appointment status (C6) in the same assumed query.
- REQ-6: "None" is shown when the assumed route returns no next appointment for a patient.
- REQ-12: "None" is plain text, as assumed in the intent; it needs no API support beyond REQ-6.

## Unknown
- U-1 (non-blocking): the facts do not show other callers of GET /api/patients. Raises Q-01.
- U-2 (non-blocking): the effect of an Inactive doctor or patient on the column is not specified, although status columns exist (C9). Raises Q-02.
- U-3 (non-blocking): the date-time display format is still open. Raises Q-03.

## Open questions
- Q-01 (non-blocking, tech lead): which other pages call GET /api/patients, and may its rows gain a next-appointment field? Proposed default: add the field; design confirms callers are unaffected.
- Q-02 (non-blocking, product owner): should an appointment with an Inactive doctor, or for an Inactive patient, still be shown? Proposed default: show it.
- Q-03 (non-blocking, product owner): what date-time format should the column use? Proposed default: the app's existing format, in Asia/Kolkata.
