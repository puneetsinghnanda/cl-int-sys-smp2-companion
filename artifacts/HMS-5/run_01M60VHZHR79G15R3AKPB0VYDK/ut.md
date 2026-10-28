---
schema: ut-result.v1
run_id: run_01M60VHZHR79G15R3AKPB0VYDK
step: ut
attempt: 2
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
    head_sha: b451b63ced3846ca885f72ff3dc835dc80bde818
    diff_sha256: f6430bb69c786924325c360d564d368d3167d588c81e12b721f6a5bf195f3b3c
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
This is attempt 2. It follows the tech lead's "changes" decision at gate G-escalate-ut-a1, and each point
of that comment is addressed:
- **S-2 AC5 (attempt-1 F-1):** a No-show-only patient is now tested in both layers. The API expects
  `{ completed: 0, no_show: 1 }`, and the web test renders Completed 0 next to No-show 1.
- **Appointment order (F-2):** the REQ-7 test asserts the exact `startsAt` order of patient 1's four
  appointments, upcoming first (Tuesday 10:00), then past, most recent first (Monday 09:30, 09:15, 09:00).
- **PATCH status (F-3):** the edit test sends a real change (`address`) together with `visit_summary`. It
  asserts status 200, that the address change applied, and that the counts in the PATCH response and a
  later GET are unchanged.
- **Far-apart dates (F-4):** a new patient has a Completed appointment on Monday 2026-10-19 and a No-show
  four weeks later on Monday 2026-11-16. Both are counted, both now and with the clock moved to
  the later date.
- **Key-set wording (F-5):** the Tests section now says that only the keys inside `visit_summary` are
  checked. The full top-level key set of the patient response is not asserted.

The tests still follow the test strategy (C1, C2), and each name starts with `HMS-5` and its REQ ids (C3).
Tests are not run in this step, and no verifier result is in the step data. tsl records the run counts in
checks.json when it runs the verifier (H3). There is no coverage figure because the verifier skips coverage (C5).
The API tests skip themselves if no PostgreSQL is available (C4).

## Tests
- api/test/patients-visit-summary.test.ts — 11 tests covering `AppointmentRepo.visitSummary` and `PatientService.get` through `GET /api/patients/:id` (C6):
  - Completed and No-show counts (REQ-2, REQ-3).
  - Scheduled and Cancelled not counted (REQ-4, REQ-5).
  - Zero edges (REQ-6): 0 and 0 for a patient with none, 0 No-shows for a Completed-only patient, and 0 Completed for a No-show-only patient (S-2 AC5).
  - Exact per-patient counts for three patients (REQ-2, threat T-7).
  - Closed appointments four weeks apart both counted, before and after the clock moves (REQ-9, S-1 AC4).
  - Exact appointment order, upcoming first (REQ-7, S-3 AC1).
  - Medical history still returned, and the keys inside `visit_summary` are only `completed` and `no_show` (REQ-8, T-11). The top-level response keys are not asserted.
  - Error path: an unknown patient is 404 with no `visit_summary`.
  - Error path: a PATCH with `visit_summary` returns 200, applies the real edit and leaves the counts derived (T-3).
- web/src/__tests__/PatientDetailPage.test.tsx — 5 tests covering `PatientDetailPage`:
  - The Visit summary heading comes before the Appointments heading (REQ-1).
  - Rendered counts (REQ-2, REQ-3).
  - 0 and 0 (REQ-6).
  - 0 Completed next to 1 No-show (REQ-6, S-2 AC5).
  - Medical history still shown (REQ-8).
  - The test data is invented (Test Patient 001, user@example.com) and fetch is stubbed, so there are no network calls.

## Known
- REQ-1: the web test checks that the "Visit summary" heading comes before the "Appointments" heading.
- REQ-4, REQ-5: patient 1 has one appointment of each status, and the counts are 1 and 1.
- REQ-6: all three zero cases (none, Completed-only, No-show-only) are tested in the API. Two of them are also tested in the web page.
- REQ-7: the exact appointment order is asserted.
- REQ-8: the API returns the allergies and the web page renders the medical history.

## Assumed
- REQ-2, REQ-3: the tests assert the field names `visit_summary.completed` and `visit_summary.no_show` as built (C6). They would change if the change step's naming question (change Q-01) is answered with camelCase.
- REQ-9: the tests assert that there is no date window, as designed, using closed appointments four weeks apart.

## Unknown
No item is unknown. No test exposed a bug while writing. The verifier run (H3) gives the actual results.

## Open questions
None new. The naming question from the change step (change Q-01) still applies to the field names these tests assert.
