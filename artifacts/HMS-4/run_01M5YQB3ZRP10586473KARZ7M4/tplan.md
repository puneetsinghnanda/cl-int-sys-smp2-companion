---
schema: test-plan.v1
run_id: run_01M5YQB3ZRP10586473KARZ7M4
step: tplan
attempt: 1
skill: test-plan@1.0.1
inputs:
  - path: artifacts/HMS-4/run_01M5YQB3ZRP10586473KARZ7M4/split.md
    sha256: 5ced1c4d84b9c46da98051a025953a0aea7312cd53a6d5686efdebadb117024e
  - path: artifacts/HMS-4/run_01M5YQB3ZRP10586473KARZ7M4/design.md
    sha256: 9c01a9c1ddd65536f8dc4ae53ecaadecbec0ffc9b6b47d6e5d21557a93acb30a
  - path: context/test-strategy.md
    sha256: e7465d17d8814cd71fe160b96a859f1ba1414836ad71ef48b9aaeb166259eee8
  - path: context/generated/test-layout.json
    sha256: fd4b5d51b15f9aba1d03b5f6a0350e4e4849d862118fb3a3bda7abfa5127e179
pack_sha:
  sample-system: bd98ff08b83f8762c5e5c073ba5fadef88169b33
claims:
  - id: C1
    text: "API tests use Vitest and `Supertest` against a throwaway PostgreSQL 16 with a fixed clock."
    cites: ["context/test-strategy.md#layers"]
  - id: C2
    text: "Web tests use Vitest with jsdom and `Testing Library`."
    cites: ["context/test-strategy.md#layers"]
  - id: C3
    text: "Every requirement has a test whose name starts with its `ID`."
    cites: ["context/test-strategy.md#requirement-ids"]
  - id: C4
    text: "No page is tested `end to end` in a browser."
    cites: ["context/test-strategy.md#gaps"]
  - id: C5
    text: "An existing API test file covers the patient list next appointment: `api/test/patients-next-appointment.test.ts`."
    cites: ["context/generated/test-layout.json#test:sample-system:api/test/patients-next-appointment.test.ts"]
  - id: C6
    text: "An existing API test file covers patients: `api/test/patients.test.ts`."
    cites: ["context/generated/test-layout.json#test:sample-system:api/test/patients.test.ts"]
  - id: C7
    text: "An existing web test file covers the patient list page: `web/src/__tests__/PatientsPage.test.tsx`."
    cites: ["context/generated/test-layout.json#test:sample-system:web/src/__tests__/PatientsPage.test.tsx"]
trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10, REQ-11, REQ-12, REQ-13]
known: [REQ-1, REQ-2, REQ-3, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10, REQ-11, REQ-12, REQ-13]
assumed: [REQ-4]
unknown: [U-1]
unknowns:
  - {id: U-1, blocking: false, text: "No browser end-to-end level exists in this system, so the Last visit column is proven by a jsdom component test plus API integration tests, not by a real browser.", question: Q-01}
questions:
  - id: Q-01
    blocking: false
    ask: tech lead
    text: "Is a jsdom component test plus API integration tests enough proof for the Last visit column, given there is no browser end-to-end level?"
    why: "The strategy has no e2e tool; adding one is outside HMS-4."
    evidence: ["context/test-strategy.md#gaps"]
    options: ["Accept component plus integration tests", "Add a manual browser check before release"]
    default: "Accept component plus integration tests"
tests:
  - {id: TP-1, title: "Patient list shows a column titled Last visit", covers: [S-1/AC1], level: unit}
  - {id: TP-2, title: "One Completed appointment gives its hospital-local day", covers: [S-1/AC2], level: integration}
  - {id: TP-3, title: "Two Completed appointments give the later start, even when the later one was closed first", covers: [S-1/AC3], level: integration}
  - {id: TP-4, title: "The day follows the configured hospital zone: same data read with Asia/Kolkata and with a zone west of UTC gives different days", covers: [S-1/AC4], level: integration}
  - {id: TP-5, title: "The page shows the day string as returned when the browser zone is west of UTC", covers: [S-1/AC4], level: unit}
  - {id: TP-6, title: "A later Cancelled appointment leaves the Completed day", covers: [S-2/AC1], level: integration}
  - {id: TP-7, title: "A later No-show appointment leaves the Completed day", covers: [S-2/AC2], level: integration}
  - {id: TP-8, title: "A Scheduled appointment never becomes the last visit", covers: [S-2/AC3], level: integration}
  - {id: TP-9, title: "A patient with only Cancelled, No-show and Scheduled appointments gets null", covers: [S-2/AC4], level: integration}
  - {id: TP-10, title: "A patient with no appointments gets null", covers: [S-2/AC5], level: integration}
  - {id: TP-11, title: "A null last visit renders as -", covers: [S-2/AC4, S-2/AC5], level: unit}
  - {id: TP-12, title: "With 21 patients, page 1 has 20 and page 2 has 1, each with last_visit_date", covers: [S-3/AC1], level: integration}
  - {id: TP-13, title: "Search by name returns the same patients, now with last_visit_date", covers: [S-3/AC2], level: integration}
  - {id: TP-14, title: "The list stays newest first", covers: [S-3/AC3], level: integration}
  - {id: TP-15, title: "Next appointment values are unchanged (existing HMS-1 suite still passes)", covers: [S-3/AC4], level: integration}
  - {id: TP-16, title: "Next appointment stays the last column, after Last visit", covers: [S-3/AC4], level: unit}
  - {id: TP-17, title: "List items carry no medical-history field or value, and no raw instant", covers: [S-4/AC1], level: integration}
  - {id: TP-18, title: "Negative: an invalid page is rejected with 400 and no list", covers: [S-3/AC1], level: integration}
  - {id: TP-19, title: "Negative: an invalid status filter is rejected with a status field error", covers: [S-3/AC2], level: integration}
  - {id: TP-20, title: "Boundary: a page past the end is empty and keeps the total", covers: [S-3/AC1], level: integration}
---
# Test plan: HMS-4

## Scope
In scope:
- The new `last_visit_date` field on GET /api/patients and how it is derived from Completed appointments in the hospital zone.
- The Last visit column on the patient list page.
- The behaviour the change must not move: paging, search, sort, the Next appointment column, and the absence of medical history.

Levels follow the test strategy:
- Integration means API tests with Vitest and Supertest on a throwaway PostgreSQL with a fixed clock (C1). These live in api/test, next to the existing patient tests (C5, C6).
- Unit means web component tests with Vitest, jsdom and Testing Library (C2), in the existing patient list page test (C7).
- Test names start with HMS-4 and the REQ id (C3).

Out of scope: browser end-to-end tests, because the system has none (C4, U-1); the patient's own page, which the change does not touch; and permission refusal, which cannot be tested while the MVP grants every permission.

## Tests
- TP-1 (unit) S-1/AC1: a column titled Last visit is present.
- TP-2 (integration) S-1/AC2: one Completed appointment gives its day.
- TP-3 (integration) S-1/AC3: of two Completed appointments, the later start wins even when it was closed first, so ordering by closing time fails the test (assumed REQ-4).
- TP-4 (integration) S-1/AC4: the same data read through Asia/Kolkata and through a zone west of UTC gives different days, proving the configured zone is used.
- TP-5 (unit) S-1/AC4: the page shows the returned day unchanged with the browser zone west of UTC.
- TP-6 (integration) S-2/AC1: a later Cancelled appointment does not count.
- TP-7 (integration) S-2/AC2: a later No-show appointment does not count.
- TP-8 (integration) S-2/AC3: a Scheduled appointment does not count.
- TP-9 (integration) S-2/AC4: only Cancelled, No-show and Scheduled appointments give null.
- TP-10 (integration) S-2/AC5: no appointments give null.
- TP-11 (unit) S-2/AC4, S-2/AC5: null renders as "-".
- TP-12 (integration) S-3/AC1: with 21 patients, pages hold 20 and 1.
- TP-13 (integration) S-3/AC2: search returns the same patients.
- TP-14 (integration) S-3/AC3: the order stays newest first.
- TP-15 (integration) S-3/AC4: the existing HMS-1 next-appointment suite still passes unchanged (C5).
- TP-16 (unit) S-3/AC4: Next appointment stays the last column.
- TP-17 (integration) S-4/AC1: no medical-history field or value, and no raw instant, appears in the list.
- TP-18 (integration, negative) S-3/AC1: an invalid page is rejected with 400.
- TP-19 (integration, negative) S-3/AC2: an invalid status filter is rejected with a field error.
- TP-20 (integration, boundary) S-3/AC1: a page past the end is empty and keeps the total.

## Known
- REQ-1: TP-1.
- REQ-2, REQ-3: TP-2, TP-4, TP-5.
- REQ-5, REQ-6, REQ-7: TP-6, TP-7, TP-8, TP-9.
- REQ-8: TP-9, TP-10, TP-11.
- REQ-9: TP-12, TP-18, TP-20.
- REQ-10: TP-13, TP-19.
- REQ-11: TP-14.
- REQ-12: TP-15, TP-16.
- REQ-13: TP-17.

## Assumed
- REQ-4: "most recent" is the latest start time. TP-3 is built so that start order and closing order disagree.

## Unknown
- U-1 (non-blocking): there is no browser end-to-end level, so the column is proven at component and API level only (Q-01).

## Open questions
- Q-01 (non-blocking, tech lead): is component plus integration testing enough for the Last visit column? Default: yes.
