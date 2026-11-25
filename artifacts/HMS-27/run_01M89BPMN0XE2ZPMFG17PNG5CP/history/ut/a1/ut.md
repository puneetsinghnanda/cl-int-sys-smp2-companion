---
schema: ut-result.v1
run_id: run_01M89BPMN0XE2ZPMFG17PNG5CP
step: ut
attempt: 1
skill: unit-test@1.3.0
inputs:
  - path: artifacts/HMS-27/run_01M89BPMN0XE2ZPMFG17PNG5CP/impl.md
    sha256: 2dabf579260515e7dff6115264f838e92ca1bcda9b1771e2d43e7a9436256b19
  - path: artifacts/HMS-27/run_01M89BPMN0XE2ZPMFG17PNG5CP/split.md
    sha256: 3f92b6f319ce0bc8d22bd48258fa93ea3a73480096486aaa8646237ad43331b4
  - path: context/test-strategy.md
    sha256: f4bbc9d5d63dfc433c9fe466a7aa71a4fad0147baafe1f47db0c06e117ce6cba
  - path: context/generated/test-layout.json
    sha256: c0752ab8543e23547679dbe34f6381f656d36b0eb5506efb2a56f94dcb729cd6
pack_sha:
  sample-system: 7681f87749883d71abb4011d118366ca1242e1cc
code:
  - repo: sample-system
    base_sha: a003fb3b8593ef6537cf59d5abda205f1d02fe62
    head_sha: cf7780e354e64c2a1eb303263006c8b2bc37a7a3
    diff_sha256: de6f99a6a6df42dd0948a02f2a7a97d21125b3f230474d60d9bf26127db5ac85
    files:
      - api/test/patients-last-visit.test.ts
      - web/src/__tests__/PatientsPage.test.tsx
      - web/src/__tests__/format.test.ts
claims:
  - id: C1
    text: "API tests use `Vitest and Supertest` against a throwaway PostgreSQL 16 with a fixed clock."
    cites: ["context/test-strategy.md#layers"]
  - id: C2
    text: "Every requirement has at least one test whose name starts with its `ID`, so new tests are named HMS-27 REQ-n."
    cites: ["context/test-strategy.md#requirement-ids"]
  - id: C3
    text: "The verifier skips coverage because the project has `no coverage script`."
    cites: ["context/test-strategy.md#gaps"]
  - id: C4
    text: "API tests build the app with `testApp` and an injectable clock."
    cites: ["context/generated/module-map.json#sym:sample-system:api/test/helpers.ts:testApp"]
  - id: C5
    text: "The shared fixture `loadFixture` creates two doctors and two patients."
    cites: ["context/generated/module-map.json#sym:sample-system:api/test/helpers.ts:loadFixture"]
  - id: C6
    text: "Local times map to UTC instants with the `at` helper (Kolkata is UTC+05:30)."
    cites: ["context/generated/module-map.json#sym:sample-system:api/test/helpers.ts:at"]
  - id: C7
    text: "Database tests are skipped without PostgreSQL binaries or `HMS_TEST_DATABASE_URL`."
    cites: ["context/test-strategy.md#database-for-tests"]
  - id: C8
    text: "The change has `PatientService` parse not_seen and map each last visit to a local date."
    cites: ["artifacts/HMS-27/run_01M89BPMN0XE2ZPMFG17PNG5CP/impl.md#claim:C3"]
trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10, REQ-11, REQ-12, REQ-13, REQ-14]
known: [S-1, S-2, S-3, S-4, S-5, S-6]
assumed: []
unknown: []
questions: []
---
# Unit tests: HMS-27

## Summary
These tests cover the HMS-27 change at both layers the strategy names (C1): API tests with Supertest against a throwaway PostgreSQL and a fixed clock, and web tests with Testing Library. Each test name starts with `HMS-27 REQ-n` (C2).

The API tests insert appointment rows directly, so that past and off-hours instants (for example 00:30 local) can be set up exactly. They pin the clock with `testApp` (C4) to 01:30 local on 25 November 2026, which is still 24 November in UTC. That makes a UTC-today or UTC-visit-date implementation fail, and a leap-year clock separates 12 calendar months from a fixed 365-day count.

Each changed function or endpoint has a normal-path test and an error or edge test: the `yearBefore` 29 February clamp, `not_seen=yes` returning 400, never-visited patients, a No-show after a visit, and filtering before paging.

Tests run and coverage are not reported here. tsl records them in checks.json when it runs the verifier (H3), and the verifier skips coverage because the project has no coverage script (C3).

## Tests
- api/test/patients-last-visit.test.ts (new):
  - `yearBefore`: same calendar date, and the 29 February clamp (REQ-7).
  - Last visit (S-1): latest COMPLETED appointment rather than a later No-show (REQ-2); the later of two Completed appointments, with its doctor (REQ-4); the local date at 00:30 local / 19:00 UTC (REQ-3); null when there are only Cancelled and Scheduled appointments (REQ-5).
  - Not-seen filter with a pinned clock just after local midnight (S-2, S-5): 24 Nov 2025 local is listed and 00:30 local on 25 Nov 2025 is not (REQ-7); an old and a recent Completed visit is not listed (REQ-7); a recent No-show still counts as not seen (REQ-7); never-visited is excluded (REQ-8); `not_seen=false` or absent leaves the list unfiltered (REQ-13); `not_seen=yes` is 400 with a `not_seen` field error (REQ-6); the leap-year cutoff 15 Mar 2027 for today 15 Mar 2028 (REQ-7).
  - Search and status (S-3, S-4, S-6): search plus filter excludes recent and non-matching patients (REQ-9); status INACTIVE plus filter excludes a recently seen Inactive patient (REQ-10); without the filter, search and status still list old, recent and never-visited patients (REQ-13); the filtered list carries no medical history for a fixture patient with allergies (REQ-12).
  - Paging (S-3): 21 not-seen patients plus 5 recently seen ones registered after them. Page 1 holds 20 not-seen patients and page 2 the 21st (REQ-11); total is 21 with the filter and 28 without (REQ-14).
- web/src/__tests__/PatientsPage.test.tsx (extended):
  - Last visit header directly after Next appointment (REQ-1).
  - "10 Mar 2026 · Test Doctor 002" for a visit (REQ-3, REQ-4), and "Never" for none (REQ-5).
  - Checkbox next to the status filter, off by default, with no `not_seen` in the first request (REQ-6).
  - Turning it on sends `not_seen=true`, keeps the status and goes back to page 1 (REQ-6, REQ-11).
- web/src/__tests__/format.test.ts (extended): `dateOnly` shows "10 Mar 2026", and the first of a month stays on its day (REQ-3).

All test data is invented: "Test Filter P1", "Test Paging Old 001" and phones in the +91 00000 3xxxx range. Nothing calls the network.

## Known
- S-1: covered by the last-visit API tests and the web column tests.
- S-2: covered by the pinned-clock and leap-year filter tests.
- S-3: covered by the search, status and paging tests.
- S-4: covered by the medical-history test with the filter on, and the existing PAT-8 key check (updated in impl).
- S-5: covered by the No-show and never-visited filter tests.
- S-6: covered by the search and status tests without the filter.

## Assumed
None.

## Unknown
None. Database tests are skipped when no PostgreSQL is available (C7); the verifier runs them with one.

## Open questions
None.
