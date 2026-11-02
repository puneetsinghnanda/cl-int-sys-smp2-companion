---
schema: impact.v1
run_id: run_01M6DN23JRRYT113FF078VYBSF
step: impact
attempt: 1
skill: impact-analysis@1.1.1
inputs:
  - path: artifacts/HMS-4/run_01M6DN23JRRYT113FF078VYBSF/req.md
    sha256: df068dbe9b4a2785e85a805729d855f730bfda32f9cae55f731603a04e2a8bca
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
    text: "The patient list API route is `/api/patients`, defined in `api/src/routes/patients.ts`."
    cites: ["context/generated/api-inventory.json#http:GET:/api/patients"]
  - id: C2
    text: "The API is layered into routes, services and repositories; each resource has its own router mounted under `/api`."
    cites: ["context/architecture.md#components"]
  - id: C3
    text: "The web app talks to the API only through `web/src/api/client.ts`."
    cites: ["context/architecture.md#components"]
  - id: C4
    text: "Instants are stored in UTC, and \"today\" uses the hospital time zone `HOSPITAL_TZ` (default Asia/Kolkata)."
    cites: ["context/architecture.md#time-and-slots"]
  - id: C5
    text: "Appointment status is one of SCHEDULED, `COMPLETED`, CANCELLED or NO_SHOW."
    cites: ["context/data-model.md#appointments"]
  - id: C6
    text: "The appointment table has a `starts_at` column."
    cites: ["context/generated/db-schema.json#column:appointment.starts_at"]
  - id: C7
    text: "The appointment table has a `status` column."
    cites: ["context/generated/db-schema.json#column:appointment.status"]
  - id: C8
    text: "Index `appointment_patient_start_idx` covers appointment (patient_id, starts_at)."
    cites: ["context/generated/db-schema.json#index:appointment_patient_start_idx"]
  - id: C9
    text: "The patient row holds the `medical history`, which leaves the database only for the patient's own page."
    cites: ["context/data-model.md#patients"]
  - id: C10
    text: "`No background jobs` exist yet."
    cites: ["context/architecture.md#open-points"]
  - id: C11
    text: "The repo has workspace modules `api`, `web` and `db`."
    cites: ["context/generated/module-map.json#module:sample-system:api", "context/generated/module-map.json#module:sample-system:web", "context/generated/module-map.json#module:sample-system:db"]
trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10, REQ-11]
known: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10, REQ-11]
assumed: []
unknown:
  - {id: U-1, blocking: false}
questions:
  - id: Q-01
    blocking: false
    ask: tech lead
    text: "Does the web patient list page load its rows from GET /api/patients (through web/src/api/client.ts), or from another route?"
    why: "The facts list the route and the client module, but no generated fact links that page to the route. Design needs to know which response to extend."
    evidence: ["context/generated/api-inventory.json#http:GET:/api/patients", "context/architecture.md#components"]
    options: ["GET /api/patients (proposed default)", "Another route; name it"]
    default: "GET /api/patients"
modules:
  - id: module:sample-system:api
    change: changed
    cites: ["context/generated/module-map.json#module:sample-system:api", "context/architecture.md#components"]
  - id: module:sample-system:web
    change: changed
    cites: ["context/generated/module-map.json#module:sample-system:web", "context/architecture.md#components"]
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
    cites: ["context/generated/db-schema.json#table:appointment", "context/data-model.md#appointments"]
  - id: table:patient
    change: existing
    cites: ["context/generated/db-schema.json#table:patient", "context/data-model.md#patients"]
---
# Impact analysis: HMS-4

## Summary
Adding a "Last visit" column changes the patient list end to end. The `GET /api/patients` response must carry each patient's latest Completed appointment start (from `appointment.status` and `appointment.starts_at`). The api module's patient route, service and repository change, and the web module's patient list page and API client change to render the new column. The database is only read. The existing index `appointment_patient_start_idx` on (patient_id, starts_at) supports the lookup, so no migration is expected. The main risks:
- breaking the existing 20-per-page paging, search, sort or Next appointment value when the list query is extended;
- getting the date wrong because instants are stored in UTC but must be shown in `HOSPITAL_TZ`;
- leaking medical history if the list query starts selecting more patient columns.

## Impacted modules
- module:sample-system:api — **changed**. The patient list route, service and repository add the latest Completed appointment start per patient to `GET /api/patients`.
- module:sample-system:web — **changed**. The patient list page adds the "Last visit" column, shown as a date in the hospital time zone and in the same format as Next appointment. The client types in `web/src/api/client.ts` gain the new field.
- module:sample-system:db — **existing**. No schema change. `appointment_patient_start_idx` already indexes (patient_id, starts_at).
- http:GET:/api/patients — **changed**. Each row in the response gains the last Completed visit (or nothing when there is none). Paging, search and sort parameters stay unchanged (REQ-7 to REQ-9).
- table:appointment — **existing**. Read for status `COMPLETED` and `starts_at`; not written.
- table:patient — **existing**. Read as today. Medical history columns must still not be selected for the list (REQ-11).
- Jobs: none. No background jobs exist.

## Known
- REQ-1, REQ-4: the new column and its date format are rendered by the web module.
- REQ-2, REQ-5, REQ-6: the API selects the latest `COMPLETED` appointment by `starts_at` per patient, ignores the other statuses, and returns nothing when there is none (shown as "-").
- REQ-3: UTC instants are converted to the `HOSPITAL_TZ` date (architecture#time-and-slots).
- REQ-7, REQ-8, REQ-9: the paging, sort and search of `GET /api/patients` are kept. Last visit is not a sort key.
- REQ-10: the existing Next appointment value in the same response is left as is.
- REQ-11: the patient columns selected for the list exclude medical history (data-model#patients).

## Assumed
None.

## Unknown
- U-1 (non-blocking): no generated fact links the web patient list page to `GET /api/patients`. See Q-01.

## Open questions
- Q-01 (non-blocking, tech lead): does the web patient list page load its rows from GET /api/patients through web/src/api/client.ts? Proposed default: yes, GET /api/patients.
