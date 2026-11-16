---
schema: ut-result.v1
run_id: run_01M7HXS1SRABSX1P1P0GQVCXP2
step: ut
attempt: 1
skill: unit-test@1.2.0
inputs:
  - {path: artifacts/HMS-5/run_01M7HXS1SRABSX1P1P0GQVCXP2/impl.md, sha256: 8e51cdc532c9d078189ef65d82908cd9cb0c30cef84798e830b88330c8773492}
  - {path: artifacts/HMS-5/run_01M7HXS1SRABSX1P1P0GQVCXP2/split.md, sha256: 0e92bcecf3892dbdc506bcfd34075fd333587ff93608dae1d0478f0464d0a588}
  - {path: context/test-strategy.md, sha256: f4bbc9d5d63dfc433c9fe466a7aa71a4fad0147baafe1f47db0c06e117ce6cba}
  - {path: context/generated/test-layout.json, sha256: c0752ab8543e23547679dbe34f6381f656d36b0eb5506efb2a56f94dcb729cd6}
pack_sha: {sample-system: 7681f87749883d71abb4011d118366ca1242e1cc}
code:
  - repo: sample-system
    base_sha: 5b59b27a5f8cc27ff5ea9ea3211c6a85ac6b9d76
    head_sha: 584915bbbd0b9031a269cf20ae2302c39b19b4f7
    diff_sha256: b33302e8cab7f43b0c66f42dda360284ef19fc16d07f41dbf8b49d22ebb6c35e
    files: [api/test/patients-visit-summary.test.ts, web/src/__tests__/PatientDetailPage.test.tsx]
claims:
  - {id: C1, text: "API tests use Vitest and `Supertest` against a throwaway PostgreSQL 16 with a fixed clock.", cites: ["context/test-strategy.md#layers"]}
  - {id: C2, text: "Web tests use Vitest with jsdom and `Testing Library`.", cites: ["context/test-strategy.md#layers"]}
  - {id: C3, text: "Every requirement has at least one test whose name starts with its ID, and `New requirements get new IDs and tests`.", cites: ["context/test-strategy.md#requirement-ids"]}
  - {id: C4, text: "Without a PostgreSQL binary or `HMS_TEST_DATABASE_URL`, database tests are skipped.", cites: ["context/test-strategy.md#database-for-tests"]}
  - {id: C5, text: "The verifier skips coverage because the project has `no coverage script`.", cites: ["context/test-strategy.md#gaps"]}
  - {id: C6, text: "The change adds `visitSummary` to the patient detail through PatientRepo and PatientService.", cites: ["artifacts/HMS-5/run_01M7HXS1SRABSX1P1P0GQVCXP2/impl.md#claim:C7", "context/generated/module-map.json#sym:sample-system:api/src/repositories/patientRepo.ts:PatientRepo", "context/generated/module-map.json#sym:sample-system:api/src/services/patientService.ts:PatientService"]}
  - {id: C7, text: "Before this change, the patient page had `no web tests`.", cites: ["context/test-strategy.md#gaps"]}
trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9]
known: [S-1, S-2, S-3]
assumed: []
unknown: []
questions: []
---
# Unit tests: HMS-5

## Summary
I added two test files, following the test strategy (C1, C2, C3). Every test name starts with `HMS-5` and its requirement id.
- **API test:** runs against its own throwaway database with a movable clock. It covers the three changed endpoints (GET, POST and PATCH on patients), each on its success path and its error path, plus the counting rules.
- **Web test:** the patient page had no web tests before this change (C7). It covers where the block sits, the counts, the zeros, the absence of a period picker, the appointments list, the medical history, the rest of the page and the load-error path.

The added field flows through PatientRepo and PatientService (C6). I don't report test counts or coverage here. tsl records the test results in checks.json when it runs the verifier (H3), and the verifier skips coverage because the project has no coverage script (C5). Database tests are skipped when no PostgreSQL is available (C4).

## Tests
- api/test/patients-visit-summary.test.ts (CO-1, CO-2, EP-1 to EP-3):
  - **REQ-2/REQ-3:** GET for patient 1 returns `{ completed: 2, noShow: 1 }` (S-1 AC1).
  - **REQ-2/REQ-3:** the summary holds exactly two integer keys.
  - **REQ-4 edge:** a Cancelled appointment with the latest start is not counted (S-2 AC1).
  - **REQ-4 edge:** a past appointment that is still Scheduled counts as neither (S-2 AC4).
  - **REQ-2 edge:** patient 2 gets only their own count, `{ 1, 0 }` (S-1 AC5).
  - **REQ-4/REQ-5:** a patient with only a Scheduled and a Cancelled appointment shows 0 and 0 (S-2 AC2).
  - **REQ-6:** the counts stay the same with the clock moved to June 2027, because they cover all time (S-1 AC3).
  - **REQ-5:** a new patient's POST response and detail both show 0 and 0 (S-2 AC3).
  - **REQ-1:** the PATCH response carries the counts; a `visitSummary` sent in the PATCH body is ignored.
  - **REQ-2 error:** GET on an unknown patient is still 404 with no summary; an invalid id is still 400.
  - **REQ-1 error:** PATCH on an unknown patient is still 404; POST with invalid fields is still 400 with one message per field and no summary.
  - **REQ-7:** the detail's appointments list keeps its order, upcoming first then past.
  - **REQ-8:** the medical history of the detail is unchanged, and the summary carries none of it.
- web/src/__tests__/PatientDetailPage.test.tsx (CO-5):
  - **REQ-1:** the Visit summary heading sits between Medical history and Appointments, directly above the Appointments heading (S-1 AC1).
  - **REQ-2/REQ-3:** the Completed and No-show values are shown, not swapped (S-1 AC1); with No-shows only, the counts are not merged (S-1 AC2).
  - **REQ-5:** 0 is shown for each count (S-2 AC2, AC3).
  - **REQ-4:** the page shows the counts the API sends, not a count of the listed rows (S-2 AC1).
  - **REQ-6:** the Visit summary offers no period picker (S-1 AC4).
  - **REQ-7:** an upcoming Scheduled row and past No-show, Completed and Cancelled rows keep the order given (S-3 AC1); a past-only patient shows the same rows in the same order (S-3 AC2); the empty list text still appears.
  - **REQ-8:** medical history with entries is shown as before (S-3 AC3); without entries the same placeholders appear (S-3 AC5).
  - **REQ-9:** the patient details, the Book appointment link and the Edit link are still shown (S-3 AC4).
  - **REQ-9 error:** a patient that cannot be loaded shows the error and no Visit summary.

## Known
- S-1: covered by the API count, scope and all-time tests, and by the web placement, count and no-period-picker tests.
- S-2: covered by the API exclusion, Scheduled-and-Cancelled-only and zero tests, and by the web zero test.
- S-3: covered by the API list-order and medical-history tests, and by the web tests for the appointments list (with upcoming, and past-only), the medical history with and without entries, and the rest of the page.

## Assumed
None.

## Unknown
None.

## Open questions
None.
