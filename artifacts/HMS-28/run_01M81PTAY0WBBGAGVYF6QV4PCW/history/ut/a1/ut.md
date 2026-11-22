---
schema: ut-result.v1
run_id: run_01M81PTAY0WBBGAGVYF6QV4PCW
step: ut
attempt: 1
skill: unit-test@1.3.0
inputs:
  - path: artifacts/HMS-28/run_01M81PTAY0WBBGAGVYF6QV4PCW/impl.md
    sha256: 307cbd9b69244e89b5711f193ecc2892cec5e4ff5e2c48f7553f8625f7587629
  - path: artifacts/HMS-28/run_01M81PTAY0WBBGAGVYF6QV4PCW/split.md
    sha256: 2a67d6bac48a0f331a38dd9686cc75735f7f339ab7c2b0be8ed789c19e99fa98
  - path: context/test-strategy.md
    sha256: f4bbc9d5d63dfc433c9fe466a7aa71a4fad0147baafe1f47db0c06e117ce6cba
  - path: context/generated/test-layout.json
    sha256: c0752ab8543e23547679dbe34f6381f656d36b0eb5506efb2a56f94dcb729cd6
pack_sha:
  sample-system: 7681f87749883d71abb4011d118366ca1242e1cc
code:
  - repo: sample-system
    base_sha: 1104d33e868767ac03d7fbb3d18a3c581851d2aa
    head_sha: e98645f46321bd0f6fdd81955ad4fb9410bf8692
    diff_sha256: 1d6034e08ced3a1d511eaea19dd989ae7dd699294f5305db3f00d44990212e46
    files:
      - api/test/doctors-works-on.test.ts
      - web/src/__tests__/DoctorsPage.test.tsx
claims:
  - id: C1
    text: "API tests use `Vitest and Supertest` against a throwaway PostgreSQL; web tests use jsdom and Testing Library."
    cites: ["context/test-strategy.md#layers"]
  - id: C2
    text: "Every test name `starts with its ID`."
    cites: ["context/test-strategy.md#requirement-ids"]
  - id: C3
    text: "Database tests run on a temporary cluster or `HMS_TEST_DATABASE_URL` and are skipped otherwise."
    cites: ["context/test-strategy.md#database-for-tests"]
  - id: C4
    text: "The project has `no coverage script`, so no run reports coverage."
    cites: ["context/test-strategy.md#gaps"]
  - id: C5
    text: "The change adds `DoctorRepo.weekdaysByDoctor` and the weekday filter."
    cites: ["artifacts/HMS-28/run_01M81PTAY0WBBGAGVYF6QV4PCW/impl.md#claim:C2"]
  - id: C6
    text: "The change adds the Works on column and filter to `DoctorsPage`."
    cites: ["artifacts/HMS-28/run_01M81PTAY0WBBGAGVYF6QV4PCW/impl.md#claim:C6"]
trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10, REQ-11, REQ-14, REQ-15]
known: [S-1, S-2, S-3, S-4, S-5, S-6]
assumed: []
unknown: []
questions: []
---
# Unit tests: HMS-28

## Summary
The tests cover the HMS-28 change (C5, C6) at both layers the strategy names (C1). Every test name starts with its HMS-28 requirement ID (C2).
- The API tests cover the normal path and the edge and error paths of the weekday filter and the `weekdays` field: Sunday coding, duplicate blocks, the 21st-row page boundary, invalid values and the without-`with_today` partition.
- The web tests cover the Works on column text, the filter options and default, the request the filter sends, the page reset and the page address.
- The API tests need PostgreSQL and are skipped without it (C3).
- I didn't run the tests, and no verifier result was given to this step, so tests and coverage are left out here. tsl records them in checks.json when it runs the verifier (H3). Coverage is not measured in this project (C4).

## Tests
- api/test/doctors-works-on.test.ts (new; serves S-1, S-2, S-3, S-5, S-6):
  - weekdays are distinct and ascending, with Sunday (7) last, even when entered Sunday first;
  - [] for a doctor with no blocks;
  - an Inactive doctor still has weekdays;
  - no weekday or an empty weekday lists every doctor;
  - weekday=7 and weekday=1 list only the matching doctors;
  - two Saturday blocks list and count a doctor once;
  - weekday combines with department, specialization, status and search;
  - 0, 8, sat, 1.5 and -1 are 400 field errors on weekday;
  - without with_today, no weekdays appear on the list or the detail;
  - the filter works without with_today;
  - the 21st Thursday doctor lands on page 2, sorted by name, with total 21 at 20 per page.
- web/src/__tests__/DoctorsPage.test.tsx (extended; serves S-1, S-2, S-3, S-4):
  - "Mon, Wed" and "Mon, Sat, Sun" short names in order;
  - "No availability";
  - an Inactive doctor shows weekdays;
  - options Any day and Monday to Sunday with values 1 to 7, defaulting to Any day;
  - Any day sends no weekday;
  - choosing Sunday sends weekday=7 and drops page=2;
  - ?weekday=6 in the address selects Saturday and filters;
  - weekday is sent with the department, status and search filters.
  - The fixture gains a third, Inactive doctor; the HMS-6 tests are unchanged.

## Known
- S-1: Works on column text, order, empty case and Inactive case, tested in both files.
- S-2: filter options, default and the Sunday filter, tested in both files.
- S-3: combination with search, department, specialization and status, tested in both files.
- S-4: weekday kept in and read from the page address, tested in the web file.
- S-5: without with_today the API response is unchanged; the HMS-6 web tests still pass unchanged apart from the header test updated in the implement step.
- S-6: page boundary at the 21st doctor, total count and duplicate blocks counted once, tested in the API file.

## Assumed
None

## Unknown
None

## Open questions
None
