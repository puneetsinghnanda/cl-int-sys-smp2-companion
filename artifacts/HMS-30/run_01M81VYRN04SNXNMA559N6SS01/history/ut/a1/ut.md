---
schema: ut-result.v1
run_id: run_01M81VYRN04SNXNMA559N6SS01
step: ut
attempt: 1
skill: unit-test@1.3.0
inputs:
  - path: artifacts/HMS-30/run_01M81VYRN04SNXNMA559N6SS01/impl.md
    sha256: b03701e7048b60a921f2709374b064311f7ced727534936ab87c31ece1c13f20
  - path: artifacts/HMS-30/run_01M81VYRN04SNXNMA559N6SS01/split.md
    sha256: 6b281327d9f889c19c469ea35fc7f1c182a3fdd5f01f45c22c6444f35aeade95
  - path: context/test-strategy.md
    sha256: f4bbc9d5d63dfc433c9fe466a7aa71a4fad0147baafe1f47db0c06e117ce6cba
  - path: context/generated/test-layout.json
    sha256: c0752ab8543e23547679dbe34f6381f656d36b0eb5506efb2a56f94dcb729cd6
pack_sha:
  sample-system: 7681f87749883d71abb4011d118366ca1242e1cc
code:
  - repo: sample-system
    base_sha: 53bbc4b9f09bc993ae3e93d8692364db7cb9fc10
    head_sha: f0767a53e24593cebea462c07b5643f39a2d0277
    diff_sha256: 0492f20c2385a6e58a08ebc58780dd1d89e6ba062aa13f19a9635082d089c701
    files:
      - api/test/appointments-status-counts.test.ts
      - web/src/__tests__/AppointmentsPage.test.tsx
claims:
  - id: C1
    text: "API tests use `Vitest and Supertest` against a throwaway PostgreSQL with a fixed clock."
    cites: ["context/test-strategy.md#layers"]
  - id: C2
    text: "Web tests use Vitest with `jsdom and Testing Library`."
    cites: ["context/test-strategy.md#layers"]
  - id: C3
    text: "Every requirement has a test whose name `starts with its ID`."
    cites: ["context/test-strategy.md#requirement-ids"]
  - id: C4
    text: "The existing list tests live in `api/test/appointments.test.ts`."
    cites: ["context/generated/test-layout.json#test:sample-system:api/test/appointments.test.ts"]
  - id: C5
    text: "Neighbouring per-feature API tests such as `api/test/doctors-no-shows.test.ts` use their own database so counts are not moved by other tests."
    cites: ["context/generated/test-layout.json#test:sample-system:api/test/doctors-no-shows.test.ts"]
  - id: C6
    text: "Web page tests follow `web/src/__tests__/PatientsPage.test.tsx`, stubbing fetch and rendering in a MemoryRouter."
    cites: ["context/generated/test-layout.json#test:sample-system:web/src/__tests__/PatientsPage.test.tsx"]
  - id: C7
    text: "The verifier `skips coverage`, so no run reports coverage."
    cites: ["context/test-strategy.md#gaps"]
trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10, REQ-11]
known: [S-1, S-2, S-3, S-4]
assumed: []
unknown: []
questions: []
---
# Unit tests: HMS-30

## Summary
New API and web tests cover the change in impl.md. The API tests cover `AppointmentRepo.countByStatus` and `AppointmentService.list`'s new `status_counts` through `GET /api/appointments`. The web tests cover the counts line and links in `AppointmentsPage`. Each changed path has a normal case and an edge or error case. Test data is the invented fixture from api/test/helpers.ts, plus "Test Patient/Doctor 001" names in the web test. tsl records the tests and coverage in checks.json when it runs the verifier (H3). The strategy says the verifier skips coverage (C7).

## Tests
- api/test/appointments-status-counts.test.ts (own database, fixed clock):
  - HMS-30 REQ-1/REQ-2/REQ-8: a doctor list returns exactly SCHEDULED, COMPLETED, CANCELLED, NO_SHOW (in that order), and their sum equals the total (S-1 AC1, S-2 AC4).
  - HMS-30 REQ-5: with Status = CANCELLED the rows and total follow the filter, but the counts do not (S-1 AC2).
  - HMS-30 REQ-4: page 2 returns no rows but the same counts as page 1 (S-2 AC1).
  - HMS-30 REQ-4 edge: an appointment at 11:45 on the last day is counted. One at Wednesday 01:00 local (19:30 UTC Tuesday) is not, so the bound follows the local day, not the UTC date (S-2 AC2).
  - HMS-30 REQ-4/REQ-6: another doctor's appointments are excluded, and empty statuses are 0 (S-2 AC3, S-1 AC3).
  - HMS-30 REQ-4: patient lists count only that patient (S-2 AC5).
  - HMS-30 REQ-6 edge: no matches gives all four statuses as 0.
  - HMS-30 REQ-9/REQ-10: start-time order and the `page`/`page_size` envelope are unchanged (S-4 AC1/AC2).
  - HMS-30 REQ-11 error: an invalid status is still a 400 field error (S-4 AC4).
- web/src/__tests__/AppointmentsPage.test.tsx (jsdom, stubbed fetch):
  - HMS-30 REQ-1/REQ-2/REQ-3: the line reads exactly "34 appointment(s) · Scheduled 4 · Completed 25 · Cancelled 3 · No-show 2" (S-1 AC1).
  - HMS-30 REQ-6: zero counts are shown and still linked (S-1 AC3).
  - HMS-30 REQ-7: the Completed link keeps doctorId, from and to, sets status=COMPLETED and drops page (S-3 AC1).
  - HMS-30 REQ-7 edge: from a patient list with Status set, the No-show link replaces the status rather than adding a second one (S-3 AC2).
  - HMS-30 REQ-9/REQ-11: the table and the Status filter are still shown (S-4).
  - HMS-30 REQ-1 error: a failed request shows the error and no counts.

## Known
- S-1: covered by the API count-shape, Status-filter and zero tests, and by the web line, zero and link tests.
- S-2: covered by the API paging, local-day boundary, other-doctor, other-patient and sum-equals-total tests.
- S-3: covered by the web link tests for doctor and patient lists.
- S-4: covered by the API order/envelope and invalid-status tests and the web table/filter test.

## Assumed
None

## Unknown
None

## Open questions
None
