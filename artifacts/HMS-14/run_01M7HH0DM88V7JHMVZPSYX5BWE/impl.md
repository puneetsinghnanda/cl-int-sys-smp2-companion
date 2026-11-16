---
schema: change.v1
run_id: run_01M7HH0DM88V7JHMVZPSYX5BWE
step: impl
attempt: 2
skill: implement@1.1.0
inputs:
  - path: artifacts/HMS-14/run_01M7HH0DM88V7JHMVZPSYX5BWE/design.md
    sha256: 4623a1e818ebe4e3423442e8697a9912d8940c0b19ebe9de19fb8b428bc6a035
  - path: artifacts/HMS-14/run_01M7HH0DM88V7JHMVZPSYX5BWE/split.md
    sha256: 88e8cd0212df1bcdf5574b26e6415ef50243df439a57102ca81559c2f68fc854
  - path: context/conventions.md
    sha256: dc53d94ec34eb193eeb06fdd0db19eb86a24c29f7d3c434e7c212d61038ca852
  - path: context/generated/module-map.json
    sha256: 4158c8fcd239c22cb8f1029dd11f61d8839867ff8088bd9ab76197e2db96db4b
pack_sha:
  sample-system: 7681f87749883d71abb4011d118366ca1242e1cc
code:
  - repo: sample-system
    base_sha: 7681f87749883d71abb4011d118366ca1242e1cc
    head_sha: 6024bbc03bdacd61913a128ab9cf8eb42cbc4556
    diff_sha256: 4f9b3791b8d56eff383aa2e793c1b00884b0cebd6ce57effa6db53c7bee8a413
    files:
      - api/src/repositories/appointmentRepo.ts
      - api/src/types.ts
      - api/test/appointments-book-again.test.ts
      - web/src/__tests__/bookAgain.test.ts
      - web/src/api/types.ts
      - web/src/pages/AppointmentDetailPage.tsx
      - web/src/pages/BookAppointmentPage.tsx
resolves:
  - id: F-1
    how: "Regression tests now pin the ACTIVE and INACTIVE status literals on the detail GET and the cancel action, and unit-test bookAgainBlockers."
claims:
  - id: C1
    text: "The record status literals are `'ACTIVE' | 'INACTIVE'` in the web types, confirming the design's assumed INACTIVE example."
    cites: ["sample-system:web/src/api/types.ts#L2"]
  - id: C2
    text: "The API record status type is the same `'ACTIVE' | 'INACTIVE'` union."
    cites: ["sample-system:api/src/types.ts#L1"]
  - id: C3
    text: "The book page already defaults the date to today in hospital time: `params.get('date') ?? todayIn(timeZone)`."
    cites: ["sample-system:web/src/pages/BookAppointmentPage.tsx#L22"]
  - id: C4
    text: "Booking already refuses an Inactive doctor server-side: `this doctor is Inactive and cannot be booked`."
    cites: ["sample-system:api/src/services/appointmentService.ts#L72"]
  - id: C5
    text: "Booking already refuses an Inactive patient server-side: `this patient is Inactive and cannot be booked`."
    cites: ["sample-system:api/src/services/appointmentService.ts#L75"]
  - id: C6
    text: "The server enforces the reason length on booking with `optionalText('reason', 300)`."
    cites: ["sample-system:api/src/services/appointmentService.ts#L66"]
  - id: C7
    text: "AppointmentService.get returns the repository detail unchanged: `new AppointmentRepo(this.db).detail(id)`."
    cites: ["sample-system:api/src/services/appointmentService.ts#L55"]
  - id: C8
    text: "The detail and list reads share one select, `SUMMARY_SELECT`, which already joins doctor and patient."
    cites: ["sample-system:api/src/repositories/appointmentRepo.ts#L21"]
  - id: C9
    text: "The detail query takes the appointment id as a bound parameter: `WHERE a.id = $1`."
    cites: ["sample-system:api/src/repositories/appointmentRepo.ts#L88"]
  - id: C10
    text: "The book page reason field keeps `maxLength={300}`."
    cites: ["sample-system:web/src/pages/BookAppointmentPage.tsx#L109"]
  - id: C11
    text: "The reason for visit is handed over in router `Navigation state (proposed default)`."
    cites: ["runs/run_01M7HH0DM88V7JHMVZPSYX5BWE/ide-answers.md#a-09"]
  - id: C12
    text: "The detail response fields are named `doctor_status/patient_status (proposed default)`."
    cites: ["runs/run_01M7HH0DM88V7JHMVZPSYX5BWE/ide-answers.md#a-06"]
  - id: C13
    text: "When both are Inactive the button shows both reasons: `Show both (proposed default)`."
    cites: ["runs/run_01M7HH0DM88V7JHMVZPSYX5BWE/ide-answers.md#a-01"]
  - id: C14
    text: "The dashboard service also reads `SUMMARY_SELECT`."
    cites: ["sample-system:api/src/services/dashboardService.ts#L6"]
  - id: C15
    text: "The doctor repository also reads `SUMMARY_SELECT`."
    cites: ["sample-system:api/src/repositories/doctorRepo.ts#L3"]
  - id: C16
    text: "The patient repository also reads `SUMMARY_SELECT`."
    cites: ["sample-system:api/src/repositories/patientRepo.ts#L3"]
  - id: C17
    text: "The action routes are regression-checked as readers of the detail shape: `Yes, include actions (proposed default)`."
    cites: ["runs/run_01M7HH0DM88V7JHMVZPSYX5BWE/ide-answers.md#a-05"]
trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10, REQ-11, REQ-12, REQ-13, REQ-14, REQ-15, REQ-16, REQ-17, REQ-18, REQ-19]
known: [S-1, S-2, S-3, S-4, S-5, S-6]
assumed: []
unknown: []
questions: []
summary: "Adds doctor_status and patient_status to the appointment detail (API repo, API and web types) and a Book again button on Cancelled and No-show appointments that opens the book page with doctor and patient in the address and the reason for visit in navigation state; disabled with the applicable Inactive reasons. Booking is unchanged. Regression tests pin the status literals on the detail and cancel responses and the blocker logic."
---
# Change: HMS-14

## Summary
- CO-5: `SUMMARY_SELECT` now also selects `d.status AS doctor_status, p.status AS patient_status` from the joins it already has (C8), and `DetailRow` and `detail()` carry them into the response. The other readers of the shared select are the list reads, the dashboard service, the doctor repository and the patient repository (C14, C15, C16). They map through `toSummary`, which keeps only its own fields, so their responses are unchanged. The query still binds the id as a parameter (C9; threat T-10).
- CO-4 / CO-3: the API and web `AppointmentDetail` types gain `doctor_status` and `patient_status` typed as `RecordStatus` (C12). The literals are `ACTIVE` / `INACTIVE` (C1, C2), which confirms the design's assumed `INACTIVE` example.
- CO-6: no change needed. `AppointmentService.get` returns the repo detail as-is (C7), so the action routes that return the detail get the fields automatically (C17).
- CO-1: `AppointmentDetailPage` shows "Book again" only for `CANCELLED` and `NO_SHOW`. The exported helper `bookAgainBlockers` returns "Doctor is inactive" and/or "Patient is inactive" for any status other than `ACTIVE` (C13; threat T-8). The button is disabled while any blocker applies and shows each reason. When enabled, it navigates to `/appointments/new?doctorId=…&patientId=…` with `{ reason }` in navigation state, only when the original has a reason. No date or slot is passed (C3, C11).
- CO-2: `BookAppointmentPage` starts its reason field from `location.state.reason` when it is a string, cut to 300 characters to match the field's limit (C10). Otherwise it starts empty, so other entry points work as before. The reason stays editable and is still validated by the server (C6; threats T-5, T-7: it is bound as an input value, never as HTML).
- Server-side Active checks already exist on booking (C4, C5), which settles threat-model assumption T-6 without new code.

Resolves:
- F-1: Regression tests now pin the ACTIVE and INACTIVE status literals on the detail GET and the cancel action, and unit-test bookAgainBlockers.

## Changes
- api/src/repositories/appointmentRepo.ts — CO-5: select and map doctor_status and patient_status.
- api/src/types.ts — CO-4: two fields on AppointmentDetail.
- web/src/api/types.ts — CO-3: two fields on AppointmentDetail.
- web/src/pages/AppointmentDetailPage.tsx — CO-1: Book again button, `bookAgainBlockers`, navigation with state.
- web/src/pages/BookAppointmentPage.tsx — CO-2: reason from navigation state.
- api/test/appointments-book-again.test.ts — regression tests the design asks for (EP-1 literals, EP-4 action route):
  - "HMS-14 REQ-9/REQ-10: the detail carries doctor_status and patient_status as ACTIVE"
  - "HMS-14 REQ-17 (A-05): the cancel action returns the same detail shape with both statuses"
  - "HMS-14 REQ-10: an Inactive patient shows as INACTIVE on the original appointment"
  - "HMS-14 REQ-9: an Inactive doctor shows as INACTIVE on the original appointment"
  - "HMS-14 REQ-19: the detail still carries no medical history"
- web/src/__tests__/bookAgain.test.ts — tests for the CO-1 blocker logic using the confirmed literals:
  - "HMS-14 REQ-1/REQ-2: no blocker when doctor and patient are both ACTIVE"
  - "HMS-14 REQ-9", "HMS-14 REQ-10", "HMS-14 REQ-11 (A-01)"

## Known
- S-1: Book again renders only when status is CANCELLED or NO_SHOW.
- S-2: doctor and patient go in the address; the date defaults to hospital-local today (C3); no slot is chosen.
- S-3: the reason comes from navigation state, empty when the original has none; the cancellation reason is never passed; the field stays editable with its 300 limit (C10, C6).
- S-4: `bookAgainBlockers` disables the button and lists each Inactive reason (C13), pinned by the web tests.
- S-5: booking is unchanged; Active, slot and overlap rules stay server-side (C4, C5).
- S-6: other pages and entry points keep working; with no state, the book page behaves as before; the cancel action is regression-checked (C17).

## Assumed
None

## Unknown
None

## Open questions
None
