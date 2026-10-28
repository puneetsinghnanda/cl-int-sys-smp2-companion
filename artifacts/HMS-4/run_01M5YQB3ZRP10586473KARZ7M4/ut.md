---
schema: ut-result.v1
run_id: run_01M5YQB3ZRP10586473KARZ7M4
step: ut
attempt: 2
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
    head_sha: 68462461ef616bab6563f07f9ef63656e89c0c86
    diff_sha256: cde82c92747a8e2651a7b15603dd097c2e23d4fbd9f0305af955454fcc886e32
    files:
      - api/test/patients-last-visit.test.ts
      - web/src/__tests__/PatientsPage.test.tsx
resolves:
  - id: F-1
    how: "Added error-path tests for GET /api/patients: an invalid page and an invalid status filter are rejected with 400, and a page past the end is empty."
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
  - id: C6
    text: "In the MVP every request is the actor `staff` and every permission is granted."
    cites: ["context/architecture.md#identity"]
trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10, REQ-11, REQ-12, REQ-13]
known: [REQ-1, REQ-2, REQ-3, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10, REQ-11, REQ-12, REQ-13]
assumed: [REQ-4]
unknown: [U-1, U-2]
unknowns:
  - {id: U-1, blocking: false, text: "No coverage figure can be reported: the project has no coverage script, so the verifier skips the coverage step.", question: Q-01}
  - {id: U-2, blocking: false, text: "A refused unauthenticated request cannot be tested: in the MVP every request is the staff actor with every permission.", question: Q-02}
questions:
  - id: Q-01
    blocking: false
    ask: tech lead
    text: "Should the project add a coverage script so runs can report coverage against the base?"
    why: "Without it the unit-test step can show tests passing but never a coverage change."
    evidence: ["context/test-strategy.md#gaps"]
    options: ["Add a coverage script in a separate ticket", "Keep coverage out of scope"]
    default: "Keep coverage out of scope for HMS-4"
  - id: Q-02
    blocking: false
    ask: tech lead
    text: "Should a permission-refusal test for GET /api/patients wait until sign-in replaces the MVP identity?"
    why: "The route keeps requirePermission, but every permission is granted today, so no request can be refused."
    evidence: ["context/architecture.md#identity"]
    options: ["Add it with the sign-in work", "Add a test-only actor now"]
    default: "Add it with the sign-in work"
---
# Unit tests: HMS-4

## Summary
The tests cover the new `last_visit_date` on GET /api/patients and the Last visit column, with normal, edge and error paths (C1, C2). Every test name starts with HMS-4 and its REQ id (C3). The API tests need the throwaway PostgreSQL database that the verifier image provides; without one they are skipped (C4). No production code changed in this step. This retry adds the error-path tests that review finding F-1 asked for, plus REQ-10 and REQ-11 tests on data that has a last visit. The verifier's test log (runs/<run_id>/verifier/ut/test.log) records test counts when tsl runs H3; checks.json records only the step status. Coverage is not available (C5, U-1).

## Tests
- api/test/patients-last-visit.test.ts (new), PatientRepo list query and PatientService list:
  - HMS-4 REQ-8: null for patients with no appointments (edge).
  - HMS-4 REQ-7: a Scheduled appointment alone gives null (edge).
  - HMS-4 REQ-2, REQ-4: two Completed appointments, with the later one closed first. The later start's day is expected, so ordering by closing time would fail (normal).
  - HMS-4 REQ-5, REQ-6, REQ-7: a later Cancelled, No-show or Scheduled appointment leaves the day unchanged (edge).
  - HMS-4 REQ-3: America/New_York gives Monday and Asia/Kolkata gives Tuesday for the same data, so the day comes from the configured zone (normal).
  - HMS-4 REQ-9, REQ-13: paging shape unchanged; only a YYYY-MM-DD string is sent, never the raw instant or medical history.
  - HMS-4 REQ-10: search still finds the patient, with its last visit (normal).
  - HMS-4 REQ-11: the list stays newest first (normal).
  - HMS-4 REQ-9: an invalid page is rejected with 400 and no list (error).
  - HMS-4 REQ-10: an invalid status filter is rejected with a status field error (error).
  - HMS-4 REQ-9: a page past the end is empty and keeps the total (edge).
- web/src/__tests__/PatientsPage.test.tsx (extended), PatientsPage Last visit column:
  - HMS-4 REQ-3 (S-1 AC4): the day string is shown as returned with the browser zone west of UTC. Renamed from "HMS-4 S-1 AC4" so it leads with its REQ id.
  - HMS-4 REQ-8: "-" for a null last visit (edge).
  - HMS-4 REQ-1, REQ-12: Last visit sits just before Next appointment, which stays the last column.

## Known
- REQ-1, REQ-8, REQ-12: the web column, its "-" placeholder and the column order are tested.
- REQ-2, REQ-5, REQ-6, REQ-7: status filtering is tested through the API.
- REQ-3: tested with two configured zones on the same data, and in the web with a zone west of UTC.
- REQ-9, REQ-10, REQ-11: paging, search, sort and their error paths are tested on data that has a last visit.
- REQ-13: the list response carries no raw instant and no medical history.

## Assumed
- REQ-4: "most recent" means the latest start time. The test sets up a case where closing order and start order disagree.

## Unknown
- U-1 (non-blocking): coverage cannot be reported; the project has no coverage script (Q-01).
- U-2 (non-blocking): a refused unauthenticated request cannot be tested while every permission is granted to the staff actor (C6, Q-02).

## Open questions
- Q-01 (non-blocking, tech lead): add a coverage script? Default: keep it out of scope for HMS-4.
- Q-02 (non-blocking, tech lead): add a permission-refusal test with the sign-in work? Default: yes, with the sign-in work.
