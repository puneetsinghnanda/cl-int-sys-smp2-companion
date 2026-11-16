---
schema: design.v1
run_id: run_01M7HH0DM88V7JHMVZPSYX5BWE
step: design
attempt: 2
skill: design@1.7.0
inputs:
  - path: artifacts/HMS-14/run_01M7HH0DM88V7JHMVZPSYX5BWE/impact.md
    sha256: 30ebb6e1817858069d5a93181df6db3c7a87be9543a54aba0c4d57f149520a02
  - path: artifacts/HMS-14/run_01M7HH0DM88V7JHMVZPSYX5BWE/split.md
    sha256: 88e8cd0212df1bcdf5574b26e6415ef50243df439a57102ca81559c2f68fc854
  - path: artifacts/HMS-14/run_01M7HH0DM88V7JHMVZPSYX5BWE/req.md
    sha256: 7f7f379c0fac62230826a6d88863ac42c7c9dd2da60c5db6afedef2d3e5ebcdb
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
resolves:
  - id: F-1
    how: "The two status fields are now marked assumed, typed RecordStatus, with the uppercase example INACTIVE that implement must confirm."
  - id: F-4
    how: "The reason for visit now travels in router navigation state, not the URL, as decided in A-09."
claims:
  - id: C1
    text: "The appointment page component is `web/src/pages/AppointmentDetailPage.tsx`."
    cites: ["context/generated/module-map.json#sym:sample-system:web/src/pages/AppointmentDetailPage.tsx:AppointmentDetailPage"]
  - id: C2
    text: "The book appointment page component is `web/src/pages/BookAppointmentPage.tsx`."
    cites: ["context/generated/module-map.json#sym:sample-system:web/src/pages/BookAppointmentPage.tsx:BookAppointmentPage"]
  - id: C3
    text: "The web detail contract type is declared in `web/src/api/types.ts`."
    cites: ["context/generated/module-map.json#sym:sample-system:web/src/api/types.ts:AppointmentDetail"]
  - id: C4
    text: "The API detail contract type is declared in `api/src/types.ts`."
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/types.ts:AppointmentDetail"]
  - id: C5
    text: "The book appointment page already defaults to today's date in hospital time via `todayIn` when the address has no date; implement must confirm in code."
    cites: ["context/generated/module-map.json#sym:sample-system:web/src/lib/format.ts:todayIn", "runs/run_01M7HH0DM88V7JHMVZPSYX5BWE/ide-answers.md#a-08"]
  - id: C6
    text: "The API detail row shape is `DetailRow` in api/src/repositories/appointmentRepo.ts."
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/repositories/appointmentRepo.ts:DetailRow"]
  - id: C7
    text: "The detail response adds `doctor_status/patient_status (proposed default)` as two top-level strings with the existing record status values."
    cites: ["runs/run_01M7HH0DM88V7JHMVZPSYX5BWE/ide-answers.md#a-06"]
  - id: C8
    text: "The web app already has a record status type `RecordStatus`; the new fields reuse it, and implement must confirm its exact literals."
    cites: ["context/generated/module-map.json#sym:sample-system:web/src/api/types.ts:RecordStatus"]
  - id: C9
    text: "The reason for visit is handed over under the key `reason (proposed default)`."
    cites: ["runs/run_01M7HH0DM88V7JHMVZPSYX5BWE/ide-answers.md#a-07"]
  - id: C10
    text: "API field names follow snake_case, as in `page_size`."
    cites: ["context/conventions.md#lists"]
  - id: C11
    text: "The doctor status comes from the existing column `column:doctor.status` and the patient status from the patient status column; no migration."
    cites: ["artifacts/HMS-14/run_01M7HH0DM88V7JHMVZPSYX5BWE/impact.md#claim:C7", "artifacts/HMS-14/run_01M7HH0DM88V7JHMVZPSYX5BWE/impact.md#claim:C8"]
  - id: C12
    text: "Medical history leaves the database only for the `patient's own page`, so the appointment page must not load the patient record."
    cites: ["context/data-model.md#patients"]
  - id: C13
    text: "The service layer `AppointmentService` holds appointment rules and sits between route and repository."
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/services/appointmentService.ts:AppointmentService"]
  - id: C14
    text: "The web app talks to the API only through `web/src/api/client.ts`."
    cites: ["context/architecture.md#components"]
  - id: C15
    text: "Overlapping bookings are refused by database `exclusion constraints`, so booking again needs no new guard."
    cites: ["context/architecture.md#double-booking"]
  - id: C16
    text: "Book again hands the reason for visit to the book page in router `Navigation state (proposed default)`, so it never enters the URL or browser history."
    cites: ["runs/run_01M7HH0DM88V7JHMVZPSYX5BWE/ide-answers.md#a-09"]
  - id: C17
    text: "The API also declares a record status type `RecordStatus` in api/src/types.ts."
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/types.ts:RecordStatus"]
  - id: C18
    text: "Existing status columns store uppercase literals such as `NO_SHOW`, the basis for the assumed INACTIVE example."
    cites: ["context/data-model.md#appointments"]
trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10, REQ-11, REQ-12, REQ-13, REQ-14, REQ-15, REQ-16, REQ-17, REQ-18, REQ-19]
known: [S-1, S-2, S-3, S-4, S-5, S-6]
assumed: [EP-1, CO-3, CO-4, CO-6]
unknown: []
questions: []
components:
  - id: CO-1
    name: AppointmentDetailPage — Book again button and reasons
    module: module:sample-system:web
    change: changed
    cites: ["context/generated/module-map.json#sym:sample-system:web/src/pages/AppointmentDetailPage.tsx:AppointmentDetailPage", "runs/run_01M7HH0DM88V7JHMVZPSYX5BWE/ide-answers.md#a-09"]
  - id: CO-2
    name: BookAppointmentPage — reason from navigation state
    module: module:sample-system:web
    change: changed
    cites: ["context/generated/module-map.json#sym:sample-system:web/src/pages/BookAppointmentPage.tsx:BookAppointmentPage", "runs/run_01M7HH0DM88V7JHMVZPSYX5BWE/ide-answers.md#a-07", "runs/run_01M7HH0DM88V7JHMVZPSYX5BWE/ide-answers.md#a-08", "runs/run_01M7HH0DM88V7JHMVZPSYX5BWE/ide-answers.md#a-09"]
  - id: CO-3
    name: web AppointmentDetail type — two status fields
    module: module:sample-system:web
    change: changed
    cites: ["context/generated/module-map.json#sym:sample-system:web/src/api/types.ts:AppointmentDetail", "context/generated/module-map.json#sym:sample-system:web/src/api/types.ts:RecordStatus", "runs/run_01M7HH0DM88V7JHMVZPSYX5BWE/ide-answers.md#a-06"]
  - id: CO-4
    name: API AppointmentDetail type — two status fields
    module: module:sample-system:api
    change: changed
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/types.ts:AppointmentDetail", "context/generated/module-map.json#sym:sample-system:api/src/types.ts:RecordStatus", "runs/run_01M7HH0DM88V7JHMVZPSYX5BWE/ide-answers.md#a-06"]
  - id: CO-5
    name: appointmentRepo DetailRow and detail query
    module: module:sample-system:api
    change: changed
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/repositories/appointmentRepo.ts:DetailRow", "runs/run_01M7HH0DM88V7JHMVZPSYX5BWE/ide-answers.md#a-06"]
  - id: CO-6
    name: AppointmentService — detail pass-through
    module: module:sample-system:api
    change: existing
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/services/appointmentService.ts:AppointmentService"]
  - id: CO-7
    name: todayIn helper — hospital-local today
    module: module:sample-system:web
    change: existing
    cites: ["context/generated/module-map.json#sym:sample-system:web/src/lib/format.ts:todayIn", "runs/run_01M7HH0DM88V7JHMVZPSYX5BWE/ide-answers.md#a-08"]
endpoints:
  - id: EP-1
    method: GET
    path: /api/appointments/:id
    change: changed
    fields:
      - {name: doctor_status, type: RecordStatus (string), nullable: false, format: "RecordStatus literal copied from doctor.status; implement must confirm the literals", example: "INACTIVE", status: assumed}
      - {name: patient_status, type: RecordStatus (string), nullable: false, format: "RecordStatus literal copied from patient.status; implement must confirm the literals", example: "INACTIVE", status: assumed}
    cites: ["context/generated/api-inventory.json#http:GET:/api/appointments/:id", "runs/run_01M7HH0DM88V7JHMVZPSYX5BWE/ide-answers.md#a-04", "runs/run_01M7HH0DM88V7JHMVZPSYX5BWE/ide-answers.md#a-06"]
  - id: EP-2
    method: POST
    path: /api/appointments
    change: existing
    fields: []
    cites: ["context/generated/api-inventory.json#http:POST:/api/appointments"]
  - id: EP-3
    method: GET
    path: /api/doctors/:id/slots
    change: existing
    fields: []
    cites: ["context/generated/api-inventory.json#http:GET:/api/doctors/:id/slots"]
  - id: EP-4
    method: POST
    path: /api/appointments/:id/cancel
    change: existing
    fields: []
    cites: ["context/generated/api-inventory.json#http:POST:/api/appointments/:id/cancel", "runs/run_01M7HH0DM88V7JHMVZPSYX5BWE/ide-answers.md#a-05"]
  - id: EP-5
    method: POST
    path: /api/appointments/:id/reschedule
    change: existing
    fields: []
    cites: ["context/generated/api-inventory.json#http:POST:/api/appointments/:id/reschedule", "runs/run_01M7HH0DM88V7JHMVZPSYX5BWE/ide-answers.md#a-05"]
  - id: EP-6
    method: POST
    path: /api/appointments/:id/complete
    change: existing
    fields: []
    cites: ["context/generated/api-inventory.json#http:POST:/api/appointments/:id/complete", "runs/run_01M7HH0DM88V7JHMVZPSYX5BWE/ide-answers.md#a-05"]
  - id: EP-7
    method: POST
    path: /api/appointments/:id/no-show
    change: existing
    fields: []
    cites: ["context/generated/api-inventory.json#http:POST:/api/appointments/:id/no-show", "runs/run_01M7HH0DM88V7JHMVZPSYX5BWE/ide-answers.md#a-05"]
  - id: EP-8
    method: GET
    path: /api/settings
    change: existing
    fields: []
    cites: ["context/generated/api-inventory.json#http:GET:/api/settings"]
answers:
  - id: A-06
    kind: question
    about: Q-01
    answer: "doctor_status/patient_status (proposed default)"
    decides: [EP-1, CO-3, CO-4, CO-5, C7]
  - id: A-07
    kind: question
    about: Q-02
    answer: "reason (proposed default)"
    decides: [CO-2, C9]
  - id: A-08
    kind: assumption
    about: C5
    answer: "Yes, confirm"
    decides: [C5, CO-2, CO-7]
  - id: A-09
    kind: question
    about: Q-03
    answer: "Navigation state (proposed default)"
    decides: [CO-1, CO-2, C16]
    mismatch:
      text: "A-07 framed `reason` as an address (URL) parameter; the user decided in A-09 to pass it in router navigation state, keeping `reason` only as the state key."
      cites: ["runs/run_01M7HH0DM88V7JHMVZPSYX5BWE/ide-answers.md#a-07"]
---
# Design: HMS-14

## Overview
"Book again" on the appointment page (CO-1) navigates to the existing book appointment route `/appointments/new`. It puts the original's doctor and patient in the address, as other links already do, and the reason for visit in router navigation state under the key `reason` (A-07, A-09). The page shows the button only for Cancelled and No-show appointments. Date and slot are left out: the book page already defaults to today in hospital time via `todayIn` (C5, A-08) and starts with no slot. The book page (CO-2) reads `reason` from navigation state into its reason for visit field, which stays editable and keeps its 300-character limit; an empty or missing reason leaves the field empty. The cancellation reason is never passed. To disable the button for an Inactive doctor or patient without loading the full patient record (C12), `GET /api/appointments/:id` gains `doctor_status` and `patient_status` (EP-1, A-04, A-06). They are read from the existing status columns through `DetailRow` (CO-5) and exposed as `RecordStatus` in the API and web `AppointmentDetail` types (CO-4, CO-3). Booking itself is unchanged (EP-2, EP-3), so the free-slot, overlap and "slot no longer available" rules and the database exclusion constraints still apply (C15). The new appointment gets its own ID, and the original is never written.

Resolves:
- F-1: The two status fields are now marked assumed, typed RecordStatus, with the uppercase example INACTIVE that implement must confirm.
- F-4: The reason for visit now travels in router navigation state, not the URL, as decided in A-09.

Trade-offs:
- Navigating to the existing book page instead of a new "rebook" endpoint keeps all booking rules in one path.
- Navigation state keeps the free-text reason out of the URL and browser history (A-09); the cost is that a reload of the book page drops the prefilled reason, and staff can retype it.
- Adding status to the detail response is one more join, versus two extra GETs; it also keeps patient data off the appointment page.
- No date in the link relies on the book page's default (C5); passing today explicitly would duplicate that logic.

## Components
- CO-1 AppointmentDetailPage — changed: show "Book again" when status is Cancelled or No-show (S-1). When doctor_status or patient_status is not the Active literal, disable the button and list each reason that applies, "Doctor is inactive" and/or "Patient is inactive" (S-4, A-01). Otherwise navigate to `/appointments/new` with doctor and patient in the address and `{ reason }` in navigation state, only when the original has a reason (S-2, S-3, A-09).
- CO-2 BookAppointmentPage — changed: read `reason` from navigation state into the reason for visit field; keep the existing doctor, patient, date and slot address handling and the 300-character limit (S-3, S-6).
- CO-3 web AppointmentDetail type (web/src/api/types.ts) — changed: add `doctor_status` and `patient_status` typed as the web `RecordStatus`.
- CO-4 API AppointmentDetail type (api/src/types.ts) — changed: add `doctor_status` and `patient_status` typed as the API `RecordStatus` (C17).
- CO-5 appointmentRepo `DetailRow` and the detail query — changed: select doctor.status and patient.status with the existing doctor and patient joins, then map to the two fields.
- CO-6 AppointmentService — existing: passes the detail through; booking rules unchanged (S-5). If it maps DetailRow to AppointmentDetail, that mapping gains the two fields (implement must confirm).
- CO-7 todayIn — existing: hospital-local today for the book page's default date (S-2 AC2).
- EP-1 GET /api/appointments/:id — changed. Fields: `doctor_status` (RecordStatus string, not null, literal copied from doctor.status, example "INACTIVE", assumed); `patient_status` (RecordStatus string, not null, literal copied from patient.status, example "INACTIVE", assumed).
- EP-2 POST /api/appointments — existing: books as today; the new appointment gets its own ID (S-5).
- EP-3 GET /api/doctors/:id/slots — existing: free slots as today (S-5 AC5).
- EP-4 POST /api/appointments/:id/cancel — existing: if it returns the detail shape, the two fields are added for free; regression-check (A-05).
- EP-5 POST /api/appointments/:id/reschedule — existing: same as EP-4.
- EP-6 POST /api/appointments/:id/complete — existing: same as EP-4.
- EP-7 POST /api/appointments/:id/no-show — existing: same as EP-4.
- EP-8 GET /api/settings — existing: source of the hospital time zone on the web side.

Story mapping: S-1 → CO-1, CO-3, EP-1; S-2 → CO-1, CO-2, CO-7; S-3 → CO-1, CO-2; S-4 → CO-1, CO-3, CO-4, CO-5, EP-1; S-5 → CO-6, EP-2, EP-3; S-6 → CO-1, CO-2, EP-4 to EP-7.

## Known
- S-1, S-4: served by CO-1 with statuses from EP-1 (A-04, A-06).
- S-2: served by CO-1 and CO-2; today's date comes from the book page default via todayIn (C5, A-08).
- S-3: served by CO-1 and CO-2, with `reason` in navigation state (A-07, A-09); only the visit reason is passed, never the cancellation reason.
- S-5: served by the existing EP-2, EP-3 and CO-6; exclusion constraints still guard overlaps (C15).
- S-6: no other change to either page; EP-4 to EP-7 regression-checked (A-05).

## Assumed
- EP-1: the exact stored status literals are not in the pack. The example "INACTIVE" follows the uppercase convention of the existing status columns (C18); implement must confirm the literals and use them in tests.
- CO-3: the web fields reuse the web `RecordStatus` type (C8); implement must confirm its literals.
- CO-4: the API fields reuse the API `RecordStatus` type (C17); implement must confirm its literals.
- CO-6: AppointmentService is assumed to pass the detail through unchanged; if it maps DetailRow to AppointmentDetail, implement adds the two fields there.

## Unknown
None

## Open questions
None open.

Answered in the IDE:
- A-06 (Q-01): field names and format for the statuses in the detail response → "doctor_status/patient_status (proposed default)". Decides EP-1, CO-3, CO-4, CO-5, C7. No mismatch.
- A-07 (Q-02): name of the parameter for the reason for visit → "reason (proposed default)". Decides CO-2, C9; the name is kept as the navigation state key.
- A-08 (C5): the book page already defaults to today in hospital time when no date is given → "Yes, confirm". Decides C5, CO-2, CO-7. Implement must still confirm in code. No mismatch.
- A-09 (Q-03): how Book again hands the reason to the book page → "Navigation state (proposed default)". Decides CO-1, CO-2, C16. Mismatch: A-07 framed `reason` as a URL parameter; A-09 moves it into navigation state.
