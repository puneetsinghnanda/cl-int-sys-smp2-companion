---
schema: ut-result.v1
run_id: run_01M8EC1C803452B4P6VTAB10M7
step: ut
attempt: 1
skill: unit-test@1.5.0
inputs:
  - {path: artifacts/HMS-23/run_01M8EC1C803452B4P6VTAB10M7/impl.md, sha256: 5f1a08cc4132d0ed0cd78adf43f0da120afc8fc78663562f2bfafcff1252965f}
  - {path: artifacts/HMS-23/run_01M8EC1C803452B4P6VTAB10M7/split.md, sha256: aff52115f9fdfadc010120f5dca462e696245658dd4c590d83c5ab683356b584}
  - {path: context/test-strategy.md, sha256: f4bbc9d5d63dfc433c9fe466a7aa71a4fad0147baafe1f47db0c06e117ce6cba}
  - {path: context/generated/test-layout.json, sha256: c0752ab8543e23547679dbe34f6381f656d36b0eb5506efb2a56f94dcb729cd6}
pack_sha: {sample-system: 7681f87749883d71abb4011d118366ca1242e1cc}
code:
  - repo: sample-system
    base_sha: 9621453acbf4c539216b3584a4222dd98fa14aa6
    head_sha: 40348b0125b3dd3f618dfeb34ecc0b7f2ba091ba
    diff_sha256: 4da6e0572cd0ab5e6236a14ffda6ac236fccf57f5ae6b33be9605420f5483a7e
    files:
      - api/test/dashboard-on-duty.test.ts
      - web/src/__tests__/DashboardPage.test.tsx
claims:
  - {id: C1, text: "API tests use `Vitest` and Supertest against a throwaway PostgreSQL with a fixed clock; web tests use Testing Library.", cites: ["context/test-strategy.md#layers"]}
  - {id: C2, text: "Each test name starts with its requirement ID, since every requirement has a test whose name `starts with its ID`.", cites: ["context/test-strategy.md#requirement-ids"]}
  - {id: C3, text: "The verifier `skips coverage`, so no coverage is reported.", cites: ["context/test-strategy.md#gaps"]}
  - {id: C4, text: "Each test gets its own database from `freshDatabase`, which copies the migrated template.", cites: ["sample-system:db/testing/test-db.ts#L18"]}
  - {id: C5, text: "The shared fixed clock `NOW` is Monday 19 October 2026, 08:00 local.", cites: ["sample-system:api/test/helpers.ts#L10"]}
  - {id: C6, text: "Local times map to UTC through the helper `at`.", cites: ["sample-system:api/test/helpers.ts#L14"]}
  - {id: C7, text: "The rows come from `onDutyToday` in the doctor service.", cites: ["sample-system:api/src/services/doctorService.ts#L162"]}
  - {id: C8, text: "A doctor whose blocks yield no slot today is skipped by `states.length === 0`.", cites: ["sample-system:api/src/services/doctorService.ts#L174"]}
  - {id: C9, text: "Rows are sorted by `b.free - a.free`, with ties kept in the repo's name order.", cites: ["sample-system:api/src/services/doctorService.ts#L187"]}
  - {id: C10, text: "On-duty doctors are read by `onDutyOn` for today's weekday.", cites: ["sample-system:api/src/repositories/doctorRepo.ts#L142"]}
  - {id: C11, text: "An unbooked slot that has started is `PAST`, neither booked nor free.", cites: ["sample-system:api/src/services/doctorService.ts#L194"]}
  - {id: C12, text: "The dashboard fills onDuty from `onDutyToday` whatever the period.", cites: ["sample-system:api/src/services/dashboardService.ts#L92"]}
  - {id: C13, text: "A booking in the past is refused (`must be in the future`), so tests book first and move the clock after.", cites: ["sample-system:api/src/services/appointmentService.ts#L144"]}
  - {id: C14, text: "The page shows `Fully booked` when nextFree is null.", cites: ["sample-system:web/src/pages/DashboardPage.tsx#L94"]}
  - {id: C15, text: "The page appends `(Inactive)` to an Inactive department.", cites: ["sample-system:web/src/pages/DashboardPage.tsx#L91"]}
  - {id: C16, text: "The doctor name links with `?date=` set to the dashboard's today.", cites: ["sample-system:web/src/pages/DashboardPage.tsx#L89"]}
  - {id: C17, text: "An empty list shows `No doctors on duty today`.", cites: ["sample-system:web/src/pages/DashboardPage.tsx#L73"]}
  - {id: C18, text: "The existing dashboard page tests live in `web/src/__tests__/DashboardPage.test.tsx`.", cites: ["context/generated/test-layout.json#test:sample-system:web/src/__tests__/DashboardPage.test.tsx"]}
  - {id: C19, text: "Existing dashboard API tests sit in their own files such as `api/test/dashboard-no-shows.test.ts`.", cites: ["context/generated/test-layout.json#test:sample-system:api/test/dashboard-no-shows.test.ts"]}
  - {id: C20, text: "A doctor whose slots have all passed stays listed: `Keep, show Fully booked`.", cites: ["runs/run_01M8EC1C803452B4P6VTAB10M7/ide-answers.md#a-01"]}
  - {id: C21, text: "An Inactive department gets the `(Inactive)` marker.", cites: ["runs/run_01M8EC1C803452B4P6VTAB10M7/ide-answers.md#a-02"]}
  - {id: C22, text: "The rows are part of the dashboard response: `Add onDuty to GET /api/dashboard`.", cites: ["runs/run_01M8EC1C803452B4P6VTAB10M7/ide-answers.md#a-03"]}
  - {id: C23, text: "The next free slot is sent as `nextFree as { start, label }`.", cites: ["runs/run_01M8EC1C803452B4P6VTAB10M7/ide-answers.md#a-04"]}
trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10, REQ-11, REQ-12, REQ-13, REQ-14, REQ-15, REQ-16, REQ-17, REQ-18, REQ-19]
known: [S-1, S-2, S-3, S-4, S-5, S-6, S-7, TP-1, TP-2]
assumed: []
unknown:
  - {id: U-1, blocking: false}
  - {id: U-2, blocking: false}
questions: []
---
# Unit tests: HMS-23

## Summary
Tests cover the new "On duty today" rows of `GET /api/dashboard` (C7, C12, C22) and the On duty today table on the dashboard page (C14 to C17). Every acceptance criterion of S-1 to S-7 has at least one test.

- **API:** 24 tests in a new file, `api/test/dashboard-on-duty.test.ts` (TP-1). They use Vitest and Supertest (C1).
  - Each test gets its own database (C4), so the doctors in one test never show up in another.
  - Each test sets its own clock (C5, C6). Some run at 04:00 local on Saturday 28 November 2026, which is still Friday in UTC.
- **Web:** 9 tests in a new describe block of `web/src/__tests__/DashboardPage.test.tsx` (TP-2, C18).
- Test names start with the requirement ID (C2).
- Every test record is invented: Test Doctor A, Test Patient 001, doctor1@example.com.

Local type-check and lint passed. No production code was changed.

tests and coverage are left out of the front matter. tsl records them in checks.json when it runs the verifier (H3). Coverage is never reported in this project, because the verifier skips it (C3).

## Tests

### api/test/dashboard-on-duty.test.ts
- HMS-23 REQ-2: lists an Active doctor with a block today and leaves out one whose block is on another day. Covers S-1 AC1.
- HMS-23 REQ-3: at 04:00 local on Saturday (22:30 UTC Friday), `today` is 2026-11-28 and only the Saturday doctor is listed, not the Friday one. Covers S-1 AC2.
- HMS-23 REQ-15: an Inactive doctor with the most free slots is not listed (C10). Covers S-1 AC3.
- HMS-23 REQ-14/REQ-15: the list is empty when the only doctor available today is Inactive and an Active doctor works on another day. Covers S-1 AC4.
- HMS-23 REQ-2/REQ-7/REQ-8: two blocks today (09:00 and 09:30, 14:00 and 14:30) with 09:00 booked give one row with booked 1, free 3 and nextFree 09:30. Covers S-1 AC5.
- HMS-23 REQ-4/REQ-5: the row carries the doctor's name and the department `{ name: Outpatient, status: ACTIVE }`. Covers S-2 AC1.
- HMS-23 REQ-6: after its department is set Inactive, the row carries `{ name: Surgery, status: INACTIVE }` (C21). Covers S-2 AC2.
- HMS-23 REQ-17: the doctor has a phone and an email stored. The row has exactly the six expected keys and contains neither value, nor the words phone or email. Covers S-2 AC3.
- HMS-23 REQ-7/REQ-8: slots at 09:00 (Completed), 09:30 (No-show), 10:00 (empty), 10:30 (Scheduled) and 11:00 (Cancelled only), at 09:45. The row shows booked 3, free 2 and next free 10:00. Covers S-3 AC1.
- HMS-23 REQ-8: unbooked slots at 09:00 and 10:00, at 09:45. The row shows booked 0 and free 1, because the passed slot counts as neither (C11). Covers S-3 AC2.
- HMS-23 REQ-7/REQ-8: two doctors with mixed bookings, one cancelled and one completed. For each doctor, booked, free and nextFree equal the doctor's day schedule (`/api/doctors/:id/slots`). Covers S-3 AC3.
- HMS-23 REQ-3/REQ-7/REQ-8: at 04:00 local on Saturday, the Saturday slots 04:30 and 05:00 (05:00 booked) give booked 1 and free 1. The three free Friday slots are not counted. Covers S-3 AC4.
- HMS-23 REQ-9: at exactly 09:30, the next free slot is `{ start: 10:00, label: "10:00" }`, and the label equals the day schedule's label (C23). Covers S-4 AC1.
- HMS-23 REQ-9: 09:00 free, 10:00 booked, 11:00 free, at 08:00. The next free slot is 09:00, not the later 11:00. Covers S-4 AC2.
- HMS-23 REQ-12: every slot after now is booked, so the row shows free 0 and nextFree null. Covers S-4 AC3.
- HMS-23 REQ-19/REQ-12: the last slot starts at 16:30, nothing is booked, and the clock is 17:00. The doctor stays listed with booked 0, free 0 and nextFree null. The test fails if such a doctor were hidden, the option A-01 rejected (C20). Covers S-4 AC4.
- HMS-23 REQ-3/REQ-9: at 04:00 local on Saturday, the next free slot is Saturday 04:30, not a Friday time and not null. Covers S-4 AC5.
- HMS-23 REQ-10: doctors A, B and C (in name order) have 1, 4 and 2 free slots. The order is B, C, A, which a name sort would get wrong (C9). Covers S-5 AC1.
- HMS-23 REQ-11: "Test Beta" is added before "Test Alpha" and both have 3 free slots. Alpha comes first, which an id sort would get wrong. Covers S-5 AC2.
- HMS-23 REQ-10/REQ-12: doctor A is Fully booked and doctor B has 1 free slot. B comes before A, although A sorts first by name. Covers S-5 AC3.
- HMS-23 REQ-13/REQ-3: at 04:00 local on Saturday, the dashboard's `today` (the date the link uses) is 2026-11-28, and that day's schedule holds the Saturday slots 04:30 and 05:00. Covers S-6 AC1 and S-6 AC2.
- HMS-23 REQ-16: onDuty is identical with no period, today, 7d and 30d (C12). Covers S-7 AC1.
- HMS-23 REQ-16 error: an unknown period is still 400 with a `period` field error.
- HMS-23 REQ-18: with appointments booked today and tomorrow, the totals and the next list keep their definitions, and the response keys are only the old ones plus onDuty. Covers S-7 AC2.
- HMS-23 REQ-18: with no appointments, the appointment totals are 0 and next is empty, while onDuty still has its row. Covers S-7 AC3.

### web/src/__tests__/DashboardPage.test.tsx (new describe "DashboardPage On duty today (HMS-23)")
- HMS-23 REQ-1: the "On duty today" heading follows the tiles and comes before "Next appointments". Covers S-1 AC1.
- HMS-23 REQ-14: an empty onDuty shows "No doctors on duty today" and no rows (C17). Covers S-1 AC4.
- HMS-23 REQ-4/REQ-5/REQ-6/REQ-7/REQ-8/REQ-9/REQ-12: each row shows the name and department, with "Surgery (Inactive)" for an Inactive department and no marker for an Active one, then booked and free. Next free shows the label ("09:30", never the ISO start) or "Fully booked" (C14, C15). Covers S-1 AC5, S-2 AC1, S-2 AC2 and S-4 AC3.
- HMS-23 REQ-12/REQ-19: a row with free 0 and nextFree null stays listed as Fully booked, and the empty-state text is not shown. Covers S-4 AC4.
- HMS-23 REQ-10/REQ-11: rows keep the API order, most free first and Fully booked last. Covers S-5 AC1 and S-5 AC3.
- HMS-23 REQ-13: the name links to `/doctors/11?date=2026-11-28` when the dashboard's today is 2026-11-28 (C16). Covers S-6 AC1 and S-6 AC2.
- HMS-23 REQ-17: even if a response carried a phone and an email on a row, the page renders neither. Covers S-2 AC3.
- HMS-23 REQ-16: changing the Period to Last 30 days reloads the tiles and keeps the same rows. Covers S-7 AC1.
- HMS-23 REQ-18: with on-duty rows present, the tiles, their links and the Next appointments heading are unchanged. Covers S-7 AC2 and S-7 AC3.

### Existing tests that still apply
- api/test/dashboard.test.ts: DSH-1..6, DSH-4 and DSH-9 still check the totals, the next list and the absence of contact details. Covers S-7 AC2 and S-2 AC3.
- web/src/__tests__/DoctorDetailPage.test.tsx: "HMS-9 REQ-8: ?date= opens the day schedule on that date" checks that the link target opens the day schedule on the given day. Covers S-6 AC1.

## Known
- S-1 to S-7: every acceptance criterion is covered by the tests listed above.
- TP-1: the API tests are in a new file, `api/test/dashboard-on-duty.test.ts`, named like the existing `dashboard-no-shows.test.ts` (C19). Trade-off: a separate file with one database per test keeps scenarios isolated, at the cost of a little more setup time than extending `dashboard.test.ts`.
- TP-2: the web tests extend the existing `DashboardPage.test.tsx` (C18) and reuse its `DATA` fixture.

## Assumed
None.

## Unknown
- U-1 (non-blocking): there is no test for an Active doctor whose block today is too short to hold one slot. The code leaves such a doctor out (C8). The product owner has not yet decided whether that is right (runs/run_01M8EC1C803452B4P6VTAB10M7/open-questions.md#q-01). Once Q-01 is answered, add a test for that answer.
- U-2 (non-blocking): the API tests need PostgreSQL 16 or `HMS_TEST_DATABASE_URL`, and are skipped without either. Question for whoever reads the results: did the verifier (H3) run them, or skip them?

## Open questions
None new. The run's existing Q-01 (non-blocking, for the product owner) is still open. U-1 depends on it.
