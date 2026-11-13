---
schema: change.v1
run_id: run_01M79NZC0GJYR61WB0MNSTYDHQ
step: impl
attempt: 1
skill: implement@1.1.0
inputs:
  - path: artifacts/HMS-13/run_01M79NZC0GJYR61WB0MNSTYDHQ/design.md
    sha256: 385aa8939befa99e8d415e886be5a4faac02d8447880d56dc1a4477012f582fb
  - path: artifacts/HMS-13/run_01M79NZC0GJYR61WB0MNSTYDHQ/split.md
    sha256: fe24d61d00ae55f25e7c5ca0830c60ca1a538922ee61b9e6460f81033870bd6c
  - path: context/conventions.md
    sha256: dc53d94ec34eb193eeb06fdd0db19eb86a24c29f7d3c434e7c212d61038ca852
  - path: context/generated/module-map.json
    sha256: 4158c8fcd239c22cb8f1029dd11f61d8839867ff8088bd9ab76197e2db96db4b
pack_sha:
  sample-system: 7681f87749883d71abb4011d118366ca1242e1cc
claims:
  - id: C1
    text: "The design changes `AppointmentService.get` to add doctorStatus and outsideAvailability."
    cites: ["artifacts/HMS-13/run_01M79NZC0GJYR61WB0MNSTYDHQ/design.md#item:CO-1"]
  - id: C2
    text: "GET /api/appointments/:id gains the field `doctorStatus`, plus outsideAvailability."
    cites: ["artifacts/HMS-13/run_01M79NZC0GJYR61WB0MNSTYDHQ/design.md#item:EP-1"]
  - id: C3
    text: "The page change is the `AppointmentDetailPage notices` component."
    cites: ["artifacts/HMS-13/run_01M79NZC0GJYR61WB0MNSTYDHQ/design.md#item:CO-7"]
  - id: C4
    text: "The design keeps the shared `SUMMARY_SELECT` and AppointmentSummary unchanged."
    cites: ["artifacts/HMS-13/run_01M79NZC0GJYR61WB0MNSTYDHQ/design.md#item:CO-5"]
  - id: C5
    text: "The availability rule reused is `withinAvailability` in the slots library."
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/lib/slots.ts:withinAvailability"]
  - id: C6
    text: "The doctor repository used for status and blocks is `DoctorRepo`."
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/repositories/doctorRepo.ts:DoctorRepo"]
  - id: C7
    text: "The appointment repository whose detail return type is narrowed is `AppointmentRepo`."
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/repositories/appointmentRepo.ts:AppointmentRepo"]
  - id: C8
    text: "The flag covers only appointments that have not started, as on the doctor page (`starts_at >= now`)."
    cites: ["runs/run_01M79NZC0GJYR61WB0MNSTYDHQ/ide-answers.md#a-01"]
  - id: C9
    text: "The doctor page's `50-appointment limit` is not copied."
    cites: ["runs/run_01M79NZC0GJYR61WB0MNSTYDHQ/ide-answers.md#a-02"]
  - id: C10
    text: "The outside-availability notice comes first and the Inactive notice second, both above the actions (`outside-availability notice comes first`)."
    cites: ["runs/run_01M79NZC0GJYR61WB0MNSTYDHQ/ide-answers.md#a-04"]
  - id: C11
    text: "Closed or already-started appointments get `false (proposed default)` for the flag."
    cites: ["runs/run_01M79NZC0GJYR61WB0MNSTYDHQ/ide-answers.md#a-08"]
  - id: C12
    text: "Logs carry `method, route and error message only`, and this change adds no logging."
    cites: ["context/conventions.md#logging-and-data"]
  - id: C13
    text: "The API type changed is `AppointmentDetail` in api/src/types.ts."
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/types.ts:AppointmentDetail"]
  - id: C14
    text: "The web type changed is `AppointmentDetail` in web/src/api/types.ts."
    cites: ["context/generated/module-map.json#sym:sample-system:web/src/api/types.ts:AppointmentDetail"]
trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9]
known: [CO-1, CO-2, CO-6, CO-7, EP-1, EP-2, EP-3, EP-4, EP-5, EP-6]
assumed: [CO-5]
unknown: []
questions: []
code:
  - repo: sample-system
    base_sha: 7681f87749883d71abb4011d118366ca1242e1cc
    head_sha: 1bec4f1ab87cd774949dcbf064c7e4d215c0b3f2
    diff_sha256: 1cdd91da50b7e2e3213df50391a66ba3db4d3dc797fc6c31b7b6b8d59c4d40f1
    files:
      - api/src/repositories/appointmentRepo.ts
      - api/src/services/appointmentService.ts
      - api/src/types.ts
      - api/test/appointments-fit.test.ts
      - web/src/api/types.ts
      - web/src/pages/AppointmentDetailPage.tsx
summary: "The appointment detail now carries doctorStatus and outsideAvailability, computed in AppointmentService.get with the existing DOC-7 rule. The appointment page shows the two fixed notices above the actions, in order, only for Scheduled appointments."
---
# Change: HMS-13

## Summary
- CO-1 `AppointmentService.get` (C1) now loads the doctor through `DoctorRepo` (C6) and copies only its status into `doctorStatus`. This is the T-1 control: no other doctor field enters the response.
- CO-1 sets `outsideAvailability` to true only when all three hold:
  - the appointment is SCHEDULED;
  - its start is at or after the service clock's now (C8);
  - `withinAvailability` (C5) is false for the doctor's blocks, its start and end, and the configured hospital zone.

  There is no list cap (C9). Every other case gets false (C11).
- CO-1 handles the doctor lookup like reschedule, because doctors with appointments are never deleted.
- The book, reschedule, cancel, complete and no-show responses (EP-2 to EP-6) return `this.get(id)`, so they gain the same two fields with no further code.
- CO-2 and CO-6 add both fields to the API and web `AppointmentDetail` types (C13, C14).
- CO-7 renders the two fixed texts as `warning` paragraphs above the actions, outside-availability first and Inactive second (C10). Both appear only when the appointment is Scheduled (C3).
- SUMMARY_SELECT and AppointmentSummary are untouched (C4). No logging was added (C12).

## Changes
- api/src/services/appointmentService.ts: CO-1. `get` adds doctorStatus and outsideAvailability; withinAvailability is imported from the slots library.
- api/src/types.ts: CO-2. `AppointmentDetail` gains `doctorStatus: RecordStatus` and `outsideAvailability: boolean`.
- api/src/repositories/appointmentRepo.ts: CO-5. Type-only change: `detail` now returns `Omit<AppointmentDetail, 'doctorStatus' | 'outsideAvailability'>`, since the service adds those fields. The SQL and SUMMARY_SELECT are unchanged.
- web/src/api/types.ts: CO-6. `AppointmentDetail` gains the same two fields.
- web/src/pages/AppointmentDetailPage.tsx: CO-7. Two fixed-text notices above the actions block, shown only when Scheduled.
- api/test/appointments-fit.test.ts: regression tests for the T-1 and T-5 controls:
  - "HMS-13 REQ-1 REQ-2" (flag matches DOC-7);
  - "HMS-13 REQ-3" (no flag after the start);
  - "HMS-13 REQ-5 REQ-7" (Inactive status alongside the flag);
  - "HMS-13 REQ-9" (only the status is added and the doctor object is unchanged);
  - "HMS-13 REQ-8" (a Cancelled appointment carries no flag).

## Known
- CO-1: built as the design states (C1), with the rule (C5), the window (C8), no cap (C9) and the false default (C11).
- CO-2: the API type is declared per the sym fact (C13).
- CO-6: the web type is declared per the sym fact (C14).
- CO-7: notices in the confirmed order (C10), shown only for Scheduled appointments (C3).
- EP-1: GET /api/appointments/:id returns both fields (C2).
- EP-2, EP-3, EP-4, EP-5, EP-6: book, reschedule, cancel, complete and no-show return the service's get, so each carries the same fields.

## Assumed
- CO-5: the design marks AppointmentRepo.detail as existing (C4). Its declared return type had to be narrowed (C7) so the code compiles once AppointmentDetail gains the two required fields. Basis: the change is type-only, and the query, SUMMARY_SELECT and runtime output are unchanged.

## Unknown
None.

## Open questions
None.
