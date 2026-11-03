---
schema: design.v1
run_id: run_01M6GDYQX07XECMVT06FY0Z6YF
step: design
attempt: 1
skill: design@1.6.0
inputs:
  - path: artifacts/HMS-4/run_01M6GDYQX07XECMVT06FY0Z6YF/impact.md
    sha256: aafe8381b8503a4a8086fdb2c26f4a90b07b0025cc8c7870cbd47450fc742758
  - path: artifacts/HMS-4/run_01M6GDYQX07XECMVT06FY0Z6YF/split.md
    sha256: 2b91c8419ea676ad0f4a6be8fc05976e887781020f53ff2feff1ce304d1b785a
  - path: artifacts/HMS-4/run_01M6GDYQX07XECMVT06FY0Z6YF/req.md
    sha256: 7092a382066b6eb79dfc22e522257e2a7c5679697745e27c13b84a0c491fcffd
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
    text: "PatientsPage gets the hospital time zone from `useSettings` (web/src/hooks/useSettings.tsx, backed by GET /api/settings), the same way it formats Next appointment."
    cites: ["context/generated/module-map.json#sym:sample-system:web/src/hooks/useSettings.tsx:useSettings", "context/generated/api-inventory.json#http:GET:/api/settings", "runs/run_01M6GDYQX07XECMVT06FY0Z6YF/ide-answers.md#a-16"]
  - id: C2
    text: "The web's patient list item type is declared in PatientsPage.tsx or next to `api` in web/src/api/client.ts; implement must confirm the file and extend the type with lastVisitAt."
    cites: ["context/generated/module-map.json#sym:sample-system:web/src/api/client.ts:api", "context/generated/module-map.json#sym:sample-system:web/src/pages/PatientsPage.tsx:PatientsPage", "runs/run_01M6GDYQX07XECMVT06FY0Z6YF/ide-answers.md#a-17"]
  - id: C3
    text: "List responses answer `{ items, page, page_size, total }`, 20 per page; the wrapper uses snake_case `page_size`."
    cites: ["context/conventions.md#lists"]
  - id: C4
    text: "web/src/lib/format.ts exists and holds `dateTime`, so the new dateOnly helper goes in that file."
    cites: ["context/generated/module-map.json#sym:sample-system:web/src/lib/format.ts:dateTime", "runs/run_01M6GDYQX07XECMVT06FY0Z6YF/ide-answers.md#a-15"]
  - id: C5
    text: "The index appointment_patient_start_idx covers patient_id and `starts_at`, so a per-patient max(starts_at) lookup can use it, with the COMPLETED status filtered on top."
    cites: ["context/generated/db-schema.json#index:appointment_patient_start_idx", "runs/run_01M6GDYQX07XECMVT06FY0Z6YF/ide-answers.md#a-06"]
  - id: C6
    text: "`Medical history` appears only on the patient's own page, never in lists."
    cites: ["context/intent-baseline.md#data-protection"]
  - id: C7
    text: "Patient SQL lives in `PatientRepo` (api/src/repositories/patientRepo.ts); repositories hold the SQL."
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/repositories/patientRepo.ts:PatientRepo", "context/architecture.md#components"]
  - id: C8
    text: "`PatientService` only passes the new field through from PatientRepo to the route."
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/services/patientService.ts:PatientService", "runs/run_01M6GDYQX07XECMVT06FY0Z6YF/ide-answers.md#a-13"]
  - id: C9
    text: "Appointment instants are stored in `UTC`; the hospital time zone is Asia/Kolkata."
    cites: ["context/architecture.md#time-and-slots"]
  - id: C10
    text: "The patient list route GET /api/patients is declared by `patientsRouter` in api/src/routes/patients.ts."
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/routes/patients.ts:patientsRouter", "context/generated/api-inventory.json#http:GET:/api/patients"]
  - id: C11
    text: "The API returns the last visit as the starts_at instant, and a date-only helper formats it in the hospital zone; this contract was fixed in impact by `A-12`."
    cites: ["runs/run_01M6GDYQX07XECMVT06FY0Z6YF/ide-answers.md#a-12"]
trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10, REQ-11, REQ-12]
known: [S-1, S-2, S-3, S-4, S-5]
assumed: []
unknown: []
questions: []
components:
  - id: CO-1
    name: PatientRepo list query (adds lastVisitAt)
    module: module:sample-system:api
    change: changed
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/repositories/patientRepo.ts:PatientRepo", "runs/run_01M6GDYQX07XECMVT06FY0Z6YF/ide-answers.md#a-05", "runs/run_01M6GDYQX07XECMVT06FY0Z6YF/ide-answers.md#a-14"]
  - id: CO-2
    name: PatientService (pass-through)
    module: module:sample-system:api
    change: existing
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/services/patientService.ts:PatientService", "runs/run_01M6GDYQX07XECMVT06FY0Z6YF/ide-answers.md#a-13"]
  - id: CO-3
    name: patientsRouter list handler
    module: module:sample-system:api
    change: existing
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/routes/patients.ts:patientsRouter"]
  - id: CO-4
    name: dateOnly(iso, timeZone) helper in web/src/lib/format.ts
    module: module:sample-system:web
    change: new
    cites: ["context/generated/module-map.json#sym:sample-system:web/src/lib/format.ts:dateTime", "runs/run_01M6GDYQX07XECMVT06FY0Z6YF/ide-answers.md#a-15"]
  - id: CO-5
    name: PatientsPage Last visit column
    module: module:sample-system:web
    change: changed
    cites: ["context/generated/module-map.json#sym:sample-system:web/src/pages/PatientsPage.tsx:PatientsPage", "runs/run_01M6GDYQX07XECMVT06FY0Z6YF/ide-answers.md#a-09"]
  - id: CO-6
    name: Web patient list item type (adds lastVisitAt)
    module: module:sample-system:web
    change: changed
    cites: ["context/generated/module-map.json#sym:sample-system:web/src/api/client.ts:api", "runs/run_01M6GDYQX07XECMVT06FY0Z6YF/ide-answers.md#a-14", "runs/run_01M6GDYQX07XECMVT06FY0Z6YF/ide-answers.md#a-17"]
  - id: CO-7
    name: useSettings (hospital time zone)
    module: module:sample-system:web
    change: existing
    cites: ["context/generated/module-map.json#sym:sample-system:web/src/hooks/useSettings.tsx:useSettings", "runs/run_01M6GDYQX07XECMVT06FY0Z6YF/ide-answers.md#a-16"]
endpoints:
  - id: EP-1
    method: GET
    path: /api/patients
    change: changed
    fields:
      - name: items[].lastVisitAt
        type: string
        nullable: true
        format: "ISO 8601 UTC instant (YYYY-MM-DDTHH:mm:ss.sssZ): the starts_at of the patient's most recent COMPLETED appointment; null when the patient has no COMPLETED appointment"
        example: "2026-03-09T19:30:00.000Z"
        status: known
    cites: ["context/generated/api-inventory.json#http:GET:/api/patients", "runs/run_01M6GDYQX07XECMVT06FY0Z6YF/ide-answers.md#a-12", "runs/run_01M6GDYQX07XECMVT06FY0Z6YF/ide-answers.md#a-14"]
  - id: EP-2
    method: GET
    path: /api/settings
    change: existing
    fields: []
    cites: ["context/generated/api-inventory.json#http:GET:/api/settings", "runs/run_01M6GDYQX07XECMVT06FY0Z6YF/ide-answers.md#a-16"]
answers:
  - id: A-14
    kind: question
    about: Q-01
    answer: "lastVisitAt, ISO UTC"
    decides: [EP-1, CO-1, CO-6]
    mismatch:
      text: "The list convention shows snake_case for the response wrapper (`page_size`), and the proposed default was `last_visit_at`. The user decided on the camelCase name `lastVisitAt`."
      cites: ["context/conventions.md#lists"]
  - id: A-15
    kind: question
    about: Q-02
    answer: "dateOnly(iso, timeZone)"
    decides: [CO-4, C4]
  - id: A-16
    kind: assumption
    about: C1
    answer: "Yes, confirm"
    decides: [C1, CO-7, EP-2]
  - id: A-17
    kind: assumption
    about: C2
    answer: "Yes, confirm"
    decides: [C2, CO-6]
---
# Design: HMS-4

## Overview
The API adds one field, `lastVisitAt`, to each item of the existing `GET /api/patients` list (EP-1). PatientRepo (CO-1) computes it as a correlated scalar subquery in the list SELECT: the max(starts_at) of the patient's appointments with status COMPLETED. The web shows it in a new Last visit column in PatientsPage (CO-5). A new `dateOnly(iso, timeZone)` helper (CO-4) formats it with dateTime's date options in the hospital time zone from useSettings (CO-7), without hour and minute. A null value shows "-".

Trade-offs:
- A scalar subquery, not a JOIN + GROUP BY: it keeps exactly one row per patient, so the existing paging, total, search and sort SQL stays untouched (S-3 AC1). The cost is one indexed lookup per listed patient, at most 20 per page, through appointment_patient_start_idx (C5).
- The API returns the UTC instant rather than a local date (C11, from impact A-12): the API stays zone-free, and the web formats in the hospital zone. The day boundary then depends on the web using the settings time zone and not the browser's (C1).
- `lastVisitAt` is camelCase, by the user's choice (A-14), even though the list wrapper uses snake_case `page_size` (C3). Implement must match how the existing item fields are mapped; the field name itself is fixed.
- The subquery adds no column to the patient SELECT, so no medical-history column enters the list (C6, S-5).

Story coverage:
- S-1 (date, time zone, format): CO-1, EP-1, CO-4, CO-5, CO-6, CO-7.
- S-2 (only COMPLETED; "-" when none): CO-1 (status filter), CO-5 (null shows "-").
- S-3 (paging, search, sort unchanged; no Last visit sort): CO-1 (scalar subquery, no join); CO-5 adds no sort option; CO-3 unchanged.
- S-4 (Next appointment unchanged): CO-5 leaves the Next appointment cell and its formatter untouched.
- S-5 (no medical history): CO-1 adds only lastVisitAt to the SELECT.

## Components
- CO-1 PatientRepo list query: **changed** (module:sample-system:api). Adds `(SELECT max(a.starts_at) FROM appointment a WHERE a.patient_id = p.id AND a.status = 'COMPLETED')` as lastVisitAt; WHERE, ORDER BY, LIMIT/OFFSET and count are unchanged.
- CO-2 PatientService: **existing**. Passes the field through (C8).
- CO-3 patientsRouter list handler: **existing**. No new parameters (C10).
- CO-4 `dateOnly(iso, timeZone)` in web/src/lib/format.ts: **new**. Same date options as dateTime, without hour and minute (C4).
- CO-5 PatientsPage: **changed**. Adds a Last visit column showing `dateOnly(lastVisitAt, timeZone)`, or "-" when null; no sort option is added.
- CO-6 web patient list item type: **changed**. Adds `lastVisitAt: string | null`; implement confirms the file (C2).
- CO-7 useSettings: **existing**. Supplies the hospital time zone (C1).
- EP-1 GET /api/patients: **changed**. New field `items[].lastVisitAt`: string, nullable, ISO 8601 UTC instant (YYYY-MM-DDTHH:mm:ss.sssZ), example `2026-03-09T19:30:00.000Z` (the S-1 AC2 instant, 01:00 Asia/Kolkata on 10 March), status known (A-14). The web formats that example as 10 March.
- EP-2 GET /api/settings: **existing**. Source of the hospital time zone.

## Known
- S-1: served by CO-1, CO-4, CO-5, CO-6, CO-7 and EP-1; the field format lets AC2 and AC3 check the exact instant and its Asia/Kolkata date (C9).
- S-2: served by the COMPLETED filter in CO-1 and the "-" for null in CO-5.
- S-3: served by the scalar subquery in CO-1, which keeps one row per patient, and by CO-5 adding no sort.
- S-4: served by CO-5, which leaves the Next appointment column unchanged.
- S-5: served by CO-1 selecting no medical-history column (C6).

## Assumed
None. C1 and C2 were confirmed in the IDE (A-16, A-17); C2 still asks implement to confirm the exact file.

## Unknown
None.

## Open questions
None open.

Answered in the IDE:
- A-14 (Q-01, field name and format): "lastVisitAt, ISO UTC". Decides EP-1, CO-1 and CO-6. Mismatch: the list convention and the proposed default used snake_case (`last_visit_at`); the user chose camelCase `lastVisitAt`.
- A-15 (Q-02, helper name): "dateOnly(iso, timeZone)". Decides CO-4 and C4. No mismatch.
- A-16 (C1, time zone from useSettings): "Yes, confirm". Decides C1, CO-7 and EP-2. No mismatch.
- A-17 (C2, where the list item type is declared): "Yes, confirm". Decides C2 and CO-6. No mismatch.
