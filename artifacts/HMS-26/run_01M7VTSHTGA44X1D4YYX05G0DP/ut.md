---
schema: ut-result.v1
run_id: run_01M7VTSHTGA44X1D4YYX05G0DP
step: ut
attempt: 3
skill: unit-test@1.3.0
inputs:
  - {path: artifacts/HMS-26/run_01M7VTSHTGA44X1D4YYX05G0DP/impl.md, sha256: b8571be7328f92bab36ac750d0239e386ae1fead0d6f3d638ed879b87ad36cc1}
  - {path: artifacts/HMS-26/run_01M7VTSHTGA44X1D4YYX05G0DP/split.md, sha256: 0505e8c204b650fbd44f6f8a04dfa8da0e035fbf38d644498edb5957afb7a071}
  - {path: context/test-strategy.md, sha256: f4bbc9d5d63dfc433c9fe466a7aa71a4fad0147baafe1f47db0c06e117ce6cba}
  - {path: context/generated/test-layout.json, sha256: c0752ab8543e23547679dbe34f6381f656d36b0eb5506efb2a56f94dcb729cd6}
pack_sha: {sample-system: 7681f87749883d71abb4011d118366ca1242e1cc}
code:
  - repo: sample-system
    base_sha: 2209ab85950036e5b88b28b2ceac974246b13b7d
    head_sha: 39f89853220f22f2231f9dff5c341ea834f32505
    diff_sha256: 6cd1d05c70d8dcd4fd53ab2c04951e61e43827c83c95f5194cfcb6c50681328c
    files:
      - api/test/patients-visits.test.ts
      - web/src/__tests__/PatientDetailPage.test.tsx
      - web/src/__tests__/format.test.ts
claims:
  - {id: C1, text: "API tests use Vitest and `Supertest` against a throwaway PostgreSQL with a fixed clock.", cites: ["context/test-strategy.md#layers"]}
  - {id: C2, text: "Web tests use Vitest with jsdom and `Testing Library`.", cites: ["context/test-strategy.md#layers"]}
  - {id: C3, text: "Test names start with their requirement `ID`.", cites: ["context/test-strategy.md#requirement-ids"]}
  - {id: C4, text: "The verifier skips `coverage`, so no run reports coverage.", cites: ["context/test-strategy.md#gaps"]}
  - {id: C5, text: "The HMS-1 next-appointment tests live in `api/test/patients-next-appointment.test.ts`; the new API file follows their fixture and clock style.", cites: ["context/generated/test-layout.json#test:sample-system:api/test/patients-next-appointment.test.ts"]}
  - {id: C6, text: "Formatting tests live in `web/src/__tests__/format.test.ts`.", cites: ["context/generated/test-layout.json#test:sample-system:web/src/__tests__/format.test.ts"]}
  - {id: C7, text: "The fixed test clock is Monday `2026-10-19` 08:00 in Asia/Kolkata.", cites: ["sample-system:api/test/helpers.ts#L11"]}
  - {id: C8, text: "`testApp` accepts a clock, so tests can move now past a start time.", cites: ["context/generated/module-map.json#sym:sample-system:api/test/helpers.ts:testApp"]}
  - {id: C9, text: "The visits must come from queries over all appointments, not the capped list (`limit = 100`).", cites: ["runs/run_01M7VTSHTGA44X1D4YYX05G0DP/ide-answers.md#a-02"]}
  - {id: C10, text: "Only Scheduled appointments are bound by the overlap constraints (`WHERE (status = 'SCHEDULED')`), so a test can insert many Cancelled rows directly.", cites: ["sample-system:db/migrations/20261016T090300__create_appointments.sql#L21"]}
  - {id: C11, text: "The capped list puts appointments starting at or after now first (`a.starts_at >= $2`), so the over-cap test must read with a clock after the Monday visits.", cites: ["sample-system:api/src/repositories/patientRepo.ts#L227"]}
trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10, REQ-11, REQ-12, REQ-13, REQ-14, REQ-15, REQ-16, REQ-18]
known: [S-1, S-2, S-3, S-4, S-5, S-6]
assumed: []
unknown: []
questions: []
---
# Unit tests: HMS-26

## Summary
These tests cover three parts of the change in impl.md: `PatientRepo.visits` through `GET /api/patients/:id`, the new `date` helper, and the VisitsSummary on PatientDetailPage.

The API tests follow the C1, C5 and C8 style, using the invented fixture and a fixed clock (C7). They move the clock forward to close appointments after they start, and they cover:
- next: the earliest Scheduled appointment after now, skipping a Cancelled one and one that already started;
- last: the later of two Completed appointments with different doctors, and never a later No-show;
- counts: every date, including future Cancelled appointments, without counting Scheduled ones;
- a patient with more appointments than the capped list holds (C9, C10);
- the empty patient, the unchanged appointment list, and 404 for an unknown patient.

The web tests (C2) render the page with a stubbed fetch. They cover:
- placement of the summary;
- the next appointment link, with local date and time across UTC midnight;
- a last visit shown as a date only;
- the settings zone, using UTC instead of the default;
- empty texts and zero counts;
- the No-show warning at one versus two;
- privacy of the summary;
- the rest of the page, unchanged.

The attempt 2 verifier run (H3) failed in the over-cap test, and the test was at fault, not the code. It read the patient with the 08:00 clock, so the Monday visits still sorted as upcoming at the top of the capped list (C11). The test now reads with a Monday 11:10 clock and also asserts that the list holds exactly 100 rows. In that run every other test passed, including all web tests.

I did not run the tests. tsl runs the verifier (H3) and records the tests run, passed and failed in checks.json. Coverage is not reported, because the verifier skips it (C4).

## Tests
- `api/test/patients-visits.test.ts` (new):
  - HMS-26 REQ-3, REQ-4, REQ-6, REQ-10, REQ-11, REQ-14 and REQ-18, plus the 404 error path.
  - REQ-7 with two Completed appointments, where the later one has the lower id (S-2 AC1).
  - REQ-7 and REQ-11 with 101 Cancelled appointments beyond the capped list.
  - At the API level, these map to S-1 AC1, AC2 and AC4; S-2 AC1; and S-3 AC1 and AC5.
- `web/src/__tests__/PatientDetailPage.test.tsx` (new):
  - REQ-1: placement (S-4 AC1).
  - REQ-4, REQ-5 and REQ-9: next appointment link and local time (S-1 AC3 and AC5).
  - REQ-8 and REQ-9: last visit as a date only (S-2 AC2 and AC3).
  - REQ-9: settings zone (S-6 AC1 and AC2).
  - REQ-6, REQ-10, REQ-12 and REQ-14: empty patient (S-3 AC5).
  - REQ-2 and REQ-13: one versus two No-shows (S-3 AC2 and AC3).
  - REQ-15 and REQ-16: privacy (S-4 AC2 and AC3).
  - REQ-18: the rest of the page (S-5 AC1).
- `web/src/__tests__/format.test.ts` (extended): HMS-26 REQ-8. `date` prints only the local date, on the local side of midnight, and follows the zone it is given (C6).

## Known
- S-1: the API next-appointment tests and the web link and time tests cover it.
- S-2: the API last-visit tests (two Completed appointments, a later No-show, past the cap), the web date-only tests and the format test cover it.
- S-3: the API counts tests (including past the cap) and the web counts and warning tests cover it.
- S-4: the web placement and privacy tests cover it. AC4 rests on the summary being mounted only in PatientDetailPage.
- S-5: the web test of the rest of the page and the API test of the unchanged appointment list cover it.
- S-6: the web test with a UTC settings zone and the format test cover it.

## Assumed
None.

## Unknown
None.

## Open questions
None.
