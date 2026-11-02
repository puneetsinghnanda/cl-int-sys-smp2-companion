---
schema: design.v1
run_id: run_01M6DN23JRRYT113FF078VYBSF
step: design
attempt: 2
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
resolves:
  - id: F-1
    how: "The hospital-zone conversion moves to the API, which owns `HOSPITAL_TZ` (architecture#time-and-slots, cited as C4/C16). GET /api/patients returns `last_visit_date` as a calendar date already in the hospital zone, so the web needs no zone for REQ-3. The web only formats a plain date for REQ-4. The one remaining gap, whether that date format matches the Next appointment column's dateTime output, is now an explicit Unknown U-2 with question Q-02 (non-blocking, tech lead) instead of an unstated assumption."
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
    text: "Database changes are only ever `New SQL files`; this design needs no migration."
    cites: ["context/conventions.md#database-changes"]
  - id: C16
    text: "`HOSPITAL_TZ` is API configuration, read in api/src/config.ts, with default Asia/Kolkata."
    cites: ["context/architecture.md#time-and-slots"]
  - id: C17
    text: "The web has a `useSettings` hook, backed by the settings route."
    cites: ["context/generated/module-map.json#sym:sample-system:web/src/hooks/useSettings.tsx:useSettings"]
  - id: C18
    text: "The API exposes `/api/settings`."
    cites: ["context/generated/api-inventory.json#http:GET:/api/settings"]
trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10, REQ-11]
known: [S-1, S-2, S-3, S-4]
assumed: []
unknown:
  - {id: U-1, blocking: false}
  - {id: U-2, blocking: false}
questions:
  - id: Q-01
    blocking: false
    ask: tech lead
    text: "Does PatientsPage load its rows from GET /api/patients through web/src/api/client.ts? (Carried from the impact analysis.)"
    why: "The design extends that response. If the page uses another route, that route changes instead."
    evidence: ["artifacts/HMS-4/run_01M6DN23JRRYT113FF078VYBSF/impact.md#open-questions", "context/architecture.md#components"]
    options: ["GET /api/patients (proposed default)", "Another route; name it"]
    default: "GET /api/patients"
  - id: Q-02
    blocking: false
    ask: tech lead
    text: "Which format does the Next appointment column's date part use (the locale and options of format.ts dateTime), so the new date-only formatter can match it exactly?"
    why: "REQ-4 requires the Last visit date to match the Next appointment column without the time. No generated fact shows dateTime's locale or options, or whether it applies a zone (for example from useSettings / GET /api/settings)."
    evidence: ["context/generated/module-map.json#sym:sample-system:web/src/lib/format.ts:dateTime", "context/generated/module-map.json#sym:sample-system:web/src/hooks/useSettings.tsx:useSettings", "context/generated/api-inventory.json#http:GET:/api/settings"]
    options: ["Reuse dateTime's date options, dropping the time fields (proposed default)", "Another format; name it"]
    default: "Reuse dateTime's date options, dropping the time fields"
components:
  - name: PatientRepo list query (adds the last Completed visit date per patient, in HOSPITAL_TZ)
    module: module:sample-system:api
    change: changed
  - name: PatientService list (passes the new field through unchanged)
    module: module:sample-system:api
    change: existing
  - name: patientsRouter GET /api/patients (same query parameters)
    module: module:sample-system:api
    change: existing
  - name: web/src/api/client.ts patient list item type (gains last_visit_date)
    module: module:sample-system:web
    change: changed
  - name: format.ts date-only formatter for a plain calendar date (new helper beside dateTime; no zone conversion)
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

**API (owns the hospital time zone).** `HOSPITAL_TZ` is API configuration (default Asia/Kolkata, architecture#time-and-slots), so the API converts to the hospital date. The repository's list query adds one value per patient, `last_visit_date`. It is a correlated scalar subquery: `max(starts_at)` over `appointment` where `patient_id` is the row's patient and status is `COMPLETED`, converted to a calendar date in `HOSPITAL_TZ` (`YYYY-MM-DD`). It is `null` when the patient has no Completed appointment. The `WHERE`, `ORDER BY`, `LIMIT/OFFSET` and `total` count of the list query are left untouched, so paging (20 per page, `{ items, page, page_size, total }`), search and sort behave as before. The existing Next appointment value is not touched. The selected patient columns stay the same, so no medical history is added to the list.

**Web (formats a plain date, no zone needed).** `last_visit_date: string | null` is added to the list item type. `format.ts` gets a date-only formatter for a plain calendar date. It uses the same date presentation as `dateTime` without the time fields (exact options per Q-02), and it does not convert zones because the date is already the hospital date. `PatientsPage` adds a "Last visit" column that renders that date, or "-" when the value is `null`. The column is not wired into any sort control.

Trade-offs:
- The hospital date is computed on the server, not by formatting a UTC instant on the web. This keeps the zone with the component that owns `HOSPITAL_TZ` (REQ-3) and removes any dependency on how the web learns the zone. The cost: the API returns a date string rather than an instant, so the field is display-only.
- A correlated scalar subquery instead of a materialised "last visit" column: no migration and always correct. `appointment_patient_start_idx` (patient_id, starts_at) bounds each lookup to that patient's appointments, but status is not in the index, so `COMPLETED` is filtered after the index range scan. With 20 rows per page this is acceptable. A partial index is a later option, not part of this change.
- Only `COMPLETED` is filtered in SQL, not in the UI. This guarantees Cancelled, No-show and Scheduled never appear (REQ-5), whatever the client does.

## Components
- PatientRepo list query — **changed** (api): add `last_visit_date` = the `HOSPITAL_TZ` calendar date of the latest `starts_at` among the patient's `COMPLETED` appointments, or null. No other change to filters, ordering or columns.
- PatientService list — **existing** (api): passes rows through. No rule changes.
- patientsRouter GET /api/patients — **existing** (api): same query parameters and validation.
- GET /api/patients — **changed** endpoint: each item gains `last_visit_date` (`YYYY-MM-DD` in the hospital zone, or null). The envelope `{ items, page, page_size, total }` is unchanged.
- client.ts patient list item type — **changed** (web): adds `last_visit_date: string | null`.
- format.ts date-only formatter — **new** (web): formats a plain calendar date in the same date presentation as `dateTime`, without the time. No zone conversion.
- PatientsPage — **changed** (web): new "Last visit" column. Shows the formatted date or "-". Not sortable.
- PatientsPage.test.tsx — **changed** (web): adjust the `nextCell` and other cell lookups for the extra column, and add Last visit cases.
- appointment table + appointment_patient_start_idx — **existing** (db): read only. No migration.

Story mapping:
- S-1 (see the last visit): PatientRepo (latest COMPLETED, hospital date), GET /api/patients, client.ts, format.ts, PatientsPage.
- S-2 (dash when never seen): PatientRepo (null), PatientsPage ("-").
- S-3 (list behaviour unchanged): PatientRepo (filters, order and paging untouched), patientsRouter (existing), PatientsPage (no sort control), PatientsPage.test.tsx.
- S-4 (no medical history): PatientRepo (selected patient columns unchanged).

## Known
- S-1: the repository computes the latest `COMPLETED` start as a `HOSPITAL_TZ` date (REQ-2, REQ-3, REQ-5). The page adds the column (REQ-1) and formats the date without the time (REQ-4; exact options per Q-02).
- S-2: a null `last_visit_date` renders "-". Traces REQ-5 and REQ-6.
- S-3: the list query's filters, order, paging and Next appointment value are untouched, and Last visit is not sortable. Traces REQ-7 to REQ-10.
- S-4: no patient columns are added to the list select, so medical history stays out. Traces REQ-11.

## Assumed
None.

## Unknown
- U-1 (non-blocking): no generated fact links PatientsPage to GET /api/patients. The design assumes the proposed default. See Q-01.
- U-2 (non-blocking): no generated fact shows the locale and options of `dateTime`, or whether the web applies a zone through `useSettings` / `GET /api/settings`. The design does not depend on the web's zone (the API sends the hospital date), but the date-only presentation must match the Next appointment column. See Q-02.

## Open questions
- Q-01 (non-blocking, tech lead): does PatientsPage load its rows from GET /api/patients through web/src/api/client.ts? Proposed default: yes.
- Q-02 (non-blocking, tech lead): which locale and date options does format.ts `dateTime` use for the Next appointment column, so the new date-only formatter matches it? Proposed default: reuse dateTime's date options without the time fields.
