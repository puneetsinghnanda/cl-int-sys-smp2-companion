---
schema: ut-result.v1
run_id: run_01M669DZD0215D4H6K675BYWA4
step: ut
attempt: 1
skill: unit-test@1.0.1
inputs:
  - path: artifacts/HMS-6/run_01M669DZD0215D4H6K675BYWA4/impl.md
    sha256: 3ef6f5eb86e6950f21bdde784f9d6b7f9bc618f94ce7e9364aaa33a0358e5834
  - path: artifacts/HMS-6/run_01M669DZD0215D4H6K675BYWA4/split.md
    sha256: 0347e867d613f325eceea3249daca63249ed02bafd70ff50bf50e83989fa7c76
  - path: context/test-strategy.md
    sha256: e7465d17d8814cd71fe160b96a859f1ba1414836ad71ef48b9aaeb166259eee8
  - path: context/generated/test-layout.json
    sha256: fd4b5d51b15f9aba1d03b5f6a0350e4e4849d862118fb3a3bda7abfa5127e179
pack_sha:
  sample-system: bd98ff08b83f8762c5e5c073ba5fadef88169b33
code:
  - repo: sample-system
    base_sha: b45bc4ee1d211ebc3c586fc44a5abb5a70c1feb7
    head_sha: e57453fbb3d84d4196af79630c6923ef05cdfdbe
    diff_sha256: edcb4ff0bfbb636483caaf7f582c4fb00a3e50a6632e0b1b540429a0e0b539e5
    files:
      - api/test/doctors-today.test.ts
      - web/src/__tests__/DoctorsPage.test.tsx
claims:
  - id: C1
    text: "API tests use Vitest and `Supertest` against a throwaway PostgreSQL 16 with a fixed clock."
    cites: ["context/test-strategy.md#layers"]
  - id: C2
    text: "Web tests use Vitest with `jsdom` and Testing Library."
    cites: ["context/test-strategy.md#layers"]
  - id: C3
    text: "Every requirement has at least one test whose name starts with its `ID`."
    cites: ["context/test-strategy.md#requirement-ids"]
  - id: C4
    text: "Without a temporary cluster or `HMS_TEST_DATABASE_URL`, database tests are skipped."
    cites: ["context/test-strategy.md#database-for-tests"]
  - id: C5
    text: "The verifier skips `coverage`; the project has no coverage script."
    cites: ["context/test-strategy.md#gaps"]
  - id: C6
    text: "The change adds `countTodayByDoctor` and the with_today parse to the doctors list."
    cites: ["artifacts/HMS-6/run_01M669DZD0215D4H6K675BYWA4/impl.md#L86"]
trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10, REQ-11]
known: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10, REQ-11]
assumed: [S-1]
unknown:
  - {id: U-1, blocking: false}
questions:
  - id: Q-01
    blocking: false
    ask: tech lead
    text: "Does the verifier container provide PostgreSQL 16 (or HMS_TEST_DATABASE_URL) so the new API tests run rather than skip?"
    why: "The API tests in doctors-today.test.ts use describe.skipIf(!pgAvailable()); if no database is available they are skipped and only the web tests prove the change."
    evidence: ["context/test-strategy.md#database-for-tests"]
    options: ["Database available; API tests run", "No database; add one to the verifier"]
    default: "Database available; API tests run"
---
# Unit tests: HMS-6

## Summary
Two new test files cover the change.
- **API tests:** an API suite against a throwaway database with the fixed Monday 08:00 Kolkata clock, covering `GET /api/doctors?with_today=true`, `countTodayByDoctor` and the `with_today` parse.
- **Web tests:** a web suite for the DoctorsPage Today column and link.

Every test name starts with `HMS-6` and the REQ ids it proves. Only test files were edited. All data is invented (Test Doctor 001, the shared invented fixture).

Results: tests run/passed and coverage are not reported here, because no verifier result exists for this step yet. tsl records them in checks.json when it runs the verifier (H3). Coverage is skipped by the project (C5).

## Tests
- api/test/doctors-today.test.ts (DoctorService list with today counts, AppointmentRepo countTodayByDoctor):
  - HMS-6 REQ-5: 0 for doctors with no appointments today.
  - HMS-6 REQ-2: counts only today's appointments (Monday ones counted, Tuesday one not).
  - HMS-6 REQ-2: a Tuesday clock counts only Tuesday (day boundary in the hospital zone).
  - HMS-6 REQ-3: a Cancelled appointment drops out of the count.
  - HMS-6 REQ-3: Completed and No-show appointments still count.
  - HMS-6 REQ-4: the counts add up to the dashboard's `appointmentsToday` tile.
  - HMS-6 REQ-8, REQ-9: 20 per page and order by name are unchanged.
  - HMS-6 REQ-10: search and department filter still apply, and the counts follow them.
  - HMS-6 REQ-10 (edge): a search with no match returns an empty page with no error, which exercises the early return for an empty id list.
  - HMS-6 (edge): without `with_today`, or with `with_today=false`, list rows and `GET /api/doctors/:id` carry no `today_count`, so the booking picker's call is unchanged.
  - HMS-6 (error): `with_today=yes` is rejected with 400 and a `with_today` field error.
- web/src/__tests__/DoctorsPage.test.tsx (DoctorsPage, api client doctorsWithToday):
  - HMS-6 REQ-1, REQ-11: a Today column after Code, Name, Specialization, Department, Fee and Status.
  - HMS-6 REQ-2: the page requests the list with `with_today=true`.
  - HMS-6 REQ-6, REQ-7: the count links to `/appointments?doctorId=1&from=<today>&to=<today>`, with today in Asia/Kolkata.
  - HMS-6 REQ-5: a 0 count shows 0 and is still a link.

## Known
- REQ-1, REQ-11: web test on the column headers.
- REQ-2: API tests for the today window and the day boundary; web test that the page asks for counts.
- REQ-3: API tests for Cancelled (excluded), and Completed and No-show (included).
- REQ-4: API test comparing the counts' sum with the dashboard tile.
- REQ-5: API test (0 from the API) and web test (0 shown and linked).
- REQ-6, REQ-7: web test on the link target.
- REQ-8, REQ-9, REQ-10: API tests on paging, order, search, filter and the empty page.

## Assumed
- S-1: assumed `pgAvailable()` is true in the verifier, so the API suite runs. Basis: the test strategy says the harness starts a temporary PostgreSQL 16 cluster or uses `HMS_TEST_DATABASE_URL` (C4). Otherwise the suite is skipped (U-1).

## Unknown
- U-1 (non-blocking): whether the verifier provides a database for the API tests → Q-01.

## Open questions
- Q-01 (non-blocking, tech lead): does the verifier provide PostgreSQL 16 for the API tests? Default: yes, the API tests run.
