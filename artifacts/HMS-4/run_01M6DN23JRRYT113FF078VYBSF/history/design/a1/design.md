---
schema: design.v1
run_id: run_01M6DN23JRRYT113FF078VYBSF
step: design
attempt: 1
skill: design@1.4.1
inputs:
  - path: artifacts/HMS-4/run_01M6DN23JRRYT113FF078VYBSF/impact.md
    sha256: 4658693bba02dbeb032244235d4d0867041d9cbb3468e01fa74be19d9f35d204
  - path: artifacts/HMS-4/run_01M6DN23JRRYT113FF078VYBSF/split.md
    sha256: 6ec1497770a29d59d4151e2a34a8eba1d74dcf11bba10281ef696c974a0366ab
  - path: artifacts/HMS-4/run_01M6DN23JRRYT113FF078VYBSF/req.md
    sha256: df068dbe9b4a2785e85a805729d855f730bfda32f9cae55f731603a04e2a8bca
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
    text: "The patient list route `/api/patients` is defined in api/src/routes/patients.ts."
    cites: ["context/generated/api-inventory.json#http:GET:/api/patients"]
  - id: C2
    text: "Routes parse input, services hold the rules and `repositories` hold the SQL."
    cites: ["context/architecture.md#components"]
  - id: C3
    text: "The web app talks to the API only through `web/src/api/client.ts`."
    cites: ["context/architecture.md#components"]
  - id: C4
    text: "Instants are stored in UTC; \"today\" uses the hospital time zone `HOSPITAL_TZ`."
    cites: ["context/architecture.md#time-and-slots"]
  - id: C5
    text: "Lists answer `{ items, page, page_size, total }`, 20 per page."
    cites: ["context/conventions.md#lists"]
  - id: C6
    text: "Logs never carry `medical history`."
    cites: ["context/conventions.md#logging-and-data"]
  - id: C7
    text: "`PatientRepo` lives in api/src/repositories/patientRepo.ts."
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/repositories/patientRepo.ts:PatientRepo"]
  - id: C8
    text: "`PatientService` lives in api/src/services/patientService.ts."
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/services/patientService.ts:PatientService"]
  - id: C9
    text: "`patientsRouter` lives in api/src/routes/patients.ts."
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/routes/patients.ts:patientsRouter"]
  - id: C10
    text: "`PatientsPage` lives in web/src/pages/PatientsPage.tsx."
    cites: ["context/generated/module-map.json#sym:sample-system:web/src/pages/PatientsPage.tsx:PatientsPage"]
  - id: C11
    text: "web/src/lib/format.ts has a `dateTime` formatter."
    cites: ["context/generated/module-map.json#sym:sample-system:web/src/lib/format.ts:dateTime"]
  - id: C12
    text: "web/src/lib/format.ts has a `todayIn` helper."
    cites: ["context/generated/module-map.json#sym:sample-system:web/src/lib/format.ts:todayIn"]
  - id: C13
    text: "The PatientsPage test has a `nextCell` helper for the Next appointment cell."
    cites: ["context/generated/module-map.json#sym:sample-system:web/src/__tests__/PatientsPage.test.tsx:nextCell"]
  - id: C14
    text: "Index `appointment_patient_start_idx` covers appointment (patient_id, starts_at)."
    cites: ["artifacts/HMS-4/run_01M6DN23JRRYT113FF078VYBSF/impact.md#impacted-modules"]
  - id: C15
    text: "No new `New SQL files` are needed; database changes would only ever be new migration files."
    cites: ["context/conventions.md#database-changes"]
trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10, REQ-11]
known: [S-1, S-2, S-3, S-4]
assumed: []
unknown:
  - {id: U-1, blocking: false}
questions:
  - id: Q-01
    blocking: false
    ask: tech lead
    text: "Does PatientsPage load its rows from GET /api/patients through web/src/api/client.ts? (Carried from the impact analysis.)"
    why: "The design extends that response. If the page uses another route, that route changes instead."
    evidence: ["artifacts/HMS-4/run_01M6DN23JRRYT113FF078VYBSF/impact.md#open-questions", "context/architecture.md#components"]
    options: ["GET /api/patients (proposed default)", "Another route; name it"]
    default: "GET /api/patients"
components:
  - name: PatientRepo list query (adds last Completed visit per patient)
    module: module:sample-system:api
    change: changed
  - name: PatientService list (passes the new field through unchanged)
    module: module:sample-system:api
    change: existing
  - name: patientsRouter GET /api/patients (same query parameters)
    module: module:sample-system:api
    change: existing
  - name: web/src/api/client.ts patient list item type (gains last_visit_at)
    module: module:sample-system:web
    change: changed
  - name: format.ts date-only formatter in the hospital time zone (new `date` helper beside dateTime)
    module: module:sample-system:web
    change: new
  - name: PatientsPage (adds the Last visit column)
    module: module:sample-system:web
    change: changed
  - name: PatientsPage.test.tsx (cell lookups updated, new Last visit cases)
    module: module:sample-system:web
    change: changed
  - name: appointment table and appointment_patient_start_idx (read only)
    module: module:sample-system:db
    change: existing
endpoints:
  - method: GET
    path: /api/patients
    change: changed
---
# Design: HMS-4

## Overview
The patient list gets a read-only "Last visit" column. The change follows the existing layers: the SQL in `PatientRepo`, the shape of the `GET /api/patients` response, and the rendering in `PatientsPage`. No new endpoint, table, migration, job or infrastructure is added.

**API.** The repository's list query adds one value per patient, `last_visit_at`. It is the latest `appointment.starts_at` where status is `COMPLETED`, computed with a correlated subquery (or LATERAL) on `appointment` that the existing `appointment_patient_start_idx` (patient_id, starts_at) serves. It is `null` when the patient has no Completed appointment. The `WHERE`, `ORDER BY`, `LIMIT/OFFSET` and `total` count of the list query are left untouched. So paging (20 per page, `{ items, page, page_size, total }`), search and sort behave exactly as before. The existing Next appointment value is not touched. The selected patient columns stay the same, so no medical history is added to the list.

**Web.** `last_visit_at` is added to the list item type next to the existing fields. `format.ts` gets a date-only formatter that mirrors `dateTime` (same locale and options, without the time part), formatted in the hospital time zone. `PatientsPage` adds a "Last visit" column that renders that date, or "-" when the value is `null`. The column is not wired into any sort control.

Trade-offs:
- The server returns a UTC instant and the web formats the date. This matches how the Next appointment column is formatted (REQ-4) and keeps one time-zone path. The cost is that the API doesn't return a ready-made "date" string.
- A correlated subquery per row instead of a materialised "last visit" column: no migration and always correct, at a small per-row cost that the existing (patient_id, starts_at) index bounds to 20 rows per page.
- Only `COMPLETED` is filtered in SQL, not in the UI. This guarantees Cancelled, No-show and Scheduled never appear (REQ-5), whatever the client does.

## Components
- PatientRepo list query — **changed** (api): add `last_visit_at` = latest `starts_at` of `COMPLETED` appointments for the patient, or null. No other change to filters, ordering or columns.
- PatientService list — **existing** (api): passes rows through. No rule changes.
- patientsRouter GET /api/patients — **existing** (api): same query parameters and validation.
- GET /api/patients — **changed** endpoint: each item gains `last_visit_at` (UTC instant or null). The envelope `{ items, page, page_size, total }` is unchanged.
- client.ts patient list item type — **changed** (web): adds `last_visit_at: string | null`.
- format.ts date-only formatter — **new** (web): like `dateTime` without the time, in the hospital time zone.
- PatientsPage — **changed** (web): new "Last visit" column. Shows the formatted date or "-". Not sortable.
- PatientsPage.test.tsx — **changed** (web): adjust the `nextCell` and other cell lookups for the extra column, and add Last visit cases.
- appointment table + appointment_patient_start_idx — **existing** (db): read only. No migration.

Story mapping:
- S-1 (see the last visit): PatientRepo, GET /api/patients, client.ts, format.ts, PatientsPage.
- S-2 (dash when never seen): PatientRepo (null), PatientsPage ("-").
- S-3 (list behaviour unchanged): PatientRepo (filters, order and paging untouched), patientsRouter (existing), PatientsPage (no sort control), PatientsPage.test.tsx.
- S-4 (no medical history): PatientRepo (selected patient columns unchanged).

## Known
- S-1: served by the repository query, the endpoint change, the client type, the date formatter and the page column. Traces REQ-1 to REQ-5.
- S-2: a null `last_visit_at` renders "-". Traces REQ-5 and REQ-6.
- S-3: the list query's filters, order, paging and Next appointment value are untouched, and Last visit is not sortable. Traces REQ-7 to REQ-10.
- S-4: no patient columns are added to the list select, so medical history stays out. Traces REQ-11.

## Assumed
None.

## Unknown
- U-1 (non-blocking): no generated fact links PatientsPage to GET /api/patients. The design assumes the proposed default. See Q-01.

## Open questions
- Q-01 (non-blocking, tech lead): does PatientsPage load its rows from GET /api/patients through web/src/api/client.ts? Proposed default: yes.
