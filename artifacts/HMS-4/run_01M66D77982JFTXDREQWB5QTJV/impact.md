---
schema: impact.v1
run_id: run_01M66D77982JFTXDREQWB5QTJV
step: impact
attempt: 1
skill: impact-analysis@1.1.1
inputs:
  - path: artifacts/HMS-4/run_01M66D77982JFTXDREQWB5QTJV/req.md
    sha256: a33bb8ac0e7a965160b9d6ba639feb8f4086518ad90e0374e053ffbfacdfaf35
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
    text: "The patient list API route is defined in `api/src/routes/patients.ts`."
    cites: ["context/generated/api-inventory.json#http:GET:/api/patients"]
  - id: C2
    text: "The `/patients` UI route is declared in `web/src/App.tsx`."
    cites: ["context/generated/api-inventory.json#ui:/patients"]
  - id: C3
    text: "The patient list page is the `PatientsPage` component."
    cites: ["context/generated/module-map.json#sym:sample-system:web/src/pages/PatientsPage.tsx:PatientsPage"]
  - id: C4
    text: "Patient SQL lives in the `PatientRepo` repository."
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/repositories/patientRepo.ts:PatientRepo"]
  - id: C5
    text: "Patient rules live in `PatientService`."
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/services/patientService.ts:PatientService"]
  - id: C6
    text: "Patient routes are mounted by `patientsRouter`."
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/routes/patients.ts:patientsRouter"]
  - id: C7
    text: "The API is layered into routes, services and `repositories`, each resource with its own router under `/api`."
    cites: ["context/architecture.md#components"]
  - id: C8
    text: "The web app talks to the API only through `web/src/api/client.ts`."
    cites: ["context/architecture.md#components"]
  - id: C9
    text: "Appointment status values include `COMPLETED`, alongside SCHEDULED, CANCELLED and NO_SHOW."
    cites: ["context/data-model.md#appointments"]
  - id: C10
    text: "Appointment `status` is a text column on table:appointment."
    cites: ["context/generated/db-schema.json#column:appointment.status"]
  - id: C11
    text: "Appointment `starts_at` is a timestamptz column."
    cites: ["context/generated/db-schema.json#column:appointment.starts_at"]
  - id: C12
    text: "Index `appointment_patient_start_idx` covers `patient_id` and starts_at on table:appointment."
    cites: ["context/generated/db-schema.json#index:appointment_patient_start_idx"]
  - id: C13
    text: "Instants are stored in `UTC`; the hospital time zone comes from `HOSPITAL_TZ`, default Asia/Kolkata."
    cites: ["context/architecture.md#time-and-slots"]
  - id: C14
    text: "A patient row holds the basic medical history, which leaves the database only for the `patient's own page`."
    cites: ["context/data-model.md#patients"]
  - id: C15
    text: "`No background jobs` exist yet."
    cites: ["context/architecture.md#open-points"]
trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10, REQ-11, REQ-12, REQ-13, REQ-14, REQ-15]
known: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10, REQ-11, REQ-12]
assumed: [REQ-13, REQ-14, REQ-15]
unknown:
  - {id: U-1, blocking: false}
  - {id: U-2, blocking: false}
questions:
  - id: Q-01
    blocking: false
    ask: tech lead
    text: Should the conversion of the Last visit instant to the hospital date happen in the API or in the web page?
    why: Instants are stored in UTC and the hospital zone is configured in the API; converting in one place avoids dates that differ from the Next appointment column.
    evidence: ["context/architecture.md#time-and-slots"]
    options: [API returns the instant and the web formats it like Next appointment, API returns a hospital-zone date]
    default: API returns the instant and the web formats it like Next appointment
  - id: Q-02
    blocking: false
    ask: tech lead
    text: Does any web page other than the patient list call GET /api/patients (for example a patient picker when booking)?
    why: The list response gains a field; other callers would receive it too. The facts list the route but not its callers.
    evidence: ["context/generated/api-inventory.json#http:GET:/api/patients", "context/architecture.md#components"]
    options: [only the patient list, other callers exist and must ignore the new field]
    default: other callers ignore the new field; it is additive
modules:
  - id: module:sample-system:api
    change: changed
    cites: ["context/generated/module-map.json#module:sample-system:api", "context/generated/module-map.json#sym:sample-system:api/src/repositories/patientRepo.ts:PatientRepo"]
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
tables:
  - id: table:appointment
    change: existing
    cites: ["context/generated/db-schema.json#table:appointment", "context/generated/db-schema.json#column:appointment.status", "context/generated/db-schema.json#column:appointment.starts_at"]
  - id: table:patient
    change: existing
    cites: ["context/generated/db-schema.json#table:patient", "context/data-model.md#patients"]
---
# Impact analysis: HMS-4

## Summary
HMS-4 changes the patient list read path in two places. The `GET /api/patients` response gains a last visit value for each patient. The `/patients` page (`PatientsPage`) shows it in a new column. The value is read from the existing `appointment` table: the latest `starts_at` among the patient's `COMPLETED` appointments. The existing `appointment_patient_start_idx` (patient_id, starts_at) supports that lookup, so no schema change or migration is expected. The main risks are:
- converting UTC instants to the hospital date in the wrong place (Q-01);
- callers of `GET /api/patients` other than the list (Q-02);
- breaking the existing page size, search, sort and Next appointment behaviour, which share the same query;
- leaking medical history columns from `patient` into the list response.

## Impacted modules
- module:sample-system:api, **changed**: `patientsRouter`, `PatientService` and `PatientRepo` must return the last visit for each listed patient.
- module:sample-system:web, **changed**: `PatientsPage` adds the Last visit column. Data comes through `web/src/api/client.ts`.
- module:sample-system:db, **existing**: no migration. The `appointment_patient_start_idx` index already covers patient_id and starts_at.
- http:GET:/api/patients, **changed**: the response gains a last visit field. Paging, search, sort and the next appointment value must stay as they are (REQ-8 to REQ-11).
- table:appointment, **existing**: read-only, filtered on `status` = COMPLETED and ordered by `starts_at`.
- table:patient, **existing**: read as today. Medical history columns must stay out of the list query (REQ-12).
- Jobs: none. No background jobs exist.

## Known
- REQ-1, REQ-2: the new column and its value touch `PatientsPage`, `GET /api/patients` and the patient repository.
- REQ-3: instants are stored in UTC and the hospital zone comes from `HOSPITAL_TZ`, so a conversion is needed (see Q-01).
- REQ-4 to REQ-7: these are filters on `appointment.status`. Only COMPLETED counts, and no match shows "-" in the web page.
- REQ-8 to REQ-11: existing behaviour of `GET /api/patients` and `PatientsPage`, which must not regress.
- REQ-12: medical history lives on table:patient and must not be added to the list response.

## Assumed
- REQ-13, REQ-15: the ranking and the displayed date use `appointment.starts_at`, which `appointment_patient_start_idx` supports.
- REQ-14: the web formats the date the same way it formats the next appointment in `PatientsPage`.

## Unknown
- U-1 (non-blocking): where the UTC-to-hospital-date conversion happens (Q-01).
- U-2 (non-blocking): whether other web pages call `GET /api/patients` and so receive the new field (Q-02).

## Open questions
- Q-01 (non-blocking, tech lead): Convert in the API or in the web page? Default: the API returns the instant and the web formats it like Next appointment.
- Q-02 (non-blocking, tech lead): Are there other callers of `GET /api/patients`? Default: the new field is additive, so other callers ignore it.
