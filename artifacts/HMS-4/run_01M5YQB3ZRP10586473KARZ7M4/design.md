---
schema: design.v1
run_id: run_01M5YQB3ZRP10586473KARZ7M4
step: design
attempt: 3
skill: design@1.4.1
inputs:
  - path: artifacts/HMS-4/run_01M5YQB3ZRP10586473KARZ7M4/impact.md
    sha256: cd2e15b797b5850df6d721524faaadfd47de3448476947cb2fb860c8688d08ea
  - path: artifacts/HMS-4/run_01M5YQB3ZRP10586473KARZ7M4/split.md
    sha256: 5ced1c4d84b9c46da98051a025953a0aea7312cd53a6d5686efdebadb117024e
  - path: artifacts/HMS-4/run_01M5YQB3ZRP10586473KARZ7M4/req.md
    sha256: 3e7adfbb0c88fcca0bb2ac95ec1a1b58c07a9930dc1a1c6e56af91e5475f7321
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
    how: "The web now renders last_visit_date as text and never parses it into a Date, so no browser zone is applied; Q-03 no longer defaults to reusing the instant formatter, and a negative-offset browser test is required for S-1 AC4."
claims:
  - id: C1
    text: "SQL lives in `api/src/repositories`, rules in `api/src/services`."
    cites: ["context/conventions.md#code-layout"]
  - id: C2
    text: "Lists answer items, page, page_size and total, `20 per page`."
    cites: ["context/conventions.md#lists"]
  - id: C3
    text: "Instants are stored in UTC; the hospital time zone is `HOSPITAL_TZ`."
    cites: ["context/architecture.md#time-and-slots"]
  - id: C4
    text: "The web app talks to the API only through `web/src/api/client.ts`."
    cites: ["context/architecture.md#components"]
  - id: C5
    text: "Patient list SQL is in `PatientRepo`."
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/repositories/patientRepo.ts:PatientRepo"]
  - id: C6
    text: "Patient rules are in `PatientService`."
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/services/patientService.ts:PatientService"]
  - id: C7
    text: "The patient list page is `PatientsPage`."
    cites: ["context/generated/module-map.json#sym:sample-system:web/src/pages/PatientsPage.tsx:PatientsPage"]
  - id: C8
    text: "PatientsPage already has a helper `startsAt`."
    cites: ["context/generated/module-map.json#sym:sample-system:web/src/pages/PatientsPage.tsx:startsAt"]
  - id: C9
    text: "The API has a helper `localDay` in api/src/lib/time.ts."
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/lib/time.ts:localDay"]
  - id: C10
    text: "The API loads its configuration in `loadConfig`."
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/config.ts:loadConfig"]
  - id: C11
    text: "The API has a day-string check `isDay` in api/src/lib/time.ts."
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/lib/time.ts:isDay"]
  - id: C12
    text: "The list route is `/api/patients` in api/src/routes/patients.ts."
    cites: ["context/generated/api-inventory.json#http:GET:/api/patients"]
  - id: C13
    text: "An index `appointment_patient_start_idx` exists."
    cites: ["context/generated/db-schema.json#index:appointment_patient_start_idx"]
  - id: C14
    text: "Logs never carry request bodies, contact details or `medical history`."
    cites: ["context/conventions.md#logging-and-data"]
  - id: C15
    text: "Database changes are `New SQL files only`."
    cites: ["context/conventions.md#database-changes"]
trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10, REQ-11, REQ-12, REQ-13]
known: [REQ-1, REQ-2, REQ-3, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10, REQ-11, REQ-12, REQ-13]
assumed: [REQ-4]
unknown: [U-1, U-2, U-3]
unknowns:
  - {id: U-1, blocking: false, text: "Other web callers of GET /api/patients are not listed in the facts; they will receive the extra field (impact U-1).", question: Q-01}
  - {id: U-2, blocking: false, text: "The field name `last_visit_date` and its day-string form are a proposal; no input fixes them.", question: Q-02}
  - {id: U-3, blocking: false, text: "How the Next appointment column lays out its date (the split Q-01 default) is not visible in the facts; only the `startsAt` helper name is.", question: Q-03}
questions:
  - id: Q-01
    blocking: false
    ask: tech lead
    text: "Is an additive, read-only field on the GET /api/patients list items acceptable for every existing caller?"
    why: "Every caller of the list route receives the changed response."
    evidence: ["context/generated/api-inventory.json#http:GET:/api/patients", "artifacts/HMS-4/run_01M5YQB3ZRP10586473KARZ7M4/impact.md#L82"]
    options: ["Additive field on the existing list", "Separate endpoint for last visits"]
    default: "Additive field on the existing list"
  - id: Q-02
    blocking: false
    ask: tech lead
    text: "Is `last_visit_date` as a hospital-local day string in the form isDay accepts (assumed YYYY-MM-DD), or null, the right name and shape?"
    why: "The name and type become part of the API contract the web app and tests use."
    evidence: ["context/conventions.md#lists", "context/architecture.md#time-and-slots"]
    options: ["last_visit_date as a hospital-local day string or null", "last_visit_at as a UTC instant converted in the web"]
    default: "last_visit_date as a hospital-local day string or null"
  - id: Q-03
    blocking: false
    ask: product owner
    text: "Should Last visit show the day string as returned, or a reordered text layout matching the Next appointment column?"
    why: "The split's Q-01 default is the Next appointment column's format; any layout must be done on the text, never through a browser-zone Date."
    evidence: ["context/generated/module-map.json#sym:sample-system:web/src/pages/PatientsPage.tsx:startsAt", "artifacts/HMS-4/run_01M5YQB3ZRP10586473KARZ7M4/split.md#L51"]
    options: ["Show the day string as returned", "Reorder the day string's parts as text to match the Next appointment layout"]
    default: "Show the day string as returned"
components:
  - {name: "PatientRepo list query", module: module:sample-system:api, change: changed}
  - {name: "PatientService list (adds hospital-local day via localDay)", module: module:sample-system:api, change: changed}
  - {name: "time.ts localDay and loadConfig HOSPITAL_TZ", module: module:sample-system:api, change: existing}
  - {name: "patients router (GET /api/patients)", module: module:sample-system:api, change: existing}
  - {name: "PatientsPage Last visit column (text render, no Date parsing)", module: module:sample-system:web, change: changed}
  - {name: "web API client patient list type", module: module:sample-system:web, change: changed}
  - {name: "appointment_patient_start_idx index", module: module:sample-system:db, change: existing}
endpoints:
  - {method: GET, path: /api/patients, change: changed}
  - {method: GET, path: /api/patients/:id, change: existing}
---
# Design: HMS-4

## Overview
The patient list query in `PatientRepo` adds, per patient row, the latest `starts_at` among that patient's appointments with status COMPLETED (a correlated `max` over the appointment table, served by the existing `appointment_patient_start_idx`). `PatientService` turns that UTC instant into a hospital-local day with the existing `localDay` helper and the `HOSPITAL_TZ` value from `loadConfig`, and GET /api/patients returns it on each list item as `last_visit_date`: a day string in the form `isDay` accepts, or null. The rest of the query is untouched: same filters, ORDER BY, page size of 20 and total, so search, sort and paging do not move (S-3). PatientsPage adds a "Last visit" column that renders `last_visit_date` as text, or "-" for null (S-1, S-2). It never builds a `Date` from the string, so the browser's time zone is never applied. The query adds only this one value to today's list columns, so no medical history enters the list (S-4). No migration, new endpoint, job or service is needed.

Trade-offs:
- Date computed in the API, not the web: the hospital zone is configured in the API, so the web needs no time-zone source. This departs from the impact Q-02 default ("same approach as the Next appointment column") because that approach is not visible in the facts and would tie S-1 AC4 to an unverified web zone source.
- The web treats `last_visit_date` as text, not as a `Date` (fixes review F-1): a day string parsed as a `Date` is read as UTC midnight and can show the previous day in browser zones west of UTC; text rendering cannot shift. The cost: any layout change (Q-03) must be done by rearranging the string, not by a date formatter.
- Correlated `max` in the list query instead of a stored column: always correct and no migration, at the cost of one indexed lookup per row on a page of 20.
- Order by `starts_at`, not by when the appointment was closed (assumed REQ-4): matches "visited" meaning the appointment time; if the product owner disagrees only the `max` column changes.

## Components
- PatientRepo list query (module:sample-system:api) — **changed**: adds the latest COMPLETED `starts_at` per patient; keeps filters, order, limit and the list columns as they are. Serves S-1, S-2, S-3, S-4.
- PatientService list (module:sample-system:api) — **changed**: maps that instant to a hospital-local day with `localDay` and `HOSPITAL_TZ` from `loadConfig`, and sets `last_visit_date` (null when there is no Completed appointment). Serves S-1 AC4.
- time.ts localDay and loadConfig HOSPITAL_TZ (module:sample-system:api) — **existing**: reused as is.
- patients router GET /api/patients (module:sample-system:api) — **existing** code; only the response shape changes.
- PatientsPage Last visit column (module:sample-system:web) — **changed**: new column that renders `last_visit_date` as text and "-" when null; no `new Date(...)` or date formatter is applied to it. The Next appointment column and its `startsAt` helper stay unchanged (S-3). Its tests must include S-1 AC4 run with the browser in a zone west of UTC.
- web API client patient list type (module:sample-system:web) — **changed**: the list item type gains `last_visit_date` as a string or null; no new call.
- appointment_patient_start_idx index (module:sample-system:db) — **existing**: supports the per-patient lookup; no migration.

### Endpoints
- GET /api/patients — **changed**: each item gains `last_visit_date` (hospital-local day string or null); `items`, `page`, `page_size`, `total` unchanged.
- GET /api/patients/:id — **existing**: the patient's own page, the only place medical history is shown; untouched.

## Known
- REQ-1, REQ-8: PatientsPage gets the "Last visit" column and shows "-" when `last_visit_date` is null.
- REQ-2, REQ-5, REQ-6, REQ-7: the query only considers appointments with status COMPLETED, so Cancelled, No-show and Scheduled never appear.
- REQ-3: the instant is stored in UTC and turned into a day in HOSPITAL_TZ (Asia/Kolkata by default) by the API; the web shows that text unchanged, so the browser zone never applies.
- REQ-9, REQ-10, REQ-11: the list keeps `{ items, page, page_size, total }` at 20 per page with the same filters and ORDER BY.
- REQ-12: the Next appointment column and its helper are not touched.
- REQ-13: the list adds one value only; medical history stays on GET /api/patients/:id, and logs never carry it.

## Assumed
- REQ-4: "most recent" is the COMPLETED appointment with the latest `starts_at`; basis in the intent's assumption (glossary: Completed is closed after its start time).
- `localDay` returns the hospital-local day of an instant for a given zone, in the form `isDay` accepts (assumed YYYY-MM-DD), and `loadConfig` exposes HOSPITAL_TZ. Basis: their names and places in api/src/lib/time.ts and api/src/config.ts, and architecture#time-and-slots; implement must confirm the signatures.

## Unknown
- U-1 (non-blocking): other callers of GET /api/patients will receive the extra field (Q-01).
- U-2 (non-blocking): the field name `last_visit_date` and its day-string form are a proposal (Q-02).
- U-3 (non-blocking): the Next appointment column's date layout is not visible in the facts (Q-03).

## Open questions
- Q-01 (non-blocking, tech lead): is an additive field on the list acceptable for all callers? Default: yes, additive.
- Q-02 (non-blocking, tech lead): is `last_visit_date` (hospital-local day string or null) the right name and shape? Default: yes.
- Q-03 (non-blocking, product owner): show the day string as returned, or rearrange it as text to match the Next appointment layout? Default: show it as returned.
