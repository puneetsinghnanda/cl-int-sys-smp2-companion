---
schema: ut-result.v1
run_id: run_01M8E1NVV8GFP3FDJ5XQMWQ952
step: ut
attempt: 1
skill: unit-test@1.4.0
inputs:
  - path: artifacts/HMS-34/run_01M8E1NVV8GFP3FDJ5XQMWQ952/impl.md
    sha256: d16f96994ae53c5ec527f12300adf0d58bbbf03ef87b7976c08cea69203ec162
  - path: artifacts/HMS-34/run_01M8E1NVV8GFP3FDJ5XQMWQ952/split.md
    sha256: 75a7d123b0e9c6c2abede2df9d1cedfa4c8cb1c4885b7f3d7d41dbe935908603
  - path: context/test-strategy.md
    sha256: f4bbc9d5d63dfc433c9fe466a7aa71a4fad0147baafe1f47db0c06e117ce6cba
  - path: context/generated/test-layout.json
    sha256: c0752ab8543e23547679dbe34f6381f656d36b0eb5506efb2a56f94dcb729cd6
pack_sha:
  sample-system: 7681f87749883d71abb4011d118366ca1242e1cc
claims:
  - id: C1
    text: "API tests use Vitest and `Supertest` against a throwaway PostgreSQL 16 with a fixed clock."
    cites: ["context/test-strategy.md#layers"]
  - id: C2
    text: "Every requirement has a test whose name starts with its ID, as in `DOC-1`."
    cites: ["context/test-strategy.md#requirement-ids"]
  - id: C3
    text: "Without a PostgreSQL 16 cluster or test database URL, database tests are `skipped`."
    cites: ["context/test-strategy.md#database-for-tests"]
  - id: C4
    text: "The verifier skips `coverage`, so no run reports it."
    cites: ["context/test-strategy.md#gaps"]
  - id: C5
    text: "The existing appointment API tests are in `api/test/appointments.test.ts`."
    cites: ["context/generated/test-layout.json#test:sample-system:api/test/appointments.test.ts"]
  - id: C6
    text: "The existing formatting tests are in `web/src/__tests__/format.test.ts`."
    cites: ["context/generated/test-layout.json#test:sample-system:web/src/__tests__/format.test.ts"]
  - id: C7
    text: "An appointment becomes Stale `24h after start time`."
    cites: ["runs/run_01M8E1NVV8GFP3FDJ5XQMWQ952/ide-answers.md#a-01"]
  - id: C8
    text: "Staff can still act on a Stale appointment: `complete, no-show, cancel, reschedule`."
    cites: ["runs/run_01M8E1NVV8GFP3FDJ5XQMWQ952/ide-answers.md#a-03"]
  - id: C9
    text: "The Stale marking is recorded with actor `system`."
    cites: ["runs/run_01M8E1NVV8GFP3FDJ5XQMWQ952/ide-answers.md#a-04"]
  - id: C10
    text: "A rescheduled Stale appointment is `Scheduled again`."
    cites: ["runs/run_01M8E1NVV8GFP3FDJ5XQMWQ952/ide-answers.md#a-06"]
  - id: C11
    text: "The new status and its history event are both `STALE`."
    cites: ["runs/run_01M8E1NVV8GFP3FDJ5XQMWQ952/ide-answers.md#a-07"]
  - id: C12
    text: "The job entry point is `markStale` on the appointment service."
    cites: ["sample-system:api/src/services/appointmentService.ts#L136"]
  - id: C13
    text: "The repository marks Scheduled rows with `starts_at <= $1` (the cutoff), so exactly 24h counts."
    cites: ["sample-system:api/src/repositories/appointmentRepo.ts#L168"]
  - id: C14
    text: "The open-state guard accepts `'STALE'` as well as Scheduled."
    cites: ["sample-system:api/src/services/appointmentService.ts#L153"]
  - id: C15
    text: "A reschedule sets `status = 'SCHEDULED'`."
    cites: ["sample-system:api/src/repositories/appointmentRepo.ts#L158"]
  - id: C16
    text: "The services, with the injected clock, are reachable in tests through `app.locals.ctx`."
    cites: ["sample-system:api/src/app.ts#L20"]
  - id: C17
    text: "server.ts runs the job with the constant `STALE_JOB_ACTOR`."
    cites: ["sample-system:api/src/server.ts#L10"]
  - id: C18
    text: "The fixed test clock is `2026-10-19T02:30:00Z`."
    cites: ["sample-system:api/test/helpers.ts#L10"]
  - id: C19
    text: "Fixture doctor 1 has `15-minute slots` on weekdays."
    cites: ["sample-system:api/test/helpers.ts#L23"]
  - id: C20
    text: "The web label for the new status is `'Stale'`."
    cites: ["sample-system:web/src/lib/format.ts#L41"]
  - id: C21
    text: "The STALE history event is shown as `Marked stale`."
    cites: ["sample-system:web/src/pages/AppointmentDetailPage.tsx#L19"]
  - id: C22
    text: "The detail page treats `a.status === 'STALE'` as open and keeps its actions."
    cites: ["sample-system:web/src/pages/AppointmentDetailPage.tsx#L46"]
trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-10, REQ-11, REQ-12, REQ-13]
known: [S-1, S-2, S-3, S-4, TP-3]
assumed: [TP-1, TP-2]
unknown:
  - {id: U-1, blocking: false}
  - {id: U-2, blocking: false}
questions:
  - id: Q-01
    blocking: false
    ask: tech lead
    text: "The hourly timer and its clean-up in api/src/server.ts run as side effects when the module loads, so no unit test covers them. Should a later change move the timer wiring into an exported function so it can be tested?"
    why: "Rule 6 asks for a test of every changed function; the job logic is tested through markStale, but the timer start, the error log and clearInterval in shutdown are not."
    evidence: ["sample-system:api/src/server.ts#L10", "sample-system:api/src/app.ts#L20"]
    options: ["Leave as is; markStale is tested", "Extract the timer wiring in a follow-up ticket"]
    default: "Leave as is; markStale is tested"
  - id: Q-02
    blocking: false
    ask: tech lead
    text: "S-5 (REQ-9, the glossary entry for Stale) is a companion-context edit, not product code, so this step has no test for it. Will the tech lead add the entry?"
    why: "The change leaves REQ-9 out of its trace; the stories still list S-5."
    evidence: ["artifacts/HMS-34/run_01M8E1NVV8GFP3FDJ5XQMWQ952/split.md#item:S-5", "runs/run_01M8E1NVV8GFP3FDJ5XQMWQ952/ide-answers.md#a-05"]
    options: ["Tech lead adds the glossary entry", "Track it in a separate ticket"]
    default: "Tech lead adds the glossary entry"
code:
  - repo: sample-system
    base_sha: 5066961fd5ffb9d167455a273f868f9250ddc8bd
    head_sha: ad6d957d249aede4dc9214d8448159730c5dd9d9
    diff_sha256: 5d7cdf413a864bc6a64ef0f500086f349aa501b4b935d31fe5949512802627fc
    files:
      - api/test/appointments-stale.test.ts
      - web/src/__tests__/AppointmentDetailPage.test.tsx
      - web/src/__tests__/format.test.ts
---
# Unit tests: HMS-34

## Summary
New API tests drive the Stale job through `markStale` with the fixed, movable test clock (C1, C12, C16, C18) and check the 24h boundary from the start instant (C7, C13), the STALE event by actor system (C9, C11), that closed or rescheduled-in-time appointments are left alone, that staff can complete, mark no-show, cancel and reschedule a Stale appointment (C8, C14), and that a rescheduled Stale appointment is Scheduled again and later Stale again (C10, C15). Web tests cover the Stale label (C20), the "Marked stale" history line (C21) and the kept actions on the detail page (C22). Test names start with the HMS-34 requirement IDs (C2). Production code is unchanged. The API tests are skipped where no PostgreSQL is available (C3). No verifier result is in data yet, so tests and coverage are not reported here: tsl records them in checks.json when it runs the verifier (H3); coverage is skipped by the verifier anyway (C4).

## Tests
- api/test/appointments-stale.test.ts (new, next to C5):
  - REQ-2 edge (S-1 AC2): one minute before 24h after the start, still Scheduled, no STALE event.
  - REQ-1/REQ-2/REQ-5/REQ-8 (S-1 AC1, AC4): one minute after 24h, the job marks it STALE with a STALE event by system.
  - REQ-2 edge (S-1 AC3): after the local day of the start has ended but before 24h, still Scheduled.
  - REQ-3/REQ-7 (S-2 AC1): a Completed appointment stays Completed with no STALE event.
  - REQ-4 (S-2 AC2): an appointment rescheduled in time stays Scheduled at its new time.
  - REQ-7 (S-2 AC3, AC4): Cancelled and No-show appointments are never marked Stale.
  - REQ-6/REQ-10/REQ-11 (S-3 AC1-AC3): staff complete, mark no-show and cancel (with its reason) Stale appointments.
  - REQ-11 error: cancelling a Stale appointment without a reason is 400 and it stays Stale.
  - REQ-7 error: a Stale appointment closed by staff can no longer be rescheduled or cancelled (409).
  - REQ-12 error: a Stale appointment cannot be rescheduled into the past (400) and stays Stale.
  - REQ-12/REQ-13 (S-4 AC1-AC3): a rescheduled Stale appointment is Scheduled at the new time, untouched by an earlier job run, and Stale again one minute after 24h past its new start.
  - REQ-5 edge: a second job run marks nothing and adds no duplicate STALE event.
  - REQ-1: the list filter accepts status STALE; an unknown status is still 400.
- web/src/__tests__/format.test.ts (extended, C6): REQ-1, the STALE label is "Stale" and the other labels are unchanged.
- web/src/__tests__/AppointmentDetailPage.test.tsx (new): REQ-1 Stale badge; REQ-8 "Marked stale" by system in the history; REQ-6/REQ-10/REQ-11/REQ-12 all four actions enabled for a Stale appointment; REQ-10 Mark completed posts to the complete endpoint; REQ-7 edge, a Completed appointment still shows no actions.

## Known
- S-1: covered by the AC1-AC4 tests in api/test/appointments-stale.test.ts.
- S-2: covered by the AC1-AC4 tests in api/test/appointments-stale.test.ts.
- S-3: covered by the AC1-AC3 test plus the missing-reason and closed-after-Stale error tests, and the web action tests.
- S-4: covered by the AC1-AC3 test plus the past-reschedule error test.
- TP-3: every API test runs on a fresh database with all migrations applied, so writing STALE rows and events also exercises the new CHECK constraints from the migration (the impl's U-2 constraint names fail there if wrong).

## Assumed
- TP-1: the story times (10:00 UTC on 9 March and so on) are not on the fixture's slot grid, so the tests keep each story's relation to the start instant (24h minus or plus one minute, local day ended before 24h, reschedule before the window) on fixture slots in the week of the fixed clock (C18, C19). Trade-off: the exact dates differ from the stories; the boundaries tested are the same.
- TP-2: the job is tested by calling `markStale` with actor system through the app's services (C16, C17), as the hourly timer does, rather than through the timer itself. Basis: the timer is started as a side effect of loading server.ts.

## Unknown
- U-1: the hourly timer, its error log and the clearInterval in shutdown (api/src/server.ts) have no unit test; see Q-01.
- U-2: S-5 (REQ-9, glossary entry) has no test in this step because no product file serves it; see Q-02.

## Open questions
- Q-01 (non-blocking, tech lead): should the timer wiring in server.ts move into an exported function in a follow-up so it can be tested? Default: leave as is; markStale is tested.
- Q-02 (non-blocking, tech lead): will the tech lead add the Stale glossary entry for S-5? Default: tech lead adds the glossary entry.
