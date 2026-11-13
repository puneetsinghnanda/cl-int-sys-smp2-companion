---
schema: design.v1
run_id: run_01M79P1N889PBTFM8J1EX89TBK
step: design
attempt: 1
skill: design@1.8.0
inputs:
  - {path: artifacts/HMS-5/run_01M79P1N889PBTFM8J1EX89TBK/impact.md, sha256: b10e7cb73c12bbff7abeae12f23ba3794012ee5148e168718f14935a18f1732d}
  - {path: artifacts/HMS-5/run_01M79P1N889PBTFM8J1EX89TBK/split.md, sha256: db8ef532a356ccb24887a9fadcb6dc8a8fbc5338ae76d37cb71b224324490de8}
  - {path: artifacts/HMS-5/run_01M79P1N889PBTFM8J1EX89TBK/req.md, sha256: a03c0b2820af5070279380915fbacc62e5ebb19c702b593d4bd9e20385a7a8f6}
  - {path: context/intent-baseline.md, sha256: 9bfd821aeb2fb9524863e346707e049e4fcfb949a6e55b90223a79194946a215}
  - {path: context/architecture.md, sha256: 9faab093d10933a46f6473b25c80728f8745e597100e650cd2537bbafb2b595a}
  - {path: context/generated/module-map.json, sha256: 4158c8fcd239c22cb8f1029dd11f61d8839867ff8088bd9ab76197e2db96db4b}
  - {path: context/generated/api-inventory.json, sha256: ba024ab1b4b4151d26ce1afa77bd2dab1cec645ea737ce37f98e0e8674f795e9}
pack_sha: {sample-system: 7681f87749883d71abb4011d118366ca1242e1cc}
claims:
  - {id: C1, text: "Patient SQL lives in the `PatientRepo` class.", cites: ["context/generated/module-map.json#sym:sample-system:api/src/repositories/patientRepo.ts:PatientRepo"]}
  - {id: C2, text: "Patient rules live in the `PatientService` class.", cites: ["context/generated/module-map.json#sym:sample-system:api/src/services/patientService.ts:PatientService"]}
  - {id: C3, text: "The API's patient detail type is `PatientDetail` in api/src/types.ts.", cites: ["context/generated/module-map.json#sym:sample-system:api/src/types.ts:PatientDetail"]}
  - {id: C4, text: "The web's patient detail type is `PatientDetail` in web/src/api/types.ts.", cites: ["context/generated/module-map.json#sym:sample-system:web/src/api/types.ts:PatientDetail"]}
  - {id: C5, text: "The patient page component is `PatientDetailPage`.", cites: ["context/generated/module-map.json#sym:sample-system:web/src/pages/PatientDetailPage.tsx:PatientDetailPage"]}
  - {id: C6, text: "The patient routes are served by `patientsRouter`.", cites: ["context/generated/module-map.json#sym:sample-system:api/src/routes/patients.ts:patientsRouter"]}
  - {id: C7, text: "Appointment status values include COMPLETED and `NO_SHOW`.", cites: ["context/data-model.md#appointments"]}
  - {id: C8, text: "The appointments loaded for the patient page are capped by a default `limit = 100`.", cites: ["artifacts/HMS-5/run_01M79P1N889PBTFM8J1EX89TBK/impact.md#claim:C6"]}
  - {id: C9, text: "The user chose to `Add to GET /api/patients/:id` for the two counts.", cites: ["runs/run_01M79P1N889PBTFM8J1EX89TBK/ide-answers.md#a-02"]}
  - {id: C10, text: "The user chose the field shape `visitSummary { completed, noShow }`.", cites: ["runs/run_01M79P1N889PBTFM8J1EX89TBK/ide-answers.md#a-03"]}
  - {id: C11, text: "PatientService.get builds the detail with `repo.appointments(id, this.clock())`.", cites: ["sample-system:api/src/services/patientService.ts#L42"]}
  - {id: C12, text: "Creating a patient returns the detail from `this.get`.", cites: ["sample-system:api/src/services/patientService.ts#L58"]}
  - {id: C13, text: "Updating a patient returns `this.get(id)`.", cites: ["sample-system:api/src/services/patientService.ts#L69"]}
  - {id: C14, text: "SQL belongs in `api/src/repositories` and rules in api/src/services.", cites: ["context/conventions.md#code-layout"]}
  - {id: C15, text: "The web app talks to the API only through `web/src/api/client.ts`.", cites: ["context/architecture.md#components"]}
  - {id: C16, text: "The patient page renders the appointments list with `AppointmentTable`.", cites: ["sample-system:web/src/pages/PatientDetailPage.tsx#L76"]}
  - {id: C17, text: "The patient page's appointments section starts with the heading `Appointments`.", cites: ["sample-system:web/src/pages/PatientDetailPage.tsx#L75"]}
  - {id: C18, text: "Index `appointment_patient_start_idx` exists on the appointment table, led by patient_id.", cites: ["context/generated/db-schema.json#index:appointment_patient_start_idx"]}
  - {id: C19, text: "Logs never carry request bodies, `contact details` or medical history.", cites: ["context/conventions.md#logging-and-data"]}
trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9]
known: [S-1, S-2, S-3]
assumed: []
unknown: []
questions: []
components:
  - {id: CO-1, name: "PatientRepo.visitSummary(id): counts the patient's COMPLETED and NO_SHOW appointments", module: "module:sample-system:api", change: changed, cites: ["context/generated/module-map.json#sym:sample-system:api/src/repositories/patientRepo.ts:PatientRepo", "context/data-model.md#appointments"]}
  - {id: CO-2, name: "PatientService.get: adds visitSummary to the detail", module: "module:sample-system:api", change: changed, cites: ["context/generated/module-map.json#sym:sample-system:api/src/services/patientService.ts:PatientService", "sample-system:api/src/services/patientService.ts#L42", "runs/run_01M79P1N889PBTFM8J1EX89TBK/ide-answers.md#a-02"]}
  - {id: CO-3, name: "API type PatientDetail: gains visitSummary", module: "module:sample-system:api", change: changed, cites: ["context/generated/module-map.json#sym:sample-system:api/src/types.ts:PatientDetail", "runs/run_01M79P1N889PBTFM8J1EX89TBK/ide-answers.md#a-03"]}
  - {id: CO-4, name: "Web type PatientDetail: gains visitSummary", module: "module:sample-system:web", change: changed, cites: ["context/generated/module-map.json#sym:sample-system:web/src/api/types.ts:PatientDetail", "runs/run_01M79P1N889PBTFM8J1EX89TBK/ide-answers.md#a-03"]}
  - {id: CO-5, name: "PatientDetailPage: Visit summary block above the Appointments heading", module: "module:sample-system:web", change: changed, cites: ["context/generated/module-map.json#sym:sample-system:web/src/pages/PatientDetailPage.tsx:PatientDetailPage", "sample-system:web/src/pages/PatientDetailPage.tsx#L75"]}
  - {id: CO-6, name: "patientsRouter GET/POST/PATCH handlers", module: "module:sample-system:api", change: existing, cites: ["context/generated/module-map.json#sym:sample-system:api/src/routes/patients.ts:patientsRouter"]}
  - {id: CO-7, name: "web api client api.patient", module: "module:sample-system:web", change: existing, cites: ["sample-system:web/src/api/client.ts#L71"]}
  - {id: CO-8, name: "AppointmentTable on the patient page", module: "module:sample-system:web", change: existing, cites: ["sample-system:web/src/pages/PatientDetailPage.tsx#L76"]}
endpoints:
  - id: EP-1
    method: GET
    path: /api/patients/:id
    change: changed
    fields:
      - {name: visitSummary, type: object, nullable: false, format: "{ completed, noShow }", example: "{\"completed\": 2, \"noShow\": 1}", status: known}
      - {name: visitSummary.completed, type: integer, nullable: false, format: "count of the patient's COMPLETED appointments, all time, >= 0", example: 2, status: known}
      - {name: visitSummary.noShow, type: integer, nullable: false, format: "count of the patient's NO_SHOW appointments, all time, >= 0", example: 1, status: known}
    cites: ["context/generated/api-inventory.json#http:GET:/api/patients/:id", "runs/run_01M79P1N889PBTFM8J1EX89TBK/ide-answers.md#a-02", "runs/run_01M79P1N889PBTFM8J1EX89TBK/ide-answers.md#a-03"]
  - id: EP-2
    method: POST
    path: /api/patients
    change: changed
    fields:
      - {name: visitSummary, type: object, nullable: false, format: "{ completed, noShow }; a new patient has both 0", example: "{\"completed\": 0, \"noShow\": 0}", status: known}
    cites: ["context/generated/api-inventory.json#http:POST:/api/patients", "sample-system:api/src/services/patientService.ts#L58", "runs/run_01M79P1N889PBTFM8J1EX89TBK/ide-answers.md#a-03"]
  - id: EP-3
    method: PATCH
    path: /api/patients/:id
    change: changed
    fields:
      - {name: visitSummary, type: object, nullable: false, format: "{ completed, noShow }, same as EP-1", example: "{\"completed\": 2, \"noShow\": 1}", status: known}
    cites: ["context/generated/api-inventory.json#http:PATCH:/api/patients/:id", "sample-system:api/src/services/patientService.ts#L69", "runs/run_01M79P1N889PBTFM8J1EX89TBK/ide-answers.md#a-03"]
answers:
  - {id: A-03, kind: question, about: Q-01, answer: "visitSummary { completed, noShow } (proposed default)", decides: [EP-1, EP-2, EP-3, CO-3, CO-4, C10]}
---
# Design: HMS-5

## Overview
The patient page gets a Visit summary showing two all-time counts: the patient's Completed and No-show appointments. The counts come from the server, because the appointments the page already loads stop at a default `limit = 100` (C8). As you chose (A-02), they are added to `GET /api/patients/:id` in the shape you picked (A-03), `visitSummary { completed, noShow }`.

**How it works:**
- PatientRepo runs one SQL count per patient, filtered by status (CO-1).
- PatientService.get attaches the result (CO-2).
- Both `PatientDetail` types gain the field (CO-3, CO-4).
- PatientDetailPage shows it just above the Appointments heading (CO-5).

**Trade-offs:**
- POST and PATCH responses also carry `visitSummary`, because they build their response with the same `get` (C12, C13). That costs one cheap extra query per write, but keeps a single code path.
- Counting in SQL by status, not by date, makes a past appointment that is still Scheduled count as neither (S-2 AC4). The count reads `appointment` by `patient_id`, which `appointment_patient_start_idx` leads with (C18), so no migration is needed.

## Components
- CO-1 PatientRepo.visitSummary(id), changed (api):
  - New method: one query, `count(*) FILTER (WHERE status = 'COMPLETED')` and `FILTER (WHERE status = 'NO_SHOW')`, `WHERE patient_id = $1`.
  - Returns `{ completed, noShow }` as numbers. A patient with no rows gets 0 and 0.
- CO-2 PatientService.get, changed (api): returns `{ ...patient, visitSummary: await repo.visitSummary(id), appointments }`. The patient and appointments parts are unchanged.
- CO-3 API type PatientDetail, changed (api): gains `visitSummary: { completed: number; noShow: number }`.
- CO-4 Web type PatientDetail, changed (web): gains the same `visitSummary` field.
- CO-5 PatientDetailPage, changed (web):
  - Adds a block headed "Visit summary" directly above the `Appointments` heading and `AppointmentTable` (C16, C17).
  - It shows "Completed" with `visitSummary.completed` and "No-show" with `visitSummary.noShow`.
  - Nothing else on the page moves or changes.
  - Default: the labels use the glossary status names. Trade-off: this keeps the terms consistent, but product may want other wording.
- CO-6 patientsRouter, existing (api): the handlers already return whatever PatientService returns.
- CO-7 web api client `api.patient`, existing (web): it is typed by `PatientDetail` (CO-4), so the client code does not change.
- CO-8 AppointmentTable, existing (web): unchanged.
- EP-1 GET /api/patients/:id, changed. Fields:
  - `visitSummary` (object, not null, `{ completed, noShow }`, example `{"completed": 2, "noShow": 1}`, known).
  - `visitSummary.completed` (integer, not null, all-time COMPLETED count >= 0, example 2, known).
  - `visitSummary.noShow` (integer, not null, all-time NO_SHOW count >= 0, example 1, known).
- EP-2 POST /api/patients, changed. Field `visitSummary` (object, not null; a new patient has `{"completed": 0, "noShow": 0}`, known).
- EP-3 PATCH /api/patients/:id, changed. Field `visitSummary` (object, not null, same as EP-1, known).

**Which components serve which story:**
- S-1 (the counts): CO-1, CO-2, CO-3, CO-4, CO-5 and EP-1. The count is scoped by patient_id, which gives S-1 AC5, and the period is all time.
- S-2 (only Completed and No-show are counted): CO-1. The status filter excludes SCHEDULED and CANCELLED, past or future, and returns 0 when nothing matches.
- S-3 (the rest of the page is unchanged): CO-5 only inserts the new block. CO-8 and the medical history section are untouched, and no logging is added (C19).

## Known
- S-1: served by CO-1 to CO-5 and EP-1. The counts are all time (A-01) and come from the server (A-02, C8) in the shape you chose (A-03).
- S-2: served by CO-1's status filter on COMPLETED and NO_SHOW (C7).
- S-3: served by CO-5, which only adds a block above the existing `Appointments` heading (C17), with CO-8 unchanged.

## Assumed
None. Every file and type named is proven by a sym fact or a product code line (C1 to C6, C16, C17).

## Unknown
None.

## Open questions
None open.

Answered in the IDE:
- A-03 (Q-01): the field name and shape for the two counts. Answer: "visitSummary { completed, noShow } (proposed default)". Decides EP-1, EP-2, EP-3, CO-3, CO-4 and C10. No mismatch with the inputs.
