---
schema: change.v1
run_id: run_01M820V4J0VSJHKNZZ5VK0BT2Y
step: impl
attempt: 1
skill: implement@1.3.0
inputs:
  - path: artifacts/HMS-31/run_01M820V4J0VSJHKNZZ5VK0BT2Y/design.md
    sha256: 57bbf0a2dd2e60cb54a59844370213fbf80c395641936daf17a0392744736cc6
  - path: artifacts/HMS-31/run_01M820V4J0VSJHKNZZ5VK0BT2Y/split.md
    sha256: bd26e52b86c31239358a3a893ff920c053cf6830e2406107bd5bda4c61c58536
  - path: context/conventions.md
    sha256: dc53d94ec34eb193eeb06fdd0db19eb86a24c29f7d3c434e7c212d61038ca852
  - path: context/generated/module-map.json
    sha256: 4158c8fcd239c22cb8f1029dd11f61d8839867ff8088bd9ab76197e2db96db4b
pack_sha:
  sample-system: 7681f87749883d71abb4011d118366ca1242e1cc
code:
  - repo: sample-system
    base_sha: 7681f87749883d71abb4011d118366ca1242e1cc
    head_sha: 199a78b6a91a667b2c9115cd2ddc1c0138e4d1a7
    diff_sha256: 1d2712d739573629b8da05c8ecaff9dae434f2221eb7b7a5c260099f7e496607
    files:
      - api/src/repositories/appointmentRepo.ts
      - api/src/types.ts
      - web/src/api/types.ts
      - web/src/pages/AppointmentDetailPage.tsx
claims:
  - id: C1
    text: "The detail read is built in `AppointmentRepo`, which now adds the doctor's specialization, department and fee."
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/repositories/appointmentRepo.ts:AppointmentRepo", "artifacts/HMS-31/run_01M820V4J0VSJHKNZZ5VK0BT2Y/design.md#item:CO-1"]
  - id: C2
    text: "The API type `AppointmentDetail` now carries an extended doctor object."
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/types.ts:AppointmentDetail", "artifacts/HMS-31/run_01M820V4J0VSJHKNZZ5VK0BT2Y/design.md#item:CO-2"]
  - id: C3
    text: "The extended doctor reuses the specialization, department and feePaise shapes of `DoctorSummary`."
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/types.ts:DoctorSummary", "runs/run_01M820V4J0VSJHKNZZ5VK0BT2Y/ide-answers.md#a-02"]
  - id: C4
    text: "The shared `SUMMARY_SELECT` is unchanged, so the appointment lists and the locked read keep their shape."
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/repositories/appointmentRepo.ts:SUMMARY_SELECT"]
  - id: C5
    text: "The web type `AppointmentDetail` mirrors the API change."
    cites: ["context/generated/module-map.json#sym:sample-system:web/src/api/types.ts:AppointmentDetail", "artifacts/HMS-31/run_01M820V4J0VSJHKNZZ5VK0BT2Y/design.md#item:CO-5"]
  - id: C6
    text: "`AppointmentDetailPage` shows Specialization, Department and Consultation fee below Doctor."
    cites: ["context/generated/module-map.json#sym:sample-system:web/src/pages/AppointmentDetailPage.tsx:AppointmentDetailPage", "artifacts/HMS-31/run_01M820V4J0VSJHKNZZ5VK0BT2Y/design.md#item:CO-6"]
  - id: C7
    text: "The fee is formatted with the existing `rupees` helper, as on the doctors list."
    cites: ["context/generated/module-map.json#sym:sample-system:web/src/lib/format.ts:rupees"]
  - id: C8
    text: "An Inactive specialization or department is shown with its name followed by `(Inactive)`."
    cites: ["runs/run_01M820V4J0VSJHKNZZ5VK0BT2Y/ide-answers.md#a-01"]
  - id: C9
    text: "Logs never carry request bodies, `contact details` or medical history; this change adds no logging."
    cites: ["context/conventions.md#logging-and-data"]
trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10, REQ-11]
known: [CO-1, CO-2, CO-5, CO-6]
assumed: []
unknown:
  - {id: U-1, blocking: false}
questions: []
summary: "The appointment detail now carries the doctor's current specialization, department and fee (in paise), read live with a detail-only join. The appointment page shows them below Doctor, adds '(Inactive)' after an Inactive name, and formats the fee with rupees. Lists, contact details and medical history are untouched."
---
# Change: HMS-31

## Summary
- CO-1: `AppointmentRepo.detail` runs one more query for the appointment's doctor. It joins
  `specialization` and `department` and selects only their id, name and status plus `fee_paise`;
  it never selects phone, email or anything about the patient. The result is merged into `doctor`.
  `SUMMARY_SELECT`, `list` and `get` are unchanged (C4). So the lists, the locked read used by the
  actions, and the rest of the page stay the same (REQ-11).
- CO-2 / CO-5: the API and web `AppointmentDetail` types gain `doctor: AppointmentDetailDoctor`,
  which is the summary doctor plus `specialization`, `department` and `feePaise` picked from
  `DoctorSummary` (A-02). `AppointmentSummary` is not touched.
- CO-6: `AppointmentDetailPage` adds the rows "Specialization", "Department" and "Consultation fee"
  right after "Doctor". The fee goes through `rupees` (REQ-4). A name whose status is INACTIVE is
  followed by " (Inactive)" (A-01, REQ-7). The doctor link, the other rows, the actions and the
  history are unchanged (REQ-8, REQ-11).
- The values are read at request time, so closed appointments and Inactive doctors show the current
  details (REQ-5, REQ-6). The action endpoints (EP-2 to EP-6) return the same detail through the
  unchanged service, as the design says.
- No tests were added: the design asks for no regression test, and no existing test pins the changed
  detail shape that I could find. Broad tests are the unit-test step's job.

## Changes
- api/src/repositories/appointmentRepo.ts — CO-1: detail-only doctor query and merge into `doctor`.
- api/src/types.ts — CO-2: `AppointmentDetailDoctor` type and the `doctor` override on `AppointmentDetail`.
- web/src/api/types.ts — CO-5: the same type change for the web app.
- web/src/pages/AppointmentDetailPage.tsx — CO-6: three new rows, `refName` for the "(Inactive)" marker, fee via `rupees`.

## Known
- CO-1: built as the design's CO-1; the location is from the module map (C1).
- CO-2: built as the design's CO-2 with the shape decided in A-02 (C2, C3).
- CO-5: built as the design's CO-5 (C5).
- CO-6: built as the design's CO-6, with the marker decided in A-01 (C6, C7, C8).

## Assumed
None.

## Unknown
- U-1 (non-blocking): the glossary has no entry for Specialization, Department or Consultation fee. This is already open as runs/run_01M820V4J0VSJHKNZZ5VK0BT2Y/open-questions.md#q-02 and does not affect the code.

## Open questions
None new. runs/run_01M820V4J0VSJHKNZZ5VK0BT2Y/open-questions.md#q-02 stays open (non-blocking, tech lead).
