---
schema: impact.v1
run_id: run_01M79P1N889PBTFM8J1EX89TBK
step: impact
attempt: 1
skill: impact-analysis@1.4.0
inputs:
  - {path: artifacts/HMS-5/run_01M79P1N889PBTFM8J1EX89TBK/req.md, sha256: a03c0b2820af5070279380915fbacc62e5ebb19c702b593d4bd9e20385a7a8f6}
  - {path: context/generated/module-map.json, sha256: 4158c8fcd239c22cb8f1029dd11f61d8839867ff8088bd9ab76197e2db96db4b}
  - {path: context/generated/api-inventory.json, sha256: ba024ab1b4b4151d26ce1afa77bd2dab1cec645ea737ce37f98e0e8674f795e9}
  - {path: context/generated/db-schema.json, sha256: e2a9d2990f6618ea5d55994a16c6264b0186b851cc3216014b812c4a4b98908a}
  - {path: context/architecture.md, sha256: 9faab093d10933a46f6473b25c80728f8745e597100e650cd2537bbafb2b595a}
  - {path: context/data-model.md, sha256: 2386ae151d54a73cdbe4fd9e0b69f8e8539c2870f5e52c12e5ed0191cfa95e55}
pack_sha: {sample-system: 7681f87749883d71abb4011d118366ca1242e1cc}
claims:
  - {id: C1, text: "The patient detail route is declared in `api/src/routes/patients.ts`.", cites: ["context/generated/api-inventory.json#http:GET:/api/patients/:id"]}
  - {id: C2, text: "The UI route for the patient page is declared in `web/src/App.tsx`.", cites: ["context/generated/api-inventory.json#ui:/patients/:id"]}
  - {id: C3, text: "The UI route for the patient page renders `PatientDetailPage`.", cites: ["sample-system:web/src/App.tsx#L32"]}
  - {id: C4, text: "PatientDetailPage loads the patient with `api.patient(id)`.", cites: ["sample-system:web/src/pages/PatientDetailPage.tsx#L18"]}
  - {id: C5, text: "The patient page shows medical history and then the appointments list through `AppointmentTable`.", cites: ["sample-system:web/src/pages/PatientDetailPage.tsx#L76"]}
  - {id: C6, text: "The appointments loaded for the patient page are capped by a default `limit = 100`, so counting them in the browser undercounts all-time totals.", cites: ["sample-system:api/src/repositories/patientRepo.ts#L180-L300"]}
  - {id: C7, text: "PatientService.get builds the detail with `repo.appointments(id, this.clock())`.", cites: ["sample-system:api/src/services/patientService.ts#L42"]}
  - {id: C8, text: "The API's patient detail type is `PatientDetail` in api/src/types.ts.", cites: ["context/generated/module-map.json#sym:sample-system:api/src/types.ts:PatientDetail"]}
  - {id: C9, text: "The web's patient detail type is `PatientDetail` in web/src/api/types.ts.", cites: ["context/generated/module-map.json#sym:sample-system:web/src/api/types.ts:PatientDetail"]}
  - {id: C10, text: "The web client fetches the patient detail as `send<PatientDetail>`.", cites: ["sample-system:web/src/api/client.ts#L71"]}
  - {id: C11, text: "The web app talks to the API only through `web/src/api/client.ts`.", cites: ["context/architecture.md#components"]}
  - {id: C12, text: "PatientFormPage also calls `api.patient(id)` when editing.", cites: ["sample-system:web/src/pages/PatientFormPage.tsx#L51"]}
  - {id: C13, text: "BookAppointmentPage also calls `api.patient(patientId)` to label a preselected patient.", cites: ["sample-system:web/src/pages/BookAppointmentPage.tsx#L33"]}
  - {id: C14, text: "Creating a patient returns the detail from `this.get`, so POST /api/patients responses carry any new detail field.", cites: ["sample-system:api/src/services/patientService.ts#L58"]}
  - {id: C15, text: "Updating a patient returns `this.get(id)`, so PATCH /api/patients/:id responses carry any new detail field.", cites: ["sample-system:api/src/services/patientService.ts#L69"]}
  - {id: C16, text: "Appointment status is one of SCHEDULED, COMPLETED, CANCELLED or `NO_SHOW`.", cites: ["context/data-model.md#appointments"]}
  - {id: C17, text: "Index `appointment_patient_start_idx` exists on the appointment table, led by patient_id.", cites: ["context/generated/db-schema.json#index:appointment_patient_start_idx"]}
  - {id: C18, text: "Medical history leaves the database only for the `patient's own page`.", cites: ["context/data-model.md#patients"]}
  - {id: C19, text: "`No background jobs` exist yet.", cites: ["context/architecture.md#open-points"]}
  - {id: C20, text: "The user chose to `Add to GET /api/patients/:id` for the two counts.", cites: ["runs/run_01M79P1N889PBTFM8J1EX89TBK/ide-answers.md#a-02"]}
  - {id: C21, text: "Fields of the patient detail type are camelCase, for example `medicalHistory`, so the counts default to a camelCase field on it.", cites: ["sample-system:api/src/types.ts#L104-L117", "runs/run_01M79P1N889PBTFM8J1EX89TBK/ide-answers.md#a-02"]}
trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9]
known: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9]
assumed: []
unknown: []
questions: []
modules:
  - {id: "module:sample-system:api", change: changed, cites: ["context/generated/module-map.json#module:sample-system:api", "sample-system:api/src/services/patientService.ts#L42", "runs/run_01M79P1N889PBTFM8J1EX89TBK/ide-answers.md#a-02"]}
  - {id: "module:sample-system:web", change: changed, cites: ["context/generated/module-map.json#module:sample-system:web", "sample-system:web/src/pages/PatientDetailPage.tsx#L76"]}
  - {id: "module:sample-system:db", change: existing, cites: ["context/generated/module-map.json#module:sample-system:db", "context/generated/db-schema.json#index:appointment_patient_start_idx"]}
apis:
  - {id: "http:GET:/api/patients/:id", change: changed, cites: ["context/generated/api-inventory.json#http:GET:/api/patients/:id", "runs/run_01M79P1N889PBTFM8J1EX89TBK/ide-answers.md#a-02"]}
  - {id: "http:POST:/api/patients", change: changed, cites: ["context/generated/api-inventory.json#http:POST:/api/patients", "sample-system:api/src/services/patientService.ts#L58"]}
  - {id: "http:PATCH:/api/patients/:id", change: changed, cites: ["context/generated/api-inventory.json#http:PATCH:/api/patients/:id", "sample-system:api/src/services/patientService.ts#L69"]}
tables:
  - {id: "table:appointment", change: existing, cites: ["context/generated/db-schema.json#table:appointment", "context/data-model.md#appointments"]}
  - {id: "table:patient", change: existing, cites: ["context/generated/db-schema.json#table:patient", "context/data-model.md#patients"]}
answers:
  - {id: A-02, kind: question, about: Q-01, answer: "Add to GET /api/patients/:id (proposed default)", decides: ["http:GET:/api/patients/:id", "module:sample-system:api", C20, C21]}
---
# Impact analysis: HMS-5

## Summary
The Visit summary is new UI on the patient page, which the `/patients/:id` route renders as `PatientDetailPage` (C3). The counts cannot come from the appointments the page already loads, because that list is capped by a default limit of 100 (C6). As you chose (A-02), `GET /api/patients/:id` gains two server-side counts, and the API and web `PatientDetail` types gain a field (C8, C9). Create and update return the same detail (C14, C15), so their responses gain the field too, and the other callers of `api.patient` see an extra field they ignore (C12, C13). No table, column or migration changes: the count reads `appointment` by patient and status (C16, C17), and medical history stays on the patient's own page (C18).

## Impacted modules
- module:sample-system:api: changed. PatientService.get and PatientRepo gain the Completed and No-show counts; the `PatientDetail` type gains a field.
- module:sample-system:web: changed. PatientDetailPage shows the Visit summary above the appointments list; the web `PatientDetail` type gains the field.
- module:sample-system:db: existing. Nothing changes; the appointment table and its patient index are only read.
- http:GET:/api/patients/:id: changed. Its response gains the counts (additive).
- http:POST:/api/patients: changed. Its response is built by the same `get`, so it carries the counts (additive).
- http:PATCH:/api/patients/:id: changed. Same reason as POST (additive).
- table:appointment: existing. Read for counts by patient and status.
- table:patient: existing. Read as today; medical history is unchanged.

## Known
- REQ-1, REQ-7, REQ-8, REQ-9: the page is PatientDetailPage (C3). The Visit summary goes above the existing `AppointmentTable` (C5), and the medical history and appointments list stay as they are.
- REQ-2, REQ-3, REQ-4: counts by status COMPLETED and NO_SHOW on table:appointment (C16). SCHEDULED and CANCELLED are not counted.
- REQ-5: a patient with no matching rows gets zero counts. Because the count is done on the server, this also covers patients with no appointments.
- REQ-6: all time, so the count must not use the capped appointments list (C6). It goes in GET /api/patients/:id (A-02).
- Naming default: the counts get a camelCase field on `PatientDetail`, such as `visitSummary` with `completed` and `noShow`, following fields like `medicalHistory` (C21). Trade-off: this keeps the convention, but design may still rename it.
- Callers found by searching web/src: PatientDetailPage, PatientFormPage and BookAppointmentPage call `api.patient` (C4, C12, C13). All of them go through web/src/api/client.ts (C10, C11), and the new field is additive for them.
- No scheduled jobs are affected (C19).

## Assumed
None.

## Unknown
None.

## Open questions
None open.

Answered in the IDE:
- A-02 (Q-01, answered by the user, eng-dbe4): where the patient page gets the two counts, given the capped appointments list. Answer: "Add to GET /api/patients/:id (proposed default)". Decides http:GET:/api/patients/:id, module:sample-system:api, C20 and C21. No mismatch with the facts.
