---
schema: ut-result.v1
run_id: run_01M5YQB3ZRP10586473KARZ7M4
step: ut
attempt: 1
skill: unit-test@1.0.1
inputs:
  - path: artifacts/HMS-4/run_01M5YQB3ZRP10586473KARZ7M4/impl.md
    sha256: 0f9e83d1d5682f454a07a45622414cba47d2b81c15471f261b4b09b016bbbdc1
  - path: artifacts/HMS-4/run_01M5YQB3ZRP10586473KARZ7M4/split.md
    sha256: 5ced1c4d84b9c46da98051a025953a0aea7312cd53a6d5686efdebadb117024e
  - path: context/test-strategy.md
    sha256: e7465d17d8814cd71fe160b96a859f1ba1414836ad71ef48b9aaeb166259eee8
  - path: context/generated/test-layout.json
    sha256: fd4b5d51b15f9aba1d03b5f6a0350e4e4849d862118fb3a3bda7abfa5127e179
pack_sha:
  sample-system: bd98ff08b83f8762c5e5c073ba5fadef88169b33
code:
  - repo: sample-system
    base_sha: 3abfaf50e32a2efa4f9b3bf2276c5d4b3204eab7
    head_sha: edebcdf5d24cb1d8ebd5b0a3ab91cf3e0619d2c8
    diff_sha256: 7862f0ef8b1a9ea30a7df7d76903ee30f87d8c57895ecd09c11e2c25c0a987b7
    files:
      - api/test/patients-last-visit.test.ts
      - web/src/__tests__/PatientsPage.test.tsx
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
    text: "Without a test database, database tests are `skipped`."
    cites: ["context/test-strategy.md#database-for-tests"]
  - id: C5
    text: "The verifier skips `coverage` because the project has no coverage script."
    cites: ["context/test-strategy.md#gaps"]
trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-12, REQ-13]
known: [REQ-1, REQ-2, REQ-3, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-12, REQ-13]
assumed: [REQ-4]
unknown: [U-1]
unknowns:
  - {id: U-1, blocking: false, text: "No coverage figure can be reported: the project has no coverage script, so the verifier skips the coverage step.", question: Q-01}
questions:
  - id: Q-01
    blocking: false
    ask: tech lead
    text: "Should the project add a coverage script so runs can report coverage against the base?"
    why: "Without it the unit-test step can show tests passing but never a coverage change."
    evidence: ["context/test-strategy.md#gaps"]
    options: ["Add a coverage script in a separate ticket", "Keep coverage out of scope"]
    default: "Keep coverage out of scope for HMS-4"
---
# Unit tests: HMS-4

## Summary
The tests cover the new `last_visit_date` on GET /api/patients and the Last visit column, with the normal path and the edge paths for each changed function (C1, C2). Every test name starts with HMS-4 and its REQ id (C3). The API tests need the throwaway PostgreSQL database, which the verifier image provides; elsewhere they are skipped (C4). No production code changed in this step. tsl records the test counts in checks.json when it runs the verifier (H3); coverage is not available (C5, U-1).

## Tests
- api/test/patients-last-visit.test.ts (new), PatientRepo list query and PatientService list:
  - HMS-4 REQ-8: null for patients with no appointments (edge path).
  - HMS-4 REQ-7: a Scheduled appointment alone gives null (edge path).
  - HMS-4 REQ-2, REQ-4: two Completed appointments on Monday and Tuesday, with Tuesday closed first. The day of the later start is expected, so ordering by closing time would fail (normal path; covers the split's S-1 AC3 finding).
  - HMS-4 REQ-5, REQ-6, REQ-7: a later Cancelled, No-show or Scheduled appointment leaves the day unchanged (edge paths).
  - HMS-4 REQ-3: the same data read through an app configured for America/New_York gives Monday, and through Asia/Kolkata gives Tuesday. The day comes from the configured hospital zone, not from the UTC date (S-1 AC4, server side).
  - HMS-4 REQ-9, REQ-13: the paging shape is unchanged, the value is a YYYY-MM-DD string, and neither the raw instant nor any medical history is sent.
- web/src/__tests__/PatientsPage.test.tsx (extended), PatientsPage Last visit column:
  - HMS-4 REQ-8: "-" for a null last visit (edge path).
  - HMS-4 REQ-1, REQ-12: Last visit sits just before Next appointment, which stays the last column.
  - Kept from the implement step: HMS-4 S-1 AC4, the day string is shown as returned with the browser zone west of UTC.

## Known
- REQ-1, REQ-8, REQ-12: web column, its "-" placeholder and the column order are tested.
- REQ-2, REQ-5, REQ-6, REQ-7: the status filtering is tested through the API.
- REQ-3: tested with two configured zones on the same data.
- REQ-9, REQ-13: tested on the list response.

## Assumed
- REQ-4: "most recent" means the latest start time; the test sets up the case where closing order and start order disagree.

## Unknown
- U-1 (non-blocking): coverage cannot be reported; the project has no coverage script (Q-01).

## Open questions
- Q-01 (non-blocking, tech lead): add a coverage script? Default: keep it out of scope for HMS-4.
