---
schema: design.v1
run_id: run_01M74D9VA0FDDS3DN77YFEXK7H
step: design
attempt: 1
skill: design@1.7.0
inputs:
  - path: artifacts/HMS-9/run_01M74D9VA0FDDS3DN77YFEXK7H/impact.md
    sha256: 6a8917d093b964f8c187482843359ed33a2b14310dd1885d78d828836927e08a
  - path: artifacts/HMS-9/run_01M74D9VA0FDDS3DN77YFEXK7H/split.md
    sha256: 7e5a680cb8c89a9d3304be453d11ee6e38652b7841fcf7f39799be1b9d6659f0
  - path: artifacts/HMS-9/run_01M74D9VA0FDDS3DN77YFEXK7H/req.md
    sha256: d91af6d2d33b27873ad261064f8454e09c4904922e83e8ea0b13ffb56bc031e8
  - path: context/intent-baseline.md
    sha256: 9bfd821aeb2fb9524863e346707e049e4fcfb949a6e55b90223a79194946a215
  - path: context/architecture.md
    sha256: 9faab093d10933a46f6473b25c80728f8745e597100e650cd2537bbafb2b595a
  - path: context/generated/module-map.json
    sha256: 07674e27e969e7fd56aba8a2a1419c1f597c1b4a9c490b574a56b82ff6571114
  - path: context/generated/api-inventory.json
    sha256: dd1254b0a78f85d6da3dd3722fd41947c0cec5177fe5b46028e49af681dc4c85
pack_sha:
  sample-system: c2709e164850ab7e177e4bccfc42a5dd9b961fdc
claims:
  - id: C1
    text: "Doctor rules, including the doctor detail and the day schedule, live in `DoctorService`."
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/services/doctorService.ts:DoctorService"]
  - id: C2
    text: "One local day's slots come from `daySlots`."
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/lib/slots.ts:daySlots"]
  - id: C3
    text: "The hospital-local date of an instant comes from `localDay`."
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/lib/time.ts:localDay"]
  - id: C4
    text: "Days are stepped with `addDays`."
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/lib/time.ts:addDays"]
  - id: C5
    text: "A local day's UTC bounds come from `dayBounds`."
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/lib/time.ts:dayBounds"]
  - id: C6
    text: "Appointments are read through `AppointmentRepo`."
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/repositories/appointmentRepo.ts:AppointmentRepo"]
  - id: C7
    text: "The doctor page is rendered by `DoctorDetailPage`."
    cites: ["context/generated/module-map.json#sym:sample-system:web/src/pages/DoctorDetailPage.tsx:DoctorDetailPage"]
  - id: C8
    text: "The web app's local today comes from `todayIn`."
    cites: ["context/generated/module-map.json#sym:sample-system:web/src/lib/format.ts:todayIn"]
  - id: C9
    text: "The API's doctor detail type `DoctorDetail` is declared in api/src/types.ts."
    cites: ["sample-system:api/src/types.ts#L60"]
  - id: C10
    text: "The web client's doctor detail type `DoctorDetail` is declared in web/src/api/types.ts."
    cites: ["sample-system:web/src/api/types.ts#L58"]
  - id: C11
    text: "The counts are added to the doctor detail response, following the HMS-7 `noShowsLast30Days` precedent."
    cites: ["artifacts/HMS-9/run_01M74D9VA0FDDS3DN77YFEXK7H/impact.md#claim:C10"]
  - id: C12
    text: "The new field is named `nextSevenDays` and holds one entry per day with date, available, booked and free."
    cites: ["runs/run_01M74D9VA0FDDS3DN77YFEXK7H/ide-answers.md#a-06"]
  - id: C13
    text: "For an Inactive doctor the field is `null`."
    cites: ["runs/run_01M74D9VA0FDDS3DN77YFEXK7H/ide-answers.md#a-07"]
  - id: C14
    text: "Availability, slots and today use the hospital time zone `HOSPITAL_TZ`."
    cites: ["context/architecture.md#time-and-slots"]
  - id: C15
    text: "The read-only summary must be ignored in a `PATCH` body, as HMS-7 did for its count."
    cites: ["artifacts/HMS-9/run_01M74D9VA0FDDS3DN77YFEXK7H/impact.md#claim:C14"]
  - id: C16
    text: "Routes parse input and services hold the rules; the web app calls the API only through `web/src/api/client.ts`."
    cites: ["context/architecture.md#components"]
  - id: C17
    text: "The day link uses a `?date=` parameter on the doctor page."
    cites: ["artifacts/HMS-9/run_01M74D9VA0FDDS3DN77YFEXK7H/split.md#claim:C5"]
trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10]
known: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10]
assumed: []
unknown: []
questions: []
components:
  - id: CO-1
    name: DoctorService.get (doctor detail) adds nextSevenDays
    module: module:sample-system:api
    change: changed
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/services/doctorService.ts:DoctorService", "runs/run_01M74D9VA0FDDS3DN77YFEXK7H/ide-answers.md#a-06", "runs/run_01M74D9VA0FDDS3DN77YFEXK7H/ide-answers.md#a-07"]
  - id: CO-2
    name: Shared slot-state rule in DoctorService, used by both schedule() and the new summary
    module: module:sample-system:api
    change: changed
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/services/doctorService.ts:DoctorService", "context/generated/module-map.json#sym:sample-system:api/src/lib/slots.ts:daySlots"]
  - id: CO-3
    name: AppointmentRepo (reads non-cancelled appointments over the 7-day range)
    module: module:sample-system:api
    change: existing
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/repositories/appointmentRepo.ts:AppointmentRepo"]
  - id: CO-4
    name: api DoctorDetail type gains nextSevenDays
    module: module:sample-system:api
    change: changed
    cites: ["sample-system:api/src/types.ts#L60", "runs/run_01M74D9VA0FDDS3DN77YFEXK7H/ide-answers.md#a-06"]
  - id: CO-5
    name: lib/slots and lib/time helpers (daySlots, localDay, addDays, dayBounds)
    module: module:sample-system:api
    change: existing
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/lib/slots.ts:daySlots", "context/generated/module-map.json#sym:sample-system:api/src/lib/time.ts:addDays"]
  - id: CO-6
    name: web DoctorDetail type gains nextSevenDays
    module: module:sample-system:web
    change: changed
    cites: ["sample-system:web/src/api/types.ts#L58", "runs/run_01M74D9VA0FDDS3DN77YFEXK7H/ide-answers.md#a-06"]
  - id: CO-7
    name: DoctorDetailPage renders the Next 7 days section and reads ?date= for the day schedule
    module: module:sample-system:web
    change: changed
    cites: ["context/generated/module-map.json#sym:sample-system:web/src/pages/DoctorDetailPage.tsx:DoctorDetailPage", "runs/run_01M74D9VA0FDDS3DN77YFEXK7H/ide-answers.md#a-02", "runs/run_01M74D9VA0FDDS3DN77YFEXK7H/ide-answers.md#a-07"]
endpoints:
  - id: EP-1
    method: GET
    path: /api/doctors/:id
    change: changed
    fields:
      - {name: nextSevenDays, type: "array of NextDay", nullable: true, format: "7 entries in date order, today to 6 days ahead; null when the doctor is Inactive", example: "[{\"date\":\"2026-11-12\",\"available\":true,\"booked\":1,\"free\":2}, ...]", status: known}
      - {name: "nextSevenDays[].date", type: string, nullable: false, format: "YYYY-MM-DD, hospital-local date (HOSPITAL_TZ, Asia/Kolkata)", example: "2026-11-12", status: known}
      - {name: "nextSevenDays[].available", type: boolean, nullable: false, format: "false when the day has no slots", example: "true", status: known}
      - {name: "nextSevenDays[].booked", type: integer, nullable: true, format: "count of BOOKED slots, as the day schedule shows them; null when available is false", example: "1", status: known}
      - {name: "nextSevenDays[].free", type: integer, nullable: true, format: "count of FREE slots, as the day schedule shows them; null when available is false", example: "2", status: known}
    cites: ["context/generated/api-inventory.json#http:GET:/api/doctors/:id", "runs/run_01M74D9VA0FDDS3DN77YFEXK7H/ide-answers.md#a-06", "runs/run_01M74D9VA0FDDS3DN77YFEXK7H/ide-answers.md#a-07"]
  - id: EP-2
    method: GET
    path: /api/doctors/:id/slots
    change: existing
    fields: []
    cites: ["context/generated/api-inventory.json#http:GET:/api/doctors/:id/slots"]
  - id: EP-3
    method: PATCH
    path: /api/doctors/:id
    change: existing
    fields: []
    cites: ["context/generated/api-inventory.json#http:PATCH:/api/doctors/:id", "artifacts/HMS-9/run_01M74D9VA0FDDS3DN77YFEXK7H/impact.md#claim:C14"]
answers:
  - id: A-06
    kind: question
    about: Q-01
    answer: "nextSevenDays array (proposed default)"
    decides: [EP-1, CO-1, CO-4, CO-6, C12]
  - id: A-07
    kind: question
    about: Q-02
    answer: "null (proposed default)"
    decides: [EP-1, CO-1, CO-7, C13]
---
# Design: HMS-9

## Overview
The doctor detail response (`GET /api/doctors/:id`) gains a read-only field, `nextSevenDays`, as decided in A-05 and A-06. The web doctor page renders it as the "Next 7 days" section.

**How the server builds it (CO-1).** In `DoctorService.get`:
1. If the doctor is Inactive, return `nextSevenDays: null` (A-07).
2. Otherwise, take today as `localDay(clock, HOSPITAL_TZ)` and build the 7 dates from today to today + 6 with `addDays`.
3. Read the doctor's non-cancelled appointments once over the whole range, using the existing AppointmentRepo (the range comes from `dayBounds` of the first and last day).
4. For each date, take `daySlots(...)` and classify every slot with the same rule the day schedule uses: BOOKED when an appointment overlaps it, else PAST when it has started, else FREE.
5. A date with no slots gets `available: false` with null counts. Otherwise the date gets `booked` = the BOOKED slots and `free` = the FREE slots, so PAST slots count as neither (REQ-4, A-01).

**Sharing the rule (CO-2).** The day schedule and the summary must not drift (REQ-4). So the slot-state rule now inside `DoctorService.schedule` is pulled into one private method that both `schedule()` and the summary call. The name is left to implement.
- Trade-off: a small refactor of `schedule()`, in return for one rule instead of two copies. The existing day-schedule tests guard the refactor.
- Trade-off: one appointment query for the whole week, instead of seven per-day queries. This costs a little in-memory filtering, but the doctor detail still makes a fixed number of queries.

**The web page (CO-7).** `DoctorDetailPage`:
- renders the section from `d.nextSevenDays` and hides it when the value is null (REQ-9);
- shows each row's date, booked and free counts, or "Not available" when `available` is false (REQ-5, REQ-6);
- links each row to `/doctors/:id?date=YYYY-MM-DD` (REQ-7, A-02);
- starts the day schedule's date state from the `date` search parameter when it holds a valid `YYYY-MM-DD`, else from `todayIn(timeZone)` as now (REQ-8);
- keeps the date state in sync when the URL's `date` changes (for example, when a row is clicked on the same page).

Trade-off: the date picker does not write back to the URL. That keeps today's picker behaviour unchanged (REQ-10), but a hand-picked date isn't bookmarkable.

**The PATCH route (EP-3).** No change. Its body reader takes only the doctor's editable fields, which is why HMS-7's read-only count is ignored (C15). `nextSevenDays` sent in a PATCH body is ignored in the same way; implement adds a test for it.

## Components
- CO-1, DoctorService.get, **changed** (api). Adds `nextSevenDays`.
- CO-2, the shared slot-state rule in DoctorService, **changed** (api). Used by `schedule()` and by the summary.
- CO-3, AppointmentRepo, **existing** (api). Reads non-cancelled appointments over the range.
- CO-4, the api DoctorDetail type in api/src/types.ts, **changed**. Gains `nextSevenDays: NextDay[] | null`.
- CO-5, the lib/slots and lib/time helpers, **existing** (api).
- CO-6, the web DoctorDetail type in web/src/api/types.ts, **changed**. Gains the same field.
- CO-7, DoctorDetailPage, **changed** (web). Adds the section and the `?date=` handling.
- EP-1, GET /api/doctors/:id, **changed**. New fields:
  - `nextSevenDays`: array or null. Null when the doctor is Inactive. Otherwise 7 entries in date order, from today to 6 days ahead.
  - `nextSevenDays[].date`: string, `YYYY-MM-DD`, the hospital-local date (Asia/Kolkata). Example: `2026-11-12`.
  - `nextSevenDays[].available`: boolean. False when the day has no slots.
  - `nextSevenDays[].booked`: integer or null. The BOOKED slots as the day schedule shows them; null when not available.
  - `nextSevenDays[].free`: integer or null. The FREE slots as the day schedule shows them; null when not available.
- EP-2, GET /api/doctors/:id/slots, **existing**. Still serves the day schedule.
- EP-3, PATCH /api/doctors/:id, **existing**. `nextSevenDays` in the body is ignored.

How the stories are served:
- S-1 → CO-1, CO-2, CO-5, CO-7, EP-1
- S-2 → CO-1, CO-7, EP-1
- S-3 → CO-7, EP-2
- S-4 → CO-1, CO-7, EP-1, EP-3

## Known
- REQ-1 to REQ-3: CO-7 renders the rows from EP-1 (C7, C12).
- REQ-2: today comes from `localDay` in HOSPITAL_TZ, and the dates are stepped with `addDays` (C3, C4, C14).
- REQ-4: CO-2 shares the day-schedule rule over `daySlots` (C1, C2).
- REQ-5, REQ-6: `available` is false only when the day has no slots; otherwise free can be 0 (C12).
- REQ-7, REQ-8: the `?date=` link and the initial date state in CO-7 (C8, C17).
- REQ-9: `nextSevenDays` is null for Inactive doctors, and the page hides the section (C13).
- REQ-10: the slots and PATCH routes are unchanged (C15), and the other page sections are untouched.

## Assumed
None.

## Unknown
None.

## Open questions
None.

Answered in the IDE:
- A-06, Q-01, the field shape: "nextSevenDays array (proposed default)". Decides EP-1, CO-1, CO-4, CO-6 and C12. No mismatch.
- A-07, Q-02, the value for an Inactive doctor: "null (proposed default)". Decides EP-1, CO-1, CO-7 and C13. No mismatch.
