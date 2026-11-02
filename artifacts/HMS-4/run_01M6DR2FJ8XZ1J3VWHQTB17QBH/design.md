---
schema: design.v1
run_id: run_01M6DR2FJ8XZ1J3VWHQTB17QBH
step: design
attempt: 1
skill: design@1.4.1
inputs:
  - path: artifacts/HMS-4/run_01M6DR2FJ8XZ1J3VWHQTB17QBH/impact.md
    sha256: 65330c107ab16a78c3328e9bd1e9cc16383e794931f97aa3a237d3f2db6cc14b
  - path: artifacts/HMS-4/run_01M6DR2FJ8XZ1J3VWHQTB17QBH/split.md
    sha256: 7dc8c8c164a449e8a67b8936cf815643b0837f3c8952f370b44182d6dcab3450
  - path: artifacts/HMS-4/run_01M6DR2FJ8XZ1J3VWHQTB17QBH/req.md
    sha256: a4368b1b80bb6173569256c5cf6e26985261df9a7da1faff3a4f3be7a67ec369
  - path: context/intent-baseline.md
    sha256: 9bfd821aeb2fb9524863e346707e049e4fcfb949a6e55b90223a79194946a215
  - path: context/architecture.md
    sha256: 9faab093d10933a46f6473b25c80728f8745e597100e650cd2537bbafb2b595a
  - path: context/generated/module-map.json
    sha256: 04f84ff09b4ad6ffaba59ec846716d25f03c8aae22292958a47ebb9247632345
  - path: context/generated/api-inventory.json
    sha256: 9ba29278d8d66b7341defc3c67d6e7d68508ce25ee2991ba256be2b37194ec61
pack_sha:
  sample-system: bd98ff08b83f8762c5e5c073ba5fadef88169b33
claims:
  - id: C1
    text: "The API is layered: routes, services, and `repositories` that hold the SQL."
    cites: ["context/architecture.md#components"]
  - id: C2
    text: "The web app talks to the API only through `web/src/api/client.ts`."
    cites: ["context/architecture.md#components"]
  - id: C3
    text: "Instants are stored in `UTC`; hospital-local dates use `HOSPITAL_TZ`, default Asia/Kolkata."
    cites: ["context/architecture.md#time-and-slots"]
  - id: C4
    text: "Lists answer `{ items, page, page_size, total }`, 20 per page."
    cites: ["context/conventions.md#lists"]
  - id: C5
    text: "Logs never carry `medical history`."
    cites: ["context/conventions.md#logging-and-data"]
  - id: C6
    text: "Schema changes are new SQL files only; HMS-4 needs `No migration`."
    cites: ["context/conventions.md#database-changes", "artifacts/HMS-4/run_01M6DR2FJ8XZ1J3VWHQTB17QBH/impact.md#impacted-modules"]
  - id: C7
    text: "The list endpoint `/api/patients` (GET) exists and is changed, not new."
    cites: ["context/generated/api-inventory.json#http:GET:/api/patients"]
  - id: C8
    text: "The patient SQL lives in `PatientRepo`."
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/repositories/patientRepo.ts:PatientRepo"]
  - id: C9
    text: "Patient rules live in `PatientService`."
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/services/patientService.ts:PatientService"]
  - id: C10
    text: "The patient routes are in `patientsRouter`."
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/routes/patients.ts:patientsRouter"]
  - id: C11
    text: "The patient list page is `PatientsPage`."
    cites: ["context/generated/module-map.json#sym:sample-system:web/src/pages/PatientsPage.tsx:PatientsPage"]
  - id: C12
    text: "`BookAppointmentPage` is a second caller of the patient list API."
    cites: ["artifacts/HMS-4/run_01M6DR2FJ8XZ1J3VWHQTB17QBH/impact.md#impacted-modules"]
  - id: C13
    text: "The index `appointment_patient_start_idx` supports the per-patient lookup by start time."
    cites: ["artifacts/HMS-4/run_01M6DR2FJ8XZ1J3VWHQTB17QBH/impact.md#impacted-modules"]
  - id: C14
    text: "The existing list tests use a `nextCell` helper."
    cites: ["context/generated/module-map.json#sym:sample-system:web/src/__tests__/PatientsPage.test.tsx:nextCell"]
  - id: C15
    text: "Medical history appears only on the patient's own page, never in `lists`."
    cites: ["context/intent-baseline.md#data-protection"]
trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10, REQ-11, REQ-12, REQ-13]
known: [S-1, S-2, S-3, S-4]
assumed: []
unknown: []
questions: []
components:
  - name: patientsRouter (GET /api/patients handler)
    module: module:sample-system:api
    change: existing
  - name: PatientService (list)
    module: module:sample-system:api
    change: changed
  - name: PatientRepo (list query)
    module: module:sample-system:api
    change: changed
  - name: PatientListItem type (api/src/types.ts)
    module: module:sample-system:api
    change: changed
  - name: PatientListItem type (web/src/api/types.ts)
    module: module:sample-system:web
    change: changed
  - name: api client (web/src/api/client.ts)
    module: module:sample-system:web
    change: existing
  - name: PatientsPage
    module: module:sample-system:web
    change: changed
  - name: PatientsPage tests (web/src/__tests__/PatientsPage.test.tsx)
    module: module:sample-system:web
    change: changed
  - name: BookAppointmentPage (patient picker)
    module: module:sample-system:web
    change: existing
  - name: appointment table and appointment_patient_start_idx
    module: module:sample-system:db
    change: existing
endpoints:
  - method: GET
    path: /api/patients
    change: changed
  - method: GET
    path: /api/patients/:id
    change: existing
---
# Design: HMS-4

## Overview
The existing `GET /api/patients` list gains one read-only field per row: the patient's last visit, as a hospital-local calendar date or null. Nothing else in the response changes. The envelope stays `{ items, page, page_size, total }` with 20 per page, and the query parameters, search and sort stay as they are. `PatientsPage` renders that field in a new "Last visit" column and shows "-" when it is null. There is no new endpoint, no migration and no new infrastructure.

How the value is computed:
- `PatientRepo`'s list query adds a correlated subquery or lateral join over `appointment`, filtered to status `COMPLETED` for that patient, taking the row with the latest `starts_at`.
- The query converts that instant to a calendar date in `HOSPITAL_TZ` inside the SQL.
- The subquery is applied after the existing paging, so it runs at most once per row on the page (20 rows). The existing `appointment_patient_start_idx` index on (patient_id, starts_at) supports it.

Trade-offs:
- **Date computed in the database rather than the browser.** Every caller gets the same hospital-local date, and S-1 AC3 is testable at the API level. The cost is passing `HOSPITAL_TZ` as a query parameter.
- **Derived per request instead of a stored `last_visit` column.** No migration and no write-path coupling to appointment status changes. The cost is one indexed subquery per row on the page.
- **Added to the existing list endpoint, not a new one.** One round trip and no clash with the API inventory. The cost is that `BookAppointmentPage`'s picker also receives the field and pays for the subquery. The page ignores the field, so this is harmless.
- **Only the date is selected, never the appointment row or patient medical columns.** This keeps REQ-10 and the logging rule intact.

## Components
- patientsRouter (GET /api/patients handler) — **existing**. Same query parameters, permission and paging.
- PatientService (list) — **changed**. Passes `HOSPITAL_TZ` to the repository and returns the new field unchanged.
- PatientRepo (list query) — **changed**. Adds the latest-COMPLETED subquery (REQ-2, REQ-4, REQ-11) with the hospital-zone date conversion (REQ-3). It keeps the existing WHERE, ORDER BY and LIMIT/OFFSET (REQ-6, REQ-7, REQ-8) and the existing column list without medical history (REQ-10).
- PatientListItem type (api/src/types.ts) — **changed**. Adds a nullable last-visit date field.
- PatientListItem type (web/src/api/types.ts) — **changed**. The same field; it must stay in step with the api type.
- api client (web/src/api/client.ts) — **existing**.
- PatientsPage — **changed**. Adds the "Last visit" column after the existing columns and renders "-" for null (REQ-1, REQ-5). There is no highlight or filter (REQ-13) and no new sort control (REQ-12). The Next appointment cell code is untouched (REQ-9).
- PatientsPage tests — **changed**. Add tests for the new column. The existing `nextCell` assertions are kept.
- BookAppointmentPage (patient picker) — **existing**. It receives the extra field and ignores it; needs a regression check.
- appointment table and appointment_patient_start_idx — **existing**. Read only; no migration.
- GET /api/patients — **changed**. The response row gains the last-visit field.
- GET /api/patients/:id — **existing**. Unchanged; it stays the only place medical history leaves the database.

Story to component mapping:
- **S-1:** PatientRepo, PatientService, PatientListItem types, PatientsPage.
- **S-2:** PatientsPage ("-" when null, no highlight or filter) and PatientRepo (null when there is no COMPLETED appointment).
- **S-3:** patientsRouter and PatientRepo (paging, search and sort unchanged), PatientsPage (Next appointment untouched, no sort option), BookAppointmentPage (regression).
- **S-4:** PatientRepo (no medical columns selected) and GET /api/patients/:id (unchanged).

## Known
- S-1: served by the repository subquery, the hospital-zone conversion and the new column.
- S-2: served by the null result and the "-" rendering; only the date is shown.
- S-3: served by leaving the router, paging, search, sort and the Next appointment cell unchanged.
- S-4: served by selecting only the date and none of the medical columns.

## Assumed
None.

## Unknown
None.

## Open questions
None.
