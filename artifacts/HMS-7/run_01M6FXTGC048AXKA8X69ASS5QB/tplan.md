---
schema: test-plan.v1
run_id: run_01M6FXTGC048AXKA8X69ASS5QB
step: tplan
attempt: 1
skill: test-plan@1.0.1
inputs:
  - path: artifacts/HMS-7/run_01M6FXTGC048AXKA8X69ASS5QB/split.md
    sha256: 4efeb373d4d22b7d74c2c0ec55e82009f5ddb45abe3a9bc94a490c762f071910
  - path: artifacts/HMS-7/run_01M6FXTGC048AXKA8X69ASS5QB/design.md
    sha256: 215531f1ce96acf3cbf36175d23fe9f75a7d793b88397e524abe7591fb3b1056
  - path: context/test-strategy.md
    sha256: e7465d17d8814cd71fe160b96a859f1ba1414836ad71ef48b9aaeb166259eee8
  - path: context/generated/test-layout.json
    sha256: fd4b5d51b15f9aba1d03b5f6a0350e4e4849d862118fb3a3bda7abfa5127e179
pack_sha:
  sample-system: bd98ff08b83f8762c5e5c073ba5fadef88169b33
claims:
  - id: C1
    text: "API tests use `Supertest` against a throwaway PostgreSQL with a fixed clock."
    cites: ["context/test-strategy.md#layers"]
  - id: C2
    text: "Web tests use `jsdom` and Testing Library."
    cites: ["context/test-strategy.md#layers"]
  - id: C3
    text: "No page is tested end to end in a `browser`."
    cites: ["context/test-strategy.md#gaps"]
  - id: C4
    text: "Test names start with the requirement `ID`."
    cites: ["context/test-strategy.md#requirement-ids"]
  - id: C5
    text: "GET /api/doctors/:id adds `noShowsLast30Days`."
    cites: ["artifacts/HMS-7/run_01M6FXTGC048AXKA8X69ASS5QB/design.md#overview"]
  - id: C6
    text: "The link's `doctorId` must be spelled exactly."
    cites: ["artifacts/HMS-7/run_01M6FXTGC048AXKA8X69ASS5QB/design.md#overview"]
  - id: C7
    text: "S-2 AC2 compares the list `total`, not the page length."
    cites: ["artifacts/HMS-7/run_01M6FXTGC048AXKA8X69ASS5QB/design.md#overview"]
trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8]
known: [S-1, S-2, TP-1, TP-2, TP-3, TP-4, TP-5, TP-6, TP-7, TP-8, TP-9, TP-10, TP-11, TP-12, TP-13, TP-14]
assumed: []
unknown:
  - {id: U-1, blocking: false}
questions:
  - id: Q-01
    blocking: false
    ask: tech lead
    text: "TP-2's Cancelled-in-window and Scheduled-in-window cases are planned but not yet in the HMS-7 test code. Add them on this branch before merge, or as a follow-up?"
    why: "S-1 AC2 lists Cancelled and Scheduled appointments inside the window; the unit-test review noted only Completed and an out-of-window Scheduled appointment are exercised."
    evidence: ["artifacts/HMS-7/run_01M6FXTGC048AXKA8X69ASS5QB/split.md#stories"]
    options: ["Add on this branch before merge", "Follow-up ticket"]
    default: "Add on this branch before merge: two bookings in the existing doctors-no-shows fixture, one cancelled and one left Scheduled earlier on the same day."
tests:
  - {id: TP-1, title: "Doctor detail counts the doctor's No-show appointments that start in the 30d window", covers: [S-1/AC1], level: integration}
  - {id: TP-2, title: "No-shows before the window, appointments after today, and Scheduled, Completed and Cancelled appointments inside the window are not counted", covers: [S-1/AC2], level: integration}
  - {id: TP-3, title: "Doctor count and window equal the dashboard 30d No-shows count and period", covers: [S-1/AC3], level: integration}
  - {id: TP-4, title: "Details section shows a row labelled No-shows (last 30 days) with the count", covers: [S-1/AC1], level: unit}
  - {id: TP-5, title: "A doctor with no No-shows in the window gets 0 from the API and shows 0", covers: [S-1/AC4], level: integration}
  - {id: TP-6, title: "Weekly availability, upcoming appointments and day schedule still render", covers: [S-1/AC5], level: unit}
  - {id: TP-7, title: "lastDays: 30 days ends today in the hospital zone; day 30 counted, day 31 not", covers: [S-1/AC2, S-1/AC3], level: unit}
  - {id: TP-8, title: "Count links to /appointments with doctorId, status NO_SHOW, from and to exactly", covers: [S-2/AC1], level: unit}
  - {id: TP-9, title: "The linked list's total equals the count shown", covers: [S-2/AC2], level: integration}
  - {id: TP-10, title: "A count of 0 is still a link and its list total is 0", covers: [S-2/AC3], level: unit}
  - {id: TP-11, title: "Negative: a PATCH body carrying noShowsLast30Days does not change the count", covers: [S-1/AC1], level: integration}
  - {id: TP-12, title: "Negative: GET /api/doctors/:id for an unknown id is still 404", covers: [S-1/AC5], level: integration}
  - {id: TP-13, title: "Regression: existing dashboard No-shows tests still pass after the 30d rule is shared", covers: [S-1/AC3], level: integration}
  - {id: TP-14, title: "Manual: open a doctor page in a browser and follow the No-shows link", covers: [S-1/AC1, S-1/AC5, S-2/AC1], level: manual}
---
# Test plan: HMS-7

## Scope
In scope: the doctor detail API response (`noShowsLast30Days`, `noShowsWindow`), the shared 30d window
rule, the AppointmentRepo count, the doctor page row and its link, and the appointment list as the link
target. Also in scope: that the dashboard's 30d No-shows figure does not change.
Out of scope: the appointment list page itself (unchanged), the dashboard page UI, sign-in and
permissions (no change in HMS-7), and load testing.
Levels follow the test strategy: API behaviour at integration level (Supertest on a throwaway
PostgreSQL with a fixed clock), page behaviour at unit level (jsdom with Testing Library), pure window
logic at unit level. There is no browser end-to-end suite, so the one browser check is manual (TP-14).

## Tests
- TP-1 (integration) — S-1/AC1: count of the doctor's No-shows in the window. In api/test/doctors-no-shows.test.ts.
- TP-2 (integration) — S-1/AC2: exclusions. Completed and out-of-window cases exist; Cancelled-in-window and Scheduled-in-window are still to add (U-1, Q-01).
- TP-3 (integration) — S-1/AC3: equals the dashboard 30d count and period. In api/test/doctors-no-shows.test.ts.
- TP-4 (unit) — S-1/AC1: the labelled row in the details list. In web/src/__tests__/DoctorDetailPage.test.tsx.
- TP-5 (integration) — S-1/AC4: 0 for a doctor with none (API); the web test also shows 0.
- TP-6 (unit) — S-1/AC5: other sections still render.
- TP-7 (unit) — S-1/AC2, S-1/AC3: window edges and hospital day. In api/test/periods.test.ts, plus the day-30/day-31 case in the API test.
- TP-8 (unit) — S-2/AC1: exact link URL, including the `doctorId` spelling the list depends on.
- TP-9 (integration) — S-2/AC2: list total equals the count; compares `total`, since lists are paged.
- TP-10 (unit) — S-2/AC3: 0 is still a link; the API test also shows its list total is 0.
- TP-11 (integration, negative for the new field) — S-1/AC1: PATCH cannot set the read-only count.
- TP-12 (integration, negative for the changed endpoint) — S-1/AC5: unknown id still 404.
- TP-13 (integration, regression) — S-1/AC3: existing api/test/dashboard.test.ts and api/test/dashboard-no-shows.test.ts.
- TP-14 (manual) — S-1/AC1, S-1/AC5, S-2/AC1: one browser walk-through, because the project has no browser end-to-end tests.

Coverage: S-1/AC1 (TP-1, TP-4, TP-11, TP-14), S-1/AC2 (TP-2, TP-7), S-1/AC3 (TP-3, TP-7, TP-13), S-1/AC4 (TP-5), S-1/AC5 (TP-6, TP-12, TP-14), S-2/AC1 (TP-8, TP-14), S-2/AC2 (TP-9), S-2/AC3 (TP-10).

## Known
- S-1, S-2: every acceptance criterion is covered by at least one test.
- TP-1, TP-3, TP-5, TP-9, TP-11, TP-12: integration tests on the doctor detail API.
- TP-2: integration test of the exclusions; partly automated (see U-1).
- TP-4, TP-6, TP-8, TP-10: unit tests of the doctor page.
- TP-7: unit test of the shared window rule.
- TP-13: existing dashboard tests reused as the regression guard.
- TP-14: manual browser check, because there is no browser end-to-end suite.

## Assumed
None.

## Unknown
- U-1 (non-blocking): the Cancelled-in-window and Scheduled-in-window cases of TP-2 are not yet in the test code — Q-01.

## Open questions
- Q-01 (non-blocking, tech lead): add TP-2's missing cases on this branch before merge, or as a follow-up? Default: add them on this branch before merge.
