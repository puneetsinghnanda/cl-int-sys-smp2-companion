---
schema: design.v1
run_id: run_01M77BM48R93EMY13TZXCDN7PZ
step: design
attempt: 1
skill: design@1.7.0
inputs:
  - path: artifacts/HMS-11/run_01M77BM48R93EMY13TZXCDN7PZ/impact.md
    sha256: b3712ec525363e682a68725a8ce7c97e0c2a43155dd944473156ef7e4c4173fd
  - path: artifacts/HMS-11/run_01M77BM48R93EMY13TZXCDN7PZ/split.md
    sha256: d4dc5ed961e49e2c861c4e3256457f48726f4adc5af1ce2363cc97491f9a4d26
  - path: artifacts/HMS-11/run_01M77BM48R93EMY13TZXCDN7PZ/req.md
    sha256: 9020c22eea6b4e433f7a024deb321a42e4042b10165a1d67058dd3d9e7f1fc2c
  - path: context/intent-baseline.md
    sha256: 9bfd821aeb2fb9524863e346707e049e4fcfb949a6e55b90223a79194946a215
  - path: context/architecture.md
    sha256: 9faab093d10933a46f6473b25c80728f8745e597100e650cd2537bbafb2b595a
  - path: context/generated/module-map.json
    sha256: 4158c8fcd239c22cb8f1029dd11f61d8839867ff8088bd9ab76197e2db96db4b
  - path: context/generated/api-inventory.json
    sha256: ba024ab1b4b4151d26ce1afa77bd2dab1cec645ea737ce37f98e0e8674f795e9
pack_sha:
  sample-system: c2709e164850ab7e177e4bccfc42a5dd9b961fdc
claims:
  - id: C1
    text: "The specializations routes live on `specializationsRouter`."
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/routes/specializations.ts:specializationsRouter"]
  - id: C2
    text: "The existing list, create, rename and delete handlers come from the shared `referenceHandlers` and stay as they are."
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/routes/reference.ts:referenceHandlers"]
  - id: C3
    text: "The day-schedule slot-state rule, also used by the HMS-9 next-7-days summary, lives in `DoctorService`."
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/services/doctorService.ts:DoctorService"]
  - id: C4
    text: "The day schedule answers with a `DaySchedule` whose slots carry the FREE, BOOKED or PAST state."
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/services/doctorService.ts:DaySchedule"]
  - id: C5
    text: "Doctor rows, availability and reference status are read through `DoctorRepo`."
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/repositories/doctorRepo.ts:DoctorRepo"]
  - id: C6
    text: "DoctorRepo already reads a specialization's status, or null when it does not exist, with `refStatus`."
    cites: ["sample-system:api/src/repositories/doctorRepo.ts#L167"]
  - id: C7
    text: "Appointments that occupy slots are read through `AppointmentRepo`."
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/repositories/appointmentRepo.ts:AppointmentRepo"]
  - id: C8
    text: "A doctor's slots for one hospital-local day, with their HH:MM label, come from `daySlots`."
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/lib/slots.ts:daySlots"]
  - id: C9
    text: "Calendar days are stepped with `addDays`."
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/lib/time.ts:addDays"]
  - id: C10
    text: "Today in the hospital zone comes from `localDay`."
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/lib/time.ts:localDay"]
  - id: C11
    text: "Hospital-local day bounds come from `dayBounds`."
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/lib/time.ts:dayBounds"]
  - id: C12
    text: "Unknown ids answer 404 through `notFound`."
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/lib/errors.ts:notFound"]
  - id: C13
    text: "The Inactive case answers 400 through `badRequest`."
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/lib/errors.ts:badRequest"]
  - id: C14
    text: "Each route declares its permission with `requirePermission`."
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/auth/identity.ts:requirePermission"]
  - id: C15
    text: "Doctor codes are produced by `formatCode`."
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/lib/codes.ts:formatCode"]
  - id: C16
    text: "Shared response types are declared in api/src/types.ts, for example `DoctorSummary`."
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/types.ts:DoctorSummary"]
  - id: C17
    text: "The new route is not in the api inventory; impact lists it as new: `earliest-slots`."
    cites: ["artifacts/HMS-11/run_01M77BM48R93EMY13TZXCDN7PZ/impact.md#claim:C1"]
  - id: C18
    text: "Each entry is flat, with `departmentName` as a string, as the user chose."
    cites: ["runs/run_01M77BM48R93EMY13TZXCDN7PZ/ide-answers.md#a-03"]
  - id: C19
    text: "The new route requires `appointments:read`, the same as the day schedule, as the user chose."
    cites: ["runs/run_01M77BM48R93EMY13TZXCDN7PZ/ide-answers.md#a-04"]
  - id: C20
    text: "Slots and today use the hospital zone `HOSPITAL_TZ`."
    cites: ["context/architecture.md#time-and-slots"]
  - id: C21
    text: "The window is `today to today+6`."
    cites: ["artifacts/HMS-11/run_01M77BM48R93EMY13TZXCDN7PZ/req.md#item:REQ-2"]
  - id: C22
    text: "The response has `no paging`."
    cites: ["artifacts/HMS-11/run_01M77BM48R93EMY13TZXCDN7PZ/req.md#item:REQ-10"]
  - id: C23
    text: "Name ties compare ignoring case, with `doctor id as the final tie-break`."
    cites: ["artifacts/HMS-11/run_01M77BM48R93EMY13TZXCDN7PZ/req.md#item:REQ-17"]
  - id: C24
    text: "Routes reach services through the `Services` registry."
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/services/index.ts:Services"]
trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10, REQ-11, REQ-12, REQ-13, REQ-14, REQ-15, REQ-16, REQ-17]
known: [S-1, S-2, S-3, S-4, S-5, S-6]
assumed: []
unknown: []
questions: []
components:
  - id: CO-1
    name: "specializationsRouter: GET /:id/earliest-slots"
    module: module:sample-system:api
    change: changed
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/routes/specializations.ts:specializationsRouter", "runs/run_01M77BM48R93EMY13TZXCDN7PZ/ide-answers.md#a-04"]
  - id: CO-2
    name: "DoctorService.earliestSlots"
    module: module:sample-system:api
    change: changed
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/services/doctorService.ts:DoctorService"]
  - id: CO-3
    name: "DoctorRepo.activeBySpecialization"
    module: module:sample-system:api
    change: changed
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/repositories/doctorRepo.ts:DoctorRepo"]
  - id: CO-4
    name: "DoctorRepo.refStatus and availability"
    module: module:sample-system:api
    change: existing
    cites: ["sample-system:api/src/repositories/doctorRepo.ts#L167", "context/generated/module-map.json#sym:sample-system:api/src/repositories/doctorRepo.ts:DoctorRepo"]
  - id: CO-5
    name: "AppointmentRepo.occupyingBetween"
    module: module:sample-system:api
    change: existing
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/repositories/appointmentRepo.ts:AppointmentRepo"]
  - id: CO-6
    name: "EarliestSlotEntry type in api/src/types.ts"
    module: module:sample-system:api
    change: new
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/types.ts:DoctorSummary", "runs/run_01M77BM48R93EMY13TZXCDN7PZ/ide-answers.md#a-03"]
  - id: CO-7
    name: "Slot, time and error helpers"
    module: module:sample-system:api
    change: existing
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/lib/slots.ts:daySlots", "context/generated/module-map.json#sym:sample-system:api/src/lib/errors.ts:notFound"]
  - id: CO-8
    name: "Web app"
    module: module:sample-system:web
    change: existing
    cites: ["context/generated/module-map.json#module:sample-system:web", "artifacts/HMS-11/run_01M77BM48R93EMY13TZXCDN7PZ/req.md#item:REQ-16"]
endpoints:
  - id: EP-1
    method: GET
    path: /api/specializations/:id/earliest-slots
    change: new
    fields:
      - {name: items, type: array, nullable: false, format: "JSON array of entries, unpaged; no page, page_size or total", example: "[{...}]", status: known}
      - {name: "items[].id", type: integer, nullable: false, format: "doctor row id", example: "2", status: known}
      - {name: "items[].code", type: string, nullable: false, format: "doctor display code, D-0001 form", example: "D-0002", status: known}
      - {name: "items[].fullName", type: string, nullable: false, format: "doctor full name as stored", example: "Dr. Test Doctor Two", status: known}
      - {name: "items[].departmentName", type: string, nullable: false, format: "department name as stored (Active or Inactive)", example: "Surgery", status: known}
      - {name: "items[].slot", type: object, nullable: true, format: "earliest FREE slot in the window; null when none", example: "{\"date\":\"2026-10-20\",\"start\":\"2026-10-20T04:00:00.000Z\",\"label\":\"09:30\"}", status: known}
      - {name: "items[].slot.date", type: string, nullable: false, format: "YYYY-MM-DD, hospital-local date (HOSPITAL_TZ)", example: "2026-10-20", status: known}
      - {name: "items[].slot.start", type: string, nullable: false, format: "ISO-8601 UTC instant with milliseconds and Z, as toISOString prints", example: "2026-10-20T04:00:00.000Z", status: known}
      - {name: "items[].slot.label", type: string, nullable: false, format: "HH:MM hospital-local start time, as the day schedule labels it", example: "09:30", status: known}
    cites: ["artifacts/HMS-11/run_01M77BM48R93EMY13TZXCDN7PZ/impact.md#claim:C1", "runs/run_01M77BM48R93EMY13TZXCDN7PZ/ide-answers.md#a-03", "runs/run_01M77BM48R93EMY13TZXCDN7PZ/ide-answers.md#a-04"]
  - id: EP-2
    method: GET
    path: /api/doctors/:id/slots
    change: existing
    fields: []
    cites: ["context/generated/api-inventory.json#http:GET:/api/doctors/:id/slots"]
  - id: EP-3
    method: GET
    path: /api/specializations
    change: existing
    fields: []
    cites: ["context/generated/api-inventory.json#http:GET:/api/specializations"]
  - id: EP-4
    method: POST
    path: /api/appointments
    change: existing
    fields: []
    cites: ["context/generated/api-inventory.json#http:POST:/api/appointments"]
answers:
  - id: A-03
    kind: question
    about: Q-01
    answer: "Flat (proposed default)"
    decides: [EP-1, CO-6, C18]
  - id: A-04
    kind: question
    about: Q-02
    answer: "appointments:read (proposed default)"
    decides: [CO-1, C19]
---
# Design: HMS-11

## Overview
One new read-only route, `GET /api/specializations/:id/earliest-slots` (EP-1), is added to the existing specializations router (CO-1) with permission `appointments:read` (A-04). The handler parses the id with `parseId` and calls a new `DoctorService.earliestSlots` (CO-2), which lives next to the private slot-state rule that the day schedule and the HMS-9 next-7-days summary already share (C3, C4). FREE therefore means exactly what the day schedule shows (S-1 AC5, S-6).

Flow:
1. CO-2 reads the specialization's status with `DoctorRepo.refStatus` (C6). Null throws `notFound` (404); a status other than ACTIVE throws `badRequest('specialization is Inactive')` (400). A malformed id is already rejected by `parseId` with a 400.
2. It loads the specialization's Active doctors, with department name and slot length, unpaged, through the new `DoctorRepo.activeBySpecialization` (CO-3).
3. It works out the window's days from `localDay(now)` with `addDays` 0 to 6 in `HOSPITAL_TZ`. The bounds run from the start of today to the end of today+6, using `dayBounds`.
4. For each doctor it loads availability and `occupyingBetween` once for the whole window. It walks the days in order and takes the first slot whose state is FREE. The slot-state rule already marks started slots PAST, ignores Cancelled appointments and marks overlapping ones BOOKED.
5. It maps each doctor to the flat entry (CO-6, A-03): id, `formatCode('D', id)`, fullName, departmentName, and slot `{ date, start: start.toISOString(), label }`, or null.
6. It sorts entries with a slot by start, then by lower-cased name, then by id. Entries with slot null come last, by lower-cased name, then by id (REQ-17).
7. The response is `{ items }`, unpaged (C22).

Trade-offs:
- **Method on DoctorService, not a new service:** this reuses the private slot-state rule unchanged. The cost is that DoctorService grows by one method.
- **Per-doctor queries:** 2 queries per doctor (availability and appointments) plus one for the doctor list. That is simple and matches nextSevenDays. It is slower for large specializations, which is acceptable because the lists are short.
- **nextSevenDays is left untouched:** the window logic is written again rather than refactored, so HMS-9 cannot regress. The cost is a few duplicated lines.
- **Mapping instead of returning DoctorSummary:** the entry is built field by field, so doctor phone and email never reach the response (S-5 AC1). Appointment summaries, which carry patient names, are used only inside the service and never returned (S-5 AC2).
- **Names for the new methods** (`earliestSlots`, `activeBySpecialization`) and the type (`EarliestSlotEntry`) follow the neighbouring code's naming. They are defaults, not questions.

## Components
- CO-1 specializationsRouter, GET /:id/earliest-slots: changed (new handler with `requirePermission('appointments:read')`; the existing handlers stay as they are, C2).
- CO-2 DoctorService.earliestSlots: changed (new method in `DoctorService`).
- CO-3 DoctorRepo.activeBySpecialization: changed (new method, Active doctors of one specialization, unpaged).
- CO-4 DoctorRepo.refStatus and availability: existing.
- CO-5 AppointmentRepo.occupyingBetween: existing.
- CO-6 EarliestSlotEntry in api/src/types.ts: new type in an existing file (C16). Shape: `{ id, code, fullName, departmentName, slot: { date, start, label } | null }`.
- CO-7 daySlots, localDay, addDays, dayBounds, notFound, badRequest: existing.
- CO-8 web app: existing, no change.
- EP-1 GET /api/specializations/:id/earliest-slots: new. Fields:
  - items: array, not null, unpaged.
  - items[].id: integer.
  - items[].code: string, D-0001 form.
  - items[].fullName: string.
  - items[].departmentName: string.
  - items[].slot: object or null.
  - items[].slot.date: YYYY-MM-DD, hospital-local.
  - items[].slot.start: ISO-8601 UTC, for example 2026-10-20T04:00:00.000Z.
  - items[].slot.label: HH:MM local, for example 09:30.
  - Errors use the standard `{ error: { code, message } }` body: 404 for an unknown id, and 400 "specialization is Inactive".
- EP-2 GET /api/doctors/:id/slots: existing, unchanged.
- EP-3 GET /api/specializations: existing, unchanged.
- EP-4 POST /api/appointments: existing, unchanged.

Story coverage:
- S-1: CO-1, CO-2, CO-3, CO-5, CO-6.
- S-2: CO-2, CO-7.
- S-3: CO-2.
- S-4: CO-1, CO-2, CO-4, CO-7.
- S-5: CO-2, CO-6.
- S-6: EP-2, EP-3, EP-4 and CO-8 unchanged.

## Known
- S-1: served by EP-1 through CO-1, CO-2, CO-3, CO-5 and CO-6, with fields fixed by A-03.
- S-2: served by the window and slot-state rule in CO-2 with the CO-7 helpers in `HOSPITAL_TZ` (C20, C21).
- S-3: served by the sort in CO-2 (C23).
- S-4: served by CO-1, CO-2, CO-4 and CO-7 (C12, C13, C22).
- S-5: served by the explicit mapping in CO-2 to the flat CO-6 type.
- S-6: no existing route, page or rule changes (EP-2, EP-3, EP-4, CO-8).

## Assumed
None.

## Unknown
None.

## Open questions
None.

Answered in the IDE:
- A-03: Q-01, which JSON shape should each entry have? "Flat (proposed default)". Decides EP-1, CO-6 and C18.
- A-04: Q-02, which permission should the new route require? "appointments:read (proposed default)". Decides CO-1 and C19.
