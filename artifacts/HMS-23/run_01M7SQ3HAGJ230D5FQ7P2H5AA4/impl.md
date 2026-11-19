---
schema: change.v1
run_id: run_01M7SQ3HAGJ230D5FQ7P2H5AA4
step: impl
attempt: 1
skill: implement@1.3.0
inputs:
  - path: artifacts/HMS-23/run_01M7SQ3HAGJ230D5FQ7P2H5AA4/design.md
    sha256: 466f928df995abc802f05f4a71710ab63b92f816648fc0c08a05ebfccf172df1
  - path: artifacts/HMS-23/run_01M7SQ3HAGJ230D5FQ7P2H5AA4/split.md
    sha256: da8271707531844c5e53000e0ed06d5a8c0f9c7f3dcf626770f37cc163f6aae6
  - path: context/conventions.md
    sha256: dc53d94ec34eb193eeb06fdd0db19eb86a24c29f7d3c434e7c212d61038ca852
  - path: context/generated/module-map.json
    sha256: 4158c8fcd239c22cb8f1029dd11f61d8839867ff8088bd9ab76197e2db96db4b
pack_sha:
  sample-system: 7681f87749883d71abb4011d118366ca1242e1cc
code:
  - repo: sample-system
    base_sha: 7681f87749883d71abb4011d118366ca1242e1cc
    head_sha: aed1ab2b5290a50e7e5fc873b0d17cdf67c57fbf
    diff_sha256: 218083a6ce184f6f9ee904ee36fdedc0d9043e3e0114763bf30c91746a214da2
    files:
      - api/src/repositories/doctorRepo.ts
      - api/src/services/dashboardService.ts
      - api/src/services/doctorService.ts
      - api/src/services/index.ts
      - api/src/types.ts
      - web/src/__tests__/DashboardPage.test.tsx
      - web/src/api/types.ts
      - web/src/pages/DashboardPage.tsx
claims:
  - id: C1
    text: "The design adds an `onDuty` array to GET /api/dashboard."
    cites: ["artifacts/HMS-23/run_01M7SQ3HAGJ230D5FQ7P2H5AA4/design.md#item:EP-1", "runs/run_01M7SQ3HAGJ230D5FQ7P2H5AA4/ide-answers.md#a-05"]
  - id: C2
    text: "Slot states come from the shared `slotStates` rule the day schedule uses."
    cites: ["sample-system:api/src/services/doctorService.ts#L157-L158"]
  - id: C3
    text: "Each doctor's appointments today are loaded with the existing `occupyingBetween`, which skips cancelled ones."
    cites: ["sample-system:api/src/repositories/appointmentRepo.ts#L113-L114"]
  - id: C4
    text: "Doctor lists sort by `lower(d.full_name), d.id`; activeOnWeekday uses the same order."
    cites: ["artifacts/HMS-23/run_01M7SQ3HAGJ230D5FQ7P2H5AA4/design.md#claim:C20"]
  - id: C5
    text: "Today's weekday is derived with `isoWeekday` from the hospital-local day."
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/lib/time.ts:isoWeekday"]
  - id: C6
    text: "SQL lives in `api/src/repositories` and rules in services, per the code layout."
    cites: ["context/conventions.md#code-layout"]
  - id: C7
    text: "The dashboard carries counts only, with no contact details (`DSH-9`)."
    cites: ["sample-system:api/src/services/dashboardService.ts#L28"]
  - id: C8
    text: "The doctor page opens today's day schedule by default via `todayIn`."
    cites: ["sample-system:web/src/pages/DoctorDetailPage.tsx#L31"]
  - id: C9
    text: "createContext builds `DashboardService`; it now receives the same DoctorService instance."
    cites: ["sample-system:api/src/services/index.ts#L37"]
  - id: C10
    text: "The dashboard page test fixture is typed as `Dashboard`, so it must carry the new field."
    cites: ["sample-system:web/src/__tests__/DashboardPage.test.tsx#L7"]
  - id: C11
    text: "Row fields mirror the slot shape (`Mirror slot shape`): nextFreeAt and nextFreeLabel."
    cites: ["runs/run_01M7SQ3HAGJ230D5FQ7P2H5AA4/ide-answers.md#a-06"]
trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10, REQ-11, REQ-12, REQ-13, REQ-14]
known: [CO-1, CO-2, CO-3, CO-4, CO-5, CO-6, CO-7, CO-8, EP-1]
assumed: []
unknown: []
questions: []
summary: "GET /api/dashboard now returns an onDuty array computed by a new DoctorService.onDutyToday() that reuses the day schedule's slotStates rule. The dashboard page renders it as an 'On duty today' section below the tiles."
---
# Change: HMS-23

## Summary
This change implements the approved design. It touches 8 files, well inside the limits of 400 lines and 15 files, and no sensitive paths.

**API**
- **CO-2:** a new `DoctorRepo.activeOnWeekday(weekday)` selects `ACTIVE` doctors that have an `availability_block` on the weekday. It returns their department name, slot length and all their blocks, sorted by `lower(full_name), id` (C4).
- **CO-1:** a new `DoctorService.onDutyToday()` works out today in the hospital zone and its weekday (C5). For each doctor it loads the day's appointments with the existing `occupyingBetween` (CO-3, C3) and runs the same private `slotStates` the day schedule uses (C2).
  - `booked` counts BOOKED slots and `free` counts FREE slots.
  - A slot that starts at or before now is PAST, so it is neither free nor the next free slot.
  - `nextFreeAt` and `nextFreeLabel` are the earliest FREE slot's `start` and `label`, or null.
  - Rows are sorted by `free`, most first. The sort is stable, so the name order from the repository settles ties.
- **CO-4:** `Dashboard` gains `onDuty: OnDutyRow[]`. `DashboardService` takes a `DoctorService` and fills `onDuty` without reading the period.
- **CO-5:** `createContext` creates one `DoctorService` and passes it to `DashboardService` (C9).
- **EP-1:** the new API type `OnDutyRow` holds only `doctorId`, `fullName`, `department`, `booked`, `free`, `nextFreeAt` and `nextFreeLabel`. No phone or email is included (C7).

**Web**
- **CO-6:** the web `Dashboard` type gains `onDuty` and `OnDutyRow`.
- **CO-7:** `DashboardPage` renders an "On duty today" section between the tiles and "Next appointments".
  - Each row shows the name (a link to `/doctors/:id`, which opens today by default per CO-8 and C8), department, booked, free, and the next free slot label.
  - It shows "Fully booked" when `free` is 0.
  - It shows "No doctors on duty today" when there are no rows.
  - Tiles and Next appointments are unchanged.

## Changes
- `api/src/types.ts`: adds the `OnDutyRow` interface (EP-1).
- `api/src/repositories/doctorRepo.ts`: adds the `OnDutyDoctor` type and `activeOnWeekday` (CO-2).
- `api/src/services/doctorService.ts`: adds `onDutyToday()`, plus imports for `isoWeekday` and `OnDutyRow` (CO-1).
- `api/src/services/dashboardService.ts`: adds `Dashboard.onDuty`, the `DoctorService` constructor argument, and sets `onDuty` in `get()` (CO-4).
- `api/src/services/index.ts`: creates one shared `DoctorService` and passes it to `DashboardService` (CO-5).
- `web/src/api/types.ts`: adds `Dashboard.onDuty` and `OnDutyRow` (CO-6).
- `web/src/pages/DashboardPage.tsx`: adds the `OnDutyToday` section below the tiles (CO-7).
- `web/src/__tests__/DashboardPage.test.tsx`: adds `onDuty: []` to the existing `DATA` fixture. The fixture is typed `Dashboard`, a type the design marks changed (C10). No new tests are added; that is the unit-test step's job.

## Known
- **CO-1:** reuses `slotStates` (C2) and `occupyingBetween` (C3), so counts match the day schedule.
- **CO-2:** a repository method, following the code layout (C6) and the existing name order (C4).
- **CO-3:** `occupyingBetween` is reused unchanged (C3).
- **CO-4:** `onDuty` placement as decided in A-05 (C1), with no contact data (C7).
- **CO-5:** the context passes the shared `DoctorService` (C9).
- **CO-6:** the row shape as decided in A-06 (C11).
- **CO-7:** the section and the "Fully booked" and empty-state texts, per the stories.
- **CO-8:** the link target opens today by default (C8).
- **EP-1:** response fields as in the design (C1, C11).

## Assumed
None.

## Unknown
None.

## Open questions
None.
