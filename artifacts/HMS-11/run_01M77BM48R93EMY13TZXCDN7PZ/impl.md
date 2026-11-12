---
schema: change.v1
run_id: run_01M77BM48R93EMY13TZXCDN7PZ
step: impl
attempt: 1
skill: implement@1.1.0
inputs:
  - path: artifacts/HMS-11/run_01M77BM48R93EMY13TZXCDN7PZ/design.md
    sha256: 5bbb2ba02b9021065e0ec9ff8410030043417595f497f433b47f9e00817a8237
  - path: artifacts/HMS-11/run_01M77BM48R93EMY13TZXCDN7PZ/split.md
    sha256: d4dc5ed961e49e2c861c4e3256457f48726f4adc5af1ce2363cc97491f9a4d26
  - path: context/conventions.md
    sha256: dc53d94ec34eb193eeb06fdd0db19eb86a24c29f7d3c434e7c212d61038ca852
  - path: context/generated/module-map.json
    sha256: 4158c8fcd239c22cb8f1029dd11f61d8839867ff8088bd9ab76197e2db96db4b
pack_sha:
  sample-system: 7681f87749883d71abb4011d118366ca1242e1cc
claims:
  - id: C1
    text: "The route is added to the existing `specializationsRouter`."
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/routes/specializations.ts:specializationsRouter"]
  - id: C2
    text: "The route requires `appointments:read`, as the design decided."
    cites: ["artifacts/HMS-11/run_01M77BM48R93EMY13TZXCDN7PZ/design.md#claim:C19"]
  - id: C3
    text: "The new service method sits in `DoctorService`, next to the shared slot-state rule."
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/services/doctorService.ts:DoctorService"]
  - id: C4
    text: "The new query sits in `DoctorRepo` with a $1 parameter, following the repository pattern."
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/repositories/doctorRepo.ts:DoctorRepo"]
  - id: C5
    text: "The specialization check reuses `refStatus`."
    cites: ["artifacts/HMS-11/run_01M77BM48R93EMY13TZXCDN7PZ/design.md#claim:C6"]
  - id: C6
    text: "Each entry is flat with `departmentName`, as the design decided."
    cites: ["artifacts/HMS-11/run_01M77BM48R93EMY13TZXCDN7PZ/design.md#claim:C18"]
  - id: C7
    text: "The new type sits in api/src/types.ts next to `DoctorSummary`."
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/types.ts:DoctorSummary"]
  - id: C8
    text: "Errors use the standard `ValidationError` and HttpError helpers: 404 for an unknown id, and 400 for an Inactive specialization."
    cites: ["context/conventions.md#errors"]
  - id: C9
    text: "The window is `today to today+6`."
    cites: ["artifacts/HMS-11/run_01M77BM48R93EMY13TZXCDN7PZ/design.md#claim:C21"]
  - id: C10
    text: "Name ties compare ignoring case, with `doctor id as the final tie-break`."
    cites: ["artifacts/HMS-11/run_01M77BM48R93EMY13TZXCDN7PZ/design.md#claim:C23"]
trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10, REQ-11, REQ-12, REQ-13, REQ-14, REQ-15, REQ-16, REQ-17]
known: [S-1, S-2, S-3, S-4, S-5, S-6]
assumed: []
unknown: []
questions: []
code:
  - repo: sample-system
    base_sha: 7681f87749883d71abb4011d118366ca1242e1cc
    head_sha: 2d8bdec036142dda7a7fccb87a90f2e49f624a0c
    diff_sha256: 5a18253e126bacd9a64a64205c0f1bebd37253da44910e7bab6a9a801ecf538f
    files:
      - api/src/repositories/doctorRepo.ts
      - api/src/routes/specializations.ts
      - api/src/services/doctorService.ts
      - api/src/types.ts
summary: "Adds GET /api/specializations/:id/earliest-slots (appointments:read). It returns every Active doctor of the specialization with their earliest FREE slot from now to the end of today+6 in the hospital zone, using the day schedule's slot-state rule. Entries are flat with no contact data, sorted by slot then name (ignoring case) then id, with null slots last. Unknown id gives 404; Inactive gives 400 'specialization is Inactive'."
---
# Change: HMS-11

## Summary
This builds the approved design (design.md) with four file changes. No migration and no web change.

- **CO-1:** `specializationsRouter` gets `GET /:id/earliest-slots`, which declares `requirePermission('appointments:read')` (C2). It parses the id with `parseId` and calls `DoctorService.earliestSlots`.
- **CO-2:** `DoctorService.earliestSlots` works as follows:
  1. It reads the specialization's status with `refStatus` (C5). Null gives `notFound` (404); not ACTIVE gives `badRequest('specialization is Inactive')` (400).
  2. It works out today to today+6 from `localDay` and `addDays` in the hospital zone (C9).
  3. For each Active doctor it loads availability and `occupyingBetween` once for the whole window. It walks the days in order through the existing private slot-state rule and takes the first FREE slot. Cancelled appointments count as free because `occupyingBetween` leaves them out.
  4. It maps the doctor to the flat entry (C6) and sorts with `byEarliestSlot` (C10).
- **CO-3:** `DoctorRepo.activeBySpecialization` is a parameterised query (C4). It selects only id, full name, department name and slot length, so phone and email are never loaded.
- **CO-6:** `EarliestSlotEntry` is a narrow type in `api/src/types.ts` (C7).

Existing routes, the day schedule, the HMS-9 next-7-days summary and booking are unchanged (S-6).

No tests were added: the design names no regression test (rule 10). The threat model's key-set test for the entries is left to the unit-test step.

## Changes
- `api/src/routes/specializations.ts`: the new `GET /:id/earliest-slots` handler (CO-1).
- `api/src/services/doctorService.ts`: the `earliestSlots` method and the `byEarliestSlot` comparator; imports `badRequest` and `EarliestSlotEntry` (CO-2).
- `api/src/repositories/doctorRepo.ts`: the `activeBySpecialization` query (CO-3).
- `api/src/types.ts`: the `EarliestSlotEntry` interface (CO-6).

## Known
- **S-1:** entries carry id, code, fullName, departmentName and slot {date, start, label}, using the day schedule's FREE rule (C3, C6).
- **S-2:** the window runs from the start of today to the end of today+6 in the hospital zone. Started slots are PAST and never chosen (C9).
- **S-3:** `byEarliestSlot` sorts by slot start, then by name ignoring case, then by id; null slots come last (C10).
- **S-4:** the response is `{ items }` with no paging. A specialization with no Active doctors gives an empty list. Unknown id gives 404, and Inactive gives 400 (C5, C8).
- **S-5:** the explicit mapping means no phone, email or patient data reaches the response (C6, C7).
- **S-6:** no existing route or behaviour is changed (C1).

## Assumed
None.

## Unknown
None.

## Open questions
None.
