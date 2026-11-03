---
schema: impact.v1
run_id: run_01M6GDYQX07XECMVT06FY0Z6YF
step: impact
attempt: 2
skill: impact-analysis@1.2.0
inputs:
  - path: artifacts/HMS-4/run_01M6GDYQX07XECMVT06FY0Z6YF/req.md
    sha256: 7092a382066b6eb79dfc22e522257e2a7c5679697745e27c13b84a0c491fcffd
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
    text: "The UI route `/patients` is rendered by `PatientsPage` in web/src/pages/PatientsPage.tsx, which gets the Last visit column."
    cites: ["context/generated/api-inventory.json#ui:/patients", "context/generated/module-map.json#sym:sample-system:web/src/pages/PatientsPage.tsx:PatientsPage", "runs/run_01M6GDYQX07XECMVT06FY0Z6YF/ide-answers.md#a-09"]
  - id: C2
    text: "The API returns the last visit as the Completed appointment's `starts_at` instant (stored in UTC). The hospital time zone (HOSPITAL_TZ, default Asia/Kolkata) is applied when the web formats the date."
    cites: ["context/generated/db-schema.json#column:appointment.starts_at", "context/architecture.md#time-and-slots", "runs/run_01M6GDYQX07XECMVT06FY0Z6YF/ide-answers.md#a-10", "runs/run_01M6GDYQX07XECMVT06FY0Z6YF/ide-answers.md#a-12"]
  - id: C3
    text: "A new date-only helper next to `dateTime` in web/src/lib/format.ts formats Last visit with dateTime's date options in the hospital time zone, without hour and minute, so its date style matches the Next appointment column."
    cites: ["context/generated/module-map.json#sym:sample-system:web/src/lib/format.ts:dateTime", "runs/run_01M6GDYQX07XECMVT06FY0Z6YF/ide-answers.md#a-11", "runs/run_01M6GDYQX07XECMVT06FY0Z6YF/ide-answers.md#a-12"]
  - id: C4
    text: "The patient list route GET /api/patients is declared in `api/src/routes/patients.ts`."
    cites: ["context/generated/api-inventory.json#http:GET:/api/patients"]
  - id: C5
    text: "`Medical history` leaves the database only for the patient's own page."
    cites: ["context/data-model.md#patients"]
  - id: C6
    text: "Appointment instants are stored in `UTC`."
    cites: ["context/architecture.md#time-and-slots", "context/data-model.md#appointments"]
  - id: C7
    text: "The index appointment_patient_start_idx on the appointment table serves patient_id and `starts_at`. The status filter (COMPLETED) is applied on top of it and is not covered by the index; the implement step checks the query cost."
    cites: ["context/generated/db-schema.json#index:appointment_patient_start_idx", "runs/run_01M6GDYQX07XECMVT06FY0Z6YF/ide-answers.md#a-06"]
  - id: C8
    text: "Appointment status is SCHEDULED, `COMPLETED`, CANCELLED or NO_SHOW."
    cites: ["context/data-model.md#appointments"]
  - id: C9
    text: "The web app talks to the API only through `web/src/api/client.ts`."
    cites: ["context/architecture.md#components"]
  - id: C10
    text: "`No background jobs` exist yet, so no job reads or writes the data this change touches."
    cites: ["context/architecture.md#open-points"]
  - id: C11
    text: "Patient SQL lives in `PatientRepo` (api/src/repositories/patientRepo.ts); routes, services and repositories are separate layers."
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/repositories/patientRepo.ts:PatientRepo", "context/architecture.md#components"]
  - id: C12
    text: "The new field in the `GET /api/patients` response is additive, so the other callers (the patient pickers named in A-07) stay unchanged."
    cites: ["context/generated/api-inventory.json#http:GET:/api/patients", "runs/run_01M6GDYQX07XECMVT06FY0Z6YF/ide-answers.md#a-07"]
  - id: C13
    text: "Impact analysis uses the context pack at `bd98ff08b83f8762c5e5c073ba5fadef88169b33` even though it is behind sample-system; the implement step re-checks against the current code."
    cites: ["context/manifest.json#sources", "runs/run_01M6GDYQX07XECMVT06FY0Z6YF/ide-answers.md#a-08"]
  - id: C14
    text: "`PatientService` (api/src/services/patientService.ts) only passes the new lastVisit field through from PatientRepo to the route; it adds no rule."
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/services/patientService.ts:PatientService", "runs/run_01M6GDYQX07XECMVT06FY0Z6YF/ide-answers.md#a-13"]
trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10, REQ-11, REQ-12]
known: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10, REQ-11, REQ-12]
assumed: []
unknown: []
questions: []
resolves:
  - id: F-1
    how: "A-12 fixes the contract: the API returns the starts_at instant, and a new date-only helper in format.ts formats it in the hospital zone (C2, C3)."
modules:
  - id: module:sample-system:api
    change: changed
    cites: ["context/generated/module-map.json#module:sample-system:api", "context/generated/module-map.json#sym:sample-system:api/src/repositories/patientRepo.ts:PatientRepo", "context/generated/module-map.json#sym:sample-system:api/src/services/patientService.ts:PatientService", "runs/run_01M6GDYQX07XECMVT06FY0Z6YF/ide-answers.md#a-05", "runs/run_01M6GDYQX07XECMVT06FY0Z6YF/ide-answers.md#a-13"]
  - id: module:sample-system:web
    change: changed
    cites: ["context/generated/module-map.json#module:sample-system:web", "context/generated/module-map.json#sym:sample-system:web/src/pages/PatientsPage.tsx:PatientsPage", "context/generated/module-map.json#sym:sample-system:web/src/lib/format.ts:dateTime", "runs/run_01M6GDYQX07XECMVT06FY0Z6YF/ide-answers.md#a-09", "runs/run_01M6GDYQX07XECMVT06FY0Z6YF/ide-answers.md#a-12"]
  - id: module:sample-system:db
    change: existing
    cites: ["context/generated/module-map.json#module:sample-system:db", "runs/run_01M6GDYQX07XECMVT06FY0Z6YF/ide-answers.md#a-06"]
apis:
  - id: http:GET:/api/patients
    change: changed
    cites: ["context/generated/api-inventory.json#http:GET:/api/patients", "runs/run_01M6GDYQX07XECMVT06FY0Z6YF/ide-answers.md#a-05", "runs/run_01M6GDYQX07XECMVT06FY0Z6YF/ide-answers.md#a-12"]
  - id: http:POST:/api/appointments/:id/complete
    change: existing
    cites: ["context/generated/api-inventory.json#http:POST:/api/appointments/:id/complete"]
tables:
  - id: table:appointment
    change: existing
    cites: ["context/generated/db-schema.json#table:appointment", "context/generated/db-schema.json#index:appointment_patient_start_idx", "runs/run_01M6GDYQX07XECMVT06FY0Z6YF/ide-answers.md#a-06"]
  - id: table:patient
    change: existing
    cites: ["context/generated/db-schema.json#table:patient", "runs/run_01M6GDYQX07XECMVT06FY0Z6YF/ide-answers.md#a-06"]
answers:
  - id: A-05
    kind: question
    about: Q-01
    answer: "Add to GET /api/patients (proposed default)"
    decides: [http:GET:/api/patients, module:sample-system:api]
  - id: A-06
    kind: question
    about: Q-02
    answer: "No migration (proposed default)"
    decides: [table:appointment, table:patient, module:sample-system:db, C7]
  - id: A-07
    kind: question
    about: Q-03
    answer: "PatientsPage + pickers (proposed default)"
    decides: [C12]
  - id: A-08
    kind: question
    about: Q-04
    answer: "Proceed, re-check in implement (proposed default)"
    decides: [C13]
  - id: A-09
    kind: assumption
    about: C1
    answer: "Yes, confirm"
    decides: [C1, module:sample-system:web]
  - id: A-10
    kind: assumption
    about: C2
    answer: "Yes, confirm"
    decides: [C2]
  - id: A-11
    kind: assumption
    about: C3
    answer: "Yes, confirm"
    decides: [C3]
  - id: A-12
    kind: question
    about: Q-05
    answer: "Instant + date-only helper (proposed default)"
    decides: [C2, C3, http:GET:/api/patients, module:sample-system:web]
    mismatch:
      text: "A-10 confirmed that the API works out the local date, and A-11 confirmed reusing dateTime unchanged. The user decided that the API returns the starts_at instant and a new date-only helper next to dateTime formats it in the hospital zone."
      cites: ["runs/run_01M6GDYQX07XECMVT06FY0Z6YF/ide-answers.md#a-10", "runs/run_01M6GDYQX07XECMVT06FY0Z6YF/ide-answers.md#a-11"]
  - id: A-13
    kind: assumption
    about: C14
    answer: "Yes, confirm"
    decides: [C14]
---
# Impact analysis: HMS-4

## Summary
HMS-4 adds a Last visit value to each patient in the existing `GET /api/patients` list response and shows it as a new column in PatientsPage. The API (module:sample-system:api) finds each patient's most recent COMPLETED appointment and returns its starts_at instant. A new date-only helper next to `dateTime` in web/src/lib/format.ts (module:sample-system:web) turns that instant into a date in the hospital time zone, with no time (A-12). There is no migration: the appointment and patient tables are only read. The main risks are a join that repeats patient rows and breaks the 20-per-page paging, getting the wrong day by formatting outside Asia/Kolkata, the status filter not being covered by the existing index, and medical-history columns slipping into the list query. The context pack is also behind sample-system (E-CTX-10), so the implement step must re-check against the current code.

## Impacted modules
- module:sample-system:api: **changed**. The patient list query in PatientRepo and the list response of `GET /api/patients` gain the last-visit instant. PatientService only passes it through (A-05, C14).
- module:sample-system:web: **changed**. PatientsPage (`/patients`) gains the Last visit column. A new date-only helper sits next to `dateTime` in web/src/lib/format.ts (A-09, A-12).
- module:sample-system:db: **existing**. No migration (A-06).
- http:GET:/api/patients: **changed**. Additive field (the starts_at instant of the most recent Completed appointment, or none); paging, search and sort stay unchanged (A-05, A-12, C12).
- http:POST:/api/appointments/:id/complete: **existing**. Sets the COMPLETED status that Last visit reads; not changed.
- table:appointment: **existing**. Read by patient_id and starts_at through index appointment_patient_start_idx, with the COMPLETED status filtered on top; the index does not cover status (C7, A-06).
- table:patient: **existing**. Read as today; medical history columns stay out of the list query (C5).

## Resolves
- F-1: A-12 fixes the contract: the API returns the starts_at instant, and a new date-only helper in format.ts formats it in the hospital zone (C2, C3).

## Known
- REQ-1, REQ-10, REQ-11: the web change in PatientsPage, display only, and the Next appointment column untouched (C1).
- REQ-4: the date-only helper reuses dateTime's date options, so the date style matches Next appointment, without the time (C3, A-12).
- REQ-2, REQ-5, REQ-6: the API reads only COMPLETED appointments from table:appointment, returns the most recent starts_at, and returns no value when there is none (C2, C8).
- REQ-3: instants are stored in UTC, and the hospital zone (HOSPITAL_TZ, Asia/Kolkata) is applied when the date is formatted (C2, C6).
- REQ-7, REQ-8, REQ-9: the paging, search and sort of `GET /api/patients` stay unchanged; the new value is additive (A-05, C12).
- REQ-12: medical history leaves the database only for the patient's own page, so the list query must not select it (C5).
- No scheduled job is affected: the architecture says no background jobs exist yet (C10).
- The web reaches the API only through web/src/api/client.ts (C9). PatientService passes the field through (C14).

## Assumed
None. C1, C2, C3 and C14 were confirmed in the IDE (A-09, A-10, A-11, A-13). C2 and C3 are now worded as A-12 decided.

## Unknown
None.

## Open questions
None open.

Answered in the IDE:
- A-05 (Q-01, where Last visit comes from): "Add to GET /api/patients (proposed default)". Answered by the user. Decides http:GET:/api/patients and module:sample-system:api (changed). No mismatch.
- A-06 (Q-02, database change): "No migration (proposed default)". Answered by the user. Decides table:appointment, table:patient, module:sample-system:db (existing) and C7. No mismatch.
- A-07 (Q-03, callers of GET /api/patients): "PatientsPage + pickers (proposed default)". Answered by the user. Decides C12. No mismatch; the facts have no call edges.
- A-08 (Q-04, context pack drift E-CTX-10): "Proceed, re-check in implement (proposed default)". Answered by the user. Decides C13. No mismatch.
- A-09 (C1, PatientsPage renders /patients): "Yes, confirm". Answered by the user. Decides C1 and module:sample-system:web. No mismatch.
- A-10 (C2, the API uses the hospital time zone): "Yes, confirm". Answered by the user. Decides C2, now worded as A-12 decided.
- A-11 (C3, Next appointment uses dateTime): "Yes, confirm". Answered by the user. Decides C3, now worded as A-12 decided.
- A-12 (Q-05, how Last visit goes from the API to the page): "Instant + date-only helper (proposed default)". Answered by the user. Decides C2, C3, http:GET:/api/patients and module:sample-system:web. Mismatch: A-10 had the API work out the local date and A-11 reused dateTime unchanged; the user decided that the API returns the starts_at instant and a new date-only helper next to dateTime formats it in the hospital zone.
- A-13 (C14, PatientService passes the field through): "Yes, confirm". Answered by the user. Decides C14. No mismatch.
