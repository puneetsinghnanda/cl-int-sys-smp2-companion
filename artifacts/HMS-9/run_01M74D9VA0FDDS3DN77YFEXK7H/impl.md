---
schema: change.v1
run_id: run_01M74D9VA0FDDS3DN77YFEXK7H
step: impl
attempt: 2
skill: implement@1.1.0
inputs:
  - path: artifacts/HMS-9/run_01M74D9VA0FDDS3DN77YFEXK7H/design.md
    sha256: 2ea8104748653909b30e76525dd490d16d20f1645fa07ff4f40b533297f55d50
  - path: artifacts/HMS-9/run_01M74D9VA0FDDS3DN77YFEXK7H/split.md
    sha256: 7e5a680cb8c89a9d3304be453d11ee6e38652b7841fcf7f39799be1b9d6659f0
  - path: context/conventions.md
    sha256: dc53d94ec34eb193eeb06fdd0db19eb86a24c29f7d3c434e7c212d61038ca852
  - path: context/generated/module-map.json
    sha256: 07674e27e969e7fd56aba8a2a1419c1f597c1b4a9c490b574a56b82ff6571114
pack_sha:
  sample-system: c2709e164850ab7e177e4bccfc42a5dd9b961fdc
code:
  - repo: sample-system
    base_sha: c2709e164850ab7e177e4bccfc42a5dd9b961fdc
    head_sha: c7d693b59a821d278fa1514a278f7608e940b564
    diff_sha256: 66af2e3a8c86e62f84f5868047f3a5a4c99531b29ec3dc7d66cfc85d3cd081d9
    files:
      - api/src/services/doctorService.ts
      - api/src/types.ts
      - api/test/doctors-next-seven-days.test.ts
      - web/src/__tests__/DoctorDetailPage.test.tsx
      - web/src/api/types.ts
      - web/src/pages/DoctorDetailPage.tsx
resolves:
  - id: F-1
    how: "Added the T-1 entry-keys test and the T-7 UTC-vs-local midnight test to api/test/doctors-next-seven-days.test.ts."
claims:
  - id: C1
    text: "Doctor rules, including the doctor detail and the day schedule, live in `DoctorService`."
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/services/doctorService.ts:DoctorService"]
  - id: C2
    text: "The new field is named `nextSevenDays` and holds one entry per day with date, available, booked and free."
    cites: ["artifacts/HMS-9/run_01M74D9VA0FDDS3DN77YFEXK7H/design.md#claim:C12"]
  - id: C3
    text: "For an Inactive doctor the field is `null`."
    cites: ["artifacts/HMS-9/run_01M74D9VA0FDDS3DN77YFEXK7H/design.md#claim:C13"]
  - id: C4
    text: "Days are stepped with `addDays` and today comes from localDay in the hospital zone."
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/lib/time.ts:addDays"]
  - id: C5
    text: "One local day's slots come from `daySlots`, now classified by a single shared rule."
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/lib/slots.ts:daySlots"]
  - id: C6
    text: "The doctor page is rendered by `DoctorDetailPage`."
    cites: ["context/generated/module-map.json#sym:sample-system:web/src/pages/DoctorDetailPage.tsx:DoctorDetailPage"]
  - id: C7
    text: "Without a valid ?date= the day schedule still starts at `todayIn`."
    cites: ["context/generated/module-map.json#sym:sample-system:web/src/lib/format.ts:todayIn"]
  - id: C8
    text: "HMS-7 set the precedent that `the read-only count in a PATCH body is ignored`; nextSevenDays gets the same treatment."
    cites: ["artifacts/HMS-9/run_01M74D9VA0FDDS3DN77YFEXK7H/design.md#claim:C15"]
  - id: C9
    text: "Logs carry `method, route and error message` only; this change adds no new log lines."
    cites: ["context/conventions.md#logging-and-data"]
  - id: C10
    text: "S-1 AC1 fixes the clock at `2026-11-11T19:00Z`, 00:30 on 12 Nov in Asia/Kolkata, with rows 12 Nov to 18 Nov."
    cites: ["artifacts/HMS-9/run_01M74D9VA0FDDS3DN77YFEXK7H/split.md#item:S-1"]
trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10]
known: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10]
assumed: []
unknown: []
questions: []
summary: "Adds a read-only nextSevenDays summary to the doctor detail. The server computes it with the day schedule's slot-state rule, now shared. The doctor page gets a 'Next 7 days' section whose rows link to ?date=, and the day schedule reads that parameter. Inactive doctors get null and no section. Regression tests cover the entry keys, the UTC-vs-local midnight case, the schedule match and PATCH ignoring the field."
---
# Change: HMS-9

Resolved review findings:
- F-1: Added the T-1 entry-keys test and the T-7 UTC-vs-local midnight test to api/test/doctors-next-seven-days.test.ts.

## Summary
- **CO-1, CO-2 (api).** `DoctorService.get` now returns `nextSevenDays`.
  - For an Inactive doctor the value is `null`, and nothing is computed (A-07).
  - Otherwise it builds 7 hospital-local days: today from `localDay(clock, timeZone)`, then `addDays` for the next six. It reads the non-cancelled appointments once for the whole range (`occupyingBetween` from the first day's start to the last day's end). It classifies every day's `daySlots` with `slotStates`, the new private method that `schedule()` now uses too, so the two can't drift.
  - Each day is reported as `available: false` with null counts when it has no slots. Otherwise it is reported as BOOKED and FREE counts, so PAST slots count as neither.
  - `create`, `update` and `replaceAvailability` already return `get()`, so their responses carry the field too, as the design says (EP-3 to EP-5). The doctor body reader is unchanged, so the field is ignored in write bodies (C8).
- **CO-4, CO-6.** A `NextDay` interface is added beside `DoctorDetail` in both type files, and `DoctorDetail` gains `nextSevenDays: NextDay[] | null`.
- **CO-7 (web).** `DoctorDetailPage`:
  - renders "Next 7 days" (date, booked, free, or "Not available") before "Upcoming appointments", only when `nextSevenDays` is not null;
  - links each date to `/doctors/:id?date=YYYY-MM-DD`;
  - starts the day schedule's date from `?date=` when `validDay` accepts it (a strict YYYY-MM-DD real date), else from `todayIn`, and follows later changes of the parameter;
  - leaves the picker and every other section unchanged.
- No new logging and no new routes.

## Changes
- `api/src/services/doctorService.ts`: CO-1 adds `nextSevenDays()` and its call in `get()`. CO-2 extracts `slotStates()`, which `schedule()` now uses.
- `api/src/types.ts`: CO-4 adds `NextDay` and `DoctorDetail.nextSevenDays`.
- `web/src/api/types.ts`: CO-6 adds `NextDay` and `DoctorDetail.nextSevenDays`.
- `web/src/pages/DoctorDetailPage.tsx`: CO-7 adds the section, `validDay` and the `?date=` handling.
- `web/src/__tests__/DoctorDetailPage.test.tsx`: the existing typed fixture gains `nextSevenDays: null`, because the DoctorDetail type is marked changed (CO-6).
- `api/test/doctors-next-seven-days.test.ts`: the regression tests the design and threat model ask implement for.
  - "HMS-9 REQ-4": the summary equals the day schedule's states for every day (T-4).
  - "HMS-9 REQ-3 (T-1)": each entry holds only date, available, booked and free.
  - "HMS-9 REQ-2 (T-7)": with the clock at 2026-11-11T19:00Z, which is 00:30 on 12 Nov in Asia/Kolkata, the dates are 2026-11-12 to 2026-11-18 (S-1 AC1, C10).
  - "HMS-9 REQ-10": `nextSevenDays` in a PATCH body is ignored (T-6).

## Known
- REQ-1 to REQ-3, REQ-5, REQ-6: CO-7 renders the rows from `nextSevenDays` (C2, C6).
- REQ-2: today and the 7 days come from the hospital zone (C4), and the midnight test pins this (C10).
- REQ-4: one shared `slotStates` rule over `daySlots` (C1, C5).
- REQ-7, REQ-8: the `?date=` link and the initial date, with a fallback to `todayIn` (C7).
- REQ-9: null for Inactive doctors, and the section is hidden (C3).
- REQ-10: the other sections and the picker are untouched. The field is ignored in write bodies (C8), and no logging is added (C9).

## Assumed
None.

## Unknown
None.

## Open questions
None.
