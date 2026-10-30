---
schema: test-plan.v1
run_id: run_01M669DZD0215D4H6K675BYWA4
step: tplan
attempt: 1
skill: test-plan@1.0.1
inputs:
  - path: artifacts/HMS-6/run_01M669DZD0215D4H6K675BYWA4/split.md
    sha256: 0347e867d613f325eceea3249daca63249ed02bafd70ff50bf50e83989fa7c76
  - path: artifacts/HMS-6/run_01M669DZD0215D4H6K675BYWA4/design.md
    sha256: 9b1662a069745e77e899b72506170a589549e1d2603b91da482f174e575ab746
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
    text: "Web tests use Vitest with `jsdom` and Testing Library."
    cites: ["context/test-strategy.md#layers"]
  - id: C3
    text: "Without a temporary cluster or `HMS_TEST_DATABASE_URL`, database tests are skipped."
    cites: ["context/test-strategy.md#database-for-tests"]
  - id: C4
    text: "No page is tested `end to end` in a browser."
    cites: ["context/test-strategy.md#gaps"]
  - id: C5
    text: "Each test name starts with its requirement `ID`."
    cites: ["context/test-strategy.md#requirement-ids"]
  - id: C6
    text: "GET /api/doctors gains the optional boolean parameter `with_today`."
    cites: ["artifacts/HMS-6/run_01M669DZD0215D4H6K675BYWA4/design.md#L181"]
  - id: C7
    text: "The `BookAppointmentPage` does not pass with_today, so nothing changes for it."
    cites: ["artifacts/HMS-6/run_01M669DZD0215D4H6K675BYWA4/design.md#L213"]
trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10, REQ-11]
known: [TP-1, TP-2, TP-3, TP-4, TP-6, TP-7, TP-8, TP-9, TP-10, TP-11, TP-12, TP-13, TP-14, TP-15, TP-16]
assumed: [TP-5]
unknown:
  - {id: U-1, blocking: false}
questions:
  - id: Q-01
    blocking: false
    ask: tech lead
    text: "Does the verifier provide PostgreSQL 16 (or HMS_TEST_DATABASE_URL) so the integration tests in this plan run instead of being skipped?"
    why: "Every integration test here needs a database; without one only the web unit tests prove the change."
    evidence: ["context/test-strategy.md#database-for-tests"]
    options: ["Database available; integration tests run", "No database; add one to the verifier"]
    default: "Database available; integration tests run"
tests:
  - {id: TP-1, title: "Doctors page shows a Today column after Code, Name, Specialization, Department, Fee and Status", covers: [S-1/AC1, S-3/AC5], level: unit}
  - {id: TP-2, title: "with_today=true counts only appointments starting today in the hospital zone", covers: [S-1/AC2], level: integration}
  - {id: TP-3, title: "Boundary: with the clock on the next day, only that day's appointments count", covers: [S-1/AC2], level: integration}
  - {id: TP-4, title: "Cancelled appointments are not counted; Completed and No-show are", covers: [S-1/AC3], level: integration}
  - {id: TP-5, title: "With all of today's appointments on one doctor, that doctor's today_count equals the dashboard appointmentsToday tile", covers: [S-1/AC4], level: integration}
  - {id: TP-6, title: "A doctor with no counted appointments today gets today_count 0", covers: [S-1/AC5], level: integration}
  - {id: TP-7, title: "Doctors page shows 0 for a doctor with no appointments, still as a link", covers: [S-1/AC5, S-2/AC1], level: unit}
  - {id: TP-8, title: "The count links to the appointment list filtered to that doctor", covers: [S-2/AC1], level: unit}
  - {id: TP-9, title: "The link's day filter is today in Asia/Kolkata, with the clock pinned", covers: [S-2/AC2], level: unit}
  - {id: TP-10, title: "The link's target, GET /api/appointments filtered by doctor and today, returns only that doctor's appointments today", covers: [S-2/AC1, S-2/AC2], level: integration}
  - {id: TP-11, title: "With more than 20 doctors, with_today=true still returns 20 per page", covers: [S-3/AC1], level: integration}
  - {id: TP-12, title: "Rows stay sorted by name with with_today=true", covers: [S-3/AC2], level: integration}
  - {id: TP-13, title: "Department, specialization and status filters return the same doctors with and without with_today", covers: [S-3/AC3], level: integration}
  - {id: TP-14, title: "Search returns the same doctors with and without with_today, including an empty result", covers: [S-3/AC4], level: integration}
  - {id: TP-15, title: "Negative: an invalid with_today value is a 400 naming the with_today field", covers: [S-3/AC3], level: integration}
  - {id: TP-16, title: "Without with_today (or with false) the list and GET /api/doctors/:id carry no today_count", covers: [S-3/AC3, S-3/AC5], level: integration}
---
# Test plan: HMS-6

## Scope
In scope: the `with_today` parameter on GET /api/doctors and its `today_count` (C6), the Today column and its link on the doctors page, and the unchanged paging, sort, filters, search and columns of the doctors list. API behaviour is planned at the integration level with Vitest and Supertest on a throwaway PostgreSQL 16 with a fixed clock (C1). Page behaviour is planned at the unit level with jsdom and Testing Library (C2). Test names start with `HMS-6` and the REQ ids (C5).

Out of scope: browser end-to-end tests, which the project does not run (C4); coverage numbers, which the verifier skips; the booking picker, which does not send `with_today` (C7) and is covered only indirectly by TP-16.

Test data is invented (Test Doctor 001 and the shared fixture). Many of these tests already exist from the unit-test step and can be reused as they are. TP-5, TP-9 and TP-11 need changes: TP-5 puts all of today's appointments on one doctor instead of summing, TP-9 pins the clock, and TP-11 needs a fixture with more than 20 doctors.

## Tests
- TP-1 (unit): Today column after the existing six columns. Covers S-1/AC1, S-3/AC5.
- TP-2 (integration): only appointments starting today count. Covers S-1/AC2.
- TP-3 (integration): day boundary with the clock moved to the next day. Covers S-1/AC2.
- TP-4 (integration): Cancelled excluded, Completed and No-show counted. Covers S-1/AC3.
- TP-5 (integration): one doctor's count equals the dashboard tile when that doctor has all of today's appointments. Covers S-1/AC4.
- TP-6 (integration): `today_count` is 0 with no appointments. Covers S-1/AC5.
- TP-7 (unit): 0 shown and still a link. Covers S-1/AC5, S-2/AC1.
- TP-8 (unit): link carries the doctor filter. Covers S-2/AC1.
- TP-9 (unit): link carries today's date in Asia/Kolkata, clock pinned. Covers S-2/AC2.
- TP-10 (integration): the link target's API filters return only that doctor's appointments today. Covers S-2/AC1, S-2/AC2.
- TP-11 (integration): 20 per page with more than 20 doctors. Covers S-3/AC1.
- TP-12 (integration): sorted by name. Covers S-3/AC2.
- TP-13 (integration): filters unchanged. Covers S-3/AC3.
- TP-14 (integration): search unchanged, including no match. Covers S-3/AC4.
- TP-15 (integration, negative): invalid `with_today` is a 400 with a field error. Covers S-3/AC3.
- TP-16 (integration): no `today_count` without the flag, or on the detail route. Covers S-3/AC3, S-3/AC5.

Coverage check: S-1/AC1 to AC5, S-2/AC1 to AC2 and S-3/AC1 to AC5 each appear in at least one test.

## Known
- TP-1, TP-7, TP-8: the column, the 0 and the doctor link come straight from S-1 and S-2.
- TP-2, TP-3, TP-4, TP-6: the counting rule and the day come from S-1 AC2, AC3 and AC5 and the design's `with_today` (C6).
- TP-9, TP-10: the day filter in Asia/Kolkata comes from S-2 AC2.
- TP-11 to TP-14: unchanged paging, sort, filters and search come from S-3.
- TP-15: the negative test for the design's one new input (C6).
- TP-16: the response without the flag is unchanged (C7).

## Assumed
- TP-5: assumed the dashboard tile counts every status except Cancelled, so one doctor's count equals the tile. Basis: the stories' Q-05 (artifacts/HMS-6/run_01M669DZD0215D4H6K675BYWA4/split.md#L73). If the tile differs, TP-5 fails and shows the gap.

## Unknown
- U-1 (non-blocking): whether the verifier has a database for the integration tests → Q-01.

## Open questions
- Q-01 (non-blocking, tech lead): does the verifier provide PostgreSQL 16 so the integration tests run? Default: yes.
