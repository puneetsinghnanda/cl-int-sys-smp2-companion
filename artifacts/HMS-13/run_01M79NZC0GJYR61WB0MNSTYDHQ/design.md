---
schema: design.v1
run_id: run_01M79NZC0GJYR61WB0MNSTYDHQ
step: design
attempt: 1
skill: design@1.7.0
inputs:
  - path: artifacts/HMS-13/run_01M79NZC0GJYR61WB0MNSTYDHQ/impact.md
    sha256: bd45142e0a0598bb74a129f97375f07f813464619554f9ec0c9b56fd02046bfd
  - path: artifacts/HMS-13/run_01M79NZC0GJYR61WB0MNSTYDHQ/split.md
    sha256: fe24d61d00ae55f25e7c5ca0830c60ca1a538922ee61b9e6460f81033870bd6c
  - path: artifacts/HMS-13/run_01M79NZC0GJYR61WB0MNSTYDHQ/req.md
    sha256: 4ac588db013a5e386b1768e14973edb892b7f1ec0225d0d53ffbbd470f7a8bd2
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
    text: "In this API, `services hold the rules`, while routes only parse input."
    cites: ["context/architecture.md#components"]
  - id: C2
    text: "The appointment detail is served by `AppointmentService` in the API module."
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/services/appointmentService.ts:AppointmentService"]
  - id: C3
    text: "The detail read is `async get(id: number): Promise<AppointmentDetail>` in the appointment service."
    cites: ["sample-system:api/src/services/appointmentService.ts#L54"]
  - id: C4
    text: "The appointment service is constructed with a `clock` it can read for now."
    cites: ["sample-system:api/src/services/appointmentService.ts#L21"]
  - id: C5
    text: "The appointment service already reads the hospital zone as `this.config.timeZone`."
    cites: ["sample-system:api/src/services/appointmentService.ts#L40"]
  - id: C6
    text: "Book ends with `return this.get(id);`, so it returns the detail shape."
    cites: ["sample-system:api/src/services/appointmentService.ts#L83"]
  - id: C7
    text: "Reschedule ends with `return this.get(id);`."
    cites: ["sample-system:api/src/services/appointmentService.ts#L101"]
  - id: C8
    text: "Cancel ends with `return this.get(id);`."
    cites: ["sample-system:api/src/services/appointmentService.ts#L115"]
  - id: C9
    text: "Complete and no-show share close, which ends with `return this.get(id);`."
    cites: ["sample-system:api/src/services/appointmentService.ts#L129"]
  - id: C10
    text: "The doctor repository class is `DoctorRepo`."
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/repositories/doctorRepo.ts:DoctorRepo"]
  - id: C11
    text: "The doctor repository's get returns a `DoctorSummary` or null."
    cites: ["sample-system:api/src/repositories/doctorRepo.ts#L99"]
  - id: C12
    text: "A DoctorSummary carries `status: RecordStatus`."
    cites: ["sample-system:api/src/types.ts#L42"]
  - id: C13
    text: "The doctor's weekly blocks are read by `availability(id: number)` in the doctor repository."
    cites: ["sample-system:api/src/repositories/doctorRepo.ts#L131"]
  - id: C14
    text: "The DOC-7 rule is `withinAvailability` in the slots library."
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/lib/slots.ts:withinAvailability"]
  - id: C15
    text: "The doctor page applies `withinAvailability` to the appointment's start and end in the configured zone."
    cites: ["sample-system:api/src/services/doctorService.ts#L69"]
  - id: C16
    text: "The doctor page considers only Scheduled appointments with `a.starts_at >= $2` (now)."
    cites: ["sample-system:api/src/repositories/doctorRepo.ts#L160"]
  - id: C17
    text: "The API detail type is `AppointmentDetail` in api/src/types.ts."
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/types.ts:AppointmentDetail"]
  - id: C18
    text: "The web detail type is `AppointmentDetail` in web/src/api/types.ts."
    cites: ["context/generated/module-map.json#sym:sample-system:web/src/api/types.ts:AppointmentDetail"]
  - id: C19
    text: "The API declares `RecordStatus` for ACTIVE and INACTIVE."
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/types.ts:RecordStatus"]
  - id: C20
    text: "The web declares `RecordStatus` too."
    cites: ["context/generated/module-map.json#sym:sample-system:web/src/api/types.ts:RecordStatus"]
  - id: C21
    text: "The appointment page component is `AppointmentDetailPage`."
    cites: ["context/generated/module-map.json#sym:sample-system:web/src/pages/AppointmentDetailPage.tsx:AppointmentDetailPage"]
  - id: C22
    text: "The page decides openness with `a.status === 'SCHEDULED'`."
    cites: ["sample-system:web/src/pages/AppointmentDetailPage.tsx#L44"]
  - id: C23
    text: "The page's actions are rendered in a `className=\"actions\"` block."
    cites: ["sample-system:web/src/pages/AppointmentDetailPage.tsx#L73"]
  - id: C24
    text: "After an action the page reloads the detail with `setVersion`."
    cites: ["sample-system:web/src/pages/AppointmentDetailPage.tsx#L35"]
  - id: C25
    text: "The doctor page shows its DOC-7 notice as a `className=\"warning\"` paragraph with role status."
    cites: ["sample-system:web/src/pages/DoctorDetailPage.tsx#L134"]
  - id: C26
    text: "The shared summary query is `SUMMARY_SELECT` in the appointment repository."
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/repositories/appointmentRepo.ts:SUMMARY_SELECT"]
  - id: C27
    text: "After booking, the booking page uses only the returned `appt.id` to navigate."
    cites: ["sample-system:web/src/pages/BookAppointmentPage.tsx#L57"]
  - id: C28
    text: "The booking page component is `BookAppointmentPage`."
    cites: ["context/generated/module-map.json#sym:sample-system:web/src/pages/BookAppointmentPage.tsx:BookAppointmentPage"]
trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9]
known: [S-1, S-2, S-3, S-4, S-5, CO-1, CO-2, CO-3, CO-4, CO-5, CO-6, CO-7, CO-8, CO-9, EP-1, EP-2, EP-3, EP-4, EP-5, EP-6]
assumed: []
unknown: []
questions: []
components:
  - id: CO-1
    name: AppointmentService.get (computes doctorStatus and outsideAvailability)
    module: module:sample-system:api
    change: changed
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/services/appointmentService.ts:AppointmentService", "sample-system:api/src/services/appointmentService.ts#L54", "runs/run_01M79NZC0GJYR61WB0MNSTYDHQ/ide-answers.md#a-05", "runs/run_01M79NZC0GJYR61WB0MNSTYDHQ/ide-answers.md#a-08", "runs/run_01M79NZC0GJYR61WB0MNSTYDHQ/ide-answers.md#a-01", "runs/run_01M79NZC0GJYR61WB0MNSTYDHQ/ide-answers.md#a-02"]
  - id: CO-2
    name: AppointmentDetail type (API)
    module: module:sample-system:api
    change: changed
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/types.ts:AppointmentDetail", "runs/run_01M79NZC0GJYR61WB0MNSTYDHQ/ide-answers.md#a-06", "runs/run_01M79NZC0GJYR61WB0MNSTYDHQ/ide-answers.md#a-07"]
  - id: CO-3
    name: DoctorRepo.get and DoctorRepo.availability
    module: module:sample-system:api
    change: existing
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/repositories/doctorRepo.ts:DoctorRepo", "sample-system:api/src/repositories/doctorRepo.ts#L99", "sample-system:api/src/repositories/doctorRepo.ts#L131"]
  - id: CO-4
    name: withinAvailability (DOC-7 rule)
    module: module:sample-system:api
    change: existing
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/lib/slots.ts:withinAvailability", "runs/run_01M79NZC0GJYR61WB0MNSTYDHQ/ide-answers.md#a-02"]
  - id: CO-5
    name: AppointmentRepo.detail, SUMMARY_SELECT and AppointmentSummary
    module: module:sample-system:api
    change: existing
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/repositories/appointmentRepo.ts:SUMMARY_SELECT", "runs/run_01M79NZC0GJYR61WB0MNSTYDHQ/ide-answers.md#a-06"]
  - id: CO-6
    name: AppointmentDetail type (web)
    module: module:sample-system:web
    change: changed
    cites: ["context/generated/module-map.json#sym:sample-system:web/src/api/types.ts:AppointmentDetail", "runs/run_01M79NZC0GJYR61WB0MNSTYDHQ/ide-answers.md#a-07"]
  - id: CO-7
    name: AppointmentDetailPage notices
    module: module:sample-system:web
    change: changed
    cites: ["context/generated/module-map.json#sym:sample-system:web/src/pages/AppointmentDetailPage.tsx:AppointmentDetailPage", "runs/run_01M79NZC0GJYR61WB0MNSTYDHQ/ide-answers.md#a-03", "runs/run_01M79NZC0GJYR61WB0MNSTYDHQ/ide-answers.md#a-04", "runs/run_01M79NZC0GJYR61WB0MNSTYDHQ/ide-answers.md#a-07"]
  - id: CO-8
    name: DoctorService.get DOC-7 flags and DoctorDetailPage
    module: module:sample-system:api
    change: existing
    cites: ["sample-system:api/src/services/doctorService.ts#L69", "context/generated/module-map.json#sym:sample-system:web/src/pages/DoctorDetailPage.tsx:DoctorDetailPage"]
  - id: CO-9
    name: BookAppointmentPage
    module: module:sample-system:web
    change: existing
    cites: ["context/generated/module-map.json#sym:sample-system:web/src/pages/BookAppointmentPage.tsx:BookAppointmentPage", "sample-system:web/src/pages/BookAppointmentPage.tsx#L57"]
endpoints:
  - id: EP-1
    method: GET
    path: /api/appointments/:id
    change: changed
    fields:
      - {name: doctorStatus, type: "RecordStatus ('ACTIVE' | 'INACTIVE')", nullable: false, format: "enum string ACTIVE or INACTIVE, the doctor's current status", example: "INACTIVE", status: known}
      - {name: outsideAvailability, type: boolean, nullable: false, format: "true only when status is SCHEDULED, startsAt >= now and withinAvailability(doctor's blocks, startsAt, endsAt, hospital zone) is false; otherwise false", example: true, status: known}
    cites: ["context/generated/api-inventory.json#http:GET:/api/appointments/:id", "runs/run_01M79NZC0GJYR61WB0MNSTYDHQ/ide-answers.md#a-05", "runs/run_01M79NZC0GJYR61WB0MNSTYDHQ/ide-answers.md#a-07", "runs/run_01M79NZC0GJYR61WB0MNSTYDHQ/ide-answers.md#a-08"]
  - id: EP-2
    method: POST
    path: /api/appointments
    change: changed
    fields:
      - {name: doctorStatus, type: "RecordStatus ('ACTIVE' | 'INACTIVE')", nullable: false, format: "same as EP-1", example: "ACTIVE", status: known}
      - {name: outsideAvailability, type: boolean, nullable: false, format: "same as EP-1", example: false, status: known}
    cites: ["context/generated/api-inventory.json#http:POST:/api/appointments", "sample-system:api/src/services/appointmentService.ts#L83", "runs/run_01M79NZC0GJYR61WB0MNSTYDHQ/ide-answers.md#a-07"]
  - id: EP-3
    method: POST
    path: /api/appointments/:id/reschedule
    change: changed
    fields:
      - {name: doctorStatus, type: "RecordStatus ('ACTIVE' | 'INACTIVE')", nullable: false, format: "same as EP-1", example: "ACTIVE", status: known}
      - {name: outsideAvailability, type: boolean, nullable: false, format: "same as EP-1", example: false, status: known}
    cites: ["context/generated/api-inventory.json#http:POST:/api/appointments/:id/reschedule", "sample-system:api/src/services/appointmentService.ts#L101", "runs/run_01M79NZC0GJYR61WB0MNSTYDHQ/ide-answers.md#a-07"]
  - id: EP-4
    method: POST
    path: /api/appointments/:id/cancel
    change: changed
    fields:
      - {name: doctorStatus, type: "RecordStatus ('ACTIVE' | 'INACTIVE')", nullable: false, format: "same as EP-1", example: "INACTIVE", status: known}
      - {name: outsideAvailability, type: boolean, nullable: false, format: "same as EP-1; always false here because the result is CANCELLED", example: false, status: known}
    cites: ["context/generated/api-inventory.json#http:POST:/api/appointments/:id/cancel", "sample-system:api/src/services/appointmentService.ts#L115", "runs/run_01M79NZC0GJYR61WB0MNSTYDHQ/ide-answers.md#a-07", "runs/run_01M79NZC0GJYR61WB0MNSTYDHQ/ide-answers.md#a-08"]
  - id: EP-5
    method: POST
    path: /api/appointments/:id/complete
    change: changed
    fields:
      - {name: doctorStatus, type: "RecordStatus ('ACTIVE' | 'INACTIVE')", nullable: false, format: "same as EP-1", example: "ACTIVE", status: known}
      - {name: outsideAvailability, type: boolean, nullable: false, format: "same as EP-1; always false here because the result is COMPLETED", example: false, status: known}
    cites: ["context/generated/api-inventory.json#http:POST:/api/appointments/:id/complete", "sample-system:api/src/services/appointmentService.ts#L129", "runs/run_01M79NZC0GJYR61WB0MNSTYDHQ/ide-answers.md#a-07", "runs/run_01M79NZC0GJYR61WB0MNSTYDHQ/ide-answers.md#a-08"]
  - id: EP-6
    method: POST
    path: /api/appointments/:id/no-show
    change: changed
    fields:
      - {name: doctorStatus, type: "RecordStatus ('ACTIVE' | 'INACTIVE')", nullable: false, format: "same as EP-1", example: "ACTIVE", status: known}
      - {name: outsideAvailability, type: boolean, nullable: false, format: "same as EP-1; always false here because the result is NO_SHOW", example: false, status: known}
    cites: ["context/generated/api-inventory.json#http:POST:/api/appointments/:id/no-show", "sample-system:api/src/services/appointmentService.ts#L129", "runs/run_01M79NZC0GJYR61WB0MNSTYDHQ/ide-answers.md#a-07", "runs/run_01M79NZC0GJYR61WB0MNSTYDHQ/ide-answers.md#a-08"]
answers:
  - id: A-07
    kind: question
    about: Q-01
    answer: "doctorStatus + outsideAvailability (proposed default)"
    decides: [EP-1, EP-2, EP-3, EP-4, EP-5, EP-6, CO-2, CO-6, CO-7]
  - id: A-08
    kind: question
    about: Q-02
    answer: "false (proposed default)"
    decides: [EP-1, EP-4, EP-5, EP-6, CO-1]
---
# Design: HMS-13

## Overview
The API computes both facts and the page only renders them. `AppointmentService.get` (CO-1) already builds the detail for GET /api/appointments/:id (C3). After reading the detail it loads the doctor with DoctorRepo.get (C11) to get `doctorStatus` (C12). For a Scheduled appointment whose start is not before now, it also loads the doctor's blocks with `availability(id)` (C13) and sets `outsideAvailability` to the negation of `withinAvailability` (C14). It uses the service's `clock` (C4) and the hospital zone `this.config.timeZone` (C5), which is exactly the doctor page's rule (C15, C16) without its list cap (A-02). Every other case returns `false` (A-08). The page (CO-7) shows the availability notice when `outsideAvailability` is true. It shows the Inactive notice when the appointment is Scheduled (C22) and `doctorStatus` is INACTIVE. Both render in that order above the actions block (C23, A-04), styled like the doctor page's notices (C25).

Trade-offs:
- The service makes up to two extra small queries per detail read instead of extending the shared `SUMMARY_SELECT` (C26). This keeps every summary response unchanged (A-06), at a small cost per detail read.
- Because book, reschedule, cancel, complete and no-show all return `this.get(id)` (C6, C7, C8, C9), they gain the two fields too (EP-2 to EP-6). This is additive. The appointment page ignores these responses and reloads (C24), and the booking page reads only `appt.id` (C27), so no consumer changes.
- `outsideAvailability` is a boolean while the doctor detail's DOC-7 field of the same name is an id list. The name was chosen in A-07, and it lives on a different type.
- Computing in the service, not the route, follows the layering (C1).

Story coverage:
- S-1 (outside-availability notice): CO-1, CO-4, CO-7, EP-1.
- S-2 (Inactive notice): CO-1, CO-3, CO-7, EP-1.
- S-3 (both, in order): CO-7.
- S-4 (closed show none): CO-1 (false for closed) and CO-7 (the Inactive notice only when Scheduled).
- S-5 (rest unchanged): CO-5, CO-7, CO-8, CO-9.

## Components
- CO-1 AppointmentService.get (api, changed): adds `doctorStatus` from DoctorRepo.get. Adds `outsideAvailability` = status is SCHEDULED, startsAt >= clock(), and not `withinAvailability(blocks, startsAt, endsAt, config.timeZone)`; otherwise `false`.
- CO-2 AppointmentDetail type in api/src/types.ts (api, changed): gains `doctorStatus: RecordStatus` (C19) and `outsideAvailability: boolean`. AppointmentSummary does not change.
- CO-3 DoctorRepo.get and DoctorRepo.availability (api, existing): reused as they are.
- CO-4 withinAvailability (api, existing): the DOC-7 rule, reused unchanged.
- CO-5 AppointmentRepo.detail, SUMMARY_SELECT and AppointmentSummary (api, existing): unchanged (A-06).
- CO-6 AppointmentDetail type in web/src/api/types.ts (web, changed): gains `doctorStatus: RecordStatus` (C20) and `outsideAvailability: boolean`, mirroring CO-2.
- CO-7 AppointmentDetailPage (web, changed): inside the Scheduled branch, above the actions block, it renders `<p className="warning" role="status">` notices. The first shows "This appointment is outside the doctor's current availability. Reschedule or cancel it." when `outsideAvailability` is true. The second shows "The doctor is Inactive. Cancel this appointment or book the patient with another doctor." when `doctorStatus` is INACTIVE. Nothing else on the page changes.
- CO-8 DoctorService.get DOC-7 flags and DoctorDetailPage (existing): unchanged, including the 50-appointment cap on its list.
- CO-9 BookAppointmentPage (web, existing): unchanged. It uses only the returned id.
- EP-1 GET /api/appointments/:id (changed): fields `doctorStatus` (RecordStatus, not null, ACTIVE or INACTIVE, example "INACTIVE") and `outsideAvailability` (boolean, not null; true only for a not-yet-started Scheduled appointment outside every block in hospital time, otherwise false; example true).
- EP-2 POST /api/appointments (changed): the same two fields as EP-1, because it returns the detail (C6).
- EP-3 POST /api/appointments/:id/reschedule (changed): the same two fields as EP-1 (C7).
- EP-4 POST /api/appointments/:id/cancel (changed): the same two fields. `outsideAvailability` is always false because the result is CANCELLED (C8).
- EP-5 POST /api/appointments/:id/complete (changed): the same two fields. `outsideAvailability` is always false because the result is COMPLETED (C9).
- EP-6 POST /api/appointments/:id/no-show (changed): the same two fields. `outsideAvailability` is always false because the result is NO_SHOW (C9).

No new endpoint, table, migration or infrastructure.

## Known
- S-1 to S-5: each is served by the components listed under Story coverage.
- CO-1: the method exists at the cited line (C3), and the rule and its inputs exist (C4, C5, C11, C13, C14).
- CO-2: the type is declared per the sym fact (C17), and RecordStatus exists (C19).
- CO-3, CO-4, CO-5, CO-8, CO-9: existing code at the cited lines and sym facts, read but not changed.
- CO-6: the type is declared per the sym fact (C18), and RecordStatus exists (C20).
- CO-7: the component is declared per the sym fact (C21). The placement anchor is the actions block (C23).
- EP-1 to EP-6: the routes exist in the api inventory. The field names come from A-07 and the false default from A-08.

## Assumed
None.

## Unknown
None.

## Open questions
None open.

Answered in the IDE:
- A-07 (Q-01): what names and shape should the two new detail fields have? Answer: "doctorStatus + outsideAvailability (proposed default)". Decides EP-1 to EP-6, CO-2, CO-6 and CO-7. No mismatch with the inputs.
- A-08 (Q-02): what should the flag return for closed or already-started appointments? Answer: "false (proposed default)". Decides EP-1, EP-4, EP-5, EP-6 and CO-1. No mismatch with the inputs.
