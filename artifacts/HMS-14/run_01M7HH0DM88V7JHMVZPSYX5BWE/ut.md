---
schema: ut-result.v1
run_id: run_01M7HH0DM88V7JHMVZPSYX5BWE
step: ut
attempt: 2
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
    head_sha: 89ab4d372426044fc3294b0779170e5a1b18f70e
    diff_sha256: c8472b885f9f3e2e6b7a81e855dc9d37347892acbdc6a379cbd3013a9ba857b4
    files:
      - api/test/appointments-book-again.test.ts
      - web/src/__tests__/AppointmentDetailPage.test.tsx
      - web/src/__tests__/BookAppointmentPage.test.tsx
resolves:
  - id: F-1
    how: "An error-path test now checks that an unknown appointment id on the detail GET still answers 404 without the new fields."
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
  - id: C9
    text: "An unknown appointment id on the detail read throws `not found` (404)."
    cites: ["sample-system:api/src/services/appointmentService.ts#L56"]
  - id: C10
    text: "The existing APT tests already pin the unchanged booking rules, including `APT-4: two simultaneous bookings of one slot leave exactly one appointment`."
    cites: ["context/generated/test-layout.json#test:sample-system:api/test/appointments.test.ts"]
  - id: C11
    text: "The existing PAT-7 test already pins `PAT-7: an Inactive patient cannot be booked`."
    cites: ["context/generated/test-layout.json#test:sample-system:api/test/patients.test.ts"]
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
- They check that a booking made again gets a new id and that the original and its history do not change.
- They check that the server still refuses an Inactive doctor and an Inactive patient.
- They check that the changed detail read still answers 404 for an unknown id, without the new fields (C9).
- The web tests cover where "Book again" appears, its disabled reasons and its navigation target and state.
- They also cover how the book page uses the reason from navigation state, and the hospital-local date across UTC midnight.
- Each changed function or route has a normal-path test and at least one edge or error test.
- Test results come from the verifier container (H3); tsl records them in checks.json. Coverage is not reported because the project has no coverage script (C4).
- `code[]` is measured by tsl from the run branch after the impl step's commit, so `base_sha` is that commit rather than the impl attempt's working head.

Resolves:
- F-1: An error-path test now checks that an unknown appointment id on the detail GET still answers 404 without the new fields.

## Tests
- api/test/appointments-book-again.test.ts (Vitest + Supertest, invented fixture, fixed clock; C5). It replaces the impl step's version of the same file: its five checks are kept, and the original appointment is now booked in `beforeAll` and PATCH statuses are asserted.
  - "HMS-14 REQ-9/REQ-10: the booking and the detail carry doctor_status and patient_status as ACTIVE"
  - "HMS-14 REQ-17 (A-05): the cancel action …", "… the reschedule action …", "… the complete and no-show actions return both statuses"
  - "HMS-14 REQ-13/REQ-14 (S-5 AC3/AC4): booking again creates a new id and leaves the original and its history as they were"
  - "HMS-14 REQ-19: the detail still carries no medical history"
  - "HMS-14 REQ-10: an Inactive patient shows as INACTIVE …" and "HMS-14 REQ-9: an Inactive doctor shows as INACTIVE …" (the PATCH status is asserted)
  - "HMS-14 REQ-9/REQ-12 error: booking again with an Inactive doctor is still refused server-side"
  - "HMS-14 REQ-10/REQ-12 error: booking again with an Inactive patient is still refused server-side"
  - "HMS-14 REQ-17 error: an unknown appointment id on the detail GET is still 404"
- web/src/__tests__/AppointmentDetailPage.test.tsx (Testing Library, stubbed fetch; every name starts with HMS-14 REQ-n):
  - REQ-1/REQ-2/REQ-3: Book again shows on Cancelled and No-show, not on Scheduled or Completed.
  - REQ-4/5/6/8/19: it navigates to `?doctorId=1&patientId=2` with `{ reason }` in state and never the cancellation reason.
  - REQ-18: no state when the original has no reason.
  - REQ-9/10/11: the exact disabled texts.
  - REQ-17: Scheduled actions are unchanged.
- web/src/__tests__/BookAppointmentPage.test.tsx (Testing Library; every name starts with HMS-14 REQ-n):
  - REQ-6/REQ-15: prefill and editing.
  - REQ-16: the 301st character is cut and maxLength is 300.
  - REQ-17/REQ-18: no or non-text state leaves the field empty.
  - REQ-7: at 19:00 UTC on 16 November the date is 2026-11-17, because Asia/Kolkata = UTC+05:30 makes that 00:30 on 17 November locally.
- The impl step's web/src/__tests__/bookAgain.test.ts is kept unchanged.

## Known
- S-1, S-2, S-3, S-4: covered by the two web test files against the changed pages.
- S-5: AC3 and AC4 are covered by the new-id and unchanged-original API test. AC1, AC2 and AC5 (APT-4, APT-3, APT-2) rely on the booking path, which is unchanged; the existing APT tests in api/test/appointments.test.ts pin them (C10). The Inactive refusals are pinned both here and by the existing PAT-7 test (C11).
- S-6: covered by the Scheduled-actions web test, the no-state book page test, the four action-route API tests and the 404 error test.

## Assumed
None

## Unknown
None

## Open questions
None
