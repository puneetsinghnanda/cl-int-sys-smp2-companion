---
schema: test-plan.v1
run_id: run_01M74D9VA0FDDS3DN77YFEXK7H
step: tplan
attempt: 2
skill: test-plan@1.1.0
inputs:
  - path: artifacts/HMS-9/run_01M74D9VA0FDDS3DN77YFEXK7H/split.md
    sha256: 7e5a680cb8c89a9d3304be453d11ee6e38652b7841fcf7f39799be1b9d6659f0
  - path: artifacts/HMS-9/run_01M74D9VA0FDDS3DN77YFEXK7H/design.md
    sha256: 2ea8104748653909b30e76525dd490d16d20f1645fa07ff4f40b533297f55d50
  - path: context/test-strategy.md
    sha256: f4bbc9d5d63dfc433c9fe466a7aa71a4fad0147baafe1f47db0c06e117ce6cba
  - path: context/generated/test-layout.json
    sha256: 57a01551ebf3a7429a78830822e19e5384fc83b3f91c6b101bbcfb9cec22038f
pack_sha:
  sample-system: c2709e164850ab7e177e4bccfc42a5dd9b961fdc
resolves:
  - id: F-1
    how: "Added TP-21, a unit test that an Inactive doctor's page with upcoming appointments keeps every other section and action as before."
  - id: F-2
    how: "Added TP-22 and TP-23, integration tests that POST ignores nextSevenDays in the body and that POST and availability PUT responses carry the computed field."
claims:
  - id: C1
    text: "API tests use Vitest and `Supertest` against a throwaway PostgreSQL 16 with a fixed clock."
    cites: ["context/test-strategy.md#layers"]
  - id: C2
    text: "Web tests use Vitest with jsdom and `Testing Library`."
    cites: ["context/test-strategy.md#layers"]
  - id: C3
    text: "Every requirement has at least one test whose name starts with its ID; new requirements get `new IDs and tests`."
    cites: ["context/test-strategy.md#requirement-ids"]
  - id: C4
    text: "No page is tested `end to end` in a browser."
    cites: ["context/test-strategy.md#gaps"]
  - id: C5
    text: "The existing doctor page tests live in `DoctorDetailPage.test.tsx`."
    cites: ["context/generated/test-layout.json#test:sample-system:web/src/__tests__/DoctorDetailPage.test.tsx"]
  - id: C6
    text: "The new field is named `nextSevenDays` and holds one entry per day with date, available, booked and free."
    cites: ["artifacts/HMS-9/run_01M74D9VA0FDDS3DN77YFEXK7H/design.md#claim:C12"]
  - id: C7
    text: "HMS-7 set the precedent that `the read-only count in a PATCH body is ignored`."
    cites: ["artifacts/HMS-9/run_01M74D9VA0FDDS3DN77YFEXK7H/design.md#claim:C15"]
  - id: C8
    text: "The day link uses a `?date=` parameter on the doctor page."
    cites: ["artifacts/HMS-9/run_01M74D9VA0FDDS3DN77YFEXK7H/design.md#claim:C17"]
  - id: C9
    text: "DoctorService.create ends with `return this.get(id);`, so the POST response is the doctor detail."
    cites: ["artifacts/HMS-9/run_01M74D9VA0FDDS3DN77YFEXK7H/design.md#claim:C18"]
  - id: C10
    text: "DoctorService.replaceAvailability ends with `return this.get(id);`, so the availability PUT response is the doctor detail."
    cites: ["artifacts/HMS-9/run_01M74D9VA0FDDS3DN77YFEXK7H/design.md#claim:C20"]
trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10]
known: [TP-1, TP-2, TP-3, TP-4, TP-5, TP-6, TP-7, TP-8, TP-9, TP-10, TP-11, TP-12, TP-13, TP-14, TP-15, TP-16, TP-17, TP-18, TP-19, TP-20, TP-21, TP-22, TP-23]
assumed: []
unknown: []
questions: []
tests:
  - {id: TP-1, title: "API: at 2026-11-11T19:00Z (00:30 on 12 Nov IST) nextSevenDays dates are 2026-11-12 to 2026-11-18", covers: [S-1/AC1], level: integration}
  - {id: TP-2, title: "Web: section 'Next 7 days' renders 7 rows with date, booked, free", covers: [S-1/AC1, S-1/AC2], level: unit}
  - {id: TP-3, title: "API: each day's booked/free equal the BOOKED/FREE slots of GET /slots for that date", covers: [S-1/AC2, S-1/AC4], level: integration}
  - {id: TP-4, title: "API: passed Completed slot counts as booked, passed empty slot as neither, future slot as free", covers: [S-1/AC3], level: integration}
  - {id: TP-5, title: "API: cancelling an appointment frees its slot in the day's counts", covers: [S-1/AC4], level: integration}
  - {id: TP-6, title: "API: a day without slots is available false with null booked and free", covers: [S-2/AC1], level: integration}
  - {id: TP-7, title: "Web: an unavailable day shows 'Not available' instead of counts", covers: [S-2/AC1], level: unit}
  - {id: TP-8, title: "API: passed No-show slot counts as booked; row shows free 0, still available", covers: [S-2/AC2], level: integration}
  - {id: TP-9, title: "API: at 18:00 local a day whose slots all passed empty shows booked 0, free 0, available true", covers: [S-2/AC3], level: integration}
  - {id: TP-10, title: "Web: free 0 row shows counts, not 'Not available'", covers: [S-2/AC2, S-2/AC3], level: unit}
  - {id: TP-11, title: "Web: each date links to /doctors/:id?date=YYYY-MM-DD, including a 'Not available' day", covers: [S-3/AC1, S-3/AC4], level: unit}
  - {id: TP-12, title: "Web: opening /doctors/:id?date=2026-11-14 sets the day schedule date to 2026-11-14", covers: [S-3/AC2, S-3/AC4], level: unit}
  - {id: TP-13, title: "Web: without ?date= the day schedule starts at local today across UTC midnight (todayIn)", covers: [S-3/AC3], level: unit}
  - {id: TP-14, title: "Web negative: an invalid ?date= is ignored and the day schedule falls back to today", covers: [S-3/AC2], level: unit}
  - {id: TP-15, title: "API: an Inactive doctor's nextSevenDays is null", covers: [S-4/AC1], level: integration}
  - {id: TP-16, title: "Web: no 'Next 7 days' section when nextSevenDays is null (Inactive doctor)", covers: [S-4/AC1, S-4/AC4], level: unit}
  - {id: TP-17, title: "Web regression: the existing HMS-7 tests and the other sections still render with the new section", covers: [S-4/AC2], level: unit}
  - {id: TP-18, title: "Web: no availability shows 7 'Not available' rows and the existing empty messages", covers: [S-4/AC3], level: unit}
  - {id: TP-19, title: "API negative: nextSevenDays sent in a PATCH body is ignored; response holds the computed value", covers: [S-4/AC2], level: integration}
  - {id: TP-20, title: "API negative: each entry holds only date, available, booked, free; unknown doctor is still 404", covers: [S-1/AC2, S-4/AC2], level: integration}
  - {id: TP-21, title: "Web regression: Inactive doctor with upcoming appointments keeps all other sections and actions", covers: [S-4/AC4], level: unit}
  - {id: TP-22, title: "API negative: POST ignores nextSevenDays in the body; response holds the computed value", covers: [S-4/AC2], level: integration}
  - {id: TP-23, title: "API: availability PUT response recomputes nextSevenDays from the new blocks", covers: [S-4/AC2, S-2/AC1], level: integration}
---
# Test plan: HMS-9

Resolved review findings:
- F-1: Added TP-21, a unit test that an Inactive doctor's page with upcoming appointments keeps every other section and action as before.
- F-2: Added TP-22 and TP-23, integration tests that POST ignores nextSevenDays in the body and that POST and availability PUT responses carry the computed field.

## Scope
**In scope:**
- The new `nextSevenDays` field on the doctor detail (C6), including how it counts against the day schedule.
- The Inactive null and the hospital-zone dates.
- The field's read-only handling and recomputation on every route that returns the doctor detail: PATCH (C7), POST (C9) and availability PUT (C10).
- The "Next 7 days" section on the doctor page, and the `?date=` link and parameter (C8).
- Regression of the rest of the page, for both active and Inactive doctors.

API behaviour is tested at the integration level with Supertest and a fixed clock (C1). Page behaviour is tested at the unit level with Testing Library (C2), in the existing doctor page test file (C5). Every test name starts with its HMS-9 requirement ID (C3).

**Out of scope:** browser end-to-end tests, because the project has none (C4). The slot picker and booking flows are unchanged.

## Tests
- TP-1, integration. The UTC-vs-local midnight case: the dates run 12 to 18 Nov. Covers S-1/AC1.
- TP-2, unit. 7 rows, each with date, booked and free. Covers S-1/AC1 and S-1/AC2.
- TP-3, integration. The counts equal the day schedule for every day. Covers S-1/AC2 and S-1/AC4.
- TP-4, integration. A passed Completed slot is booked, a passed empty slot counts as neither, and a future slot is free. Covers S-1/AC3.
- TP-5, integration. A Cancelled appointment frees its slot. Covers S-1/AC4.
- TP-6, integration. A day with no slots: available false, null counts. Covers S-2/AC1.
- TP-7, unit. "Not available" is shown. Covers S-2/AC1.
- TP-8, integration. A passed No-show slot is booked; free 0, still available. Covers S-2/AC2.
- TP-9, integration. Slots all passed empty at 18:00: booked 0, free 0. Covers S-2/AC3.
- TP-10, unit. A free 0 row shows counts, not "Not available". Covers S-2/AC2 and S-2/AC3.
- TP-11, unit. Row links use `?date=`, including on a "Not available" day. Covers S-3/AC1 and S-3/AC4.
- TP-12, unit. `?date=` sets the day schedule's date. Covers S-3/AC2 and S-3/AC4.
- TP-13, unit. Without `?date=`, the date falls back to local today across UTC midnight. Covers S-3/AC3.
- TP-14, unit, negative. An invalid `?date=` (for example 2026-02-30, markup, or the wrong format) is ignored, and the date falls back to today. Covers S-3/AC2.
- TP-15, integration. An Inactive doctor gets null. Covers S-4/AC1.
- TP-16, unit. The section is hidden for an Inactive doctor. Covers S-4/AC1 and S-4/AC4.
- TP-17, unit, regression. The HMS-7 tests and the other sections are unchanged. Covers S-4/AC2.
- TP-18, unit. No availability: 7 "Not available" rows, and the existing empty messages stay. Covers S-4/AC3.
- TP-19, integration, negative. A `nextSevenDays` value sent in a PATCH body is ignored; the response holds the computed value. Covers S-4/AC2.
- TP-20, integration, negative. Entry keys only, and an unknown doctor is still 404. Covers S-1/AC2 and S-4/AC2.
- TP-21, unit, regression. An Inactive doctor with upcoming appointments keeps the existing Inactive warning, upcoming list, details and actions as before; only the new section is absent. Covers S-4/AC4.
- TP-22, integration, negative. POST ignores `nextSevenDays` in the body, and the response holds the value computed for the new doctor. Covers S-4/AC2.
- TP-23, integration. The availability PUT response recomputes `nextSevenDays` from the new blocks, so a day that gains a block becomes available. Covers S-4/AC2 and S-2/AC1.

## Known
- TP-1 to TP-23: each maps to story criteria and a level the strategy names (C1 to C3). Every S-n/ACm from S-1/AC1 to S-4/AC4 appears in at least one covers list. The negative tests TP-14, TP-19, TP-20 and TP-22 cover the new `?date=` input and the new response field on every route that returns it.

## Assumed
None.

## Unknown
None.

## Open questions
None.
