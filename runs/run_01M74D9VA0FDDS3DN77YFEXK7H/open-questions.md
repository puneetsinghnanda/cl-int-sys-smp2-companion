---
schema: open-questions.v1
run_id: run_01M74D9VA0FDDS3DN77YFEXK7H
ticket: HMS-9
workflow: brownfield-change@1.4.0
step: secrev@1.1.0
created_at: '2026-11-11T08:14:58Z'
status: open
---

# Open questions — HMS-9 (impl step)

Answer each question below the line `answer:`. Keep the ids. Then run:
`/truminds:resume run_01M74D9VA0FDDS3DN77YFEXK7H`

## Q-01 · non-blocking · ask: tech lead
**Question:** Does the verifier's node-22@1 test step have Postgres available, so that the HMS-9 api tests (describe.skipIf(!pgAvailable())) actually ran rather than being skipped?
**Proposed default:** none
answer:
answered_by:
decision:

## Q-02 · non-blocking · ask: tech lead · round 2
**Question:** checks.json shows the test step passed, but its report_sha256 is null, so there is no per-test report. Does the node-22@1 verifier provide PostgreSQL 16 or HMS_TEST_DATABASE_URL, so that the HMS-9 API tests actually ran rather than skipped?
**Proposed default:** none
answer:
answered_by:
decision:

## Q-03 · non-blocking · ask: tech lead · round 2
**Question:** Does the verifier's test step provide PostgreSQL 16 or HMS_TEST_DATABASE_URL, so that the HMS-9 API tests run rather than skip?
**Why it matters:** The API tests skip without a database (C4), so a green test step alone does not prove they ran.
**Evidence:** context/test-strategy.md#database-for-tests
**Options:**
  A. Yes, the verifier provides a database
  B. No, add one to the verifier
**Proposed default:** Yes, the verifier provides a database
answer:
answered_by:
decision:

## Q-04 · non-blocking · ask: tech lead · round 3
**Question:** When sign-in splits roles, should the per-day booked/free counts on the doctor detail need appointments:read, like the day schedule, or stay under doctors:read, like the HMS-7 No-shows count?
**Why it matters:** In the MVP every permission is granted (C1), so nothing changes today; after sign-in, a doctors:read-only user would see appointment-derived load (C2, C3).
**Evidence:** sample-system:api/src/routes/doctors.ts#L30, sample-system:api/src/routes/doctors.ts#L64, context/architecture.md#identity
**Options:**
  A. Keep doctors:read (aggregate counts only, as HMS-7 does)
  B. Return null unless the caller also has appointments:read
**Proposed default:** Keep doctors:read (aggregate counts only, as HMS-7 does)
answer:
answered_by:
decision:
