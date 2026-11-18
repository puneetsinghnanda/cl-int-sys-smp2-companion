---
schema: ut-result.v1
run_id: run_01M7Q1DS00W5S7BFVX9Y70NPZG
step: ut
attempt: 2
skill: unit-test@1.3.0
inputs:
  - path: artifacts/HMS-21/run_01M7Q1DS00W5S7BFVX9Y70NPZG/impl.md
    sha256: 51eee980084e44b77b1b62655028e5caf19eb3b25ffe99e828772b7c8a3082a5
  - path: artifacts/HMS-21/run_01M7Q1DS00W5S7BFVX9Y70NPZG/split.md
    sha256: b54499b009a5c3dc2f2280aa5d7a1ddbb5c87d032fedcf19338b3faa1e0f64a1
  - path: context/test-strategy.md
    sha256: f4bbc9d5d63dfc433c9fe466a7aa71a4fad0147baafe1f47db0c06e117ce6cba
  - path: context/generated/test-layout.json
    sha256: c0752ab8543e23547679dbe34f6381f656d36b0eb5506efb2a56f94dcb729cd6
pack_sha:
  sample-system: 7681f87749883d71abb4011d118366ca1242e1cc
resolves:
  - id: F-1
    how: "A new test moves the clock to 23 November, when Last 30 days starts on 25 October, and checks the old appointments still count in every period and appear in the started list."
claims:
  - id: C1
    text: "API tests use Vitest and `Supertest` against a throwaway PostgreSQL 16 with a fixed clock."
    cites: ["context/test-strategy.md#layers"]
  - id: C2
    text: "Every requirement has a test whose name starts with its ID (`DOC-1, APT-4`)."
    cites: ["context/test-strategy.md#requirement-ids"]
  - id: C3
    text: "Without a PostgreSQL binary or `HMS_TEST_DATABASE_URL`, database tests are skipped."
    cites: ["context/test-strategy.md#database-for-tests"]
  - id: C4
    text: "The verifier skips coverage because the project has `no coverage script`."
    cites: ["context/test-strategy.md#gaps"]
  - id: C5
    text: "Dashboard API tests with their own database live beside `api/test/dashboard-no-shows.test.ts`."
    cites: ["context/generated/test-layout.json#test:sample-system:api/test/dashboard-no-shows.test.ts"]
  - id: C6
    text: "Web dashboard tests live in `web/src/__tests__/DashboardPage.test.tsx`."
    cites: ["context/generated/test-layout.json#test:sample-system:web/src/__tests__/DashboardPage.test.tsx"]
  - id: C7
    text: "The test clock is fixed at Monday 19 October 2026, 08:00 in `Asia/Kolkata`."
    cites: ["sample-system:api/test/helpers.ts#L9"]
  - id: C8
    text: "Doctor 1 has 15-minute slots, `Mon–Fri 09:00–12:00`, in the invented fixture."
    cites: ["sample-system:api/test/helpers.ts#L23"]
  - id: C9
    text: "The invented patient 1 has `allergies: ['Penicillin'], medicalNotes: 'Invented note'`."
    cites: ["sample-system:api/test/helpers.ts#L46"]
  - id: C10
    text: "Earlier ticket tests prefix the ticket, for example `HMS-3 REQ-2/REQ-4`."
    cites: ["context/generated/test-layout.json#test:sample-system:api/test/dashboard-no-shows.test.ts"]
trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10, REQ-11, REQ-12, REQ-13]
known: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10, REQ-11, REQ-12, REQ-13]
assumed: []
unknown: []
questions: []
code:
  - repo: sample-system
    base_sha: 6f2b51edb792119ab6cba47335452b601b974b3e
    head_sha: 5ed8a52fc458c62f4baf621088e2921611eb8f00
    diff_sha256: 8ead89b63f6bdcf79b51221825c30f7da0092464a7940a6289beb1f63584f27e
    files:
      - api/test/dashboard-awaiting-outcome.test.ts
      - web/src/__tests__/AppointmentsPage.test.tsx
      - web/src/__tests__/DashboardPage.test.tsx
---
# Unit tests: HMS-21

## Resolves
- F-1: A new test moves the clock to 23 November, when Last 30 days starts on 25 October, and checks the old appointments still count in every period and appear in the started list.

## Summary
The change is tested at both layers the strategy names (C1):
- **API:** a new file with its own throwaway database, like the No-shows tests (C5).
- **Web:** new cases in the DashboardPage tests (C6), plus a new AppointmentsPage test file.

Every test name starts with `HMS-21 REQ-n`, following the ticket-prefixed style of earlier tickets (C2, C10). The tests cover the normal path, the error path (a bad `started` value is a 400 field error) and these edges:
- the boundary at now, strictly before
- zero
- the previous day
- appointments that started weeks before the Last 30 days window began
- the period being ignored
- the list matching the count
- the other totals and the next list being unchanged
- no personal data

Only test files were edited, and all data comes from the invented fixture (C7, C8, C9). This step's data holds no verifier result, so `tests` and `coverage` are left out; tsl records the test counts in checks.json when it runs the verifier (H3). Coverage is not reported because the project has no coverage script (C4). The database tests run only where a PostgreSQL binary or test database URL is available (C3).

Not covered at unit level:
- **S-1 AC3, an appointment at 00:30 local:** that instant falls on the previous UTC day. The fixture's doctors have no availability before 09:00 (C8), so no such appointment can be booked through the API. Because instants are compared rather than dates, this behaves like the tested previous-day case. It is left for the test-plan step.
- **S-5 AC3, the 11th upcoming appointment:** the limit of 10 on the next list did not change. The existing DSH-4 test still covers the list's order.

## Tests
`api/test/dashboard-awaiting-outcome.test.ts` (new, 15 tests). The fixture books these appointments on Monday 19 October, with the clock at 10:00:
- 09:00 (stays Scheduled)
- 09:15 (Completed)
- 09:30 (Cancelled)
- 09:45 (No-show)
- 10:15 (Scheduled)

It also books Tuesday at 09:00 (Scheduled).

The tests:
- `HMS-21 REQ-1/REQ-2`: the count is 1.
- `HMS-21 REQ-3/REQ-4/REQ-5`: Completed, Cancelled and No-show are not counted.
- `HMS-21 REQ-6 edge`: the count is 1 at 10:14 and at exactly 10:15, and 2 one millisecond later.
- `HMS-21 REQ-9 edge`: the count is 0 before any appointment starts.
- `HMS-21 REQ-7`: Today, Last 7 days and Last 30 days give the same count.
- `HMS-21 REQ-7 edge`, next day: on Tuesday, Monday's started appointments still count under Today.
- `HMS-21 REQ-7 edge`, weeks later (S-1 AC4, S-3 AC2, S-4 AC2):
  - The clock is Monday 23 November, when Last 30 days starts on 25 October and holds no Completed appointment.
  - The three appointments that started in October still count under Today, Last 7 days and Last 30 days.
  - The started list holds exactly those three.
- `HMS-21 REQ-8`: the started list's ids and total equal the count, including at the boundary and on Tuesday.
- `HMS-21 REQ-8`: without `started`, the Scheduled list is unchanged.
- `HMS-21 REQ-8 error`: `started` set to `false`, `1` or `TRUE` gives 400 with `{ started: 'must be one of true' }`.
- `HMS-21 REQ-10/REQ-11`: `upcoming` and the next list hold only future appointments.
- `HMS-21 REQ-10`: with an appointment awaiting an outcome, the whole totals object is asserted with `toEqual`. Patients, doctors, appointments today, upcoming, completed, cancelled and No-shows all keep their values.
- `HMS-21 REQ-12/REQ-13`: the dashboard and started-list responses must not contain the fixture patient's phone (`+91`), email, allergies (`Penicillin`) or medical notes (`Invented note`) (C9). They must also not contain the words "medical" or "blood".
  - The fixture records no blood group, chronic conditions or current medications. Those fields are covered only by the general match on "medical" and "blood", not by seeded values.
- `HMS-21 REQ-2`: completing the started appointment removes it from the count and the list.

`web/src/__tests__/DashboardPage.test.tsx` (4 new tests):
- `HMS-21 REQ-1/REQ-8`: the tile shows 5 and links to `/appointments?status=SCHEDULED&started=true`.
- `HMS-21 REQ-7`: after switching to Last 7 days, the tile's count and link are unchanged.
- `HMS-21 REQ-9 edge`: the tile shows 0.
- `HMS-21 REQ-10`: all eight tiles' counts and links are asserted in order.

`web/src/__tests__/AppointmentsPage.test.tsx` (new, 2 tests):
- `HMS-21 REQ-8`: `started=true` and `status=SCHEDULED` from the URL reach the API call.
- `HMS-21 REQ-8 edge`: without `started` in the URL, the API call carries none.

## Known
- REQ-1 to REQ-13: each has at least one test above, named with its id (C2).

## Assumed
None.

## Unknown
None.

## Open questions
None.
