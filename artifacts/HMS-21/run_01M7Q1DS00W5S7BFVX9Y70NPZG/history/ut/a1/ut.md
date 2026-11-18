---
schema: ut-result.v1
run_id: run_01M7Q1DS00W5S7BFVX9Y70NPZG
step: ut
attempt: 1
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
trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10, REQ-11, REQ-12, REQ-13]
known: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10, REQ-11, REQ-12, REQ-13]
assumed: []
unknown: []
questions: []
code:
  - repo: sample-system
    base_sha: 6f2b51edb792119ab6cba47335452b601b974b3e
    head_sha: 3d8dd546d2eda873a6823c80e7f083d721856c89
    diff_sha256: f9743a85ee262c412987868c79c20e12c68cbfd5d831006c4ee737d5b82a9dad
    files:
      - api/test/dashboard-awaiting-outcome.test.ts
      - web/src/__tests__/AppointmentsPage.test.tsx
      - web/src/__tests__/DashboardPage.test.tsx
---
# Unit tests: HMS-21

## Summary
The run's change is tested at both layers the strategy names (C1), with test names starting with their requirement ids (C2):
- **API tests:** a new file with its own throwaway database, like the No-shows tests (C5).
- **Web tests:** new cases in the DashboardPage tests (C6), plus a new AppointmentsPage test file.

What is covered, for the changed count, the filter and the page code:
- **Normal path:** the count, the tile and the link.
- **Error path:** a bad `started` value returns a 400 field error.
- **Boundary at now:** an appointment counts only once its start is strictly before now.
- **Previous day:** appointments that started the day before still count.
- **Period selector:** the count and the link ignore it.
- **List matches the count:** the linked list holds exactly the counted appointments.
- **Unchanged parts:** the other tiles and the next-10 list.
- **Personal data:** none appears on the dashboard or the started list.

Only test files were edited. All data comes from the invented fixture (C7, C8). The step's data holds no verifier result yet, so `tests` and `coverage` are left out. tsl records the test counts in checks.json when it runs the verifier (H3). Coverage is not reported because the project has no coverage script (C4). The database tests run only where a PostgreSQL binary or test database URL is available (C3).

Not covered at unit level:
- **S-1 AC3 (00:30 local, before 05:30, UTC date of the day before):** the fixture's doctors have no availability before 09:00 (C8), so no such appointment can be booked through the API. Instants are compared, not dates, so a local-midnight case behaves like the previous-day case tested here. It is left for the test-plan step.
- **S-5 AC3 (the 11th upcoming appointment):** the code that limits the next list to 10 did not change. The existing DSH-4 test still covers its order.

## Tests
- `api/test/dashboard-awaiting-outcome.test.ts` (new, 13 tests). It books Monday appointments at 09:00 (stays Scheduled), 09:15 (Completed), 09:30 (Cancelled), 09:45 (No-show) and 10:15, plus Tuesday at 09:00, with the clock at Monday 10:00:
  - REQ-1/REQ-2: the count is 1.
  - REQ-3/REQ-4/REQ-5: Completed, Cancelled and No-show are not counted.
  - REQ-6 edge: at 10:14 and at exactly 10:15 the count is 1; 1 ms later it is 2.
  - REQ-9: before any start, the count is 0.
  - REQ-7: Today, Last 7 days and Last 30 days all give the same count. On Tuesday, Today has no Completed appointment, but Monday's started appointments still count.
  - REQ-8: the started list's ids and total equal the count, including at the boundary and on Tuesday. Without `started`, the Scheduled list is unchanged. `started` values `false`, `1` and `TRUE` each return 400 with `{ started: 'must be one of true' }`.
  - REQ-10/REQ-11: `upcoming` and the next list hold only future appointments.
  - REQ-12/REQ-13: the dashboard and started-list responses carry no phone, email, allergies, notes or other medical data.
  - REQ-2: completing the started appointment removes it from the count and the list.
- `web/src/__tests__/DashboardPage.test.tsx` (4 new tests):
  - REQ-1/REQ-8: the tile shows "5" and links to `/appointments?status=SCHEDULED&started=true`.
  - REQ-7: after switching to Last 7 days, the tile's count and link are unchanged.
  - REQ-9: the tile shows 0.
  - REQ-10: all eight tiles' counts and links are asserted in order, so the seven existing tiles are pinned.
- `web/src/__tests__/AppointmentsPage.test.tsx` (new, 2 tests):
  - REQ-8: `started=true` and `status=SCHEDULED` from the URL reach the API call.
  - REQ-8 edge: without `started` in the URL, the call carries no `started`.

## Known
- REQ-1 to REQ-13: each has at least one test above, named with its id (C2).

## Assumed
None.

## Unknown
None.

## Open questions
None.
