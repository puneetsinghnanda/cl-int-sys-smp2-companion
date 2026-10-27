---
schema: impact.v1
run_id: run_01M5YQB3ZRP10586473KARZ7M4
step: impact
attempt: 1
skill: impact-analysis@1.1.1
inputs:
  - path: artifacts/HMS-4/run_01M5YQB3ZRP10586473KARZ7M4/req.md
    sha256: 3e7adfbb0c88fcca0bb2ac95ec1a1b58c07a9930dc1a1c6e56af91e5475f7321
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
    text: "The patient list route is defined in `api/src/routes/patients.ts`."
    cites: ["context/generated/api-inventory.json#http:GET:/api/patients"]
  - id: C2
    text: "The web app talks to the API only through `web/src/api/client.ts`."
    cites: ["context/architecture.md#components"]
  - id: C3
    text: "Routes parse input, services hold the rules and `repositories` hold the SQL."
    cites: ["context/architecture.md#components"]
  - id: C4
    text: "Instants are stored in UTC and \"today\" uses the hospital time zone `HOSPITAL_TZ`."
    cites: ["context/architecture.md#time-and-slots"]
  - id: C5
    text: "Appointment status is SCHEDULED, `COMPLETED`, CANCELLED or NO_SHOW."
    cites: ["context/data-model.md#appointments"]
  - id: C6
    text: "The patient row holds the basic medical history and it leaves the database only for the `patient's own page`."
    cites: ["context/data-model.md#patients"]
  - id: C7
    text: "The appointment table has a `starts_at` column."
    cites: ["context/generated/db-schema.json#column:appointment.starts_at"]
  - id: C8
    text: "The appointment table has a `status` column."
    cites: ["context/generated/db-schema.json#column:appointment.status"]
  - id: C9
    text: "An index `appointment_patient_start_idx` exists on appointments."
    cites: ["context/generated/db-schema.json#index:appointment_patient_start_idx"]
  - id: C10
    text: "Patient SQL lives in `PatientRepo`."
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/repositories/patientRepo.ts:PatientRepo"]
  - id: C11
    text: "Patient rules live in `PatientService`."
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/services/patientService.ts:PatientService"]
  - id: C12
    text: "The patient list page is `PatientsPage`."
    cites: ["context/generated/module-map.json#sym:sample-system:web/src/pages/PatientsPage.tsx:PatientsPage"]
  - id: C13
    text: "An existing web test helper `nextCell` checks the Next appointment cell of the patient list."
    cites: ["context/generated/module-map.json#sym:sample-system:web/src/__tests__/PatientsPage.test.tsx:nextCell"]
  - id: C14
    text: "Appointments become Completed through `/api/appointments/:id/complete`."
    cites: ["context/generated/api-inventory.json#http:POST:/api/appointments/:id/complete"]
  - id: C15
    text: "`No background jobs` exist yet."
    cites: ["context/architecture.md#open-points"]
  - id: C16
    text: "The web app has a date formatter `dateTime`."
    cites: ["context/generated/module-map.json#sym:sample-system:web/src/lib/format.ts:dateTime"]
trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10, REQ-11, REQ-12, REQ-13]
known: [REQ-1, REQ-2, REQ-3, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10, REQ-11, REQ-12, REQ-13]
assumed: [REQ-4]
unknown: [U-1, U-2]
unknowns:
  - {id: U-1, blocking: false, text: "Other web callers of GET /api/patients (for example pickers) are not listed in the facts; they would also receive the new field.", question: Q-01}
  - {id: U-2, blocking: false, text: "Whether the date should be computed in the API (in HOSPITAL_TZ) or formatted in the web app from a UTC instant is not fixed by the facts.", question: Q-02}
questions:
  - id: Q-01
    blocking: false
    ask: tech lead
    text: "Which other screens call GET /api/patients, and is an extra read-only field in its list response acceptable for them?"
    why: "A changed list response reaches every caller; the facts list the route and the client but not each call site."
    evidence: ["context/generated/api-inventory.json#http:GET:/api/patients", "context/architecture.md#components"]
    options: ["Additive field on the existing list response", "Separate endpoint for last visits"]
    default: "Additive field on the existing list response; existing callers ignore it"
  - id: Q-02
    blocking: false
    ask: tech lead
    text: "Should the API return the Last visit as a UTC instant (web converts with the hospital time zone) or as a hospital-time date?"
    why: "REQ-3 needs the Asia/Kolkata date; instants are stored in UTC and the hospital zone is configured in the API."
    evidence: ["context/architecture.md#time-and-slots", "artifacts/HMS-4/run_01M5YQB3ZRP10586473KARZ7M4/req.md#L59"]
    options: ["UTC instant, converted in the web app", "Date computed in the API with HOSPITAL_TZ"]
    default: "Same approach the Next appointment column already uses"
modules:
  - {id: module:sample-system:api, change: changed, cites: ["context/generated/module-map.json#sym:sample-system:api/src/repositories/patientRepo.ts:PatientRepo", "context/architecture.md#components"]}
  - {id: module:sample-system:web, change: changed, cites: ["context/generated/module-map.json#sym:sample-system:web/src/pages/PatientsPage.tsx:PatientsPage", "context/architecture.md#components"]}
  - {id: module:sample-system:db, change: existing, cites: ["context/generated/db-schema.json#index:appointment_patient_start_idx"]}
apis:
  - {id: "http:GET:/api/patients", change: changed, cites: ["context/generated/api-inventory.json#http:GET:/api/patients"]}
  - {id: "http:GET:/api/patients/:id", change: existing, cites: ["context/generated/api-inventory.json#http:GET:/api/patients/:id", "context/data-model.md#patients"]}
  - {id: "http:POST:/api/appointments/:id/complete", change: existing, cites: ["context/generated/api-inventory.json#http:POST:/api/appointments/:id/complete"]}
tables:
  - {id: table:appointment, change: existing, cites: ["context/generated/db-schema.json#table:appointment", "context/data-model.md#appointments"]}
  - {id: table:patient, change: existing, cites: ["context/generated/db-schema.json#table:patient", "context/data-model.md#patients"]}
---
# Impact analysis: HMS-4

## Summary
HMS-4 adds a read-only "Last visit" value to the patient list. The patient list response (GET /api/patients) and the patient list page change. The patient list query in the API reads the latest Completed appointment per patient from the existing appointment table; no table, column or migration changes are needed, and an index on patient and start time already exists. The main risks are changing the list query's paging, search or sort while adding the value (REQ-9 to REQ-11), taking the date in UTC instead of the hospital time zone (REQ-3), and letting medical history columns into the list query (REQ-13). No background jobs exist, so none are affected.

## Impacted modules
- module:sample-system:api — **changed**: the patient list SQL in PatientRepo (and PatientService if it shapes the list) adds the last Completed appointment date per patient. Reason: repositories hold the SQL (architecture#components).
- module:sample-system:web — **changed**: PatientsPage adds the "Last visit" column; the existing PatientsPage test (nextCell) covers the Next appointment column that must stay unchanged.
- module:sample-system:db — **existing**: no migration; the appointment table already has status and starts_at, and the appointment_patient_start_idx index covers patient plus start time.
- http:GET:/api/patients — **changed**: each list item gains a last-visit value; page size, search and sort stay as they are. Reason: the web app reaches data only through the API client.
- http:GET:/api/patients/:id — **existing**: the patient's own page, the only place medical history leaves the database; not touched.
- http:POST:/api/appointments/:id/complete — **existing**: marks appointments Completed, which is what feeds the column; behaviour unchanged.
- table:appointment — **existing**: read for status COMPLETED and starts_at; not changed.
- table:patient — **existing**: read by the list as today; medical history columns must stay out of the list query.

## Known
- REQ-1, REQ-8: new column and "-" placeholder on PatientsPage (module:sample-system:web).
- REQ-2, REQ-5, REQ-6, REQ-7: the value comes from table:appointment rows with status COMPLETED only, read by the patient list SQL (module:sample-system:api, http:GET:/api/patients).
- REQ-3: instants are stored in UTC and the hospital zone is HOSPITAL_TZ, so the date must be converted (architecture#time-and-slots).
- REQ-9, REQ-10, REQ-11: paging, search and sort live in the same list query and route that change (http:GET:/api/patients).
- REQ-12: the Next appointment column on PatientsPage is existing; its test helper nextCell guards it.
- REQ-13: medical history sits on table:patient and leaves the database only for http:GET:/api/patients/:id.

## Assumed
- REQ-4: "most recent" orders by appointment start time; the appointment_patient_start_idx index and the starts_at column support that ordering without a migration (module:sample-system:db existing).

## Unknown
- U-1 (non-blocking): other web callers of GET /api/patients are not in the facts (Q-01).
- U-2 (non-blocking): where the UTC instant becomes a hospital-time date is not fixed (Q-02).

## Open questions
- Q-01 (non-blocking, tech lead): which other screens call GET /api/patients, and is an additive field acceptable? Default: additive field.
- Q-02 (non-blocking, tech lead): return a UTC instant or a hospital-time date? Default: the same approach as the Next appointment column.
