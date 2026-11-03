---
schema: design.v1
run_id: run_01M6GRSJH8H42FGV284MDTDAZY
step: design
attempt: 1
skill: design@1.7.0
inputs:
- path: artifacts/HMS-4/run_01M6GRSJH8H42FGV284MDTDAZY/impact.md
  sha256: 7ff5132a54c71433b4ab24b8444992f62cc2aacf0ca55668cb58a8b85768dbea
- path: artifacts/HMS-4/run_01M6GRSJH8H42FGV284MDTDAZY/split.md
  sha256: 1097c5a30f050a771deca3a06a8eb429ba26a1b31dd0aada1164d17b95a31518
- path: artifacts/HMS-4/run_01M6GRSJH8H42FGV284MDTDAZY/req.md
  sha256: 46b4592f9dc5abdb3a91db9291a7078ecdbd2594a7c01a3cb7a5228916e0da85
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
  text: "The patient list SQL lives in `PatientRepo` (api/src/repositories/patientRepo.ts); its list query gains the last-visit instant."
  cites: ["context/generated/module-map.json#sym:sample-system:api/src/repositories/patientRepo.ts:PatientRepo"]
- id: C2
  text: "`PatientService` (api/src/services/patientService.ts) sits between the patients route and PatientRepo and carries lastVisitAt; whether it only passes it through or maps list items is open, so implement must confirm."
  cites: ["context/generated/module-map.json#sym:sample-system:api/src/services/patientService.ts:PatientService", "runs/run_01M6GRSJH8H42FGV284MDTDAZY/ide-answers.md#a-09"]
- id: C3
  text: "GET /api/patients is served by `patientsRouter` in api/src/routes/patients.ts."
  cites: ["context/generated/module-map.json#sym:sample-system:api/src/routes/patients.ts:patientsRouter", "context/generated/api-inventory.json#http:GET:/api/patients"]
- id: C4
  text: "Last visit is formatted with the existing `dateTime` helper in web/src/lib/format.ts, with the same call and time zone as the Next appointment column, so it shows a date and an hour:minute."
  cites: ["context/generated/module-map.json#sym:sample-system:web/src/lib/format.ts:dateTime", "runs/run_01M6GRSJH8H42FGV284MDTDAZY/ide-answers.md#a-02"]
- id: C5
  text: "The patient list page is `PatientsPage` (web/src/pages/PatientsPage.tsx), which gains the Last visit column."
  cites: ["context/generated/module-map.json#sym:sample-system:web/src/pages/PatientsPage.tsx:PatientsPage"]
- id: C6
  text: "The web's patient list item type is assumed to sit next to the `api` object in web/src/api/client.ts and gains lastVisitAt; no sym fact declares the type, so implement must confirm."
  cites: ["context/generated/module-map.json#sym:sample-system:web/src/api/client.ts:api", "runs/run_01M6GRSJH8H42FGV284MDTDAZY/ide-answers.md#a-10"]
- id: C7
  text: "The existing index `appointment_patient_start_idx` serves the lookup by patient and start, so no migration is needed."
  cites: ["context/generated/db-schema.json#index:appointment_patient_start_idx"]
- id: C8
  text: "Last visit is read from `starts_at` of the patient's latest COMPLETED appointment."
  cites: ["context/generated/db-schema.json#column:appointment.starts_at"]
- id: C9
  text: "Instants are stored in `UTC`."
  cites: ["context/architecture.md#time-and-slots"]
- id: C10
  text: "Appointment status values include `COMPLETED`; only that status counts as a visit."
  cites: ["context/data-model.md#appointments"]
- id: C11
  text: "`Medical history` leaves the database only for the patient's own page, so the list query adds no medical-history column."
  cites: ["context/data-model.md#patients"]
- id: C12
  text: "The API is layered: routes parse input, services hold the rules, `repositories hold the SQL`."
  cites: ["context/architecture.md#components"]
- id: C13
  text: "Each item of GET /api/patients carries `lastVisitAt`: an ISO 8601 UTC instant string, or null when the patient has no Completed appointment."
  cites: ["runs/run_01M6GRSJH8H42FGV284MDTDAZY/ide-answers.md#a-11"]
- id: C14
  text: "The web callers of GET /api/patients are PatientsPage and the patient picker in `BookAppointmentPage`; the picker ignores the additive field."
  cites: ["context/generated/module-map.json#sym:sample-system:web/src/pages/BookAppointmentPage.tsx:BookAppointmentPage", "runs/run_01M6GRSJH8H42FGV284MDTDAZY/ide-answers.md#a-08"]
trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10, REQ-11, REQ-12, REQ-13, REQ-14, REQ-15]
known: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10, REQ-11, REQ-12, REQ-13, REQ-14, REQ-15]
assumed: [CO-2, CO-6]
unknown: []
questions: []
components:
- id: CO-1
  name: PatientRepo list query (adds lastVisitAt per patient)
  module: module:sample-system:api
  change: changed
  cites: ["context/generated/module-map.json#sym:sample-system:api/src/repositories/patientRepo.ts:PatientRepo", "context/generated/module-map.json#module:sample-system:api"]
- id: CO-2
  name: PatientService list (carries lastVisitAt)
  module: module:sample-system:api
  change: changed
  cites: ["context/generated/module-map.json#sym:sample-system:api/src/services/patientService.ts:PatientService"]
- id: CO-3
  name: patientsRouter GET /api/patients (returns lastVisitAt per item)
  module: module:sample-system:api
  change: changed
  cites: ["context/generated/module-map.json#sym:sample-system:api/src/routes/patients.ts:patientsRouter", "runs/run_01M6GRSJH8H42FGV284MDTDAZY/ide-answers.md#a-11"]
- id: CO-4
  name: PatientsPage Last visit column
  module: module:sample-system:web
  change: changed
  cites: ["context/generated/module-map.json#sym:sample-system:web/src/pages/PatientsPage.tsx:PatientsPage", "runs/run_01M6GRSJH8H42FGV284MDTDAZY/ide-answers.md#a-11"]
- id: CO-5
  name: dateTime helper (reused unchanged)
  module: module:sample-system:web
  change: existing
  cites: ["context/generated/module-map.json#sym:sample-system:web/src/lib/format.ts:dateTime"]
- id: CO-6
  name: Patient list item type (gains lastVisitAt)
  module: module:sample-system:web
  change: changed
  cites: ["context/generated/module-map.json#sym:sample-system:web/src/api/client.ts:api", "runs/run_01M6GRSJH8H42FGV284MDTDAZY/ide-answers.md#a-11"]
- id: CO-7
  name: appointment table and appointment_patient_start_idx (read only)
  module: module:sample-system:db
  change: existing
  cites: ["context/generated/module-map.json#module:sample-system:db", "context/generated/db-schema.json#index:appointment_patient_start_idx"]
endpoints:
- id: EP-1
  method: GET
  path: /api/patients
  change: changed
  fields:
  - name: items[].lastVisitAt
    type: string
    nullable: true
    format: "ISO 8601 UTC instant (starts_at of the patient's COMPLETED appointment with the latest starts_at); null when the patient has none"
    example: "2026-03-09T19:30:00.000Z"
    status: known
  cites: ["context/generated/api-inventory.json#http:GET:/api/patients", "runs/run_01M6GRSJH8H42FGV284MDTDAZY/ide-answers.md#a-05", "runs/run_01M6GRSJH8H42FGV284MDTDAZY/ide-answers.md#a-11"]
answers:
- id: A-11
  kind: question
  about: Q-01
  answer: "lastVisitAt, ISO UTC (proposed default)"
  decides: [EP-1, CO-3, CO-4, CO-6, C13]
---
# Design: HMS-4

## Overview
The patient list gains a Last visit column by adding one nullable field, `lastVisitAt`, to each item of the existing `GET /api/patients` response (EP-1). PatientRepo's list query (CO-1) computes, per patient on the current page, the starts_at of that patient's COMPLETED appointment with the latest starts_at. PatientService (CO-2) carries it, and patientsRouter (CO-3) returns it as an ISO 8601 UTC string, or null. PatientsPage (CO-4) shows it with the existing `dateTime` helper (CO-5), the same call and time zone as Next appointment, and shows "-" when it is null. The web list item type (CO-6) gains the field. No migration: the appointment table is only read, through appointment_patient_start_idx (CO-7).

Trade-offs:
- Per-patient correlated subquery (or LATERAL with LIMIT 1) instead of a JOIN plus GROUP BY: one row per patient by construction, so the 20-per-page paging and search can't repeat rows (S-3 AC1, AC2). The cost is one index probe per listed patient, bounded by the page size.
- Return the instant, not a formatted string: the web already formats instants with `dateTime` in the hospital zone, so Last visit matches Next appointment exactly and the API stays format-free.
- `lastVisitAt` is not added to the sort keys, so the current sort order and options are unchanged (REQ-11, REQ-15).

## Components
- CO-1 PatientRepo list query (module:sample-system:api): **changed**. Adds lastVisitAt per patient via a per-patient subquery on COMPLETED appointments; selects no medical-history column (C1, C10, C11).
- CO-2 PatientService list (module:sample-system:api): **changed**, assumed. Carries lastVisitAt from CO-1 to CO-3; whether it also maps list items is unconfirmed, and implement must confirm (C2).
- CO-3 patientsRouter GET /api/patients (module:sample-system:api): **changed**. Returns lastVisitAt in each list item; paging, search and sort parameters unchanged (C3, A-11).
- CO-4 PatientsPage Last visit column (module:sample-system:web): **changed**. New column "Last visit" rendered as `dateTime(lastVisitAt, <the time zone Next appointment uses>)`, or "-" when null; not sortable; Next appointment column untouched (C4, C5, A-11).
- CO-5 dateTime helper (module:sample-system:web): **existing**. Reused unchanged (C4).
- CO-6 Patient list item type (module:sample-system:web): **changed**, assumed. Gains `lastVisitAt: string | null`; assumed next to `api` in web/src/api/client.ts, and implement must confirm (C6, A-11).
- CO-7 appointment table and appointment_patient_start_idx (module:sample-system:db): **existing**. Read only; no migration (C7, C8).
- EP-1 GET /api/patients: **changed**. Fields:
  - `items[].lastVisitAt`: string, nullable; ISO 8601 UTC instant (starts_at of the patient's COMPLETED appointment with the latest starts_at), null when none; example `2026-03-09T19:30:00.000Z` (01:00 Asia/Kolkata on 10 March); status known (C13, A-11).

Story coverage:
- S-1 (last visit shown, latest start, time zone, format, no highlight): CO-1, CO-2, CO-3, CO-4, CO-5, CO-6, EP-1.
- S-2 (only Completed counts, "-" otherwise): CO-1 (COMPLETED filter), CO-4 ("-" for null).
- S-3 (paging, search, sort unchanged, not sortable): CO-1 (one row per patient), CO-3 (parameters unchanged), CO-4 (no sort on the column).
- S-4 (Next appointment unchanged): CO-4 leaves that column and its data untouched.
- S-5 (no medical history): CO-1 selects no medical-history column (C11).

## Known
- REQ-1 to REQ-15: served by CO-1 to CO-7 and EP-1 as mapped above; the stories S-1 to S-5 trace them.
- EP-1's field is fixed by A-11 (C13).
- C14: the booking picker in BookAppointmentPage also calls GET /api/patients and ignores the additive field (A-08).

## Assumed
- CO-2: PatientService carries lastVisitAt from the repo to the route. Basis: the architecture puts services between routes and repositories (C12), and the user was not sure whether it maps list items (A-09). Implement must confirm against current main.
- CO-6: the patient list item type sits next to `api` in web/src/api/client.ts. Basis: the web reaches the API through client.ts and no sym fact declares the type; the user was not sure (A-10). Implement must confirm against current main.

## Unknown
None.

## Open questions
None open. Impact's Q-04 and Q-05 are carried here as the assumptions on CO-2 and CO-6, for implement to confirm.

Answered in the IDE:
- A-11 (Q-01, the field each GET /api/patients item carries): "lastVisitAt, ISO UTC (proposed default)". Decides EP-1, CO-3, CO-4, CO-6 and C13. No mismatch.
