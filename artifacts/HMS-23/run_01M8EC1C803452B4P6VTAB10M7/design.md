---
schema: design.v1
run_id: run_01M8EC1C803452B4P6VTAB10M7
step: design
attempt: 1
skill: design@1.10.0
inputs:
  - {path: artifacts/HMS-23/run_01M8EC1C803452B4P6VTAB10M7/split.md, sha256: aff52115f9fdfadc010120f5dca462e696245658dd4c590d83c5ab683356b584}
  - {path: artifacts/HMS-23/run_01M8EC1C803452B4P6VTAB10M7/req.md, sha256: 69e5f24b8ab6d28047031767176af38a9746fac0c500328b4336461cd9a40365}
  - {path: context/intent-baseline.md, sha256: 9bfd821aeb2fb9524863e346707e049e4fcfb949a6e55b90223a79194946a215}
  - {path: context/architecture.md, sha256: 9faab093d10933a46f6473b25c80728f8745e597100e650cd2537bbafb2b595a}
  - {path: context/generated/module-map.json, sha256: 4158c8fcd239c22cb8f1029dd11f61d8839867ff8088bd9ab76197e2db96db4b}
  - {path: context/generated/api-inventory.json, sha256: ba024ab1b4b4151d26ce1afa77bd2dab1cec645ea737ce37f98e0e8674f795e9}
pack_sha: {sample-system: 7681f87749883d71abb4011d118366ca1242e1cc}
claims:
  - {id: C1, text: "GET /api/dashboard is served by `api/src/routes/dashboard.ts`.", cites: ["context/generated/api-inventory.json#http:GET:/api/dashboard"]}
  - {id: C2, text: "The API response type `Dashboard` is declared in the dashboard service.", cites: ["context/generated/module-map.json#sym:sample-system:api/src/services/dashboardService.ts:Dashboard"]}
  - {id: C3, text: "The dashboard response already carries `today`, the hospital-local date as YYYY-MM-DD.", cites: ["sample-system:api/src/services/dashboardService.ts#L14"]}
  - {id: C4, text: "The services container constructs `DashboardService` with db, config and clock only.", cites: ["sample-system:api/src/services/index.ts#L37"]}
  - {id: C5, text: "The services container constructs `DoctorService`.", cites: ["sample-system:api/src/services/index.ts#L34"]}
  - {id: C6, text: "The day schedule and Next 7 days share the private rule `slotStates`.", cites: ["sample-system:api/src/services/doctorService.ts#L158"]}
  - {id: C7, text: "Next 7 days treats a day as not available when `states.length === 0`.", cites: ["sample-system:api/src/services/doctorService.ts#L177"]}
  - {id: C8, text: "`occupyingBetween` loads one doctor's non-cancelled appointments in a UTC range.", cites: ["sample-system:api/src/repositories/appointmentRepo.ts#L114"]}
  - {id: C9, text: "`DoctorRepo` holds the doctor SQL, including availability(id).", cites: ["context/generated/module-map.json#sym:sample-system:api/src/repositories/doctorRepo.ts:DoctorRepo"]}
  - {id: C10, text: "The doctor SELECT joins the department and reads `department_status`.", cites: ["sample-system:api/src/repositories/doctorRepo.ts#L24"]}
  - {id: C11, text: "Doctor lists order by `lower(d.full_name)`, then id.", cites: ["sample-system:api/src/repositories/doctorRepo.ts#L94"]}
  - {id: C12, text: "A slot's `label` is its hospital-local start as HH:MM.", cites: ["sample-system:api/src/lib/slots.ts#L42"]}
  - {id: C13, text: "`isoWeekday` turns a local day into weekday 1 (Monday) to 7.", cites: ["context/generated/module-map.json#sym:sample-system:api/src/lib/time.ts:isoWeekday"]}
  - {id: C14, text: "`localDay` gives the hospital-local date of an instant.", cites: ["context/generated/module-map.json#sym:sample-system:api/src/lib/time.ts:localDay"]}
  - {id: C15, text: "The web type `Dashboard` is declared in `web/src/api/types.ts`.", cites: ["context/generated/module-map.json#sym:sample-system:web/src/api/types.ts:Dashboard"]}
  - {id: C16, text: "The dashboard is rendered by `DashboardPage`.", cites: ["context/generated/module-map.json#sym:sample-system:web/src/pages/DashboardPage.tsx:DashboardPage"]}
  - {id: C17, text: "The web client fetches `/api/dashboard` with the period only.", cites: ["sample-system:web/src/api/client.ts#L49"]}
  - {id: C18, text: "The doctor page opens its day schedule on a day through a `?date=` link.", cites: ["sample-system:web/src/pages/DoctorDetailPage.tsx#L115"]}
  - {id: C19, text: "The doctor page appends `(Inactive)` to an Inactive department.", cites: ["sample-system:web/src/pages/DoctorDetailPage.tsx#L74"]}
  - {id: C20, text: "A doctor summary carries `phone` and email, so the row must not copy it.", cites: ["sample-system:api/src/repositories/doctorRepo.ts#L36"]}
  - {id: C21, text: "Instants are stored in UTC; availability, slots and today use `HOSPITAL_TZ`.", cites: ["context/architecture.md#time-and-slots"]}
  - {id: C22, text: "The page shows the tiles and then `Next appointments`.", cites: ["sample-system:web/src/pages/DashboardPage.tsx#L61"]}
trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10, REQ-11, REQ-12, REQ-13, REQ-14, REQ-15, REQ-16, REQ-17, REQ-18, REQ-19]
known: [CO-1, CO-2, CO-3, CO-4, CO-5, CO-6, CO-7, CO-8, EP-1, EP-2]
assumed: []
unknown: []
questions: []
components:
  - {id: CO-1, name: "DoctorRepo.onDutyOn(weekday): Active doctors with an availability block on that weekday, with department name and status", module: "module:sample-system:api", change: changed, cites: ["context/generated/module-map.json#sym:sample-system:api/src/repositories/doctorRepo.ts:DoctorRepo", "sample-system:api/src/repositories/doctorRepo.ts#L24"]}
  - {id: CO-2, name: "DoctorService.onDutyToday(): rows with booked, free and nextFree from slotStates, sorted", module: "module:sample-system:api", change: changed, cites: ["sample-system:api/src/services/doctorService.ts#L158", "runs/run_01M8EC1C803452B4P6VTAB10M7/ide-answers.md#a-04"]}
  - {id: CO-3, name: "DashboardService.get() and Dashboard type: add onDuty, independent of period", module: "module:sample-system:api", change: changed, cites: ["context/generated/module-map.json#sym:sample-system:api/src/services/dashboardService.ts:Dashboard", "runs/run_01M8EC1C803452B4P6VTAB10M7/ide-answers.md#a-03"]}
  - {id: CO-4, name: "services/index.ts: pass the DoctorService into DashboardService", module: "module:sample-system:api", change: changed, cites: ["sample-system:api/src/services/index.ts#L37"]}
  - {id: CO-5, name: "web/src/api/types.ts: Dashboard gains onDuty; new OnDutyRow interface", module: "module:sample-system:web", change: changed, cites: ["context/generated/module-map.json#sym:sample-system:web/src/api/types.ts:Dashboard", "runs/run_01M8EC1C803452B4P6VTAB10M7/ide-answers.md#a-04"]}
  - {id: CO-6, name: "DashboardPage: On duty today section (OnDutyTable) between the tiles and Next appointments", module: "module:sample-system:web", change: changed, cites: ["context/generated/module-map.json#sym:sample-system:web/src/pages/DashboardPage.tsx:DashboardPage", "runs/run_01M8EC1C803452B4P6VTAB10M7/ide-answers.md#a-04"]}
  - {id: CO-7, name: "Slot rule (slotStates, daySlots) and time helpers (localDay, isoWeekday, dayBounds)", module: "module:sample-system:api", change: existing, cites: ["sample-system:api/src/services/doctorService.ts#L158", "context/generated/module-map.json#sym:sample-system:api/src/lib/time.ts:isoWeekday"]}
  - {id: CO-8, name: "DoctorDetailPage day schedule opened by ?date=", module: "module:sample-system:web", change: existing, cites: ["sample-system:web/src/pages/DoctorDetailPage.tsx#L115"]}
endpoints:
  - id: EP-1
    method: GET
    path: /api/dashboard
    change: changed
    fields:
      - {name: onDuty, type: "array of OnDutyRow", nullable: false, format: "sorted by free descending, then lower(fullName), then doctorId; empty array when no doctor is on duty; ignores ?period", example: "[]", status: known}
      - {name: "onDuty[].doctorId", type: integer, nullable: false, format: "doctor id", example: "3", status: known}
      - {name: "onDuty[].fullName", type: string, nullable: false, format: "doctor full name", example: "Doctor Alpha", status: known}
      - {name: "onDuty[].department.name", type: string, nullable: false, format: "department name", example: "Cardiology", status: known}
      - {name: "onDuty[].department.status", type: string, nullable: false, format: "ACTIVE or INACTIVE", example: "INACTIVE", status: known}
      - {name: "onDuty[].booked", type: integer, nullable: false, format: "count of BOOKED slots today, as the day schedule", example: "1", status: known}
      - {name: "onDuty[].free", type: integer, nullable: false, format: "count of FREE slots today, as the day schedule", example: "1", status: known}
      - {name: "onDuty[].nextFree", type: "object", nullable: true, format: "earliest FREE slot today; null when free is 0 (Fully booked)", example: "{\"start\": \"2026-11-27T23:00:00.000Z\", \"label\": \"04:30\"}", status: known}
      - {name: "onDuty[].nextFree.start", type: string, nullable: false, format: "ISO 8601 UTC instant (Asia/Kolkata = UTC+05:30, so 04:30 local on 28 November is 23:00 UTC on 27 November)", example: "2026-11-27T23:00:00.000Z", status: known}
      - {name: "onDuty[].nextFree.label", type: string, nullable: false, format: "HH:MM hospital-local, the slot label", example: "04:30", status: known}
    cites: ["context/generated/api-inventory.json#http:GET:/api/dashboard", "runs/run_01M8EC1C803452B4P6VTAB10M7/ide-answers.md#a-03", "runs/run_01M8EC1C803452B4P6VTAB10M7/ide-answers.md#a-04"]
  - {id: EP-2, method: GET, path: "/api/doctors/:id/slots", change: existing, fields: [], cites: ["context/generated/api-inventory.json#http:GET:/api/doctors/:id/slots"]}
answers:
  - {id: A-03, kind: question, about: Q-01, answer: "Add onDuty to GET /api/dashboard (proposed default)", decides: [EP-1, CO-3]}
  - {id: A-04, kind: question, about: Q-02, answer: "nextFree as { start, label }", decides: [EP-1, CO-2, CO-5, CO-6]}
---
# Design: HMS-23

## Overview
`GET /api/dashboard` gains an `onDuty` array (A-03). The page draws it as an "On duty today" section between the tiles and Next appointments (C22).

**How the rows are built:**
- `DoctorService.onDutyToday()` takes today as `localDay(now, HOSPITAL_TZ)` (C14, C21) and its weekday from `isoWeekday` (C13).
- It loads Active doctors with an availability block on that weekday (CO-1).
- For each doctor it loads the blocks (`availability`, C9) and today's non-cancelled appointments (`occupyingBetween`, C8). It then runs the same private `slotStates` rule that the day schedule and Next 7 days use (C6).

**Row values:**
- booked and free are counts of BOOKED and FREE slots.
- nextFree is the earliest FREE slot as { start, label } (A-04), or null when free is 0.
- A doctor whose block gives no slot today is left out, matching Next 7 days' `states.length === 0` rule (C7).
- A doctor whose slots have all passed stays in, with free 0 and nextFree null (REQ-19).

**Sorting:** rows are ordered by free (most first), then `lower(fullName)` as doctor lists do (C11), then doctorId.

**Period:** the period query parameter is not used for `onDuty`, so changing the Period selector gives the same rows (REQ-16).

**Contact details:** a row copies only id, name and department from the doctor summary, never `phone` or email (C20, DSH-9).

Trade-offs:
- **Extending `GET /api/dashboard` (A-03) rather than adding a route:** one request per page view. The cost is that the section is refetched when the Period changes, with identical values.
- **Putting the computation in `DoctorService` and injecting it into `DashboardService` (CO-4):** reuses the private `slotStates` unchanged, so counts cannot drift from the day schedule. The cost is one more constructor argument in the container (C4, C5).
- **Two queries per on-duty doctor (blocks and appointments):** the existing repo methods are reused as they are. A batched query would cut round trips, but it would duplicate the occupancy SQL.
- **`nextFree.start` as a UTC instant plus `label` (A-04):** tests can assert the exact instant, and the page shows the label the day schedule shows (C12).

Story map:
- S-1 (who is on duty): CO-1, CO-2, CO-3, CO-6.
- S-2 (name, department, no contact details): CO-1, CO-2, CO-5, CO-6.
- S-3 (booked and free): CO-2, CO-7.
- S-4 (next free and Fully booked): CO-2, CO-6.
- S-5 (order): CO-2.
- S-6 (link): CO-6, CO-8, using the existing `today` field (C3).
- S-7 (Period and rest unchanged): CO-3, CO-6.

## Components
- CO-1 (changed): new method `onDutyOn(weekday)` on `DoctorRepo` in `api/src/repositories/doctorRepo.ts` (C9). It reuses the existing doctor SELECT with the department join (C10), adds `WHERE d.status = 'ACTIVE' AND EXISTS (availability_block on that weekday)`, and orders by `lower(d.full_name), d.id`.
- CO-2 (changed): new public method `onDutyToday()` on `DoctorService` in `api/src/services/doctorService.ts`. It calls `slotStates` (C6) per doctor, builds OnDutyRow values and sorts them.
- CO-3 (changed): the `Dashboard` interface in `api/src/services/dashboardService.ts` (C2) gains `onDuty: OnDutyRow[]`, with OnDutyRow declared next to it. `get()` sets `onDuty` from `doctors.onDutyToday()` whatever the period. All other fields are unchanged.
- CO-4 (changed): `api/src/services/index.ts` passes the `DoctorService` instance into `new DashboardService(...)` (C4, C5).
- CO-5 (changed): in `web/src/api/types.ts` (C15), `Dashboard` gains `onDuty: OnDutyRow[]`, and a new OnDutyRow interface mirrors EP-1.
- CO-6 (changed): `DashboardPage` (C16) adds an `<h2>On duty today</h2>` section with a new `OnDutyTable` function in the same file, placed after the tiles and before Next appointments. Columns: Doctor, Department, Booked, Free, Next free.
  - Doctor is a link to `/doctors/${doctorId}?date=${data.today}` (C3, C18).
  - Department shows ` (Inactive)` appended when its status is INACTIVE (C19).
  - Next free shows `nextFree.label`, or "Fully booked" when nextFree is null.
  - An empty array shows "No doctors on duty today".
  - The web client call stays `api.dashboard(period)` (C17).
- CO-7 (existing): `slotStates`, `daySlots`, `localDay`, `isoWeekday` and `dayBounds`, used without change.
- CO-8 (existing): the DoctorDetailPage day schedule, opened by `?date=` (C18).
- EP-1 (changed) `GET /api/dashboard` adds these fields:
  - `onDuty`: array, never null; sorted by free descending, then lower(fullName), then doctorId; empty when nobody is on duty; ignores `?period`. Example: `[]`.
  - `onDuty[].doctorId`: integer. Example: `3`.
  - `onDuty[].fullName`: string. Example: `Doctor Alpha`.
  - `onDuty[].department.name`: string. Example: `Cardiology`.
  - `onDuty[].department.status`: `ACTIVE` or `INACTIVE`.
  - `onDuty[].booked`: integer count of BOOKED slots today.
  - `onDuty[].free`: integer count of FREE slots today.
  - `onDuty[].nextFree`: object, or null when free is 0.
  - `onDuty[].nextFree.start`: ISO 8601 UTC instant. Example: `2026-11-27T23:00:00.000Z`, which is 04:30 local on 28 November (Asia/Kolkata = UTC+05:30).
  - `onDuty[].nextFree.label`: `HH:MM` hospital-local. Example: `04:30`.
- EP-2 (existing): `GET /api/doctors/:id/slots`, the day schedule the counts must match. Unchanged.

## Known
- CO-1: `DoctorRepo` and its department join exist (C9, C10).
- CO-2: `slotStates` exists in `DoctorService` (C6); `nextFree` shape decided by A-04.
- CO-3: `Dashboard` is declared in the dashboard service (C2); `onDuty` placement decided by A-03.
- CO-4: both services are built in `services/index.ts` (C4, C5).
- CO-5: the web `Dashboard` type exists (C15); row shape decided by A-04.
- CO-6: `DashboardPage` exists (C16); the link and the Inactive marker follow the doctor page (C18, C19).
- CO-7, CO-8: existing code used unchanged (C6, C13, C14, C18).
- EP-1: the route exists (C1); its new fields were decided by A-03 and A-04.
- EP-2: the route exists and is unchanged.

## Assumed
None.

## Unknown
None.

## Open questions
None open.

Answered in the IDE:
- A-03 (Q-01): How should the page get the rows? Answer: "Add onDuty to GET /api/dashboard (proposed default)". Decides EP-1 and CO-3. No mismatch with the inputs.
- A-04 (Q-02): What shape should a row have, and how should the next free slot be sent? Answer: "nextFree as { start, label }". Decides EP-1, CO-2, CO-5 and CO-6. No mismatch: the page still shows the slot label, as the requirements review stated.
