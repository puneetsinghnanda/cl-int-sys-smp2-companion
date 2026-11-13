---
schema: ut-result.v1
run_id: run_01M79P1N889PBTFM8J1EX89TBK
step: ut
attempt: 2
skill: unit-test@1.2.0
inputs:
  - {path: artifacts/HMS-5/run_01M79P1N889PBTFM8J1EX89TBK/impl.md, sha256: 72eafcaf6291816266d51f848b24dd9000dc0c3aab1c974d8f12b91e8fe0e1a4}
  - {path: artifacts/HMS-5/run_01M79P1N889PBTFM8J1EX89TBK/split.md, sha256: db8ef532a356ccb24887a9fadcb6dc8a8fbc5338ae76d37cb71b224324490de8}
  - {path: context/test-strategy.md, sha256: f4bbc9d5d63dfc433c9fe466a7aa71a4fad0147baafe1f47db0c06e117ce6cba}
  - {path: context/generated/test-layout.json, sha256: c0752ab8543e23547679dbe34f6381f656d36b0eb5506efb2a56f94dcb729cd6}
pack_sha: {sample-system: 7681f87749883d71abb4011d118366ca1242e1cc}
code:
  - repo: sample-system
    base_sha: a5bc7f4a1a8b6639560ddda9dd7df04b81fd66a0
    head_sha: f83b3bd5d21a60dcfa306adda6afb5dfc42b0102
    diff_sha256: 86a3a0961caed030746aa51eb4a106e95bdce7d6c49443acdbe06c9242da2241
    files: [api/test/patients-visit-summary.test.ts, web/src/__tests__/PatientDetailPage.test.tsx]
claims:
  - {id: C1, text: "API tests use Vitest and `Supertest` against a throwaway PostgreSQL 16 with a fixed clock.", cites: ["context/test-strategy.md#layers"]}
  - {id: C2, text: "Web tests use Vitest with jsdom and `Testing Library`.", cites: ["context/test-strategy.md#layers"]}
  - {id: C3, text: "Every requirement has at least one test whose name starts with its ID, and `New requirements get new IDs and tests`.", cites: ["context/test-strategy.md#requirement-ids"]}
  - {id: C4, text: "Without a PostgreSQL binary or `HMS_TEST_DATABASE_URL`, database tests are skipped.", cites: ["context/test-strategy.md#database-for-tests"]}
  - {id: C5, text: "The verifier skips coverage because the project has `no coverage script`.", cites: ["context/test-strategy.md#gaps"]}
  - {id: C6, text: "The change adds `visitSummary` to the patient detail through PatientRepo and PatientService.", cites: ["artifacts/HMS-5/run_01M79P1N889PBTFM8J1EX89TBK/impl.md#claim:C7", "context/generated/module-map.json#sym:sample-system:api/src/repositories/patientRepo.ts:PatientRepo", "context/generated/module-map.json#sym:sample-system:api/src/services/patientService.ts:PatientService"]}
  - {id: C7, text: "Before this change, the patient page had `no web tests`.", cites: ["context/test-strategy.md#gaps"]}
trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9]
known: [S-1, S-2, S-3]
assumed: []
unknown: []
questions: []
resolves:
  - {id: F-2, how: "The web test now renders an upcoming and a past appointment and checks the row order, and adds a past-only case for S-3 AC2."}
  - {id: F-1, how: "The API test now checks that PATCH on an unknown patient returns 404 and that POST with invalid fields returns 400 with field errors."}
---
# Unit tests: HMS-5

## Summary
I added two test files, following the test strategy (C1, C2, C3). Every test name starts with its HMS-5 requirement id.
- **API test:** runs against its own throwaway database. It covers the counts for both patients, the excluded statuses, a patient with only Scheduled and Cancelled appointments, the all-time window, the zero case, the POST and PATCH responses, their error paths, the 404 error path and the field shape.
- **Web test:** the patient page had no web tests before this change (C7). It covers where the block sits, the counts, the zeros, the absence of a period picker, the appointments list order, and the medical history with and without entries.

The added field flows through PatientRepo and PatientService (C6). I don't report test counts or coverage here. tsl records the test results in checks.json when it runs the verifier (H3), and the verifier skips coverage because the project has no coverage script (C5). Database tests are skipped when no PostgreSQL is available (C4).

Retry fixes:
- F-2: The web test now renders an upcoming and a past appointment and checks the row order, and adds a past-only case for S-3 AC2.
- F-1: The API test now checks that PATCH on an unknown patient returns 404 and that POST with invalid fields returns 400 with field errors.
- I also fixed the minor findings:
  - New tests for S-2 AC2 at the API and for S-1 AC4.
  - The shape test is now tagged REQ-2/REQ-3.
  - The web REQ-8 test has a new title.
  - C6 now also cites the PatientRepo and PatientService module-map facts.

## Tests
- api/test/patients-visit-summary.test.ts (CO-1, CO-2, EP-1 to EP-3):
  - **REQ-2/REQ-3:** patient 1 counts `{ completed: 2, noShow: 1 }` (S-1 AC1).
  - **REQ-4 edge:** a past appointment still Scheduled, and a Cancelled one with the latest start, are not counted (S-2 AC1, AC4).
  - **REQ-2 edge:** patient 2 gets only their own count, `{ 1, 0 }` (S-1 AC5).
  - **REQ-6:** the counts stay the same with the clock moved months ahead, because they cover all time (S-1 AC3).
  - **REQ-5:** a new patient's POST response and detail both show 0 and 0 (S-2 AC3).
  - **REQ-1:** the PATCH response carries the counts.
  - **REQ-2 error:** an unknown patient still returns 404.
  - **REQ-1 error:**
    - PATCH on an unknown patient still returns 404.
    - POST with invalid fields still returns 400 with one field error per field, and no summary.
  - **REQ-4/REQ-5:** a patient with only a Scheduled and a Cancelled appointment shows 0 and 0 (S-2 AC2).
  - **REQ-2/REQ-3:** the summary holds only two integer keys.
- web/src/__tests__/PatientDetailPage.test.tsx (CO-5):
  - **REQ-1:** the Visit summary heading comes before the Appointments heading.
  - **REQ-2/REQ-3:** the Completed and No-show values are shown (S-1 AC1).
  - **REQ-5:** 0 is shown for each count (S-2 AC2).
  - **REQ-6:** the Visit summary offers no period picker (S-1 AC4).
  - **REQ-7/REQ-9:** the medical history and the appointment link are still shown (S-3 AC4).
  - **REQ-7:** an upcoming and a past appointment are shown upcoming first, in the order given (S-3 AC1).
  - **REQ-7:** a patient with only past appointments shows the same rows in the same order (S-3 AC2).
  - **REQ-7 edge:** the empty list text still appears.
  - **REQ-8:** medical history with entries is shown (S-3 AC3).
  - **REQ-8 edge:** without medical history, "None recorded" and "Not recorded" appear (S-3 AC5).

## Known
- S-1: covered by the API count, scope and all-time tests, and by the web count and no-period-picker tests.
- S-2: covered by the API exclusion, Scheduled-and-Cancelled-only and zero tests, and by the web zero test.
- S-3: covered by the web tests for appointments list order (with upcoming, and past-only), the other sections, and medical history with and without entries.

## Assumed
None.

## Unknown
None.

## Open questions
None.
