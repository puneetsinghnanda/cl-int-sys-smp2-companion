---
schema: impact.v1
run_id: run_01M79NZC0GJYR61WB0MNSTYDHQ
step: impact
attempt: 1
skill: impact-analysis@1.3.0
inputs:
  - path: artifacts/HMS-13/run_01M79NZC0GJYR61WB0MNSTYDHQ/req.md
    sha256: 4ac588db013a5e386b1768e14973edb892b7f1ec0225d0d53ffbbd470f7a8bd2
  - path: context/generated/module-map.json
    sha256: 4158c8fcd239c22cb8f1029dd11f61d8839867ff8088bd9ab76197e2db96db4b
  - path: context/generated/api-inventory.json
    sha256: ba024ab1b4b4151d26ce1afa77bd2dab1cec645ea737ce37f98e0e8674f795e9
  - path: context/generated/db-schema.json
    sha256: e2a9d2990f6618ea5d55994a16c6264b0186b851cc3216014b812c4a4b98908a
  - path: context/architecture.md
    sha256: 9faab093d10933a46f6473b25c80728f8745e597100e650cd2537bbafb2b595a
  - path: context/data-model.md
    sha256: 2386ae151d54a73cdbe4fd9e0b69f8e8539c2870f5e52c12e5ed0191cfa95e55
pack_sha:
  sample-system: 7681f87749883d71abb4011d118366ca1242e1cc
claims:
  - id: C1
    text: "The web app talks to the API only through `web/src/api/client.ts`."
    cites: ["context/architecture.md#components"]
  - id: C2
    text: "The appointment page route /appointments/:id is declared in `web/src/App.tsx`."
    cites: ["context/generated/api-inventory.json#ui:/appointments/:id"]
  - id: C3
    text: "The appointment page loads its data with `api.appointment(id)`."
    cites: ["sample-system:web/src/pages/AppointmentDetailPage.tsx#L26"]
  - id: C4
    text: "The client's appointment call requests /api/appointments/:id and expects an `AppointmentDetail`."
    cites: ["sample-system:web/src/api/client.ts#L78"]
  - id: C5
    text: "The GET /api/appointments/:id handler lives in `api/src/routes/appointments.ts`."
    cites: ["context/generated/api-inventory.json#http:GET:/api/appointments/:id"]
  - id: C6
    text: "The appointment service builds the GET /:id response from the repository's `detail(id)`."
    cites: ["sample-system:api/src/services/appointmentService.ts#L55"]
  - id: C7
    text: "Reschedule, cancel, complete and no-show each end with `return this.get(id);`, so they return the same AppointmentDetail shape as GET /:id."
    cites: ["sample-system:api/src/services/appointmentService.ts#L101", "sample-system:api/src/services/appointmentService.ts#L115", "sample-system:api/src/services/appointmentService.ts#L129"]
  - id: C8
    text: "After an action the page reloads the appointment with `setVersion` rather than using the action's response."
    cites: ["sample-system:web/src/pages/AppointmentDetailPage.tsx#L35"]
  - id: C9
    text: "The appointment detail's doctor object holds id, code and `fullName` only, with no status."
    cites: ["sample-system:web/src/api/types.ts#L51"]
  - id: C10
    text: "The detail row is read with the shared summary query, which joins the doctor table but selects only `d.full_name AS doctor_name`."
    cites: ["sample-system:api/src/repositories/appointmentRepo.ts#L22"]
  - id: C11
    text: "The doctor repository reuses the shared `SUMMARY_SELECT` for the doctor's upcoming appointments."
    cites: ["sample-system:api/src/repositories/doctorRepo.ts#L3"]
  - id: C12
    text: "The dashboard service reuses the shared `SUMMARY_SELECT` for its upcoming appointments."
    cites: ["sample-system:api/src/services/dashboardService.ts#L6"]
  - id: C13
    text: "The patient repository reuses the shared `SUMMARY_SELECT`."
    cites: ["sample-system:api/src/repositories/patientRepo.ts#L3"]
  - id: C14
    text: "The doctor's status is stored in a column of `table:doctor`."
    cites: ["context/generated/db-schema.json#column:doctor.status"]
  - id: C15
    text: "Weekly availability is stored as `availability_block` rows per doctor."
    cites: ["context/data-model.md#doctors"]
  - id: C16
    text: "The DOC-7 rule is `withinAvailability` in the slots library."
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/lib/slots.ts:withinAvailability"]
  - id: C17
    text: "The doctor detail computes its DOC-7 flags as `outsideAvailability` from the doctor's upcoming appointments."
    cites: ["sample-system:api/src/services/doctorService.ts#L68"]
  - id: C18
    text: "Availability uses the hospital time zone `HOSPITAL_TZ`."
    cites: ["context/architecture.md#time-and-slots"]
  - id: C19
    text: "`No background jobs exist yet`, so no job reads or writes the changed data."
    cites: ["context/architecture.md#open-points"]
trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9]
known: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9]
assumed: []
unknown: []
questions: []
modules:
  - id: module:sample-system:web
    change: changed
    cites: ["context/generated/module-map.json#module:sample-system:web", "context/generated/api-inventory.json#ui:/appointments/:id", "runs/run_01M79NZC0GJYR61WB0MNSTYDHQ/ide-answers.md#a-05"]
  - id: module:sample-system:api
    change: changed
    cites: ["context/generated/module-map.json#module:sample-system:api", "context/generated/api-inventory.json#http:GET:/api/appointments/:id", "runs/run_01M79NZC0GJYR61WB0MNSTYDHQ/ide-answers.md#a-05", "runs/run_01M79NZC0GJYR61WB0MNSTYDHQ/ide-answers.md#a-06"]
  - id: module:sample-system:db
    change: existing
    cites: ["context/generated/module-map.json#module:sample-system:db", "context/generated/db-schema.json#column:doctor.status", "context/generated/db-schema.json#table:availability_block"]
apis:
  - id: http:GET:/api/appointments/:id
    change: changed
    cites: ["context/generated/api-inventory.json#http:GET:/api/appointments/:id", "runs/run_01M79NZC0GJYR61WB0MNSTYDHQ/ide-answers.md#a-05", "runs/run_01M79NZC0GJYR61WB0MNSTYDHQ/ide-answers.md#a-06"]
  - id: http:POST:/api/appointments/:id/reschedule
    change: changed
    cites: ["context/generated/api-inventory.json#http:POST:/api/appointments/:id/reschedule", "sample-system:api/src/services/appointmentService.ts#L101", "runs/run_01M79NZC0GJYR61WB0MNSTYDHQ/ide-answers.md#a-05"]
  - id: http:POST:/api/appointments/:id/cancel
    change: changed
    cites: ["context/generated/api-inventory.json#http:POST:/api/appointments/:id/cancel", "sample-system:api/src/services/appointmentService.ts#L115", "runs/run_01M79NZC0GJYR61WB0MNSTYDHQ/ide-answers.md#a-05"]
  - id: http:POST:/api/appointments/:id/complete
    change: changed
    cites: ["context/generated/api-inventory.json#http:POST:/api/appointments/:id/complete", "sample-system:api/src/services/appointmentService.ts#L129", "runs/run_01M79NZC0GJYR61WB0MNSTYDHQ/ide-answers.md#a-05"]
  - id: http:POST:/api/appointments/:id/no-show
    change: changed
    cites: ["context/generated/api-inventory.json#http:POST:/api/appointments/:id/no-show", "sample-system:api/src/services/appointmentService.ts#L129", "runs/run_01M79NZC0GJYR61WB0MNSTYDHQ/ide-answers.md#a-05"]
  - id: http:GET:/api/appointments
    change: existing
    cites: ["context/generated/api-inventory.json#http:GET:/api/appointments", "runs/run_01M79NZC0GJYR61WB0MNSTYDHQ/ide-answers.md#a-06"]
  - id: http:GET:/api/doctors/:id
    change: existing
    cites: ["context/generated/api-inventory.json#http:GET:/api/doctors/:id", "sample-system:api/src/services/doctorService.ts#L68", "runs/run_01M79NZC0GJYR61WB0MNSTYDHQ/ide-answers.md#a-06"]
  - id: http:GET:/api/dashboard
    change: existing
    cites: ["context/generated/api-inventory.json#http:GET:/api/dashboard", "sample-system:api/src/services/dashboardService.ts#L6", "runs/run_01M79NZC0GJYR61WB0MNSTYDHQ/ide-answers.md#a-06"]
tables:
  - id: table:appointment
    change: existing
    cites: ["context/generated/db-schema.json#table:appointment"]
  - id: table:doctor
    change: existing
    cites: ["context/generated/db-schema.json#table:doctor", "context/generated/db-schema.json#column:doctor.status"]
  - id: table:availability_block
    change: existing
    cites: ["context/generated/db-schema.json#table:availability_block"]
  - id: table:appointment_event
    change: existing
    cites: ["context/generated/db-schema.json#table:appointment_event"]
answers:
  - id: A-05
    kind: question
    about: Q-01
    answer: "Add both to GET /:id (proposed default)"
    decides: [http:GET:/api/appointments/:id, http:POST:/api/appointments/:id/reschedule, http:POST:/api/appointments/:id/cancel, http:POST:/api/appointments/:id/complete, http:POST:/api/appointments/:id/no-show, module:sample-system:api, module:sample-system:web]
  - id: A-06
    kind: question
    about: Q-02
    answer: "AppointmentDetail only (proposed default)"
    decides: [http:GET:/api/appointments/:id, http:GET:/api/appointments, http:GET:/api/doctors/:id, http:GET:/api/dashboard, module:sample-system:api]
---
# Impact analysis: HMS-13

## Summary
The change touches the API and the web app. No migration is needed: the doctor's status column (C14) and the weekly `availability_block` rows (C15) already exist. Per A-05, GET /api/appointments/:id gains the doctor's status and an outside-availability result, computed on the server with the existing DOC-7 rule `withinAvailability` (C16) in hospital time (C18). The appointment page then renders the two notices from that response. Per A-06, the new fields go on AppointmentDetail only, because the detail is built from the shared summary query (C10), which the doctor, dashboard and patient queries share (C11, C12, C13). The main risk is accidentally changing that shared query and with it every appointment-summary response. A second risk: the four action endpoints return the same detail shape (C7), so they gain the fields too.

## Impacted modules
- module:sample-system:web (changed): the appointment page at /appointments/:id (C2, C3) shows the notices. Its AppointmentDetail type in web/src/api/types.ts gains the doctor's status and the availability result (C4, C9). It talks to the API only through the client (C1).
- module:sample-system:api (changed): the appointment detail (C5, C6) adds the doctor's status and the result of `withinAvailability` (C16), and the API's own AppointmentDetail type changes to match. AppointmentSummary and the shared summary query stay as they are (A-06).
- module:sample-system:db (existing): read only. No new table, column or migration (C14, C15).
- http:GET:/api/appointments/:id (changed): the response gains the doctor's status and an outside-availability flag (A-05). It is additive.
- http:POST:/api/appointments/:id/reschedule (changed): returns the same detail shape (C7), so it gains the same fields. It is additive. The page ignores this response and reloads (C8).
- http:POST:/api/appointments/:id/cancel (changed): the same as reschedule (C7).
- http:POST:/api/appointments/:id/complete (changed): the same as reschedule (C7).
- http:POST:/api/appointments/:id/no-show (changed): the same as reschedule (C7).
- http:GET:/api/appointments (existing): returns summaries, which are unchanged (A-06).
- http:GET:/api/doctors/:id (existing): its DOC-7 `outsideAvailability` flags (C17) and the 50-appointment cap stay as they are. It shares the rule, not the code path (A-06).
- http:GET:/api/dashboard (existing): reuses the shared summary query (C12), which stays unchanged (A-06).
- table:appointment (existing): read for the detail.
- table:doctor (existing): its status column is read (C14).
- table:availability_block (existing): read to run the availability rule (C15).
- table:appointment_event (existing): history is read as today; the change adds no event.

No background job is affected (C19).

## Known
- REQ-1, REQ-2, REQ-3, REQ-4: the availability result is computed in the API with `withinAvailability` (C16) for not-yet-started Scheduled appointments, with no list cap, and returned by GET /:id (A-05).
- REQ-5: the doctor's status is read from `table:doctor` (C14) and returned by GET /:id (A-05).
- REQ-6, REQ-7: order and co-display are rendering concerns of the web module only.
- REQ-8: the web module hides both notices for closed statuses, using data already in the detail.
- REQ-9: the rest of the page and the summary endpoints stay unchanged (A-06).

## Assumed
None.

## Unknown
None.

## Open questions
None open.

Answered in the IDE:
- A-05 (Q-01, answered in the IDE): where should the appointment page get the doctor's status and the availability result? Answer: "Add both to GET /:id (proposed default)". Decides http:GET:/api/appointments/:id, the four action endpoints, module:sample-system:api and module:sample-system:web. No mismatch with the facts.
- A-06 (Q-02, answered in the IDE): should the new fields go on AppointmentDetail only? Answer: "AppointmentDetail only (proposed default)". Decides http:GET:/api/appointments/:id, http:GET:/api/appointments, http:GET:/api/doctors/:id, http:GET:/api/dashboard and module:sample-system:api. No mismatch with the facts.
