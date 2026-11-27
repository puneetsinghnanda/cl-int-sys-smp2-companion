---
schema: change.v1
run_id: run_01M8EC1C803452B4P6VTAB10M7
step: impl
attempt: 1
skill: implement@1.5.0
inputs:
  - {path: artifacts/HMS-23/run_01M8EC1C803452B4P6VTAB10M7/design.md, sha256: a86e1c9b62e44012e519875200e02e518bd8103f4e383932dd9658479930fb09}
  - {path: artifacts/HMS-23/run_01M8EC1C803452B4P6VTAB10M7/split.md, sha256: aff52115f9fdfadc010120f5dca462e696245658dd4c590d83c5ab683356b584}
  - {path: context/conventions.md, sha256: dc53d94ec34eb193eeb06fdd0db19eb86a24c29f7d3c434e7c212d61038ca852}
  - {path: context/generated/module-map.json, sha256: 4158c8fcd239c22cb8f1029dd11f61d8839867ff8088bd9ab76197e2db96db4b}
pack_sha: {sample-system: 7681f87749883d71abb4011d118366ca1242e1cc}
code:
  - repo: sample-system
    base_sha: 7681f87749883d71abb4011d118366ca1242e1cc
    head_sha: 870965ee81e165d3649fad34a0e4d7764bda5f85
    diff_sha256: cb3e9b7e8e18fd29835bc3a2f54bcaf18fcc4839a36dd0740c1353f5a522090f
    files:
      - api/src/repositories/doctorRepo.ts
      - api/src/services/dashboardService.ts
      - api/src/services/doctorService.ts
      - api/src/services/index.ts
      - web/src/__tests__/DashboardPage.test.tsx
      - web/src/api/types.ts
      - web/src/pages/DashboardPage.tsx
claims:
  - {id: C1, text: "The counts reuse the private slot rule `slotStates` shared by the day schedule and Next 7 days.", cites: ["sample-system:api/src/services/doctorService.ts#L191"]}
  - {id: C2, text: "Next 7 days skips a day when `states.length === 0`; on-duty rows use the same test.", cites: ["sample-system:api/src/services/doctorService.ts#L177"]}
  - {id: C3, text: "Doctor queries reuse the SELECT that joins the department and reads `department_status`.", cites: ["sample-system:api/src/repositories/doctorRepo.ts#L24"]}
  - {id: C4, text: "Doctor lists order by `lower(d.full_name)`, then id; onDutyOn uses the same order.", cites: ["sample-system:api/src/repositories/doctorRepo.ts#L94"]}
  - {id: C5, text: "`occupyingBetween` loads a doctor's non-cancelled appointments for the day.", cites: ["sample-system:api/src/repositories/appointmentRepo.ts#L114"]}
  - {id: C6, text: "The services container built `DashboardService` with db, config and clock; it now also gets the doctor service.", cites: ["sample-system:api/src/services/index.ts#L37"]}
  - {id: C7, text: "The existing web test fixture typed as `Dashboard` must carry the new field.", cites: ["sample-system:web/src/__tests__/DashboardPage.test.tsx#L7"]}
  - {id: C8, text: "The dashboard response already carries `today`, used for the doctor link.", cites: ["sample-system:api/src/services/dashboardService.ts#L14"]}
  - {id: C9, text: "The Inactive marker copies the doctor page's `(Inactive)` rendering.", cites: ["sample-system:web/src/pages/DoctorDetailPage.tsx#L74"]}
  - {id: C10, text: "nextFree is sent as { start, label } as answered for `nextFree`.", cites: ["runs/run_01M8EC1C803452B4P6VTAB10M7/ide-answers.md#a-04"]}
  - {id: C11, text: "onDuty is added to `GET /api/dashboard` as answered.", cites: ["runs/run_01M8EC1C803452B4P6VTAB10M7/ide-answers.md#a-03"]}
trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10, REQ-11, REQ-12, REQ-13, REQ-14, REQ-15, REQ-16, REQ-17, REQ-18, REQ-19]
known: [CO-1, CO-2, CO-3, CO-4, CO-5, CO-6, EP-1]
assumed: []
unknown: []
questions: []
summary: "GET /api/dashboard now returns onDuty: Active doctors with slots today in the hospital zone, with booked, free and nextFree { start, label } counted by the day schedule's slotStates rule, sorted by free then name. DashboardPage shows them in an On duty today table between the tiles and Next appointments."
---
# Change: HMS-23

## Summary
The change implements design components CO-1 to CO-6 and endpoint EP-1.

**API:**
- **CO-1:** `DoctorRepo.onDutyOn(weekday)` returns Active doctors with an availability block on that weekday. It uses the existing department-joined SELECT (C3) and the usual `lower(d.full_name), d.id` order (C4).
- **CO-2:** `DoctorService.onDutyToday()` takes today's hospital-local date with `localDay`, then:
  - loads each on-duty doctor's blocks and `occupyingBetween` appointments (C5);
  - runs the unchanged `slotStates` rule (C1), so booked and free match the day schedule;
  - skips a doctor whose blocks yield no slot today, the same `states.length === 0` test as Next 7 days (C2);
  - sets `nextFree` to the earliest FREE slot as `{ start, label }` (C10), or null when none is left;
  - sorts by free, descending. The sort is stable, so ties keep the repo's name order.
- **CO-3:** `Dashboard` gains `onDuty`, and `OnDutyRow` is declared next to it. `get()` fills `onDuty` whatever the period (C11).
- **CO-4:** the services container builds one `DoctorService` and passes it to `DashboardService` (C6).

**Rows carry only:** doctorId, fullName, department name and status, booked, free and nextFree. They never carry phone or email (DSH-9).

**Web:**
- **CO-5:** `Dashboard` gains `onDuty`, plus a new `OnDutyRow` interface.
- **CO-6:** a new `OnDutyTable` renders "On duty today" between the tiles and "Next appointments":
  - the doctor's name links to `/doctors/{id}?date={today}` (C8);
  - an Inactive department gets " (Inactive)" (C9);
  - Next free shows the label, or "Fully booked";
  - an empty list shows "No doctors on duty today".

Local type-check and lint passed. The change touches 7 files, within the limits of 15 files and 400 lines, and no sensitive path.

## Changes
- `api/src/repositories/doctorRepo.ts`: new `onDutyOn(weekday)` (CO-1).
- `api/src/services/doctorService.ts`: new `onDutyToday()` (CO-2).
- `api/src/services/dashboardService.ts`: `OnDutyRow`, `Dashboard.onDuty`, and the constructor takes the doctor service (CO-3, EP-1).
- `api/src/services/index.ts`: passes the shared `DoctorService` to `DashboardService` (CO-4).
- `web/src/api/types.ts`: `Dashboard.onDuty` and `OnDutyRow` (CO-5).
- `web/src/pages/DashboardPage.tsx`: the On duty today section and `OnDutyTable` (CO-6).
- `web/src/__tests__/DashboardPage.test.tsx`: the existing fixture gains `onDuty: []`, because the `Dashboard` type changed under CO-5 (C7). No new tests: the design asks for none, and broad tests belong to the unit-test step.

## Known
- CO-1: built as designed on the existing doctor SELECT (C3, C4).
- CO-2: built on `slotStates` and `occupyingBetween` (C1, C2, C5); the nextFree shape follows A-04 (C10).
- CO-3: `onDuty` added to the dashboard response as A-03 decided (C11).
- CO-4: container wiring changed (C6).
- CO-5: the web type mirrors EP-1 (C10).
- CO-6: the link and Inactive marker follow the doctor page (C8, C9).
- EP-1: `GET /api/dashboard` returns `onDuty`; its other fields are unchanged (C11).

## Assumed
None.

## Unknown
None of this step's own. The run's open product-owner question (runs/run_01M8EC1C803452B4P6VTAB10M7/open-questions.md#q-01, non-blocking) asks about a doctor whose block today is too short for one slot. The code follows the approved design and leaves such a doctor out (C2). If the product owner decides otherwise, only the `states.length === 0` skip in `onDutyToday()` changes.

## Open questions
None.
