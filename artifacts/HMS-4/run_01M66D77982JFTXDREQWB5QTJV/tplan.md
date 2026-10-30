---
schema: test-plan.v1
run_id: run_01M66D77982JFTXDREQWB5QTJV
step: tplan
attempt: 1
skill: test-plan@1.0.1
inputs:
  - path: artifacts/HMS-4/run_01M66D77982JFTXDREQWB5QTJV/split.md
    sha256: f49a45c822dcc797bdba004c411c08fa32db8a41468d2957a34eb264d72f9764
  - path: artifacts/HMS-4/run_01M66D77982JFTXDREQWB5QTJV/design.md
    sha256: b31d420e0453c1145cb4585599d95a4f6cec5281f16b6f5300467e2fc94ed5d3
  - path: context/test-strategy.md
    sha256: e7465d17d8814cd71fe160b96a859f1ba1414836ad71ef48b9aaeb166259eee8
  - path: context/generated/test-layout.json
    sha256: fd4b5d51b15f9aba1d03b5f6a0350e4e4849d862118fb3a3bda7abfa5127e179
pack_sha:
  sample-system: bd98ff08b83f8762c5e5c073ba5fadef88169b33
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
    text: "Patient list next appointment tests already exist in `api/test/patients-next-appointment.test.ts`."
    cites: ["context/generated/test-layout.json#test:sample-system:api/test/patients-next-appointment.test.ts"]
  - id: C6
    text: "Patient API tests, including the no-medical-history check, live in `api/test/patients.test.ts`."
    cites: ["context/generated/test-layout.json#test:sample-system:api/test/patients.test.ts"]
  - id: C7
    text: "Web tests for the patient list live in `web/src/__tests__/PatientsPage.test.tsx`."
    cites: ["context/generated/test-layout.json#test:sample-system:web/src/__tests__/PatientsPage.test.tsx"]
  - id: C8
    text: "No page is tested end to end in a browser, and the verifier reports no `coverage`."
    cites: ["context/test-strategy.md#gaps"]
trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10, REQ-11, REQ-12, REQ-13, REQ-14, REQ-15]
known: [TP-1, TP-5, TP-6, TP-7, TP-8, TP-9, TP-10, TP-11, TP-12, TP-13, TP-14, TP-15, TP-16]
assumed: [TP-2, TP-3, TP-4]
unknown:
  - {id: U-1, blocking: false}
  - {id: U-2, blocking: false}
questions:
  - id: Q-01
    blocking: false
    ask: tech lead
    text: Does the verifier provide PostgreSQL so the integration tests run, or are they proven only locally and in CI?
    why: Every integration test here skips without a database, so a passing verifier may prove only the web unit tests.
    evidence: ["context/test-strategy.md#database-for-tests"]
    options: [verifier provides PostgreSQL, run npm test with PostgreSQL in CI before merge]
    default: run npm test with PostgreSQL in CI before merge
  - id: Q-02
    blocking: false
    ask: tech lead
    text: Can TP-10 register more than 20 invented patients through POST /api/patients inside the test, since the shared fixture has fewer?
    why: Proving 20 per page with the new field needs a second page; no existing test pages past one page.
    evidence: ["context/generated/test-layout.json#test:sample-system:api/test/patients.test.ts"]
    options: [register invented patients in the test, extend the shared fixture]
    default: register invented patients in the test
tests:
  - id: TP-1
    title: "HMS-4 REQ-1: the list shows a Last visit column header"
    covers: [S-1/AC1]
    level: unit
  - id: TP-2
    title: "HMS-4 REQ-2, REQ-13: last_visit_at is the start of the later of two Completed appointments"
    covers: [S-1/AC2]
    level: integration
  - id: TP-3
    title: "HMS-4 REQ-3, REQ-15: an instant whose UTC and Asia/Kolkata dates differ shows the Kolkata date"
    covers: [S-1/AC3]
    level: unit
  - id: TP-4
    title: "HMS-4 REQ-14: the Last visit text uses the same formatter as Next appointment"
    covers: [S-1/AC4]
    level: unit
  - id: TP-5
    title: "HMS-4 REQ-4: a later Cancelled appointment leaves the earlier Completed date"
    covers: [S-2/AC1]
    level: integration
  - id: TP-6
    title: "HMS-4 REQ-5: a later No-show appointment leaves the earlier Completed date"
    covers: [S-2/AC2]
    level: integration
  - id: TP-7
    title: "HMS-4 REQ-6: a Scheduled appointment, even one already started, never shows"
    covers: [S-2/AC3]
    level: integration
  - id: TP-8
    title: "HMS-4 REQ-7: the cell shows '-' when last_visit_at is null"
    covers: [S-2/AC4]
    level: unit
  - id: TP-9
    title: "HMS-4 REQ-7: last_visit_at is null for a patient with no Completed appointment"
    covers: [S-2/AC4]
    level: integration
  - id: TP-10
    title: "HMS-4 REQ-8: with more than 20 patients, page 1 holds 20 and page 2 items still carry last_visit_at"
    covers: [S-3/AC1]
    level: integration
  - id: TP-11
    title: "HMS-4 REQ-9: a search returns the same patients, with their last visit"
    covers: [S-3/AC2]
    level: integration
  - id: TP-12
    title: "HMS-4 REQ-10: the list stays newest first"
    covers: [S-3/AC3]
    level: integration
  - id: TP-13
    title: "HMS-4 REQ-11: the Next appointment cell keeps its link and doctor beside a Last visit"
    covers: [S-3/AC4]
    level: unit
  - id: TP-14
    title: "HMS-4 REQ-12: list items carry exactly the expected keys and no medical history"
    covers: [S-4/AC1]
    level: integration
  - id: TP-15
    title: "HMS-4 REQ-9 negative: an invalid list filter is still rejected with a field error"
    covers: [S-3/AC2]
    level: integration
  - id: TP-16
    title: "HMS-4 REQ-4 negative: a patient whose only appointment is Cancelled gets null, not another patient's visit"
    covers: [S-2/AC1, S-2/AC4]
    level: integration
---
# Test plan: HMS-4

## Scope
**In scope:** the new `last_visit_at` field on `GET /api/patients` (the PatientRepo list query) and the PatientsPage Last visit column, for every acceptance criterion of S-1 to S-4.

**Levels:**
- **Web unit tests** (jsdom and Testing Library) prove rendering and formatting.
- **API integration tests** (Supertest with PostgreSQL) prove the query: status filtering, latest visit, paging, search, sort order and response keys.

**Out of scope:**
- End-to-end browser tests. None exist in the project, and every criterion is proven at a lower level.
- Coverage. The verifier reports none.
- Performance of the per-row subquery. There is no load test setup.

Most tests already exist from the implement and unit-test steps. TP-10 and TP-12 still need writing. TP-14 is the existing PAT-8 key test, updated for the new field.

## Tests
| Id | Level | Covers | What it checks | Status |
|---|---|---|---|---|
| TP-1 | unit | S-1/AC1 | Last visit header | exists in PatientsPage.test.tsx |
| TP-2 | integration | S-1/AC2 | later of two Completed appointments | exists in patients-last-visit.test.ts |
| TP-3 | unit | S-1/AC3 | Kolkata date at a UTC/Kolkata boundary | exists; should also assert the 28 Oct date directly |
| TP-4 | unit | S-1/AC4 | same formatter as Next appointment | exists |
| TP-5 | integration | S-2/AC1 | later Cancelled appointment ignored | exists |
| TP-6 | integration | S-2/AC2 | later No-show appointment ignored | exists |
| TP-7 | integration | S-2/AC3 | Scheduled appointment ignored, even after its start | exists |
| TP-8 | unit | S-2/AC4 | "-" fallback | exists |
| TP-9 | integration | S-2/AC4 | null with no Completed appointment | exists |
| TP-10 | integration | S-3/AC1 | 20 per page with more than 20 patients | to write (Q-02) |
| TP-11 | integration | S-3/AC2 | search keeps patients and their last visit | exists |
| TP-12 | integration | S-3/AC3 | newest first | to write, mirroring the HMS-1 order test |
| TP-13 | unit | S-3/AC4 | Next appointment unchanged | exists |
| TP-14 | integration | S-4/AC1 | exact keys, no medical history | exists, the PAT-8 test |
| TP-15 | integration | S-3/AC2 | **negative:** invalid filter gives 400 with a field error | exists |
| TP-16 | integration | S-2/AC1, S-2/AC4 | **negative:** only-Cancelled patient gets null, no cross-patient leak | exists |

Coverage check: S-1/AC1 to AC4, S-2/AC1 to AC4, S-3/AC1 to AC4 and S-4/AC1 are each covered by at least one test.

## Known
- TP-1, TP-8, TP-13: rendering facts from known REQ-1, REQ-7 and REQ-11.
- TP-5, TP-6, TP-7, TP-9, TP-16: status rules from known REQ-4 to REQ-7.
- TP-10, TP-11, TP-12, TP-15: unchanged list behaviour from known REQ-8 to REQ-10.
- TP-14: no medical history, from known REQ-12.

## Assumed
- TP-2: "latest" means the later start time (assumed REQ-13).
- TP-3: the displayed date is the start instant in the hospital zone (assumed REQ-15).
- TP-4: the format is the Next appointment formatter (assumed REQ-14).

## Unknown
- U-1 (non-blocking): whether the verifier runs the integration tests or skips them without PostgreSQL (Q-01).
- U-2 (non-blocking): how TP-10 gets more than 20 patients (Q-02).

## Open questions
- Q-01 (non-blocking, tech lead): Does the verifier provide PostgreSQL? Default: run `npm test` with PostgreSQL in CI before merging.
- Q-02 (non-blocking, tech lead): How should TP-10 create more than 20 patients? Default: register invented patients inside the test.
