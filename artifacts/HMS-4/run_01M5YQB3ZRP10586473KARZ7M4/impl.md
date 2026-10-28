---
schema: change.v1
run_id: run_01M5YQB3ZRP10586473KARZ7M4
step: impl
attempt: 1
skill: implement@1.0.1
inputs:
  - path: artifacts/HMS-4/run_01M5YQB3ZRP10586473KARZ7M4/design.md
    sha256: 9c01a9c1ddd65536f8dc4ae53ecaadecbec0ffc9b6b47d6e5d21557a93acb30a
  - path: artifacts/HMS-4/run_01M5YQB3ZRP10586473KARZ7M4/split.md
    sha256: 5ced1c4d84b9c46da98051a025953a0aea7312cd53a6d5686efdebadb117024e
  - path: context/conventions.md
    sha256: dc53d94ec34eb193eeb06fdd0db19eb86a24c29f7d3c434e7c212d61038ca852
  - path: context/generated/module-map.json
    sha256: 04f84ff09b4ad6ffaba59ec846716d25f03c8aae22292958a47ebb9247632345
pack_sha:
  sample-system: bd98ff08b83f8762c5e5c073ba5fadef88169b33
code:
  - repo: sample-system
    base_sha: bd98ff08b83f8762c5e5c073ba5fadef88169b33
    head_sha: 1221e71c4b446915fc566bf0cdb500e9ced51388
    diff_sha256: 4c84321e0fc36ccb1f569423adc5e7b26220b4dcafe0efd2241bfd7d6918ba7d
    files:
      - api/src/repositories/patientRepo.ts
      - api/src/services/patientService.ts
      - api/src/types.ts
      - api/test/patients.test.ts
      - web/src/__tests__/PatientsPage.test.tsx
      - web/src/api/types.ts
      - web/src/pages/PatientsPage.tsx
claims:
  - id: C1
    text: "SQL lives in `api/src/repositories` and rules in api/src/services."
    cites: ["context/conventions.md#code-layout"]
  - id: C2
    text: "Lists answer items, page, page_size and total, `20 per page`."
    cites: ["context/conventions.md#lists"]
  - id: C3
    text: "Logs never carry request bodies, contact details or `medical` history."
    cites: ["context/conventions.md#logging-and-data"]
  - id: C4
    text: "Database changes are `New SQL files only`; this change adds none."
    cites: ["context/conventions.md#database-changes"]
  - id: C5
    text: "Patient list SQL is in `PatientRepo`."
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/repositories/patientRepo.ts:PatientRepo"]
  - id: C6
    text: "Patient list rules are in `PatientService`."
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/services/patientService.ts:PatientService"]
  - id: C7
    text: "The hospital-local day comes from `localDay`."
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/lib/time.ts:localDay"]
  - id: C8
    text: "The patient list page is `PatientsPage`."
    cites: ["context/generated/module-map.json#sym:sample-system:web/src/pages/PatientsPage.tsx:PatientsPage"]
  - id: C9
    text: "The existing web test helper `nextCell` reads the last cell of a row."
    cites: ["context/generated/module-map.json#sym:sample-system:web/src/__tests__/PatientsPage.test.tsx:nextCell"]
trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10, REQ-11, REQ-12, REQ-13]
known: [REQ-1, REQ-2, REQ-3, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10, REQ-11, REQ-12, REQ-13]
assumed: [REQ-4]
unknown: [U-1]
unknowns:
  - {id: U-1, blocking: false, text: "The design's open questions (additive field, field name and shape, layout of the day string) are still unanswered; the code follows their defaults.", question: Q-01}
questions:
  - id: Q-01
    blocking: false
    ask: tech lead
    text: "Do the design defaults stand: an additive last_visit_date field, a YYYY-MM-DD hospital-local day or null, shown as returned?"
    why: "The code and the API contract follow these defaults; a different answer changes the service mapping and the page cell."
    evidence: ["artifacts/HMS-4/run_01M5YQB3ZRP10586473KARZ7M4/design.md#L97", "artifacts/HMS-4/run_01M5YQB3ZRP10586473KARZ7M4/design.md#L105"]
    options: ["Keep the defaults", "Change the name, shape or layout"]
    default: "Keep the defaults"
summary: "GET /api/patients list items gain last_visit_date: the hospital-local day (via localDay and the configured time zone) of the patient's latest COMPLETED appointment by starts_at, or null. PatientsPage shows it as text in a new Last visit column before Next appointment, with '-' when null. No migration, endpoint or log change."
---
# Change: HMS-4

## Summary
- **PatientRepo list query** (changed): the page query adds a correlated `max(starts_at)` over appointments with the literal status `COMPLETED` for each row on the page (`last_visit_at`). Filters, ORDER BY, LIMIT/OFFSET, the count query and `LIST_COLUMNS` are unchanged, so paging, search, sort and the no-medical-history column list stay as they are (C1, C2, C5). No request value enters the new subquery.
- **PatientService list** (changed): maps `last_visit_at` to `last_visit_date` with `localDay(instant, config.timeZone)` (config.timeZone is read from HOSPITAL_TZ by loadConfig, default Asia/Kolkata), or null; the raw instant is not sent (C6, C7). Nothing is logged (C3).
- **web API client patient list type** (changed): `PatientListItem` gains `last_visit_date: string | null`; the client call is unchanged.
- **PatientsPage Last visit column** (changed): new "Last visit" header and cell rendering `last_visit_date` as text, "-" when null; no Date parsing or formatter (C8). The column sits before Next appointment so the Next appointment cell stays last and its helper `nextCell` still finds it (C9).
- **time.ts localDay and loadConfig HOSPITAL_TZ**, **patients router**, **appointment_patient_start_idx index**: existing, untouched. No migration (C4).

## Changes
- api/src/repositories/patientRepo.ts — PatientRepo list query: `last_visit_at` subquery, `PatientListRow` return type.
- api/src/services/patientService.ts — PatientService list: hospital-local `last_visit_date`.
- api/src/types.ts — API list item type gains `last_visit_date`.
- web/src/api/types.ts — web API client patient list type gains `last_visit_date`.
- web/src/pages/PatientsPage.tsx — PatientsPage Last visit column.
- api/test/patients.test.ts — existing PAT-8 test pins the list item keys, which the design changes; `last_visit_date` added to the expected keys (still no medical history).
- web/src/__tests__/PatientsPage.test.tsx — fixture items gain `last_visit_date`; new test "HMS-4 S-1 AC4" renders with the browser zone America/New_York (west of UTC) and expects the day string unchanged, as the design requires.

## Known
- REQ-1, REQ-8: Last visit column with "-" for null on PatientsPage.
- REQ-2, REQ-5, REQ-6, REQ-7: only status COMPLETED is read, so Cancelled, No-show and Scheduled never count.
- REQ-3: the API converts the UTC instant with localDay in the configured hospital zone; the web shows text.
- REQ-9, REQ-10, REQ-11: filters, order, page size and total untouched.
- REQ-12: the Next appointment column and startsAt helper are untouched and stay the last cell.
- REQ-13: LIST_COLUMNS unchanged; only one derived value is added.

## Assumed
- REQ-4: "most recent" is the COMPLETED appointment with the latest starts_at, as the design assumes.

## Unknown
- U-1 (non-blocking): the design's open questions are unanswered; the code follows their defaults (Q-01).

## Open questions
- Q-01 (non-blocking, tech lead): do the design defaults stand for the field name, shape and layout? Default: keep them.
