---
schema: ut-result.v1
run_id: run_01M60VHZHR79G15R3AKPB0VYDK
step: ut
attempt: 1
skill: unit-test@1.0.1
inputs:
  - path: artifacts/HMS-5/run_01M60VHZHR79G15R3AKPB0VYDK/impl.md
    sha256: 0137364324d03689efa60e79f71467d9f328f737017200b453c89dfae4bf98ec
  - path: artifacts/HMS-5/run_01M60VHZHR79G15R3AKPB0VYDK/split.md
    sha256: 70a37e2163c9ffeee2153255649beda54907325e5980a65864572ae50d997b4a
  - path: context/test-strategy.md
    sha256: e7465d17d8814cd71fe160b96a859f1ba1414836ad71ef48b9aaeb166259eee8
  - path: context/generated/test-layout.json
    sha256: fd4b5d51b15f9aba1d03b5f6a0350e4e4849d862118fb3a3bda7abfa5127e179
pack_sha:
  sample-system: bd98ff08b83f8762c5e5c073ba5fadef88169b33
code:
  - repo: sample-system
    base_sha: fadc14a15f0bca3eabb25f89c53c43f5a8797246
    head_sha: 14ff7f40304cc050061bc0edf2eabfd2a555f24c
    diff_sha256: 7167f160892eea5670d15a19c1ee17bf5e7a7c599e9fe55e7eae2162ab314cf9
    files:
      - api/test/patients-visit-summary.test.ts
      - web/src/__tests__/PatientDetailPage.test.tsx
claims:
  - id: C1
    text: "API tests use `Supertest` against a throwaway PostgreSQL 16 with a fixed clock."
    cites: ["context/test-strategy.md#layers"]
  - id: C2
    text: "Web tests use Vitest with jsdom and `Testing Library`."
    cites: ["context/test-strategy.md#layers"]
  - id: C3
    text: "Every requirement has a test whose name `starts with its ID`."
    cites: ["context/test-strategy.md#requirement-ids"]
  - id: C4
    text: "Without PostgreSQL binaries or `HMS_TEST_DATABASE_URL`, database tests are skipped."
    cites: ["context/test-strategy.md#database-for-tests"]
  - id: C5
    text: "The verifier skips coverage because the project has `no coverage script`."
    cites: ["context/test-strategy.md#gaps"]
  - id: C6
    text: "The change adds the counts to the patient response as `visit_summary`."
    cites: ["artifacts/HMS-5/run_01M60VHZHR79G15R3AKPB0VYDK/impl.md#summary"]
trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9]
known: [REQ-1, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8]
assumed: [REQ-2, REQ-3, REQ-9]
unknown: []
questions: []
---
# Unit tests: HMS-5

## Summary
This step adds two test files, one per layer, following the test strategy (C1, C2). Each test name starts
with `HMS-5` and the requirement ids it covers (C3).
- The API file covers `AppointmentRepo.visitSummary` and `PatientService.get` through `GET /api/patients/:id` (C6). It uses its own database and a movable fixed clock. It tests the normal path (REQ-2, REQ-3), the exclusions (REQ-4, REQ-5), zero edges (REQ-6), separate counts per patient (threat T-7), no date window (REQ-9) and the unchanged list and medical history (REQ-7, REQ-8). It also covers two error paths: an unknown patient still returns 404, and a `visit_summary` sent in a PATCH is ignored (threat T-3).
- The web file covers `PatientDetailPage`: the block sits above the appointments list (REQ-1), shows the counts (REQ-2, REQ-3), shows 0 for none (REQ-6), and keeps the medical history (REQ-8).

Tests are not run in this step, and no verifier result is in the step data. tsl records the run counts in
checks.json when it runs the verifier (H3). There is no coverage figure because the verifier skips coverage (C5).
The API tests skip themselves if no PostgreSQL is available (C4).

## Tests
- api/test/patients-visit-summary.test.ts — 9 tests. Counts for Completed and No-show (REQ-2, REQ-3); Scheduled and Cancelled not counted (REQ-4, REQ-5); 0 and 0 for a patient with none, and 0 No-shows for a Completed-only patient (REQ-6); per-patient isolation (REQ-2, T-7); still counted the next day (REQ-9); list length, medical history and key set unchanged (REQ-7, REQ-8, T-11); unknown patient 404 (error); PATCH echo ignored (error, T-3).
- web/src/__tests__/PatientDetailPage.test.tsx — 4 tests. Heading order (REQ-1); rendered counts (REQ-2, REQ-3); zero counts (REQ-6); medical history still shown (REQ-8). The test data is invented (Test Patient 001, user@example.com) and fetch is stubbed, so there are no network calls.

## Known
- REQ-1: the web test checks that the "Visit summary" heading comes before the "Appointments" heading.
- REQ-4, REQ-5: the API test gives patient 1 one appointment of each status and expects counts of 1 and 1.
- REQ-6: the API and web tests both check 0 for each count, and 0 for one count alone.
- REQ-7, REQ-8: the API test checks the four appointments and the allergies. The web test checks that the medical history renders.

## Assumed
- REQ-2, REQ-3: the tests assert the field names `visit_summary.completed` and `visit_summary.no_show` as built (C6). They would change if the change's naming question (change Q-01) is answered with camelCase.
- REQ-9: the tests assert that there is no date window, as designed. Closed appointments still count when the clock moves to the next day.

## Unknown
No item is unknown. No test exposed a bug while writing. The verifier run (H3) gives the actual results.

## Open questions
None new. The naming question from the change step (change Q-01) still applies to the field names these tests assert.
