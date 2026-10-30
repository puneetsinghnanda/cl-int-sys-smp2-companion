---
schema: design.v1
run_id: run_01M66D77982JFTXDREQWB5QTJV
step: design
attempt: 2
skill: design@1.4.1
inputs:
  - path: artifacts/HMS-4/run_01M66D77982JFTXDREQWB5QTJV/impact.md
    sha256: 349e24faf9740b35e49bea15217759dbe806d6c78acadbf0bee04e178d3e931d
  - path: artifacts/HMS-4/run_01M66D77982JFTXDREQWB5QTJV/split.md
    sha256: f49a45c822dcc797bdba004c411c08fa32db8a41468d2957a34eb264d72f9764
  - path: artifacts/HMS-4/run_01M66D77982JFTXDREQWB5QTJV/req.md
    sha256: a33bb8ac0e7a965160b9d6ba639feb8f4086518ad90e0374e053ffbfacdfaf35
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
    how: "The web-formatting trade-off now cites impact Q-01 (convert in the API or the web page), not the non-existent Q-06, and records that its default (the API returns the instant and the web formats it) was adopted."
claims:
  - id: C1
    text: "The API is layered: routes, services, and `repositories` that hold the SQL."
    cites: ["context/architecture.md#components"]
  - id: C2
    text: "SQL lives in `api/src/repositories`."
    cites: ["context/conventions.md#code-layout"]
  - id: C3
    text: "Lists answer items, page, page_size and total, `20 per page`."
    cites: ["context/conventions.md#lists"]
  - id: C4
    text: "Patient list SQL lives in `PatientRepo`."
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/repositories/patientRepo.ts:PatientRepo"]
  - id: C5
    text: "The patient list page is `PatientsPage` in the web module."
    cites: ["context/generated/module-map.json#sym:sample-system:web/src/pages/PatientsPage.tsx:PatientsPage"]
  - id: C6
    text: "`PatientsPage.tsx` already has a `startsAt` helper used for appointment times."
    cites: ["context/generated/module-map.json#sym:sample-system:web/src/pages/PatientsPage.tsx:startsAt"]
  - id: C7
    text: "The patient list endpoint is defined in `api/src/routes/patients.ts`."
    cites: ["context/generated/api-inventory.json#http:GET:/api/patients"]
  - id: C8
    text: "Patient routes are mounted by `patientsRouter`."
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/routes/patients.ts:patientsRouter"]
  - id: C9
    text: "Instants are stored in `UTC`; the hospital zone comes from `HOSPITAL_TZ`."
    cites: ["context/architecture.md#time-and-slots"]
  - id: C10
    text: "The web app talks to the API only through `web/src/api/client.ts`."
    cites: ["context/architecture.md#components"]
  - id: C11
    text: "Medical history never appears in `lists`."
    cites: ["context/intent-baseline.md#data-protection"]
  - id: C12
    text: "The db module holds the migrations and schema for `db`."
    cites: ["context/generated/module-map.json#module:sample-system:db"]
trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10, REQ-11, REQ-12, REQ-13, REQ-14, REQ-15]
known: [S-2, S-3, S-4]
assumed: [S-1]
unknown:
  - {id: U-1, blocking: false}
  - {id: U-2, blocking: false}
questions:
  - id: Q-01
    blocking: false
    ask: tech lead
    text: What should the new list field be called?
    why: The name is not in any input; the web page and tests depend on it.
    evidence: ["context/conventions.md#lists", "context/generated/api-inventory.json#http:GET:/api/patients"]
    options: [last_visit_at, lastVisitAt]
    default: "last_visit_at, matching the snake_case of appointment starts_at"
  - id: Q-02
    blocking: false
    ask: tech lead
    text: Does any web page other than the patient list read GET /api/patients?
    why: Those callers receive the new field; it is additive, so they should be unaffected.
    evidence: ["context/generated/api-inventory.json#http:GET:/api/patients", "context/architecture.md#components"]
    options: [only the patient list, other callers exist and ignore the new field]
    default: other callers exist and ignore the new field
components:
  - name: PatientRepo list query
    module: module:sample-system:api
    change: changed
  - name: PatientService list
    module: module:sample-system:api
    change: existing
  - name: patientsRouter GET /api/patients
    module: module:sample-system:api
    change: existing
  - name: PatientsPage Last visit column
    module: module:sample-system:web
    change: changed
  - name: API client (web/src/api/client.ts)
    module: module:sample-system:web
    change: existing
  - name: appointment table and appointment_patient_start_idx
    module: module:sample-system:db
    change: existing
endpoints:
  - method: GET
    path: /api/patients
    change: changed
---
# Design: HMS-4

## Overview
The last visit is computed in SQL, inside the existing patient list query in `PatientRepo`. Each listed patient gets one extra field (Q-01, default `last_visit_at`): the latest `appointment.starts_at` among that patient's appointments with status `COMPLETED`, or null if there are none. `GET /api/patients` returns that field as a UTC instant. `PatientsPage` shows it in a new "Last visit" column, formatted with the existing `startsAt` helper so it matches the Next appointment column and the hospital zone. When the field is null, the column shows "-".

Trade-offs:
- **Correlated subquery, not a join with GROUP BY.** The subquery leaves the existing paging, search, sort and total count untouched (S-3), at the cost of one index lookup per row. The page is 20 rows, and the lookup uses the existing `appointment_patient_start_idx`.
- **Format in the web, not the API.** This adopts the default of impact Q-01 (convert in the API or in the web page): the API returns the instant and the web formats it. Reusing `startsAt` keeps one formatting path for REQ-14, at the cost of the API returning an instant rather than a ready-made date.
- **No migration.** The existing index covers (patient_id, starts_at). A partial index on COMPLETED appointments would be faster at scale, but the inputs give no volume that would justify it.
- **The status filter is an explicit `= 'COMPLETED'`.** This keeps Cancelled, No-show and Scheduled out (S-2) without depending on any change to the status set.

## Components
- **PatientRepo list query**, api, *changed*. Adds the last-visit subquery to the list SELECT. The WHERE, ORDER BY, LIMIT/OFFSET and count query stay as they are (S-3). The SELECT still lists only today's columns plus the new field, so no medical history column is added (S-4). Serves S-1, S-2, S-3, S-4.
- **PatientService list**, api, *existing*. Passes repository rows through unchanged.
- **patientsRouter GET /api/patients**, api, *existing*. Route, parameters and the list envelope (`items, page, page_size, total`, 20 per page) are unchanged.
- **PatientsPage Last visit column**, web, *changed*. Adds the "Last visit" column. It shows `startsAt(value)` when present and "-" when null. The Next appointment column is untouched. Serves S-1, S-2, S-3.
- **API client (`web/src/api/client.ts`)**, web, *existing*. Same call. Only the patient list item type gains the optional field.
- **appointment table and `appointment_patient_start_idx`**, db, *existing*. Read only; no migration.
- **Endpoint GET /api/patients**, *changed*. Each item gains the new field, which is additive. No new endpoint.

Story map: S-1 → PatientRepo, PatientsPage. S-2 → the PatientRepo status filter and the PatientsPage "-" fallback. S-3 → the PatientRepo query shape, patientsRouter, PatientsPage. S-4 → the PatientRepo SELECT list.

## Known
- S-2: the status filter `= 'COMPLETED'` and the "-" fallback come straight from known REQ-4 to REQ-7.
- S-3: paging, search, sort and Next appointment are left as they are, per known REQ-8 to REQ-11 and the list convention.
- S-4: the SELECT gains no medical history column, per known REQ-12 and the data-protection rule.

## Assumed
- S-1: relies on assumed REQ-13 and REQ-15 (rank and display by `starts_at`) and REQ-14 (format with the existing `startsAt` helper). The user chose to continue on these defaults.

## Unknown
- U-1 (non-blocking): the name of the new response field (Q-01).
- U-2 (non-blocking): other callers of `GET /api/patients` (Q-02).

## Open questions
- Q-01 (non-blocking, tech lead): What is the field name? Default: `last_visit_at`.
- Q-02 (non-blocking, tech lead): Are there other callers of `GET /api/patients`? Default: they exist and ignore the new field.
