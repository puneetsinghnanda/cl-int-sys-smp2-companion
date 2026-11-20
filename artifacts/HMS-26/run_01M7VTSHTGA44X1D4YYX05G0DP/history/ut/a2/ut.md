---
schema: ut-result.v1
run_id: run_01M7VTSHTGA44X1D4YYX05G0DP
step: ut
attempt: 2
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
    head_sha: 0ddfdf1a065599644ad8e0b958322de1208dac34
    diff_sha256: e45d4c07a5fe141bbdd86753155611b3c9ebfa449ad6572ff564f9db28d90b33
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
trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10, REQ-11, REQ-12, REQ-13, REQ-14, REQ-15, REQ-16, REQ-18]
known: [S-1, S-2, S-3, S-4, S-5, S-6]
assumed: []
unknown: []
questions: []
resolves:
  - {id: F-1, how: "A new API test gives a patient 101 future Cancelled appointments that push both Completed visits out of the capped list, and asserts last and counts still cover all."}
  - {id: F-2, how: "A new API test completes two appointments with different doctors, booked later-first, and asserts the later one is last."}
---
# Unit tests: HMS-26

## Summary
The tests cover the change in impl.md: `PatientRepo.visits` through `GET /api/patients/:id`, the new `date` helper, and the VisitsSummary on PatientDetailPage.

The API tests follow the neighbouring test file's style (C1, C5, C8) and use the invented fixture with a fixed clock (C7). They move the clock forward to close appointments after they start, and they cover:
- next appointment: the earliest Scheduled appointment after now, skipping a Cancelled one and one that already started;
- last visit: the later of two Completed appointments with different doctors, and never a later No-show;
- counts: every date, including future Cancelled appointments, without counting Scheduled ones;
- a patient with more appointments than the capped list holds (C9, C10);
- the empty patient, the unchanged appointment list, and 404 for an unknown patient.

The web tests (C2) render the page with a stubbed fetch, and they cover:
- placement between the name and the details;
- the next appointment link, and the local date and time across UTC midnight;
- a last visit shown as a date only;
- the settings zone (UTC instead of the default), which catches a hard-coded zone;
- empty texts and zero counts;
- the No-show warning at one No-show versus two;
- no medical history or contact details in the summary;
- the rest of the page, unchanged.

I did not run the tests. tsl runs the verifier (H3) and records tests run, passed and failed in checks.json. Coverage is not reported because the verifier skips it (C4).

Resolves F-1: A new API test gives a patient 101 future Cancelled appointments that push both Completed visits out of the capped list, and asserts last and counts still cover all.

Resolves F-2: A new API test completes two appointments with different doctors, booked later-first, and asserts the later one is last.

## Tests
- `api/test/patients-visits.test.ts` (new) covers these API cases:
  - HMS-26 REQ-3, REQ-4, REQ-6, REQ-10, REQ-11, REQ-14, REQ-18 and the 404 error path.
  - REQ-7 at S-2 AC1: two Completed appointments, the later one with the lower id.
  - REQ-7 and REQ-11 with 101 Cancelled appointments beyond the capped list.
  - At the API level: S-1 AC1, AC2 and AC4; S-2 AC1; S-3 AC1 and AC5.
- `web/src/__tests__/PatientDetailPage.test.tsx` (new) covers these page cases:
  - REQ-1 placement (S-4 AC1).
  - REQ-4, REQ-5 and REQ-9: the next appointment link and local time (S-1 AC3 and AC5).
  - REQ-8 and REQ-9: the last visit as a date only (S-2 AC2 and AC3).
  - REQ-9: the settings zone (S-6 AC1 and AC2).
  - REQ-6, REQ-10, REQ-12 and REQ-14: a patient with no appointments (S-3 AC5).
  - REQ-2 and REQ-13: one No-show versus two (S-3 AC2 and AC3).
  - REQ-15 and REQ-16: privacy (S-4 AC2 and AC3).
  - REQ-18: the rest of the page (S-5 AC1).
- `web/src/__tests__/format.test.ts` (extended) covers HMS-26 REQ-8: `date` prints only the local date, on the local side of midnight, and follows the zone it is given (C6).

## Known
- S-1: covered by the API next-appointment tests and the web link and time tests.
- S-2: covered by the API last-visit tests (two Completed appointments, a later No-show, and beyond the capped list), the web date-only tests and the format test.
- S-3: covered by the API counts tests, including the one past the capped list, and the web counts and warning tests.
- S-4: covered by the web placement and privacy tests. AC4 rests on the summary being mounted only in PatientDetailPage.
- S-5: covered by the web test of the rest of the page and the API test of the unchanged appointment list.
- S-6: covered by the web test with a UTC settings zone and by the format test.

## Assumed
None.

## Unknown
None.

## Open questions
None.
