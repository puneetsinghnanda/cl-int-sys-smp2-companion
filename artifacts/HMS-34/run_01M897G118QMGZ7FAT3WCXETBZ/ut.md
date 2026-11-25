---
schema: ut-result.v1
run_id: run_01M897G118QMGZ7FAT3WCXETBZ
step: ut
attempt: 1
skill: unit-test@1.4.0
inputs:
  - path: artifacts/HMS-34/run_01M897G118QMGZ7FAT3WCXETBZ/impl.md
    sha256: cb0d95bc7f6acc19c5972731cefe7bd1eeddb4d512ad74a15273a44f7ba4ec27
  - path: artifacts/HMS-34/run_01M897G118QMGZ7FAT3WCXETBZ/split.md
    sha256: cb0b1c68ceef02d6fb6550506b35cd99814cb26aded4e14a0ff8670de9a0a06a
  - path: context/test-strategy.md
    sha256: f4bbc9d5d63dfc433c9fe466a7aa71a4fad0147baafe1f47db0c06e117ce6cba
  - path: context/generated/test-layout.json
    sha256: c0752ab8543e23547679dbe34f6381f656d36b0eb5506efb2a56f94dcb729cd6
pack_sha:
  sample-system: 7681f87749883d71abb4011d118366ca1242e1cc
claims:
  - id: C1
    text: "API tests use Vitest and `Supertest` against a throwaway PostgreSQL with a fixed clock."
    cites: ["context/test-strategy.md#layers"]
  - id: C2
    text: "Web tests use Vitest with jsdom and `Testing Library`."
    cites: ["context/test-strategy.md#layers"]
  - id: C3
    text: "Every requirement has a test whose name `starts with its ID`, so each new test name starts with HMS-34 and a REQ id."
    cites: ["context/test-strategy.md#requirement-ids"]
  - id: C4
    text: "Database tests run against a temporary cluster or `HMS_TEST_DATABASE_URL` and are skipped without either."
    cites: ["context/test-strategy.md#database-for-tests"]
  - id: C5
    text: "The existing appointment API tests live in `api/test/appointments.test.ts`; the new Stale API tests sit beside it in their own file and database, as the HMS-3 tests do."
    cites: ["context/generated/test-layout.json#test:sample-system:api/test/appointments.test.ts"]
  - id: C6
    text: "The existing formatting tests live in `web/src/__tests__/format.test.ts`, which the new STATUS_LABEL test extends."
    cites: ["context/generated/test-layout.json#test:sample-system:web/src/__tests__/format.test.ts"]
  - id: C7
    text: "Before this step `other pages have no web tests`, including the appointment page, so a new page test file is added."
    cites: ["context/test-strategy.md#gaps"]
  - id: C8
    text: "The test fixture fixes now at `2026-10-19T02:30:00Z` (Monday 08:00 in the hospital zone), so the tests use Monday to Friday of that week instead of the stories' December dates."
    cites: ["sample-system:api/test/helpers.ts#L10"]
  - id: C9
    text: "Fixture doctor 1 has `15-minute slots` on weekdays, which the tests book."
    cites: ["sample-system:api/test/helpers.ts#L23"]
  - id: C10
    text: "The tests call the service through `app.locals.ctx`, where createApp keeps the services."
    cites: ["sample-system:api/src/app.ts#L20"]
  - id: C11
    text: "The job runs every `60 * MINUTE`."
    cites: ["sample-system:api/src/jobs/staleJob.ts#L5"]
  - id: C12
    text: "The job records its changes as `STALE_JOB_ACTOR`, the value system."
    cites: ["sample-system:api/src/jobs/staleJob.ts#L6"]
  - id: C13
    text: "The job runs hourly, as answered: `Every hour`."
    cites: ["runs/run_01M897G118QMGZ7FAT3WCXETBZ/ide-answers.md#a-08"]
  - id: C14
    text: "The job's actor is `system`, as answered."
    cites: ["runs/run_01M897G118QMGZ7FAT3WCXETBZ/ide-answers.md#a-09"]
  - id: C15
    text: "The service's cutoff is now minus `DAY_MS`."
    cites: ["sample-system:api/src/services/appointmentService.ts#L134"]
  - id: C16
    text: "The repository marks only rows with `status = 'SCHEDULED' AND starts_at <= $1`, so closed, moved and future appointments are untouched."
    cites: ["sample-system:api/src/repositories/appointmentRepo.ts#L168"]
  - id: C17
    text: "A reschedule sets `status = 'SCHEDULED'`."
    cites: ["sample-system:api/src/repositories/appointmentRepo.ts#L158"]
  - id: C18
    text: "Staff actions accept a Stale appointment because the check allows `current.status !== 'STALE'`."
    cites: ["sample-system:api/src/services/appointmentService.ts#L147"]
  - id: C19
    text: "Staff can still `close or reschedule` a Stale appointment, as answered."
    cites: ["runs/run_01M897G118QMGZ7FAT3WCXETBZ/ide-answers.md#a-03"]
  - id: C20
    text: "The web label is `STALE: 'Stale'`."
    cites: ["sample-system:web/src/lib/format.ts#L41"]
  - id: C21
    text: "The appointment page labels the job's history event `Marked stale`."
    cites: ["sample-system:web/src/pages/AppointmentDetailPage.tsx#L19"]
  - id: C22
    text: "The reschedule picker starts on `ownDay < today ? today : ownDay`."
    cites: ["sample-system:web/src/pages/AppointmentDetailPage.tsx#L110"]
  - id: C23
    text: "The doctor page lists only appointments where `a.status = 'SCHEDULED'`."
    cites: ["sample-system:api/src/repositories/doctorRepo.ts#L160"]
  - id: C24
    text: "The glossary entry for S-6 is made in the `context pack` and is not part of the code diff."
    cites: ["artifacts/HMS-34/run_01M897G118QMGZ7FAT3WCXETBZ/impl.md#claim:C11"]
  - id: C25
    text: "`The verifier skips coverage`, so this artifact reports none."
    cites: ["context/test-strategy.md#gaps"]
trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10, REQ-11, REQ-12, REQ-13, REQ-14, REQ-16]
known: [S-1, S-2, S-3, S-4, S-5, S-7]
assumed: []
unknown: [{id: U-1, blocking: false}]
questions: []
code:
  - repo: sample-system
    base_sha: b84fa8f4589c1f8384f7d6f68cea7a09a88c1940
    head_sha: 4dc17c613f6fbf8859491cc340c01fcc77dffcf2
    diff_sha256: da7c1f2fcc63b07ae1c64193d63125e7a6d20a3b1e0539910044f0aeb713825e
    files:
      - api/test/appointments-stale.test.ts
      - api/test/staleJob.test.ts
      - web/src/__tests__/AppointmentDetailPage.test.tsx
      - web/src/__tests__/format.test.ts
---
# Unit tests: HMS-34

## Summary
Tests cover the Stale status end to end at the unit and API level: the hourly job (C11 to C14), the service and repository rule that marks Scheduled appointments 24h past their start (C15, C16), staff actions on a Stale appointment (C17 to C19), the list filter, patient page, doctor page and dashboard (C23), the web label (C20) and the appointment page (C21, C22). Each changed function has a normal-path test and an error or edge test. Tests follow the strategy's layers and naming (C1 to C4), beside the existing files (C5 to C7). Dates come from the fixed fixture week (C8, C9). Services are called through the app context (C10). No production code was changed. Test and coverage results are not in data: tsl records them in checks.json when it runs the verifier (H3), and the verifier skips coverage (C25).

## Tests
- api/test/staleJob.test.ts (new, no database): the job runs at start and then every hour with actor system; the stop function ends the runs; a failed run logs only the error message and the next run still happens.
- api/test/appointments-stale.test.ts (new, own database): one minute before 24h nothing changes (S-1 AC2); staff complete, reschedule, cancel or mark No-show within the day and none becomes Stale (S-2 AC1 to AC4); one minute past 24h the job marks Stale with a STALE history event by system (S-1 AC1); a future appointment stays Scheduled (S-1 AC3); a second run changes nothing; the list shows and filters by STALE (S-5 AC1, AC3) and an unknown status is still a 400 field error; the patient page shows Stale next to Scheduled (S-7 AC1); the doctor page lists the Scheduled one and not the Stale one (S-7 AC2); the dashboard has no Stale tile (S-5 AC4); staff complete, mark No-show and cancel Stale appointments (S-3); cancel without a reason is still 400; a closed one is still 409; a move into the past is 400; reschedule makes a Stale appointment Scheduled at the new time with the history kept (S-4 AC1); the job a day before the new time leaves it Scheduled (S-4 AC2).
- web/src/__tests__/AppointmentDetailPage.test.tsx (new): the page badge shows Stale with class badge-stale (S-5 AC2); all four staff actions are enabled for Stale; the history shows "Marked stale" by system; the reschedule picker of a past Stale appointment starts on today; a Completed appointment is still closed with no actions.
- web/src/__tests__/format.test.ts (extended): STATUS_LABEL names STALE "Stale" and keeps the other labels.

## Known
- S-1: covered by the API tests at 09:59, 10:01 and on a future day, and the job tests (C11 to C16).
- S-2: covered by the four within-the-day cases (C16).
- S-3: covered by complete, No-show and cancel on Stale appointments plus the error paths (C18, C19).
- S-4: covered by the reschedule and the later job run (C17).
- S-5: covered by the list filter, the dashboard check, the page badge and the label test (C20, C21).
- S-7: covered by the patient and doctor page checks (C23).

## Assumed
None.

## Unknown
- U-1 (non-blocking): S-6 (REQ-15), the glossary entry, has no code to test because it is made in the context pack (C24). Should the glossary owner confirm the entry is in place before the PR?

## Open questions
None.
