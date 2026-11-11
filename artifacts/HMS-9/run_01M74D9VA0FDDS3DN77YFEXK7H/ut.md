---
schema: ut-result.v1
run_id: run_01M74D9VA0FDDS3DN77YFEXK7H
step: ut
attempt: 1
skill: unit-test@1.1.0
inputs:
  - path: artifacts/HMS-9/run_01M74D9VA0FDDS3DN77YFEXK7H/impl.md
    sha256: 2d2c9c19b6546cddf013f0ee6fe1041dfc59d388f1933e82c049e2c70345a311
  - path: artifacts/HMS-9/run_01M74D9VA0FDDS3DN77YFEXK7H/split.md
    sha256: 7e5a680cb8c89a9d3304be453d11ee6e38652b7841fcf7f39799be1b9d6659f0
  - path: context/test-strategy.md
    sha256: f4bbc9d5d63dfc433c9fe466a7aa71a4fad0147baafe1f47db0c06e117ce6cba
  - path: context/generated/test-layout.json
    sha256: 57a01551ebf3a7429a78830822e19e5384fc83b3f91c6b101bbcfb9cec22038f
pack_sha:
  sample-system: c2709e164850ab7e177e4bccfc42a5dd9b961fdc
code:
  - repo: sample-system
    base_sha: 0707c8893f0cf5e4750bea719f7a7ebd58ec68cf
    head_sha: e9036302dafba218d786875c165c0d99de89a374
    diff_sha256: 89538c71d4f28c1221241f67784281fe9aed33607757e54ac851bde8a90e0753
    files:
      - api/test/doctors-next-seven-days-edges.test.ts
      - web/src/__tests__/DoctorDetailPage.test.tsx
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
    text: "Without a temporary cluster or `HMS_TEST_DATABASE_URL`, database tests are skipped."
    cites: ["context/test-strategy.md#database-for-tests"]
  - id: C5
    text: "The verifier skips coverage because the project has no `coverage script`."
    cites: ["context/test-strategy.md#gaps"]
  - id: C6
    text: "The doctor page test file `DoctorDetailPage.test.tsx` already holds the HMS-7 tests and is extended here."
    cites: ["context/generated/test-layout.json#test:sample-system:web/src/__tests__/DoctorDetailPage.test.tsx"]
  - id: C7
    text: "The change adds `nextSevenDays` to the doctor detail, null for Inactive doctors."
    cites: ["artifacts/HMS-9/run_01M74D9VA0FDDS3DN77YFEXK7H/impl.md#claim:C2"]
trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10]
known: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10]
assumed: []
unknown: [{id: U-1, blocking: false}]
questions:
  - id: Q-01
    blocking: false
    ask: tech lead
    text: "Does the verifier's test step provide PostgreSQL 16 or HMS_TEST_DATABASE_URL, so that the HMS-9 API tests run rather than skip?"
    why: "The API tests skip without a database (C4), so a green test step alone does not prove they ran."
    evidence: ["context/test-strategy.md#database-for-tests"]
    options: ["Yes, the verifier provides a database", "No, add one to the verifier"]
    default: "Yes, the verifier provides a database"
---
# Unit tests: HMS-9

## Summary
These tests extend the change's own regression tests (impl: entry keys, the UTC midnight case, the day-schedule match and PATCH ignoring the field) to the remaining normal, edge and error paths of `nextSevenDays` and the doctor page section. The API tests use Supertest with a fixed clock (C1); the web tests use Testing Library (C2). Every test name starts with its HMS-9 requirement ID (C3). The test data is the existing invented fixture plus "Test cancellation".

No test or coverage numbers appear here: tsl records them in checks.json when it runs the verifier (H3). Coverage is skipped by the verifier, because the project has no coverage script (C5).

## Tests
- **api/test/doctors-next-seven-days-edges.test.ts** (new; uses its own database and clock):
  - REQ-2/REQ-3: the dates run from today to 6 days ahead, and today's row shows booked 0, free 2.
  - REQ-5: days without slots are `available: false` with null counts.
  - REQ-4: a Scheduled appointment counts as booked.
  - REQ-4/REQ-6: at 09:45 local, the passed booked slot stays booked and the passed empty slot counts as neither, so the row shows booked 1, free 0.
  - REQ-4 edge: cancelling an appointment frees its slot.
  - REQ-9: an Inactive doctor gets null.
  - REQ-10 error: an unknown doctor is still 404.
- **web/src/__tests__/DoctorDetailPage.test.tsx** (extended, C6):
  - REQ-1/REQ-3: 7 rows, each with the date, booked and free.
  - REQ-6: free 0 is shown, not "Not available".
  - REQ-5: a "Not available" row.
  - REQ-7: the row link is `/doctors/1?date=…`.
  - REQ-9: no section when the value is null.
  - REQ-10: the other sections are still shown.
  - REQ-8: `?date=` sets the day schedule's date.
  - REQ-8 error: an invalid `?date=` is ignored.
  - REQ-8 edge: `validDay` accepts only real YYYY-MM-DD days.

## Known
- REQ-1 to REQ-10: each is covered by at least one test named with its ID (C3, C7).

## Assumed
None.

## Unknown
- U-1: Whether the verifier's test step has a database. Without one, the API tests skip (C4). Raised as Q-01.

## Open questions
- Q-01 (non-blocking, tech lead): Does the verifier's test step provide PostgreSQL 16 or HMS_TEST_DATABASE_URL, so that the HMS-9 API tests run rather than skip? Default: yes.
