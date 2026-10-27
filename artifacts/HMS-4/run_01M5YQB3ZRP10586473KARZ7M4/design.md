---
schema: design.v1
run_id: run_01M5YQB3ZRP10586473KARZ7M4
step: design
attempt: 1
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
    text: "The web app has a date formatter `dateTime`."
    cites: ["context/generated/module-map.json#sym:sample-system:web/src/lib/format.ts:dateTime"]
  - id: C10
    text: "The web app reads settings through `useSettings`."
    cites: ["context/generated/module-map.json#sym:sample-system:web/src/hooks/useSettings.tsx:useSettings"]
  - id: C11
    text: "A settings route `/api/settings` exists."
    cites: ["context/generated/api-inventory.json#http:GET:/api/settings"]
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
unknown: [U-1, U-2]
unknowns:
  - {id: U-1, blocking: false, text: "Other web callers of GET /api/patients are not listed in the facts; they will receive the extra field (impact U-1).", question: Q-01}
  - {id: U-2, blocking: false, text: "The field name `last_visit_at` follows the snake_case of page_size but is not fixed by any input.", question: Q-02}
questions:
  - id: Q-01
    blocking: false
    ask: tech lead
    text: "Is an additive, read-only field on the GET /api/patients list items acceptable for every existing caller?"
    why: "Every caller of the list route receives the changed response."
    evidence: ["context/generated/api-inventory.json#http:GET:/api/patients", "artifacts/HMS-4/run_01M5YQB3ZRP10586473KARZ7M4/impact.md#L80"]
    options: ["Additive field on the existing list", "Separate endpoint for last visits"]
    default: "Additive field on the existing list"
  - id: Q-02
    blocking: false
    ask: tech lead
    text: "Is `last_visit_at` (UTC instant or null) the right name and shape for the new list field?"
    why: "The name and type become part of the API contract the web app and tests use."
    evidence: ["context/conventions.md#lists", "context/architecture.md#time-and-slots"]
    options: ["last_visit_at as a UTC instant or null", "last_visit_date as a hospital-time date string"]
    default: "last_visit_at as a UTC instant or null"
components:
  - {name: "PatientRepo list query", module: module:sample-system:api, change: changed}
  - {name: "PatientService list", module: module:sample-system:api, change: existing}
  - {name: "patients router (GET /api/patients)", module: module:sample-system:api, change: existing}
  - {name: "PatientsPage Last visit column", module: module:sample-system:web, change: changed}
  - {name: "web API client patient list type", module: module:sample-system:web, change: changed}
  - {name: "format.ts dateTime and useSettings (hospital time zone)", module: module:sample-system:web, change: existing}
  - {name: "appointment_patient_start_idx index", module: module:sample-system:db, change: existing}
endpoints:
  - {method: GET, path: /api/patients, change: changed}
  - {method: GET, path: /api/patients/:id, change: existing}
  - {method: GET, path: /api/settings, change: existing}
---
# Design: HMS-4

## Overview
The patient list query in `PatientRepo` adds, per patient row, the latest `starts_at` among that patient's appointments with status COMPLETED (a correlated `max` over the appointment table, served by the existing `appointment_patient_start_idx`). GET /api/patients returns it on each list item as `last_visit_at`, a UTC instant or null. The rest of the query is untouched: the same filters, ORDER BY, page size of 20 and total, so search, sort and paging do not move (S-3). PatientsPage adds a "Last visit" column that converts the instant to a date in the hospital time zone with the existing date helpers and settings, and shows "-" for null (S-1, S-2). The query selects only the new value in addition to today's list columns, so no medical history enters the list (S-4). No migration, new endpoint, job or service is needed.

Trade-offs:
- Correlated `max` in the list query instead of a stored column: always correct and no migration, at the cost of one indexed lookup per row on a page of 20.
- Return a UTC instant and convert in the web app instead of a server-computed date: matches how instants are stored and keeps one conversion place in the UI, but the web must use the hospital zone, not the browser zone.
- Order by `starts_at`, not by when the appointment was closed (assumed REQ-4): matches "visited" meaning the appointment time; if the product owner disagrees only the `max` column changes.

## Components
- PatientRepo list query (module:sample-system:api) — **changed**: adds `last_visit_at` = latest COMPLETED `starts_at` per patient; keeps filters, order, limit and the list columns as they are. Serves S-1, S-2, S-3, S-4.
- PatientService list (module:sample-system:api) — **existing**: passes the repository rows through; changes only if it builds the list item by hand.
- patients router GET /api/patients (module:sample-system:api) — **existing** code; the route's response shape changes (see endpoints).
- PatientsPage Last visit column (module:sample-system:web) — **changed**: new column after Next appointment; date in hospital time zone, "-" when null. Serves S-1, S-2; the Next appointment column and its `startsAt` helper stay unchanged (S-3).
- web API client patient list type (module:sample-system:web) — **changed**: the list item type gains `last_visit_at`; no new call.
- format.ts dateTime and useSettings (module:sample-system:web) — **existing**: used to render the date in the hospital time zone.
- appointment_patient_start_idx index (module:sample-system:db) — **existing**: supports the per-patient lookup; no migration.
- GET /api/patients — **changed**: each item gains `last_visit_at` (UTC instant or null); `items`, `page`, `page_size`, `total` unchanged.
- GET /api/patients/:id — **existing**: the patient's own page, the only place medical history is shown; untouched.
- GET /api/settings — **existing**: source of the hospital time zone for the web app (assumed, see below).

## Known
- REQ-1, REQ-8: PatientsPage gets the "Last visit" column and shows "-" when `last_visit_at` is null.
- REQ-2, REQ-5, REQ-6, REQ-7: the query only considers appointments with status COMPLETED, so Cancelled, No-show and Scheduled never appear.
- REQ-3: the instant is stored in UTC and rendered in the hospital time zone (HOSPITAL_TZ).
- REQ-9, REQ-10, REQ-11: the list keeps `{ items, page, page_size, total }` at 20 per page with the same filters and ORDER BY.
- REQ-12: the Next appointment column and its helper are not touched.
- REQ-13: the list query adds one value only; medical history stays on GET /api/patients/:id, and logs never carry it.

## Assumed
- REQ-4: "most recent" is the COMPLETED appointment with the latest `starts_at`; basis in the intent's assumption (glossary: Completed is closed after its start time).
- The web app gets the hospital time zone from GET /api/settings through `useSettings`. Basis: the settings route and hook exist in the facts; what the settings payload holds is not in the facts, so implement must confirm it.

## Unknown
- U-1 (non-blocking): other callers of GET /api/patients will receive the extra field (Q-01).
- U-2 (non-blocking): the field name `last_visit_at` and its UTC-instant shape are a proposal, not fixed by any input (Q-02).

## Open questions
- Q-01 (non-blocking, tech lead): is an additive field on the list acceptable for all callers? Default: yes, additive.
- Q-02 (non-blocking, tech lead): is `last_visit_at` (UTC instant or null) the right name and shape? Default: yes.
