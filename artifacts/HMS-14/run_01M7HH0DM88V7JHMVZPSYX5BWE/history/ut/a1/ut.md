---
schema: ut-result.v1
run_id: run_01M7HH0DM88V7JHMVZPSYX5BWE
step: ut
attempt: 1
skill: unit-test@1.1.0
inputs:
  - path: artifacts/HMS-14/run_01M7HH0DM88V7JHMVZPSYX5BWE/impl.md
    sha256: a34a17f3621e8c2ea198aa79c62757daaea0286c964804d0f596c1292edb2fcc
  - path: artifacts/HMS-14/run_01M7HH0DM88V7JHMVZPSYX5BWE/split.md
    sha256: 88e8cd0212df1bcdf5574b26e6415ef50243df439a57102ca81559c2f68fc854
  - path: context/test-strategy.md
    sha256: f4bbc9d5d63dfc433c9fe466a7aa71a4fad0147baafe1f47db0c06e117ce6cba
  - path: context/generated/test-layout.json
    sha256: c0752ab8543e23547679dbe34f6381f656d36b0eb5506efb2a56f94dcb729cd6
pack_sha:
  sample-system: 7681f87749883d71abb4011d118366ca1242e1cc
code:
  - repo: sample-system
    base_sha: fd6a0c3789c63eb26f68198b71035ae127ac999e
    head_sha: 77aa86d7a4032dfc5fab3e59bce65e47c36985d0
    diff_sha256: fcc2c5d63322329d314f378fd18cf74592e48ba5bd88836d9ff40e0e91b4c36f
    files:
      - api/test/appointments-book-again.test.ts
      - web/src/__tests__/AppointmentDetailPage.test.tsx
      - web/src/__tests__/BookAppointmentPage.test.tsx
claims:
  - id: C1
    text: "API tests use Vitest and Supertest against a throwaway PostgreSQL 16 with a fixed clock: `Vitest and Supertest`."
    cites: ["context/test-strategy.md#layers"]
  - id: C2
    text: "Web tests use Vitest with jsdom and `Testing Library`."
    cites: ["context/test-strategy.md#layers"]
  - id: C3
    text: "Every test name starts with its requirement id, as the strategy asks: `Every PRD requirement has at least one test whose name starts with its ID`."
    cites: ["context/test-strategy.md#requirement-ids"]
  - id: C4
    text: "The verifier skips coverage because `the project has no coverage script`."
    cites: ["context/test-strategy.md#gaps"]
  - id: C5
    text: "API tests share the helpers' fixed clock, `export const NOW`, and invented fixture."
    cites: ["sample-system:api/test/helpers.ts#L10"]
  - id: C6
    text: "The change under test adds `doctor_status` and `patient_status` and the Book again flow."
    cites: ["artifacts/HMS-14/run_01M7HH0DM88V7JHMVZPSYX5BWE/impl.md#claim:C12"]
  - id: C7
    text: "Database tests are skipped without PostgreSQL binaries or `HMS_TEST_DATABASE_URL`."
    cites: ["context/test-strategy.md#database-for-tests"]
  - id: C8
    text: "Existing API appointment tests live in `api/test/appointments.test.ts`; the HMS-14 API tests sit beside it."
    cites: ["context/generated/test-layout.json#test:sample-system:api/test/appointments.test.ts"]
trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10, REQ-11, REQ-12, REQ-13, REQ-14, REQ-15, REQ-16, REQ-17, REQ-18, REQ-19]
known: [S-1, S-2, S-3, S-4, S-5, S-6]
assumed: []
unknown: []
questions: []
---
# Unit tests: HMS-14

## Summary
These tests cover the HMS-14 change across both layers and follow the strategy's frameworks and requirement-id naming (C1, C2, C3).
- The API tests pin `doctor_status` and `patient_status` as `ACTIVE` or `INACTIVE` on the detail GET and on all four action routes: cancel, reschedule, complete and no-show.
- They also check that a booking made again gets a new id and that the original and its history do not change.
- They check that the server still refuses a booking with an Inactive doctor.
- The web tests cover where "Book again" appears, its disabled reasons and its navigation target and state.
- They also cover how the book page uses the reason from navigation state, and the hospital-local date across UTC midnight.
- Each changed function or route has a normal-path test and at least one edge or error test.
- Test results come from the verifier container (H3); tsl records them in checks.json. Coverage is not reported because the project has no coverage script (C4).

## Tests
- api/test/appointments-book-again.test.ts (Vitest + Supertest, invented fixture, fixed clock; C5):
  - HMS-14 REQ-9/REQ-10: statuses are ACTIVE on the detail.
  - HMS-14 REQ-17 (A-05): the cancel, reschedule, complete and no-show actions return both statuses.
  - HMS-14 REQ-13/REQ-14: booking again gets a new id; the original and its history are unchanged.
  - HMS-14 REQ-19: no medical history in the detail.
  - HMS-14 REQ-9/REQ-10: INACTIVE after the PATCH, with the PATCH status asserted.
  - HMS-14 REQ-9/REQ-12 error: an Inactive doctor is still refused on booking.
- web/src/__tests__/AppointmentDetailPage.test.tsx (Testing Library, stubbed fetch):
  - S-1: Book again shows on Cancelled and No-show, not on Scheduled or Completed.
  - S-2/S-3: it navigates to `?doctorId=1&patientId=2` with `{ reason }` in state and never the cancellation reason; with no original reason it passes no state.
  - S-4: the doctor, patient and both-Inactive cases are disabled, with the exact texts.
  - S-6 AC1: the actions on a Scheduled appointment are unchanged.
- web/src/__tests__/BookAppointmentPage.test.tsx (Testing Library):
  - S-3: the reason from state fills the field and stays editable; the 301st character is cut and maxLength is 300.
  - S-6 / edge: no state or a non-text state leaves the field empty.
  - S-2 AC2: at 19:00 UTC on 16 November the date is 2026-11-17, because Asia/Kolkata = UTC+05:30 makes that 00:30 on 17 November locally.
- The tests from the impl step (web/src/__tests__/bookAgain.test.ts) are kept unchanged.

## Known
- S-1, S-2, S-3, S-4: covered by the two web test files against the changed pages.
- S-5: covered by the API new-id, unchanged-original and Inactive-refusal tests.
- S-6: covered by the Scheduled-actions web test, the no-state book page test and the four action-route API tests.

## Assumed
None

## Unknown
None

## Open questions
None
