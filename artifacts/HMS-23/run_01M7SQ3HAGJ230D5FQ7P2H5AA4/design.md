---
schema: design.v1
run_id: run_01M7SQ3HAGJ230D5FQ7P2H5AA4
step: design
attempt: 1
skill: design@1.9.0
inputs:
  - path: artifacts/HMS-23/run_01M7SQ3HAGJ230D5FQ7P2H5AA4/split.md
    sha256: da8271707531844c5e53000e0ed06d5a8c0f9c7f3dcf626770f37cc163f6aae6
  - path: artifacts/HMS-23/run_01M7SQ3HAGJ230D5FQ7P2H5AA4/req.md
    sha256: 0fbd5a70a16b61c0acd828a804affa01fc7aa1b37e6ae354918b533638b9179a
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
    text: "The day schedule and the next 7 days share one slot-state rule, `slotStates`, in DoctorService."
    cites: ["sample-system:api/src/services/doctorService.ts#L157-L158"]
  - id: C2
    text: "A slot overlapping an occupying appointment is BOOKED; otherwise a slot whose start is at or before now is `PAST`, else FREE."
    cites: ["sample-system:api/src/services/doctorService.ts#L161"]
  - id: C3
    text: "The day schedule loads the day's appointments with `occupyingBetween`."
    cites: ["sample-system:api/src/services/doctorService.ts#L149"]
  - id: C4
    text: "DoctorService is declared in `api/src/services/doctorService.ts`."
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/services/doctorService.ts:DoctorService"]
  - id: C5
    text: "The dashboard response type `Dashboard` is declared in the dashboard service."
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/services/dashboardService.ts:Dashboard"]
  - id: C6
    text: "DashboardService is declared in `api/src/services/dashboardService.ts`."
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/services/dashboardService.ts:DashboardService"]
  - id: C7
    text: "GET /api/dashboard is served by `api/src/routes/dashboard.ts`."
    cites: ["context/generated/api-inventory.json#http:GET:/api/dashboard"]
  - id: C8
    text: "createContext builds `DashboardService` from db, config and clock."
    cites: ["sample-system:api/src/services/index.ts#L37"]
  - id: C9
    text: "DoctorRepo is declared in `api/src/repositories/doctorRepo.ts`."
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/repositories/doctorRepo.ts:DoctorRepo"]
  - id: C10
    text: "The doctor query already joins the department name as `department_name`."
    cites: ["sample-system:api/src/repositories/doctorRepo.ts#L24"]
  - id: C11
    text: "Availability blocks are read per doctor from `availability_block`."
    cites: ["sample-system:api/src/repositories/doctorRepo.ts#L135"]
  - id: C12
    text: "Slot labels are built as zero-padded local hours and minutes with `padStart` in daySlots."
    cites: ["sample-system:api/src/lib/slots.ts#L42"]
  - id: C13
    text: "Availability, slots and today use the hospital time zone `HOSPITAL_TZ`."
    cites: ["context/architecture.md#time-and-slots"]
  - id: C14
    text: "The web `Dashboard` type is declared in `web/src/api/types.ts`."
    cites: ["context/generated/module-map.json#sym:sample-system:web/src/api/types.ts:Dashboard"]
  - id: C15
    text: "DashboardPage is declared in `web/src/pages/DashboardPage.tsx`."
    cites: ["context/generated/module-map.json#sym:sample-system:web/src/pages/DashboardPage.tsx:DashboardPage"]
  - id: C16
    text: "The doctor page opens the day schedule on today when no date is in the URL, using `todayIn`."
    cites: ["sample-system:web/src/pages/DoctorDetailPage.tsx#L31"]
  - id: C17
    text: "The dashboard service shows counts only, with no contact details (`DSH-9`)."
    cites: ["sample-system:api/src/services/dashboardService.ts#L28"]
  - id: C18
    text: "The rows are returned as an `onDuty` array on GET /api/dashboard."
    cites: ["runs/run_01M7SQ3HAGJ230D5FQ7P2H5AA4/ide-answers.md#a-05"]
  - id: C19
    text: "Each row mirrors the slot shape with `Mirror slot shape` fields nextFreeAt and nextFreeLabel."
    cites: ["runs/run_01M7SQ3HAGJ230D5FQ7P2H5AA4/ide-answers.md#a-06"]
  - id: C20
    text: "Doctor lists sort by `lower(d.full_name), d.id`."
    cites: ["sample-system:api/src/repositories/doctorRepo.ts#L94"]
trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10, REQ-11, REQ-12, REQ-13, REQ-14]
known: [CO-1, CO-2, CO-3, CO-4, CO-5, CO-6, CO-7, CO-8, EP-1]
assumed: []
unknown: []
questions: []
components:
  - id: CO-1
    name: DoctorService.onDutyToday (new public method on existing class)
    module: module:sample-system:api
    change: changed
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/services/doctorService.ts:DoctorService", "sample-system:api/src/services/doctorService.ts#L157-L158"]
  - id: CO-2
    name: DoctorRepo.activeOnWeekday (new method)
    module: module:sample-system:api
    change: changed
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/repositories/doctorRepo.ts:DoctorRepo"]
  - id: CO-3
    name: AppointmentRepo.occupyingBetween (reused unchanged)
    module: module:sample-system:api
    change: existing
    cites: ["sample-system:api/src/services/doctorService.ts#L149"]
  - id: CO-4
    name: DashboardService and Dashboard interface (adds onDuty; takes DoctorService)
    module: module:sample-system:api
    change: changed
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/services/dashboardService.ts:DashboardService", "context/generated/module-map.json#sym:sample-system:api/src/services/dashboardService.ts:Dashboard", "runs/run_01M7SQ3HAGJ230D5FQ7P2H5AA4/ide-answers.md#a-05"]
  - id: CO-5
    name: createContext (passes the DoctorService to DashboardService)
    module: module:sample-system:api
    change: changed
    cites: ["sample-system:api/src/services/index.ts#L37"]
  - id: CO-6
    name: web Dashboard type (adds onDuty rows)
    module: module:sample-system:web
    change: changed
    cites: ["context/generated/module-map.json#sym:sample-system:web/src/api/types.ts:Dashboard", "runs/run_01M7SQ3HAGJ230D5FQ7P2H5AA4/ide-answers.md#a-06"]
  - id: CO-7
    name: DashboardPage (new "On duty today" section below the tiles)
    module: module:sample-system:web
    change: changed
    cites: ["context/generated/module-map.json#sym:sample-system:web/src/pages/DashboardPage.tsx:DashboardPage"]
  - id: CO-8
    name: DoctorDetailPage (link target; opens today's day schedule by default)
    module: module:sample-system:web
    change: existing
    cites: ["sample-system:web/src/pages/DoctorDetailPage.tsx#L31"]
endpoints:
  - id: EP-1
    method: GET
    path: /api/dashboard
    change: changed
    fields:
      - {name: onDuty, type: "array of OnDutyRow", nullable: false, format: "sorted by free desc, then lower-case fullName, then doctorId; [] when no one is on duty", example: "[]", status: known}
      - {name: "onDuty[].doctorId", type: integer, nullable: false, format: "doctor id", example: 3, status: known}
      - {name: "onDuty[].fullName", type: string, nullable: false, format: "doctor full name", example: "Dr Rao", status: known}
      - {name: "onDuty[].department", type: string, nullable: false, format: "department name", example: "Cardiology", status: known}
      - {name: "onDuty[].booked", type: integer, nullable: false, format: "count of BOOKED slots today, as the day schedule", example: 2, status: known}
      - {name: "onDuty[].free", type: integer, nullable: false, format: "count of FREE slots today, as the day schedule (start after now)", example: 5, status: known}
      - {name: "onDuty[].nextFreeAt", type: string, nullable: true, format: "ISO 8601 UTC instant of the earliest FREE slot start; null when free is 0", example: "2026-11-19T06:00:00.000Z", status: known}
      - {name: "onDuty[].nextFreeLabel", type: string, nullable: true, format: "HH:MM in the hospital time zone (Asia/Kolkata), the slot label; null when free is 0", example: "11:30", status: known}
    cites: ["context/generated/api-inventory.json#http:GET:/api/dashboard", "runs/run_01M7SQ3HAGJ230D5FQ7P2H5AA4/ide-answers.md#a-05", "runs/run_01M7SQ3HAGJ230D5FQ7P2H5AA4/ide-answers.md#a-06"]
answers:
  - id: A-05
    kind: question
    about: Q-01
    answer: "Add `onDuty` to dashboard (proposed default)"
    decides: [EP-1, CO-4, C18]
  - id: A-06
    kind: question
    about: Q-02
    answer: "Mirror slot shape (proposed default)"
    decides: [EP-1, CO-6, C19]
---
# Design: HMS-23

## Overview
The dashboard gets an "On duty today" section. GET /api/dashboard gains an `onDuty` array (A-05), so the page still makes one request.

**API**
- `DashboardService` asks a new public `DoctorService.onDutyToday()` for the rows.
- That method takes today in the hospital zone and loads the Active doctors with a block on today's weekday (new `DoctorRepo.activeOnWeekday`). For each doctor it loads the day's appointments with the existing `occupyingBetween` and runs them through the same private `slotStates` rule the day schedule uses (C1). Booked, free and passed slots are therefore counted exactly as the day schedule shows them (REQ-4, A-02).
- `slotStates` marks a slot `PAST` when its start is at or before now (C2). A slot starting exactly now is therefore neither free nor the next free slot (S-2 AC4).
- Rows are sorted on the server: free slots descending, then lower-case name, then id, as doctor lists already sort (C20).

**Web**
- `DashboardPage` renders the rows below the tiles and above "Next appointments".
- A doctor with 0 free slots shows "Fully booked". The name links to `/doctors/:id`, which opens today's day schedule by default (C16, A-04).

**Trade-offs**
- **Reusing `slotStates` inside DoctorService:** this guarantees the same counts as the day schedule, but `DashboardService` now depends on `DoctorService`. The alternative, copying the rule, risks the two drifting apart.
- **One `occupyingBetween` query per on-duty doctor:** this reuses tested code with no new SQL, at the cost of N small queries per dashboard load. That is acceptable for one hospital's doctors on duty on one day.
- **`onDuty` is recomputed when the Period selector changes**, but never reads the period (REQ-12). This keeps one request and costs a little extra work per Period change.
- **No contact data:** the row carries only id, name, department, counts and the next slot. `phone` and `email` from `DoctorSummary` are never copied (REQ-13, C17).
- **No schema change:** no migration, store or service is added.

## Components
- **CO-1** `DoctorService.onDutyToday()` in `api/src/services/doctorService.ts`, **changed**. It is a new public method using the existing `slotStates`, `daySlots`, `localDay` and `dayBounds`. It returns rows with `booked` (count of BOOKED slots), `free` (count of FREE slots), and `nextFreeAt` / `nextFreeLabel` (the first FREE slot's `start` and `label`, or null).
- **CO-2** `DoctorRepo.activeOnWeekday(weekday)` in `api/src/repositories/doctorRepo.ts`, **changed**. It is a new method that selects doctors with `status = 'ACTIVE'` that have an `availability_block` on the weekday, with department name and slot length, plus their blocks. Inactive doctors are excluded (REQ-11).
- **CO-3** `AppointmentRepo.occupyingBetween`, **existing**. It is reused unchanged for each doctor's appointments today.
- **CO-4** `DashboardService` / `Dashboard` in `api/src/services/dashboardService.ts`, **changed**. `Dashboard` gains `onDuty: OnDutyRow[]`. The constructor gains a `DoctorService` argument. `get()` sets `onDuty` without reading `period`.
- **CO-5** `createContext` in `api/src/services/index.ts`, **changed**. It passes the `doctors` service into `DashboardService`.
- **CO-6** The web `Dashboard` type in `web/src/api/types.ts`, **changed**. It gains `onDuty` with the EP-1 row fields.
- **CO-7** `DashboardPage` in `web/src/pages/DashboardPage.tsx`, **changed**. It gets a new "On duty today" section between the tiles and "Next appointments". Each row shows name (a link to `/doctors/:id`), department, booked, free and either `nextFreeLabel` or "Fully booked" when `free` is 0. An empty `onDuty` shows "No doctors on duty today". Tiles and Next appointments are unchanged (REQ-14).
- **CO-8** `DoctorDetailPage`, **existing**. It is the link target and opens today's day schedule when no `date` is given.
- **EP-1** `GET /api/dashboard`, **changed**. The existing fields are unchanged. It adds `onDuty` with these fields:
  - `onDuty`: array, never null. Sorted by free slots (most first), then lower-case `fullName`, then `doctorId`. Empty (`[]`) when no one is on duty.
  - `doctorId`: integer, for example 3.
  - `fullName`: string, for example "Dr Rao".
  - `department`: string, the department name, for example "Cardiology".
  - `booked`: integer, for example 2.
  - `free`: integer, for example 5.
  - `nextFreeAt`: string or null. An ISO 8601 UTC instant, for example "2026-11-19T06:00:00.000Z"; null when `free` is 0.
  - `nextFreeLabel`: string or null. `HH:MM` in hospital time (Asia/Kolkata), for example "11:30"; null when `free` is 0.

**Story to component map**
- S-1: CO-1, CO-2, CO-4, CO-7, EP-1
- S-2: CO-1, CO-3, EP-1
- S-3: CO-1, CO-7
- S-4: CO-1, EP-1
- S-5: CO-7, CO-8
- S-6: CO-4, CO-7

## Known
- **CO-1:** `DoctorService` and `slotStates` exist (C1, C4). The new method reuses them.
- **CO-2:** `DoctorRepo` exists (C9). Its query already joins the department name (C10), and blocks come from `availability_block` (C11).
- **CO-3:** `occupyingBetween` is what the day schedule uses (C3).
- **CO-4:** `Dashboard` and `DashboardService` exist (C5, C6). The `onDuty` placement was decided in A-05.
- **CO-5:** `createContext` builds `DashboardService` (C8).
- **CO-6:** the web `Dashboard` type exists (C14). The row shape was decided in A-06.
- **CO-7:** `DashboardPage` exists (C15).
- **CO-8:** the doctor page defaults to today (C16).
- **EP-1:** the route exists (C7). The fields were decided in A-05 and A-06, and labels follow `daySlots` (C12) in the hospital zone (C13).

## Assumed
None.

## Unknown
None.

## Open questions
None open.

Answered in the IDE:
- **A-05 (Q-01):** where should the API return the rows? Answer: "Add `onDuty` to dashboard (proposed default)". Decides EP-1, CO-4 and C18. No mismatch.
- **A-06 (Q-02):** what is the row shape for doctor, department and next free slot? Answer: "Mirror slot shape (proposed default)". Decides EP-1, CO-6 and C19. No mismatch.
