---
schema: ut-result.v1
run_id: run_01M66D77982JFTXDREQWB5QTJV
step: ut
attempt: 1
skill: unit-test@1.0.1
inputs:
  - path: artifacts/HMS-4/run_01M66D77982JFTXDREQWB5QTJV/impl.md
    sha256: 26fe0697b5932aacb3488d8d1530b60856684ba8bcb1ffd38247920447b82599
  - path: artifacts/HMS-4/run_01M66D77982JFTXDREQWB5QTJV/split.md
    sha256: f49a45c822dcc797bdba004c411c08fa32db8a41468d2957a34eb264d72f9764
  - path: context/test-strategy.md
    sha256: e7465d17d8814cd71fe160b96a859f1ba1414836ad71ef48b9aaeb166259eee8
  - path: context/generated/test-layout.json
    sha256: fd4b5d51b15f9aba1d03b5f6a0350e4e4849d862118fb3a3bda7abfa5127e179
pack_sha:
  sample-system: bd98ff08b83f8762c5e5c073ba5fadef88169b33
code:
  - repo: sample-system
    base_sha: fa76fd997d00e9e941c88df1fa4eede92478666c
    head_sha: 7b76ff29548208c2257ee905c20693774f40d028
    diff_sha256: cb497862ae9b05c62d4420ce521d205fce19f9cc409add17ad7382fc3b18539e
    files:
      - api/test/patients-last-visit.test.ts
      - web/src/__tests__/PatientsPage.test.tsx
claims:
  - id: C1
    text: "API tests use `Supertest` against a throwaway PostgreSQL with a fixed clock."
    cites: ["context/test-strategy.md#layers"]
  - id: C2
    text: "Web tests use `jsdom` and Testing Library."
    cites: ["context/test-strategy.md#layers"]
  - id: C3
    text: "Without a PostgreSQL cluster or `HMS_TEST_DATABASE_URL`, database tests are skipped."
    cites: ["context/test-strategy.md#database-for-tests"]
  - id: C4
    text: "Every requirement has a test whose name starts with its ID, such as `DOC-1`."
    cites: ["context/test-strategy.md#requirement-ids"]
  - id: C5
    text: "The verifier skips coverage because the project has no `coverage script`."
    cites: ["context/test-strategy.md#gaps"]
  - id: C6
    text: "Existing patient list tests live in `api/test/patients-next-appointment.test.ts`."
    cites: ["context/generated/test-layout.json#test:sample-system:api/test/patients-next-appointment.test.ts"]
  - id: C7
    text: "Existing web tests for the list live in `web/src/__tests__/PatientsPage.test.tsx`."
    cites: ["context/generated/test-layout.json#test:sample-system:web/src/__tests__/PatientsPage.test.tsx"]
trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-11, REQ-13, REQ-14]
known: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-11]
assumed: [REQ-13, REQ-14]
unknown:
  - {id: U-1, blocking: false}
  - {id: U-2, blocking: false}
questions:
  - id: Q-01
    blocking: false
    ask: tech lead
    text: Does the verifier container provide PostgreSQL 16 or HMS_TEST_DATABASE_URL, so the API tests run instead of being skipped?
    why: All API tests, including the HMS-4 ones, skip without a database; a passing H3 would then not prove the last-visit query.
    evidence: ["context/test-strategy.md#database-for-tests"]
    options: [verifier provides PostgreSQL, API tests run only locally and in CI]
    default: API tests run only locally and in CI; run npm test with PostgreSQL before merging
  - id: Q-02
    blocking: false
    ask: tech lead
    text: Does the search "patient one" match only fixture patient 1?
    why: The REQ-8/REQ-9 test assumes it does, mirroring the existing HMS-1 search for "patient two".
    evidence: ["context/generated/test-layout.json#test:sample-system:api/test/patients-next-appointment.test.ts"]
    options: ["yes", "no, use another search term"]
    default: "yes"
---
# Unit tests: HMS-4

## Summary
The new tests cover each changed function and the endpoint on a normal path and an edge or error path.
- **API (`GET /api/patients` through `PatientRepo.list`):**
  - the latest of two Completed appointments;
  - null when there is none, and when the only appointment is Cancelled;
  - Cancelled, No-show and Scheduled appointments never count;
  - one patient's visit never shows on another;
  - search and paging still return the last visit;
  - an invalid filter is still rejected with a field error.
- **Web (PatientsPage Last visit column):**
  - the header sits before Next appointment;
  - "-" when null;
  - hospital-zone formatting at a UTC/Kolkata date boundary;
  - the cell is plain text, not a link;
  - the Next appointment column is unchanged.

No test counts or coverage appear here. No verifier result was in the data for this step, so tsl records them in `checks.json` when it runs the verifier (H3). The project has no coverage script, so no run reports coverage.

## Tests
- `api/test/patients-last-visit.test.ts` (extended from the impl step):
  - HMS-4 REQ-7: null with no Completed appointment.
  - HMS-4 REQ-2, REQ-13: the latest of two Completed appointments.
  - HMS-4 REQ-4, REQ-7: an only-Cancelled appointment gives null, not another patient's visit. *(new)*
  - HMS-4 REQ-8, REQ-9: search and paging keep the last visit. *(new)*
  - HMS-4 REQ-9: an invalid filter is still a 400 with a field error. *(new)*
  - HMS-4 REQ-4, REQ-5, REQ-6: later Cancelled, No-show and Scheduled appointments are ignored.
- `web/src/__tests__/PatientsPage.test.tsx`:
  - HMS-4 REQ-1: the column header.
  - HMS-4 REQ-7: the "-" fallback.
  - HMS-4 REQ-3, REQ-14: hospital-zone formatting.
  - HMS-4 REQ-1: plain text, no link. *(new)*
  - HMS-4 REQ-11: Next appointment is unchanged beside a Last visit. *(new)*

## Known
- REQ-1, REQ-3, REQ-7, REQ-11: web tests on the rendered column, using an invented fixture (Test Patient 001/002).
- REQ-2, REQ-4, REQ-5, REQ-6: API tests seed each appointment status through the public endpoints.
- REQ-8, REQ-9: API tests check the list envelope (20 per page), search and the filter error are unchanged.

## Assumed
- REQ-13: "latest" is the later start time, as the change implements.
- REQ-14: the expected text is the existing `dateTime` in Asia/Kolkata, the formatter Next appointment uses.

## Unknown
- U-1 (non-blocking): whether the verifier provides PostgreSQL, so the API tests run rather than skip (Q-01).
- U-2 (non-blocking): whether the search "patient one" matches only fixture patient 1 (Q-02).

## Open questions
- Q-01 (non-blocking, tech lead): Does the verifier provide PostgreSQL? Default: the API tests run locally and in CI only.
- Q-02 (non-blocking, tech lead): Does "patient one" match only patient 1? Default: yes.
