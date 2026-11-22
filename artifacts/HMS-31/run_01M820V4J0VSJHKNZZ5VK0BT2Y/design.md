---
schema: design.v1
run_id: run_01M820V4J0VSJHKNZZ5VK0BT2Y
step: design
attempt: 1
skill: design@1.9.0
inputs:
  - path: artifacts/HMS-31/run_01M820V4J0VSJHKNZZ5VK0BT2Y/split.md
    sha256: bd26e52b86c31239358a3a893ff920c053cf6830e2406107bd5bda4c61c58536
  - path: artifacts/HMS-31/run_01M820V4J0VSJHKNZZ5VK0BT2Y/req.md
    sha256: d970576e048aa037a9ebd47efc879fe8e0b4bac02f0aa652abd98bc55edb9e2c
  - path: context/intent-baseline.md
    sha256: 9bfd821aeb2fb9524863e346707e049e4fcfb949a6e55b90223a79194946a215
  - path: context/architecture.md
    sha256: 9faab093d10933a46f6473b25c80728f8745e597100e650cd2537bbafb2b595a
  - path: context/generated/module-map.json
    sha256: 4158c8fcd239c22cb8f1029dd11f61d8839867ff8088bd9ab76197e2db96db4b
  - path: context/generated/api-inventory.json
    sha256: ba024ab1b4b4151d26ce1afa77bd2dab1cec645ea737ce37f98e0e8674f795e9
pack_sha:
  sample-system: 7681f87749883d71abb4011d118366ca1242e1cc
claims:
  - id: C1
    text: "The appointment detail response type `AppointmentDetail` is declared in api/src/types.ts."
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/types.ts:AppointmentDetail"]
  - id: C2
    text: "The web app's copy of `AppointmentDetail` is declared in web/src/api/types.ts."
    cites: ["context/generated/module-map.json#sym:sample-system:web/src/api/types.ts:AppointmentDetail"]
  - id: C3
    text: "Appointment SQL lives in `AppointmentRepo`, which builds the detail response."
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/repositories/appointmentRepo.ts:AppointmentRepo"]
  - id: C4
    text: "The shared `SUMMARY_SELECT` query serves the appointment lists and the detail read alike."
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/repositories/appointmentRepo.ts:SUMMARY_SELECT"]
  - id: C5
    text: "`DoctorSummary` already carries specialization and department as {id, name, status} and the fee as feePaise."
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/types.ts:DoctorSummary"]
  - id: C6
    text: "The web helper `rupees` turns paise into ₹ with two decimals and is what the doctors list uses."
    cites: ["context/generated/module-map.json#sym:sample-system:web/src/lib/format.ts:rupees"]
  - id: C7
    text: "The appointment page is `AppointmentDetailPage`."
    cites: ["context/generated/module-map.json#sym:sample-system:web/src/pages/AppointmentDetailPage.tsx:AppointmentDetailPage"]
  - id: C8
    text: "GET /api/appointments/:id exists in `api/src/routes/appointments.ts`."
    cites: ["context/generated/api-inventory.json#http:GET:/api/appointments/:id"]
  - id: C9
    text: "`AppointmentService` answers get, book, reschedule, cancel and close with the same appointment detail."
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/services/appointmentService.ts:AppointmentService"]
  - id: C10
    text: "The web app talks to the API only through `web/src/api/client.ts`."
    cites: ["context/architecture.md#components"]
  - id: C11
    text: "Logs never carry request bodies, `contact details` or medical history."
    cites: ["context/conventions.md#logging-and-data"]
  - id: C12
    text: "Only in `AppointmentDetail`, the doctor object gains specialization, department and feePaise in the DoctorSummary shapes; lists stay unchanged."
    cites: ["runs/run_01M820V4J0VSJHKNZZ5VK0BT2Y/ide-answers.md#a-02", "context/generated/module-map.json#sym:sample-system:api/src/types.ts:AppointmentDetail"]
  - id: C13
    text: "An Inactive specialization or department is shown with its name followed by `(Inactive)`."
    cites: ["runs/run_01M820V4J0VSJHKNZZ5VK0BT2Y/ide-answers.md#a-01"]
trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10, REQ-11]
known: [CO-1, CO-2, CO-3, CO-4, CO-5, CO-6, CO-7, CO-8, EP-1, EP-2, EP-3, EP-4, EP-5, EP-6]
assumed: []
unknown:
  - {id: U-1, blocking: false}
questions: []
components:
  - id: CO-1
    name: AppointmentRepo.detail (joins the doctor's specialization and department)
    module: module:sample-system:api
    change: changed
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/repositories/appointmentRepo.ts:AppointmentRepo", "runs/run_01M820V4J0VSJHKNZZ5VK0BT2Y/ide-answers.md#a-02"]
  - id: CO-2
    name: AppointmentDetail type (api)
    module: module:sample-system:api
    change: changed
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/types.ts:AppointmentDetail", "runs/run_01M820V4J0VSJHKNZZ5VK0BT2Y/ide-answers.md#a-02"]
  - id: CO-3
    name: AppointmentService (get, book, reschedule, cancel, close)
    module: module:sample-system:api
    change: existing
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/services/appointmentService.ts:AppointmentService"]
  - id: CO-4
    name: appointmentsRouter
    module: module:sample-system:api
    change: existing
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/routes/appointments.ts:appointmentsRouter"]
  - id: CO-5
    name: AppointmentDetail type (web)
    module: module:sample-system:web
    change: changed
    cites: ["context/generated/module-map.json#sym:sample-system:web/src/api/types.ts:AppointmentDetail", "runs/run_01M820V4J0VSJHKNZZ5VK0BT2Y/ide-answers.md#a-02"]
  - id: CO-6
    name: AppointmentDetailPage (Specialization, Department and Consultation fee rows below Doctor)
    module: module:sample-system:web
    change: changed
    cites: ["context/generated/module-map.json#sym:sample-system:web/src/pages/AppointmentDetailPage.tsx:AppointmentDetailPage", "runs/run_01M820V4J0VSJHKNZZ5VK0BT2Y/ide-answers.md#a-01"]
  - id: CO-7
    name: rupees fee formatter
    module: module:sample-system:web
    change: existing
    cites: ["context/generated/module-map.json#sym:sample-system:web/src/lib/format.ts:rupees"]
  - id: CO-8
    name: api client (appointment read and actions)
    module: module:sample-system:web
    change: existing
    cites: ["context/generated/module-map.json#sym:sample-system:web/src/api/client.ts:api"]
endpoints:
  - id: EP-1
    method: GET
    path: /api/appointments/:id
    change: changed
    fields:
      - {name: doctor.specialization.id, type: integer, nullable: false, format: row id, example: 1, status: known}
      - {name: doctor.specialization.name, type: string, nullable: false, format: current specialization name, example: "<specialization name>", status: known}
      - {name: doctor.specialization.status, type: string, nullable: false, format: "ACTIVE | INACTIVE", example: "INACTIVE", status: known}
      - {name: doctor.department.id, type: integer, nullable: false, format: row id, example: 1, status: known}
      - {name: doctor.department.name, type: string, nullable: false, format: current department name, example: "<department name>", status: known}
      - {name: doctor.department.status, type: string, nullable: false, format: "ACTIVE | INACTIVE", example: "ACTIVE", status: known}
      - {name: doctor.feePaise, type: integer, nullable: false, format: integer paise (INR), example: 50000, status: known}
    cites: ["context/generated/api-inventory.json#http:GET:/api/appointments/:id", "runs/run_01M820V4J0VSJHKNZZ5VK0BT2Y/ide-answers.md#a-02"]
  - id: EP-2
    method: POST
    path: /api/appointments
    change: changed
    fields:
      - {name: doctor.specialization, type: "object {id, name, status}", nullable: false, format: same as EP-1, example: "{\"id\": 1, \"name\": \"<specialization name>\", \"status\": \"ACTIVE\"}", status: known}
      - {name: doctor.department, type: "object {id, name, status}", nullable: false, format: same as EP-1, example: "{\"id\": 1, \"name\": \"<department name>\", \"status\": \"ACTIVE\"}", status: known}
      - {name: doctor.feePaise, type: integer, nullable: false, format: integer paise (INR), example: 50000, status: known}
    cites: ["context/generated/api-inventory.json#http:POST:/api/appointments", "runs/run_01M820V4J0VSJHKNZZ5VK0BT2Y/ide-answers.md#a-02"]
  - id: EP-3
    method: POST
    path: /api/appointments/:id/reschedule
    change: changed
    fields:
      - {name: doctor.specialization, type: "object {id, name, status}", nullable: false, format: same as EP-1, example: "{\"id\": 1, \"name\": \"<specialization name>\", \"status\": \"ACTIVE\"}", status: known}
      - {name: doctor.department, type: "object {id, name, status}", nullable: false, format: same as EP-1, example: "{\"id\": 1, \"name\": \"<department name>\", \"status\": \"ACTIVE\"}", status: known}
      - {name: doctor.feePaise, type: integer, nullable: false, format: integer paise (INR), example: 50000, status: known}
    cites: ["context/generated/api-inventory.json#http:POST:/api/appointments/:id/reschedule", "runs/run_01M820V4J0VSJHKNZZ5VK0BT2Y/ide-answers.md#a-02"]
  - id: EP-4
    method: POST
    path: /api/appointments/:id/cancel
    change: changed
    fields:
      - {name: doctor.specialization, type: "object {id, name, status}", nullable: false, format: same as EP-1, example: "{\"id\": 1, \"name\": \"<specialization name>\", \"status\": \"ACTIVE\"}", status: known}
      - {name: doctor.department, type: "object {id, name, status}", nullable: false, format: same as EP-1, example: "{\"id\": 1, \"name\": \"<department name>\", \"status\": \"ACTIVE\"}", status: known}
      - {name: doctor.feePaise, type: integer, nullable: false, format: integer paise (INR), example: 50000, status: known}
    cites: ["context/generated/api-inventory.json#http:POST:/api/appointments/:id/cancel", "runs/run_01M820V4J0VSJHKNZZ5VK0BT2Y/ide-answers.md#a-02"]
  - id: EP-5
    method: POST
    path: /api/appointments/:id/complete
    change: changed
    fields:
      - {name: doctor.specialization, type: "object {id, name, status}", nullable: false, format: same as EP-1, example: "{\"id\": 1, \"name\": \"<specialization name>\", \"status\": \"ACTIVE\"}", status: known}
      - {name: doctor.department, type: "object {id, name, status}", nullable: false, format: same as EP-1, example: "{\"id\": 1, \"name\": \"<department name>\", \"status\": \"ACTIVE\"}", status: known}
      - {name: doctor.feePaise, type: integer, nullable: false, format: integer paise (INR), example: 50000, status: known}
    cites: ["context/generated/api-inventory.json#http:POST:/api/appointments/:id/complete", "runs/run_01M820V4J0VSJHKNZZ5VK0BT2Y/ide-answers.md#a-02"]
  - id: EP-6
    method: POST
    path: /api/appointments/:id/no-show
    change: changed
    fields:
      - {name: doctor.specialization, type: "object {id, name, status}", nullable: false, format: same as EP-1, example: "{\"id\": 1, \"name\": \"<specialization name>\", \"status\": \"ACTIVE\"}", status: known}
      - {name: doctor.department, type: "object {id, name, status}", nullable: false, format: same as EP-1, example: "{\"id\": 1, \"name\": \"<department name>\", \"status\": \"ACTIVE\"}", status: known}
      - {name: doctor.feePaise, type: integer, nullable: false, format: integer paise (INR), example: 50000, status: known}
    cites: ["context/generated/api-inventory.json#http:POST:/api/appointments/:id/no-show", "runs/run_01M820V4J0VSJHKNZZ5VK0BT2Y/ide-answers.md#a-02"]
answers:
  - id: A-02
    kind: question
    about: Q-01
    answer: "Extend detail doctor (proposed default)"
    decides: [CO-1, CO-2, CO-5, EP-1, EP-2, EP-3, EP-4, EP-5, EP-6, C12]
---
# Design: HMS-31

## Overview
The appointment detail response gains the doctor's current specialization, department and fee, and
the appointment page shows them in three rows below "Doctor". No new endpoint, table, migration or
service is needed.

- API (CO-1, CO-2): `AppointmentRepo.detail` loads the doctor's specialization {id, name, status},
  department {id, name, status} and `fee_paise` by joining `doctor`, `specialization` and `department`
  for the appointment's doctor, and returns them inside `doctor`. Only `AppointmentDetail` is
  extended (A-02); `AppointmentSummary` and its lists are unchanged.
  Trade-off: a separate detail-only join rather than editing the shared `SUMMARY_SELECT` (C4). This
  keeps the list queries and the locked `get(id, true)` read used by the actions unchanged (REQ-11),
  at the cost of one more join in the detail read.
- Current details (REQ-5, REQ-6): the values are read live from the doctor and reference rows at
  request time and are never copied onto the appointment. So closed appointments and Inactive doctors
  show the doctor's current values. Trade-off: the page cannot show what the fee was at booking, which
  the ticket does not ask for.
- No extra personal data (REQ-9, REQ-10): the detail read selects only the specialization, department
  and fee columns of the doctor, never its phone or email, and no patient medical history. So neither
  reaches the response (S-3 AC2, AC3). Trade-off: the `DoctorSummary` mapping cannot be reused as a
  whole, because it carries phone and email.
- Web (CO-5, CO-6): the web `AppointmentDetail` type mirrors the API. `AppointmentDetailPage` adds
  "Specialization", "Department" and "Consultation fee" after the "Doctor" row. The fee goes through
  the existing `rupees` helper (CO-7), so it matches the doctors list (REQ-4). A name whose status is
  INACTIVE is followed by " (Inactive)" (A-01, REQ-7). The doctor name link, the other rows, the
  actions and the history stay exactly as they are (REQ-8, REQ-11).
- The action endpoints (EP-2 to EP-6) answer with the same detail as `GET`, because
  `AppointmentService` returns `get(id)` after each action (C9). So their responses gain the same
  fields, additively. Trade-off: no extra code, but those responses change shape for any other
  client (only the web app uses them, through `web/src/api/client.ts`, C10).

Story mapping: S-1 → CO-1, CO-2, CO-5, CO-6, CO-7, EP-1. S-2 → CO-1, CO-6. S-3 → CO-1, CO-6.
S-4 → CO-3, CO-4, CO-6 (existing behaviour kept).

## Components
- CO-1 AppointmentRepo.detail — changed — adds the detail-only join for the specialization, department and fee.
- CO-2 AppointmentDetail type (api) — changed — `doctor` becomes `{ id, code, fullName, specialization: {id, name, status}, department: {id, name, status}, feePaise }`.
- CO-3 AppointmentService — existing — get, book, reschedule, cancel and close already return the detail.
- CO-4 appointmentsRouter — existing — no route change.
- CO-5 AppointmentDetail type (web) — changed — mirrors CO-2.
- CO-6 AppointmentDetailPage — changed — three new rows below Doctor, with "(Inactive)" after an Inactive name.
- CO-7 rupees — existing — formats feePaise as ₹ with two decimals.
- CO-8 api client — existing — `api.appointment` and the action calls are unchanged.
- EP-1 GET /api/appointments/:id — changed — new fields: doctor.specialization.id (integer), doctor.specialization.name (string), doctor.specialization.status ("ACTIVE" | "INACTIVE"), doctor.department.id (integer), doctor.department.name (string), doctor.department.status ("ACTIVE" | "INACTIVE"), doctor.feePaise (integer paise, for example 50000 shown as ₹500.00). None is nullable.
- EP-2 POST /api/appointments — changed — same new doctor fields as EP-1 in its detail response.
- EP-3 POST /api/appointments/:id/reschedule — changed — same new doctor fields as EP-1.
- EP-4 POST /api/appointments/:id/cancel — changed — same new doctor fields as EP-1.
- EP-5 POST /api/appointments/:id/complete — changed — same new doctor fields as EP-1.
- EP-6 POST /api/appointments/:id/no-show — changed — same new doctor fields as EP-1.

## Known
- CO-1, CO-2, CO-5: the locations come from the module map (C1, C2, C3); the shape was decided in A-02.
- CO-3, CO-4, CO-8: existing symbols from the module map (C9); no change needed.
- CO-6: the page symbol comes from the module map (C7); the Inactive marker was decided in A-01.
- CO-7: the existing `rupees` helper (C6).
- EP-1 to EP-6: listed in the api inventory (C8); the fields reuse the DoctorSummary shapes (C5) per A-02.

## Assumed
None.

## Unknown
- U-1 (non-blocking): the glossary has no entry for Specialization, Department or Consultation fee. This is already open as runs/run_01M820V4J0VSJHKNZZ5VK0BT2Y/open-questions.md#q-02 and does not affect the design.

## Open questions
None new. runs/run_01M820V4J0VSJHKNZZ5VK0BT2Y/open-questions.md#q-02 stays open (non-blocking, tech lead).

Answered in the IDE:
- A-02 (Q-01): how the detail response carries the new fields → "Extend detail doctor (proposed default)". It decides CO-1, CO-2, CO-5, EP-1 to EP-6 and C12. No mismatch with the inputs.
