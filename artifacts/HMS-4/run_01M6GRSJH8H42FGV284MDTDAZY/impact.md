---
schema: impact.v1
run_id: run_01M6GRSJH8H42FGV284MDTDAZY
step: impact
attempt: 2
skill: impact-analysis@1.3.0
inputs:
- path: artifacts/HMS-4/run_01M6GRSJH8H42FGV284MDTDAZY/req.md
  sha256: 46b4592f9dc5abdb3a91db9291a7078ecdbd2594a7c01a3cb7a5228916e0da85
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
  text: "The UI route `/patients` (declared in web/src/App.tsx) is the patient list, rendered by PatientsPage, which gets the Last visit column."
  cites: ["context/generated/api-inventory.json#ui:/patients", "context/generated/module-map.json#sym:sample-system:web/src/pages/PatientsPage.tsx:PatientsPage"]
- id: C2
  text: "The patient list route GET /api/patients is declared in `api/src/routes/patients.ts`; each list item gains the start instant of the patient's latest Completed appointment."
  cites: ["context/generated/api-inventory.json#http:GET:/api/patients", "runs/run_01M6GRSJH8H42FGV284MDTDAZY/ide-answers.md#a-05"]
- id: C3
  text: "Appointment instants are stored in `UTC`."
  cites: ["context/architecture.md#time-and-slots"]
- id: C4
  text: "The hospital time zone comes from `HOSPITAL_TZ`, default Asia/Kolkata."
  cites: ["context/architecture.md#time-and-slots"]
- id: C5
  text: "The web callers of GET /api/patients are PatientsPage and the patient picker in `BookAppointmentPage`; the new field is additive, so the picker is unchanged."
  cites: ["context/generated/module-map.json#sym:sample-system:web/src/pages/BookAppointmentPage.tsx:BookAppointmentPage", "context/generated/module-map.json#sym:sample-system:web/src/pages/PatientsPage.tsx:PatientsPage", "runs/run_01M6GRSJH8H42FGV284MDTDAZY/ide-answers.md#a-08"]
- id: C6
  text: "Next appointment is formatted with `dateTime` in web/src/lib/format.ts (a date and an hour:minute), and Last visit reuses it unchanged, as decided in the req step."
  cites: ["context/generated/module-map.json#sym:sample-system:web/src/lib/format.ts:dateTime", "runs/run_01M6GRSJH8H42FGV284MDTDAZY/ide-answers.md#a-02"]
- id: C7
  text: "The index `appointment_patient_start_idx` on the appointment table serves patient_id and starts_at, so no migration is needed; the COMPLETED status filter is applied on top of it."
  cites: ["context/generated/db-schema.json#index:appointment_patient_start_idx"]
- id: C8
  text: "Appointment status is SCHEDULED, `COMPLETED`, CANCELLED or NO_SHOW."
  cites: ["context/data-model.md#appointments"]
- id: C9
  text: "`Medical history` leaves the database only for the patient's own page, so the list query must not select it."
  cites: ["context/data-model.md#patients"]
- id: C10
  text: "The web app talks to the API through `web/src/api/client.ts`."
  cites: ["context/architecture.md#components"]
- id: C11
  text: "`No background jobs` exist yet, so no job reads or writes the data this change touches."
  cites: ["context/architecture.md#open-points"]
- id: C12
  text: "The API is layered route, service, repository; the patients route reaches the patient SQL through PatientService, and the list query in `PatientRepo` (api/src/repositories/patientRepo.ts) gains the last visit."
  cites: ["context/generated/module-map.json#sym:sample-system:api/src/repositories/patientRepo.ts:PatientRepo", "context/architecture.md#components"]
- id: C13
  text: "Impact analysis uses the context pack at `bd98ff08b83f8762c5e5c073ba5fadef88169b33` although it is behind sample-system; the implement step re-checks every touched file against current main."
  cites: ["artifacts/HMS-4/run_01M6GRSJH8H42FGV284MDTDAZY/req.md#L15", "runs/run_01M6GRSJH8H42FGV284MDTDAZY/ide-answers.md#a-06"]
- id: C14
  text: "This run builds its own branch from `main` and does not reuse draft PR #7 of the earlier HMS-4 run."
  cites: ["runs/run_01M6GRSJH8H42FGV284MDTDAZY/ide-answers.md#a-07"]
- id: C15
  text: "Last visit is read from `starts_at` of the patient's COMPLETED appointment with the latest start, returned as a UTC instant."
  cites: ["context/generated/db-schema.json#column:appointment.starts_at"]
- id: C16
  text: "`PatientService` (api/src/services/patientService.ts) sits between the patients route and PatientRepo and is changed at least to carry the new field; whether it also maps list items is open (Q-04)."
  cites: ["context/generated/module-map.json#sym:sample-system:api/src/services/patientService.ts:PatientService", "context/architecture.md#components"]
- id: C17
  text: "The web's patient list item type is reached through the `api` object in web/src/api/client.ts; where the item type is declared, and so whether client.ts changes, is open (Q-05)."
  cites: ["context/generated/module-map.json#sym:sample-system:web/src/api/client.ts:api"]
- id: C18
  text: "The existing web test helper `nextCell` in web/src/__tests__/PatientsPage.test.tsx reads PatientsPage table cells and is a regression reader for the unchanged Next appointment column."
  cites: ["context/generated/module-map.json#sym:sample-system:web/src/__tests__/PatientsPage.test.tsx:nextCell"]
trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10, REQ-11, REQ-12, REQ-13, REQ-14, REQ-15]
known: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10, REQ-11, REQ-12, REQ-13, REQ-14, REQ-15]
assumed: []
unknown:
- {id: U-1, blocking: false}
- {id: U-2, blocking: false}
questions:
- id: Q-04
  blocking: false
  ask: tech lead
  text: "Does PatientService only pass the new last-visit field from PatientRepo through to the patients route, or does it build the list item shape and need the field added to that mapping?"
  why: "Design must say which API files change; the facts list PatientService but show no call edges or bodies, and the local checkout is on PR #7's branch, not the pinned SHA."
  evidence: ["context/generated/module-map.json#sym:sample-system:api/src/services/patientService.ts:PatientService", "context/architecture.md#components", "runs/run_01M6GRSJH8H42FGV284MDTDAZY/ide-answers.md#a-09"]
  options: ["Pass-through only", "Service maps list items"]
  default: "Pass-through only; implement confirms against current main"
- id: Q-05
  blocking: false
  ask: tech lead
  text: "Where is the web's patient list item type declared: next to `api` in web/src/api/client.ts, or in PatientsPage.tsx?"
  why: "Design must name the file whose type gains the field; no sym fact declares the list item type."
  evidence: ["context/generated/module-map.json#sym:sample-system:web/src/api/client.ts:api", "context/generated/module-map.json#sym:sample-system:web/src/pages/PatientsPage.tsx:PatientsPage", "runs/run_01M6GRSJH8H42FGV284MDTDAZY/ide-answers.md#a-10"]
  options: ["In web/src/api/client.ts", "In PatientsPage.tsx"]
  default: "In web/src/api/client.ts; implement confirms against current main"
resolves:
- id: F-1
  how: "C12 now states the route, service, repository path, and C16 assesses PatientService as changed to carry the field, with its mapping role open in Q-04."
modules:
- id: module:sample-system:api
  change: changed
  cites: ["context/generated/module-map.json#module:sample-system:api", "context/generated/module-map.json#sym:sample-system:api/src/repositories/patientRepo.ts:PatientRepo", "context/generated/module-map.json#sym:sample-system:api/src/services/patientService.ts:PatientService", "runs/run_01M6GRSJH8H42FGV284MDTDAZY/ide-answers.md#a-05"]
- id: module:sample-system:web
  change: changed
  cites: ["context/generated/module-map.json#module:sample-system:web", "context/generated/module-map.json#sym:sample-system:web/src/pages/PatientsPage.tsx:PatientsPage", "context/generated/module-map.json#sym:sample-system:web/src/lib/format.ts:dateTime", "context/generated/module-map.json#sym:sample-system:web/src/api/client.ts:api"]
- id: module:sample-system:db
  change: existing
  cites: ["context/generated/module-map.json#module:sample-system:db", "context/generated/db-schema.json#index:appointment_patient_start_idx"]
apis:
- id: http:GET:/api/patients
  change: changed
  cites: ["context/generated/api-inventory.json#http:GET:/api/patients", "runs/run_01M6GRSJH8H42FGV284MDTDAZY/ide-answers.md#a-05"]
- id: http:POST:/api/appointments/:id/complete
  change: existing
  cites: ["context/generated/api-inventory.json#http:POST:/api/appointments/:id/complete"]
tables:
- id: table:appointment
  change: existing
  cites: ["context/generated/db-schema.json#table:appointment", "context/generated/db-schema.json#index:appointment_patient_start_idx"]
- id: table:patient
  change: existing
  cites: ["context/generated/db-schema.json#table:patient"]
answers:
- id: A-05
  kind: question
  about: Q-01
  answer: "Add to GET /api/patients (proposed default)"
  decides: [http:GET:/api/patients, module:sample-system:api, C2]
- id: A-06
  kind: question
  about: Q-02
  answer: "Proceed, re-check in implement (proposed default)"
  decides: [C13]
- id: A-07
  kind: question
  about: Q-03
  answer: "Build fresh from main (proposed default)"
  decides: [C14]
- id: A-08
  kind: assumption
  about: C5
  answer: "Yes, confirm"
  decides: [C5]
- id: A-09
  kind: question
  about: Q-04
  answer: "Not sure"
  decides: []
- id: A-10
  kind: question
  about: Q-05
  answer: "Not sure"
  decides: []
---
# Impact analysis: HMS-4

## Summary
HMS-4 adds a Last visit value to each item of the existing `GET /api/patients` list response and shows it as a new column on PatientsPage (`/patients`). Through the route, service and repository layers, PatientRepo's list query finds each patient's COMPLETED appointment with the latest starts_at, and PatientService carries that UTC instant to the route. The web formats it with the existing `dateTime` helper, exactly like Next appointment, so no new helper is needed. No migration: the appointment and patient tables are only read. The main risks are a join that repeats patient rows and breaks the 20-per-page paging, medical-history columns slipping into the list query, the COMPLETED filter not being covered by the index, two open file-placement questions (Q-04, Q-05), and a context pack that is 3 commits and 16 files behind sample-system.

## Impacted modules
Modules:
- module:sample-system:api: **changed**. PatientRepo's list query gains the last-visit instant; PatientService carries it to the patients route; the `GET /api/patients` list response gains the field (A-05, C2, C12, C16; mapping role open in Q-04).
- module:sample-system:web: **changed**. PatientsPage (`/patients`) gains the Last visit column, formatted with the existing `dateTime` (C1, C6). The list item type gains the field; its file is open in Q-05 (C17). The test helper nextCell in PatientsPage.test.tsx is a regression reader for the Next appointment column (C18).
- module:sample-system:db: **existing**. No migration; the existing index serves the lookup (C7).

APIs:
- http:GET:/api/patients: **changed**. Additive field per item; paging, search and sort unchanged (A-05, C2, C5).
- http:POST:/api/appointments/:id/complete: **existing**. Sets the COMPLETED status that Last visit reads; not changed (C8).

Tables:
- table:appointment: **existing**. Read by patient_id and starts_at through appointment_patient_start_idx, with the COMPLETED filter on top (C7, C15).
- table:patient: **existing**. Read as today; medical history columns stay out of the list query (C9).

## Resolves
- F-1: C12 now states the route, service, repository path, and C16 assesses PatientService as changed to carry the field, with its mapping role open in Q-04.

## Known
- REQ-1, REQ-12: PatientsPage renders `/patients` and gains the column; the Next appointment column is untouched, guarded by the existing nextCell test (C1, C18).
- REQ-2, REQ-3, REQ-4: the API reads COMPLETED appointments and returns the starts_at of the latest one (C8, C15).
- REQ-5, REQ-6: instants are stored in UTC; the web formats with `dateTime` in the hospital zone from HOSPITAL_TZ, exactly like Next appointment (C3, C4, C6).
- REQ-7, REQ-8: only COMPLETED appointments count; a patient without one gets no value and the page shows "-" (C8).
- REQ-9, REQ-10, REQ-11, REQ-15: paging, search and sort of `GET /api/patients` stay unchanged; the new field is additive and not a sort key (A-05, C2, C5).
- REQ-13: medical history leaves the database only for the patient's own page, so the list query must not select it (C9).
- REQ-14: no highlight or filter; no extra query or parameter (C2).
- No scheduled job is affected (C11). The web reaches the API through web/src/api/client.ts (C10).
- The pack drift is accepted for this step and re-checked in implement (A-06, C13). This run ignores draft PR #7 and builds fresh from main (A-07, C14).

## Assumed
None. C5 was confirmed in the IDE (A-08). C16 and C17 were put to the user, who was not sure; they are open questions Q-04 and Q-05, not assumptions.

## Unknown
- U-1 (non-blocking): does PatientService only pass the field through, or map list items? Raises Q-04.
- U-2 (non-blocking): where is the web's patient list item type declared? Raises Q-05.

## Open questions
- Q-04 (non-blocking, tech lead): Does PatientService only pass the new last-visit field through, or build the list item shape? Proposed default: pass-through only; implement confirms against current main.
- Q-05 (non-blocking, tech lead): Is the web's patient list item type declared next to `api` in web/src/api/client.ts or in PatientsPage.tsx? Proposed default: in web/src/api/client.ts; implement confirms against current main.

Answered in the IDE (this run, by gaurav-saini-truminds):
- A-05 (Q-01, where Last visit comes from): "Add to GET /api/patients (proposed default)". Decides http:GET:/api/patients, module:sample-system:api and C2. No mismatch.
- A-06 (Q-02, context pack drift E-CTX-10): "Proceed, re-check in implement (proposed default)". Decides C13. No mismatch.
- A-07 (Q-03, earlier draft PR #7 of run_01M66D77982JFTXDREQWB5QTJV): "Build fresh from main (proposed default)". Decides C14. No mismatch. PR #7 is to be closed outside this run.
- A-08 (C5, callers of GET /api/patients): "Yes, confirm". Decides C5. No mismatch; the facts have no call edges.
- A-09 (attempt 2, PatientService pass-through, now Q-04): "Not sure". Decides nothing; Q-04 stays open for the tech lead.
- A-10 (attempt 2, location of the web list item type, now Q-05): "Not sure". Decides nothing; Q-05 stays open for the tech lead.
