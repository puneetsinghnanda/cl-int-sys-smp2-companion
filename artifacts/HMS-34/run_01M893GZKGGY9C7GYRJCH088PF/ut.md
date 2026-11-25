---
schema: ut-result.v1
run_id: run_01M893GZKGGY9C7GYRJCH088PF
step: ut
attempt: 1
skill: unit-test@1.4.0
inputs:
  - path: artifacts/HMS-34/run_01M893GZKGGY9C7GYRJCH088PF/impl.md
    sha256: fbf0c4c88fe15ebf38cffe9d865138ebcca27be34da2894740e6333efe700a43
  - path: artifacts/HMS-34/run_01M893GZKGGY9C7GYRJCH088PF/split.md
    sha256: a7155fb4e16dcf12bf1ec55dc77f49a05fccbeef9c9084fb822e43d519892151
  - path: context/test-strategy.md
    sha256: f4bbc9d5d63dfc433c9fe466a7aa71a4fad0147baafe1f47db0c06e117ce6cba
  - path: context/generated/test-layout.json
    sha256: c0752ab8543e23547679dbe34f6381f656d36b0eb5506efb2a56f94dcb729cd6
pack_sha:
  sample-system: 7681f87749883d71abb4011d118366ca1242e1cc
claims:
  - id: C1
    text: "API tests use Vitest and `Supertest` against a throwaway PostgreSQL 16 with a fixed clock, so the new API tests do the same."
    cites: ["context/test-strategy.md#layers"]
  - id: C2
    text: "Web tests use Vitest with `jsdom` and Testing Library, so the new page tests do the same."
    cites: ["context/test-strategy.md#layers"]
  - id: C3
    text: "Without PostgreSQL 16 binaries or `HMS_TEST_DATABASE_URL` the database tests are skipped."
    cites: ["context/test-strategy.md#database-for-tests"]
  - id: C4
    text: "Every requirement has a test whose name `starts with its ID`, so each new test name starts with HMS-34 and its REQ id."
    cites: ["context/test-strategy.md#requirement-ids"]
  - id: C5
    text: "The existing appointment API tests live in `api/test/appointments.test.ts`; the new API file sits next to it."
    cites: ["context/generated/test-layout.json#test:sample-system:api/test/appointments.test.ts"]
  - id: C6
    text: "Web page tests live in `web/src/__tests__/DoctorDetailPage.test.tsx` and its neighbours; the new page tests sit there too."
    cites: ["context/generated/test-layout.json#test:sample-system:web/src/__tests__/DoctorDetailPage.test.tsx"]
  - id: C7
    text: "The Stale job runs `hourly, actor system`; the job tests check both."
    cites: ["artifacts/HMS-34/run_01M893GZKGGY9C7GYRJCH088PF/impl.md#claim:C5"]
  - id: C8
    text: "AppointmentRepo gains `markStale`; the API tests drive it through AppointmentService.markStale."
    cites: ["artifacts/HMS-34/run_01M893GZKGGY9C7GYRJCH088PF/impl.md#claim:C3"]
  - id: C9
    text: "AppointmentService's `scheduled() accepts STALE`; the S-3 and S-4 tests complete, mark No-show, cancel and reschedule a Stale appointment."
    cites: ["artifacts/HMS-34/run_01M893GZKGGY9C7GYRJCH088PF/impl.md#claim:C4"]
  - id: C10
    text: "`AppointmentDetailPage` shows actions for STALE and starts the picker on today; the page tests check both."
    cites: ["artifacts/HMS-34/run_01M893GZKGGY9C7GYRJCH088PF/impl.md#claim:C9"]
  - id: C11
    text: "`STATUS_LABEL` gains STALE 'Stale'; the list tests check the badge and the filter option."
    cites: ["artifacts/HMS-34/run_01M893GZKGGY9C7GYRJCH088PF/impl.md#claim:C8"]
  - id: C12
    text: "The 24h cutoff uses `DAY_MS`, so the boundary tests sit one minute either side of 24h."
    cites: ["sample-system:api/src/lib/time.ts#L9"]
  - id: C13
    text: "The doctor page lists only appointments where `a.status = 'SCHEDULED'`, so S-7 AC2 checks the Stale one is absent."
    cites: ["sample-system:api/src/repositories/doctorRepo.ts#L160"]
  - id: C14
    text: "S-1 AC1 runs the job at `10:01 on 4 December` for an appointment at 10:00 on 3 December; the API tests use those times."
    cites: ["artifacts/HMS-34/run_01M893GZKGGY9C7GYRJCH088PF/split.md#item:S-1"]
  - id: C15
    text: "The S-4 tests reschedule to `10:00 on 6 December`, a Sunday, so the test adds a doctor with slots every day."
    cites: ["artifacts/HMS-34/run_01M893GZKGGY9C7GYRJCH088PF/split.md#item:S-4"]
  - id: C16
    text: "`The verifier skips coverage`, so this step reports no coverage."
    cites: ["context/test-strategy.md#gaps"]
trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10, REQ-11, REQ-12, REQ-13, REQ-15]
known: [S-1, S-2, S-3, S-4, S-5, S-6, S-7]
assumed: [TP-1, TP-2]
unknown: []
questions: []
code:
  - repo: sample-system
    base_sha: 5c75aad92008432fe2b1015de9d0afad8a90948b
    head_sha: 0785af2e72880aa707921f660495acfe84e6676d
    diff_sha256: 0b5d97fc847c1b4b4ac2011e06d9ca4a9b5f8dc007cd06f673ab444f2d174b7f
    files:
      - api/test/appointments-stale.test.ts
      - api/test/stale-job.test.ts
      - web/src/__tests__/AppointmentDetailPage.test.tsx
      - web/src/__tests__/AppointmentsPage.test.tsx
---
# Unit tests: HMS-34

## Summary
New tests cover the Stale change from impl C1 to C10:
- The job and its timer (C7, C8).
- The 24h boundary (C12, C14).
- Staff actions on a Stale appointment (C9).
- The list, filter, appointment page, patient page, doctor page and dashboard (C10, C11, C13).

The API tests follow the strategy: Vitest and Supertest on a throwaway database with a fixed clock (C1). Without a database they are skipped (C3). The web tests use jsdom and Testing Library (C2). Every test name starts with HMS-34 and its REQ id (C4). The files sit next to the existing tests (C5, C6).

Each test in api/test/appointments-stale.test.ts gets its own fresh database. The job marks every overdue appointment, so a shared database would let one test's leftovers change another's counts. No production code was changed.

No test results or coverage are reported here. tsl records the tests in checks.json when it runs the verifier (H3). The verifier skips coverage (C16).

## Tests
- api/test/appointments-stale.test.ts (API, database):
  - S-1 / REQ-1, REQ-2, REQ-3: the job marks an appointment Stale one minute past 24h after its start, with a STALE history event by system. One minute before 24h it stays Scheduled, and so does a future appointment. Exactly 24h counts as Stale (TP-1). Several overdue appointments are all marked, each with one event, and a second run changes nothing.
  - S-2 / REQ-4, REQ-5, REQ-6: an appointment marked Completed, rescheduled, cancelled or marked No-show at 09:59 on 4 December is not Stale when the job runs at 10:01.
  - S-3 / REQ-7, REQ-8, REQ-9: a Stale appointment can be marked Completed or No-show, or cancelled with a reason, which it then shows. Error paths:
    - A cancel without a reason is 400 and the appointment stays Stale.
    - Once Completed, a former Stale appointment can no longer change (409).
    - An unknown id is 404.
  - S-4 / REQ-10, REQ-15: a Stale appointment rescheduled to 10:00 on 6 December becomes Scheduled there, with the move in its history. The job at 10:01 on 5 December leaves it Scheduled, and it becomes Stale again 24h after its new start (TP-2). Error paths: a past time is 400, a taken slot is 409, and in both cases the appointment stays Stale at its old time.
  - S-5 / REQ-11, REQ-12, REQ-13: the list and the appointment both carry STALE, and the status=STALE filter lists only the Stale one. An unknown status is still a 400 field error. The dashboard keeps the same seven tiles, with equal counts before and after the job.
  - S-7 / REQ-11: the patient page lists the Stale one as STALE and the other as SCHEDULED. The doctor page lists only the upcoming Scheduled one (C13).
- api/test/stale-job.test.ts (API, no database, fake timers):
  - REQ-1: startStaleJob runs at start and then every hour, as actor system (C7), and the stop function ends the runs.
  - Error paths: a failed run logs only "[stale-job]" and the message, and the next hour still runs. A thrown non-Error value is logged as text.
- web/src/__tests__/AppointmentDetailPage.test.tsx (web):
  - REQ-11 (S-5 AC2): the page shows the badge Stale with class badge-stale.
  - REQ-7 to REQ-10: a Stale appointment keeps the Reschedule, Cancel, Mark completed and Mark no-show actions. Mark completed posts and then shows the appointment closed. A refused close shows the error and keeps the actions.
  - Reschedule edge: for a Stale appointment the picker starts on today and asks for today's slots. For a future appointment it starts on that appointment's own day.
  - The history shows "Marked stale" by system. A Completed appointment still has no actions.
- web/src/__tests__/AppointmentsPage.test.tsx (web):
  - REQ-11 (S-5 AC1): the list shows Stale and Scheduled per row.
  - REQ-12 (S-5 AC3): the Status filter offers Stale with the value STALE and requests status=STALE.
  - A failed list shows the error. StatusBadge renders STALE as "Stale".

## Known
- S-1: covered by the S-1 API tests and the job timer tests (C7, C8, C12, C14).
- S-2: covered by the four S-2 API tests, each acting at 09:59 on 4 December.
- S-3: covered by the S-3 API tests and the page action tests (C9, C10).
- S-4: covered by the S-4 API tests on a doctor with Sunday slots (C15) and the picker tests (C10).
- S-5: covered by the S-5 API tests and both web files (C11).
- S-6: no unit test. The glossary entry is made in the context pack, outside the product diff (impl C11).
- S-7: covered by the two S-7 API tests (C13). The patient page uses the same AppointmentTable as the list tests.

## Assumed
- TP-1: exactly 24h after its start, an appointment counts as Stale. Basis: the rule is "by 24h after its start time" (C14), and the cutoff is the clock minus `DAY_MS` (C12). Trade-off: if the product wanted a strictly later cutoff, only this edge test changes.
- TP-2: a rescheduled appointment that is left open becomes Stale again 24h after its new start. Basis: after a reschedule it is Scheduled again (C9), and S-1's rule applies to every Scheduled appointment (C14). Trade-off: this edge test changes only if the product exempts appointments that were once Stale.

## Unknown
None.

## Open questions
None.
