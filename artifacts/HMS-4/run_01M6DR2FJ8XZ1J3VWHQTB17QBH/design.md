---
schema: design.v1
run_id: run_01M6DR2FJ8XZ1J3VWHQTB17QBH
step: design
attempt: 3
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
  - id: C16
    text: "API list fields use snake_case names such as `page_size`."
    cites: ["context/conventions.md#lists"]
  - id: C17
    text: "A shared API types file `api/src/types.ts` exists; the module map lists its constant `BLOOD_GROUPS`."
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/types.ts:BLOOD_GROUPS"]
trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10, REQ-11, REQ-12, REQ-13]
known: [S-3, S-4]
assumed: [S-1, S-2]
unknown: []
questions: []
resolves:
  - id: F-1
    how: "Added an Assumed entry for the two list row types, naming the files, the type and the basis; api/src/types.ts is cited from the module map (C17)."
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
  - name: Patient list row type (api, api/src/types.ts)
    module: module:sample-system:api
    change: changed
  - name: Patient list row type (web, web/src/api/types.ts)
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
The existing `GET /api/patients` list gains one read-only field per row: `last_visit`. Its type is `string | null`, and the value is the patient's last visit as a hospital-local calendar date in the form `YYYY-MM-DD`, or null. Nothing else in the response changes. The envelope stays `{ items, page, page_size, total }` with 20 per page, and the query parameters, search and sort stay as they are. `PatientsPage` renders `last_visit` in a new "Last visit" column and shows "-" when it is null. There is no new endpoint, no migration and no new infrastructure.

How the value is computed:
- `PatientRepo`'s list query adds a correlated subquery or lateral join over `appointment`, filtered to status `COMPLETED` for that patient, taking the row with the latest `starts_at`.
- The query converts that instant to a calendar date in `HOSPITAL_TZ` inside the SQL and returns it as a `YYYY-MM-DD` string.
- The service passes the zone name from config as a bound query parameter, never as interpolated SQL.
- The subquery runs at most once per row on the page (20 rows). The existing `appointment_patient_start_idx` index on (patient_id, starts_at) supports it; status is not an index column, so COMPLETED is filtered within the patient's rows.

API contract (both row types must match):
- JSON: `"last_visit": "<YYYY-MM-DD>"` or `"last_visit": null`
- TypeScript: `last_visit: string | null` on the patient list row type in `api/src/types.ts` and in `web/src/api/types.ts`.
- Test at API level (S-1 AC3): an appointment completed in the early morning hospital time has `last_visit` equal to the Asia/Kolkata date, which is one day later than its UTC date.

Trade-offs:
- **Plain date string rather than a UTC instant.** The UI shows it as-is with no browser time-zone conversion, and API tests compare exact strings. The cost is that a caller wanting the time must look up the appointment.
- **Date computed in the database rather than the browser.** Every caller gets the same hospital-local date. The cost is passing `HOSPITAL_TZ` as a query parameter.
- **Derived per request instead of a stored column.** No migration and no write-path coupling to status changes. The cost is one indexed subquery per row on the page.
- **Added to the existing list endpoint, not a new one.** One round trip and no clash with the API inventory. The cost is that `BookAppointmentPage`'s picker also receives `last_visit` and runs the subquery on every picker search. This needs a regression and latency check; it is not free.
- **Only the date is selected, never the appointment row or patient medical columns.** This keeps REQ-10 and the logging rule intact.

## Components
- patientsRouter (GET /api/patients handler) — **existing**. Same query parameters, permission and paging.
- PatientService (list) — **changed**. Passes `HOSPITAL_TZ` to the repository and returns `last_visit` unchanged.
- PatientRepo (list query) — **changed**. Adds the latest-COMPLETED subquery (REQ-2, REQ-4, REQ-11), returning a `YYYY-MM-DD` string in the hospital zone or null (REQ-3, REQ-5). It keeps the existing WHERE, ORDER BY and LIMIT/OFFSET (REQ-6, REQ-7, REQ-8) and the existing column list without medical history (REQ-10).
- Patient list row type (api, api/src/types.ts) — **changed**. Adds `last_visit: string | null`. The file location is cited (C17); the exact type name is assumed (see Assumed).
- Patient list row type (web, web/src/api/types.ts) — **changed**. Adds the same `last_visit: string | null`, in step with the api type. The file location and type name are assumed (see Assumed).
- api client (web/src/api/client.ts) — **existing**.
- PatientsPage — **changed**. Adds the "Last visit" column and renders `last_visit` as given, or "-" for null (REQ-1, REQ-5). There is no highlight or filter (REQ-13) and no new sort control (REQ-12). The Next appointment cell code is untouched (REQ-9).
- PatientsPage tests — **changed**. Add tests for the new column. The existing `nextCell` assertions are kept.
- BookAppointmentPage (patient picker) — **existing**. It receives `last_visit` and ignores it; needs a regression and latency check.
- appointment table and appointment_patient_start_idx — **existing**. Read only; no migration.

Endpoints:
- GET /api/patients — **changed**. Each item gains `last_visit`.
- GET /api/patients/:id — **existing**. Unchanged; it stays the only place medical history leaves the database.

Story to component mapping:
- **S-1:** PatientRepo, PatientService, both list row types, PatientsPage.
- **S-2:** PatientsPage ("-" when null, no highlight or filter) and PatientRepo (null when there is no COMPLETED appointment).
- **S-3:** patientsRouter and PatientRepo (paging, search and sort unchanged), PatientsPage (Next appointment untouched, no sort option), BookAppointmentPage (regression).
- **S-4:** PatientRepo (no medical columns selected) and GET /api/patients/:id (unchanged).

## Known
- S-3: served by leaving the router, paging, search, sort and the Next appointment cell unchanged.
- S-4: served by selecting only the date and none of the medical columns.

## Assumed
- S-1: relies on the field name `last_visit` and the `YYYY-MM-DD` format. No input fixes either.
  - The name follows the snake_case style of the list envelope (`page_size`, conventions#lists).
  - The format is a calendar date because REQ-3 asks for "the calendar date in the hospital time zone" (req.md REQ-3). ISO `YYYY-MM-DD` is the plain unambiguous form.
- S-1: also relies on where the list row shape is declared, which the module map does not list.
  - API side: `api/src/types.ts` exists in the module map as the shared API types file (C17). The patient list row type is assumed to be declared there.
  - Web side: a matching type is assumed in `web/src/api/types.ts`, beside the only API client `web/src/api/client.ts` (C2).
  - The type name (for example a `PatientListItem` interface) is not in the facts. Implement must confirm both declarations in source and update the ones that actually type the list rows. If the web types live elsewhere, update that file instead.
- S-2: relies on the same field being null when there is no COMPLETED appointment, which the page renders as "-" (REQ-5).

## Unknown
None.

## Open questions
None.

## Resolves
- F-1: The two list row type files are now an explicit assumption under Assumed, naming the files, the type and the basis. `api/src/types.ts` is cited from the module map (C17).
