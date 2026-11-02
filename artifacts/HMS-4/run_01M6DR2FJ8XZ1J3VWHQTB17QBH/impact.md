---
schema: impact.v1
run_id: run_01M6DR2FJ8XZ1J3VWHQTB17QBH
step: impact
attempt: 2
skill: impact-analysis@1.1.1
inputs:
  - path: artifacts/HMS-4/run_01M6DR2FJ8XZ1J3VWHQTB17QBH/req.md
    sha256: a4368b1b80bb6173569256c5cf6e26985261df9a7da1faff3a4f3be7a67ec369
  - path: context/generated/module-map.json
    sha256: 04f84ff09b4ad6ffaba59ec846716d25f03c8aae22292958a47ebb9247632345
  - path: context/generated/api-inventory.json
    sha256: 9ba29278d8d66b7341defc3c67d6e7d68508ce25ee2991ba256be2b37194ec61
  - path: context/generated/db-schema.json
    sha256: f047583e5b452649edd65cf48083feaad4c664531202fba78652d06b415749f5
  - path: context/architecture.md
    sha256: 9faab093d10933a46f6473b25c80728f8745e597100e650cd2537bbafb2b595a
  - path: context/data-model.md
    sha256: 2386ae151d54a73cdbe4fd9e0b69f8e8539c2870f5e52c12e5ed0191cfa95e55
pack_sha:
  sample-system: bd98ff08b83f8762c5e5c073ba5fadef88169b33
claims:
  - id: C1
    text: "The patient list is served by the route `/api/patients` (GET)."
    cites: ["context/generated/api-inventory.json#http:GET:/api/patients"]
  - id: C2
    text: "The patient list UI route is `/patients`, declared in `web/src/App.tsx`."
    cites: ["context/generated/api-inventory.json#ui:/patients"]
  - id: C3
    text: "The web app talks to the API only through `web/src/api/client.ts`."
    cites: ["context/architecture.md#components"]
  - id: C4
    text: "The API is layered into routes, services and `repositories` that hold the SQL."
    cites: ["context/architecture.md#components"]
  - id: C5
    text: "Appointment status is one of SCHEDULED, `COMPLETED`, CANCELLED or NO_SHOW."
    cites: ["context/data-model.md#appointments"]
  - id: C6
    text: "Appointment instants are stored in `UTC`."
    cites: ["context/data-model.md#appointments", "context/architecture.md#time-and-slots"]
  - id: C7
    text: "Hospital-local dates use `HOSPITAL_TZ`, default Asia/Kolkata."
    cites: ["context/architecture.md#time-and-slots"]
  - id: C8
    text: "An index `appointment_patient_start_idx` exists on appointment (patient_id, starts_at)."
    cites: ["context/generated/db-schema.json#index:appointment_patient_start_idx"]
  - id: C9
    text: "Medical history leaves the database only for the `patient's own page`."
    cites: ["context/data-model.md#patients"]
  - id: C10
    text: "The patient SQL lives in `PatientRepo`."
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/repositories/patientRepo.ts:PatientRepo"]
  - id: C11
    text: "Patient rules live in `PatientService`."
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/services/patientService.ts:PatientService"]
  - id: C12
    text: "The patient list routes are in `patientsRouter`."
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/routes/patients.ts:patientsRouter"]
  - id: C13
    text: "The patient list page is `PatientsPage`."
    cites: ["context/generated/module-map.json#sym:sample-system:web/src/pages/PatientsPage.tsx:PatientsPage"]
  - id: C14
    text: "The existing patient list tests include a `nextCell` helper for the Next appointment column."
    cites: ["context/generated/module-map.json#sym:sample-system:web/src/__tests__/PatientsPage.test.tsx:nextCell"]
  - id: C15
    text: "No `background jobs` exist yet."
    cites: ["context/architecture.md#open-points"]
  - id: C16
    text: "The `appointment` table references `table:patient`."
    cites: ["context/generated/db-schema.json#table:appointment"]
  - id: C17
    text: "The booking page `BookAppointmentPage` is a second web caller of the patient list API (its patient picker)."
    cites: ["context/generated/module-map.json#sym:sample-system:web/src/pages/BookAppointmentPage.tsx:BookAppointmentPage"]
  - id: C18
    text: "The booking UI route is `/appointments/new`, declared in `web/src/App.tsx`."
    cites: ["context/generated/api-inventory.json#ui:/appointments/new"]
trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10, REQ-11, REQ-12, REQ-13]
known: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10, REQ-11, REQ-12, REQ-13]
assumed: []
unknown: []
questions: []
resolves:
  - id: F-1
    how: "Named BookAppointmentPage (ui:/appointments/new) as a second caller of http:GET:/api/patients. It is marked existing: it must keep working when list rows gain the last-visit field, and the per-row lookup also runs on its picker searches. Removed the 'only caller' statement."
modules:
  - id: module:sample-system:api
    change: changed
    cites: ["context/generated/module-map.json#module:sample-system:api", "context/architecture.md#components"]
  - id: module:sample-system:web
    change: changed
    cites: ["context/generated/module-map.json#module:sample-system:web", "context/generated/api-inventory.json#ui:/patients", "context/generated/module-map.json#sym:sample-system:web/src/pages/BookAppointmentPage.tsx:BookAppointmentPage"]
  - id: module:sample-system:db
    change: existing
    cites: ["context/generated/module-map.json#module:sample-system:db", "context/generated/db-schema.json#index:appointment_patient_start_idx"]
apis:
  - id: http:GET:/api/patients
    change: changed
    cites: ["context/generated/api-inventory.json#http:GET:/api/patients"]
  - id: http:GET:/api/patients/:id
    change: existing
    cites: ["context/generated/api-inventory.json#http:GET:/api/patients/:id", "context/data-model.md#patients"]
tables:
  - id: table:appointment
    change: existing
    cites: ["context/generated/db-schema.json#table:appointment", "context/data-model.md#appointments"]
  - id: table:patient
    change: existing
    cites: ["context/generated/db-schema.json#table:patient", "context/data-model.md#patients"]
---
# Impact analysis: HMS-4

## Summary
HMS-4 adds a read-only "Last visit" value to the patient list. The list API `GET /api/patients` changes shape: each row gains the date of the patient's latest COMPLETED appointment. That changes the api module (route `patientsRouter`, `PatientService`, `PatientRepo` SQL) and the web module (`PatientsPage` and its tests). The same API also feeds the patient picker on `BookAppointmentPage`, which must keep working and will also pay for the extra lookup. No table changes. `appointment` and `patient` are only read, and the existing `appointment_patient_start_idx` index on (patient_id, starts_at) supports the per-patient lookup. Status is not an index column, so the query still has to filter COMPLETED rows. Main risks:
- converting UTC `starts_at` to the Asia/Kolkata date;
- regressing the paging, search or sort, the Next appointment column (HMS-1) or the booking picker;
- leaking medical history into the list payload.

## Impacted modules
- module:sample-system:api — **changed**. The list route, service and repository add the last-visit value (REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-11). They must keep paging, search and sort (REQ-6, REQ-7, REQ-8) and keep medical history out of the list (REQ-10).
- module:sample-system:web — **changed**.
  - `PatientsPage` at UI route `/patients` adds the Last visit column with "-" when the value is empty (REQ-1, REQ-5, REQ-13). The Next appointment column stays unchanged (REQ-9), and the page adds no sort option (REQ-12). The `PatientsPage.test.tsx` tests (`nextCell`) must keep passing.
  - `BookAppointmentPage` (UI route `/appointments/new`) is **existing**. It is a second caller of `GET /api/patients` for its patient picker. It is not meant to change, but it receives the wider row and runs the per-row last-visit lookup on each picker search, so it needs a regression check.
- module:sample-system:db — **existing**. No migration. The `appointment_patient_start_idx` index already supports the per-patient lookup by start time.
- http:GET:/api/patients — **changed**. Each list row gains a last-visit field. Known web callers go through `web/src/api/client.ts`: `PatientsPage` (changed) and `BookAppointmentPage` (existing).
- http:GET:/api/patients/:id — **existing**. Unchanged; it stays the only place medical history leaves the database (REQ-10).
- table:appointment — **existing**. Read only, filtered to status COMPLETED, latest `starts_at` per patient (REQ-2, REQ-4, REQ-11).
- table:patient — **existing**. Read only, as today.
- Scheduled jobs: none. No background jobs exist, and none are needed.

## Known
- REQ-1, REQ-5, REQ-9, REQ-12, REQ-13: web change on `PatientsPage` (`ui:/patients`).
- REQ-2, REQ-4, REQ-11: api reads `table:appointment` by status and `starts_at`. The statuses come from data-model#appointments.
- REQ-3: instants are stored in UTC and shown in the `HOSPITAL_TZ` date (architecture#time-and-slots).
- REQ-6, REQ-7, REQ-8: the existing paging, search and sort of `GET /api/patients` must be kept. This includes the `BookAppointmentPage` picker, which uses the same search.
- REQ-10: medical history leaves the database only for the patient's own page (data-model#patients). The list payload must not gain it.

## Assumed
None.

## Unknown
None.

## Open questions
None.

## Resolves
- F-1: `BookAppointmentPage` is now named as a second caller of `GET /api/patients`. It is marked existing, with a regression and performance note.
