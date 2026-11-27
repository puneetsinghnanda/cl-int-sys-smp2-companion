---
schema: ut-result.v1
run_id: run_01M89M2JW0X6H7Q7W6EJQ53R4B
step: ut
attempt: 1
skill: unit-test@1.4.0
inputs:
  - path: artifacts/HMS-34/run_01M89M2JW0X6H7Q7W6EJQ53R4B/impl.md
    sha256: 2d835da3fbc0460fe9ad19bb5c4326b5b6575c1874e8b329d650af3e92a390ae
  - path: artifacts/HMS-34/run_01M89M2JW0X6H7Q7W6EJQ53R4B/split.md
    sha256: ffdc205028446dc36d5b0a750fdfe01f2de2f7347891a727faae856bcb6899c4
  - path: context/test-strategy.md
    sha256: f4bbc9d5d63dfc433c9fe466a7aa71a4fad0147baafe1f47db0c06e117ce6cba
  - path: context/generated/test-layout.json
    sha256: c0752ab8543e23547679dbe34f6381f656d36b0eb5506efb2a56f94dcb729cd6
pack_sha:
  sample-system: 7681f87749883d71abb4011d118366ca1242e1cc
claims:
  - id: C1
    text: "API tests use `Vitest and Supertest` against a throwaway PostgreSQL 16 with a fixed clock."
    cites: ["context/test-strategy.md#layers"]
  - id: C2
    text: "Without a test database, `database tests are skipped`."
    cites: ["context/test-strategy.md#database-for-tests"]
  - id: C3
    text: "The verifier measures no coverage: `The verifier skips coverage`."
    cites: ["context/test-strategy.md#gaps"]
  - id: C4
    text: "Test names start with the requirement ID: `Every PRD requirement has at least one test whose name starts with its ID`."
    cites: ["context/test-strategy.md#requirement-ids"]
  - id: C5
    text: "The API test helpers fix now at `2026-10-19T02:30:00Z`, Monday 08:00 in Asia/Kolkata."
    cites: ["sample-system:api/test/helpers.ts#L10"]
  - id: C6
    text: "The existing appointment tests live in `api/test/appointments.test.ts`."
    cites: ["context/generated/test-layout.json#test:sample-system:api/test/appointments.test.ts"]
  - id: C7
    text: "The existing formatting tests live in `web/src/__tests__/format.test.ts`."
    cites: ["context/generated/test-layout.json#test:sample-system:web/src/__tests__/format.test.ts"]
  - id: C8
    text: "The change adds `markStale` to AppointmentService and lets its actions accept STALE."
    cites: ["artifacts/HMS-34/run_01M89M2JW0X6H7Q7W6EJQ53R4B/impl.md#claim:C4"]
  - id: C9
    text: "The job interval is `STALE_JOB_INTERVAL_MS = 60 * MINUTE`."
    cites: ["sample-system:api/src/jobs/staleJob.ts#L5"]
trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10, REQ-11, REQ-12, REQ-13, REQ-16]
known: [S-1, S-2, S-3, S-4, S-5]
assumed: []
unknown: []
questions: []
code:
  - repo: sample-system
    base_sha: 303a76d72faecd80a0a849d2fb943b70d26129ea
    head_sha: 2f089ad20d228914acbaa4e09ce7fa2d8d49ffd4
    diff_sha256: da04e4f115ee23ed0b3789f4c232ea059208d4da42a3483472b4a30e9ded321d
    files:
      - api/test/appointments-stale.test.ts
      - api/test/staleJob.test.ts
      - web/src/__tests__/AppointmentDetailPage.test.tsx
      - web/src/__tests__/format.test.ts
---
# Unit tests: HMS-34

## Summary
The tests cover the Stale job, the service and repository changes behind it, the staff actions on a
Stale appointment, the list filter and the web appointment page. They use the project's Vitest set-up
(C1) and name each test with HMS-34 and its REQ ids (C4). The job tests need no database and use fake
timers. The API tests run against a test database with the fixed clock from the helpers (C5), and
are skipped when the verifier has none (C2). No test data is real. tsl records the tests run and
passed in checks.json when it runs the verifier (H3); coverage is not measured (C3).

## Tests
- api/test/staleJob.test.ts (new): the job runs once at start with actor system, again every hour (C9) and not before, stops on the stop function, logs only the error message of a failed run and keeps running.
- api/test/appointments-stale.test.ts (new, database): S-1 one minute before and after 24h past the start, and a future appointment untouched; S-2 Completed, Cancelled, No-show and rescheduled within the day are not marked Stale; the STALE history event by system; a second run changes nothing; S-5 the list filter by status STALE and an unknown status still rejected; S-3 complete, no-show and cancel (reason required) on Stale, and closed ones still refused; S-4 reschedule from Stale to a past slot refused, to a future slot Scheduled again, and the job then leaves it Scheduled. It exercises `markStale` (C8) through the app's service.
- web/src/__tests__/AppointmentDetailPage.test.tsx (new): the Stale badge, every action offered on Stale, the "Marked stale" history label by system, the reschedule picker starting on today for a past appointment and on its own day for a future one, and a closed appointment still offering no actions.
- web/src/__tests__/format.test.ts (extended, C7): STATUS_LABEL labels STALE as Stale, after the existing statuses (the list filter's order).

The existing appointment tests (C6) are left as they are; their behaviour does not change.

## Known
- S-1: covered by the API database tests for one minute either side of 24h and for a future appointment.
- S-2: covered by the API database tests for each way an appointment leaves Scheduled within the day.
- S-3: covered by the API database tests and the web page test for the actions.
- S-4: covered by the API database tests for reschedule and by the web test for the picker's start day.
- S-5: covered by the list filter test, the format test and the web badge test.

## Assumed
None.

## Unknown
None.

## Open questions
None.
