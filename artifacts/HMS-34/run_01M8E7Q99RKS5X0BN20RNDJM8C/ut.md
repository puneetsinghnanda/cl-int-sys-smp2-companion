---
schema: ut-result.v1
run_id: run_01M8E7Q99RKS5X0BN20RNDJM8C
step: ut
attempt: 1
skill: unit-test@1.4.0
inputs:
  - path: artifacts/HMS-34/run_01M8E7Q99RKS5X0BN20RNDJM8C/impl.md
    sha256: 9c7839882c5643e20ac9cee5bc8a726a3b83b90d618288ee399d2e821e07a428
  - path: artifacts/HMS-34/run_01M8E7Q99RKS5X0BN20RNDJM8C/split.md
    sha256: de964b65dc63706622cc2da9b738e4978b15ad686429c50dcbb5d068f9d8336e
  - path: context/test-strategy.md
    sha256: f4bbc9d5d63dfc433c9fe466a7aa71a4fad0147baafe1f47db0c06e117ce6cba
  - path: context/generated/test-layout.json
    sha256: c0752ab8543e23547679dbe34f6381f656d36b0eb5506efb2a56f94dcb729cd6
pack_sha:
  sample-system: 7681f87749883d71abb4011d118366ca1242e1cc
claims:
  - id: C1
    text: "API tests use `Vitest and Supertest` against a throwaway PostgreSQL with a fixed clock; web tests use Vitest with jsdom and Testing Library."
    cites: ["context/test-strategy.md#layers"]
  - id: C2
    text: "Every requirement has a test whose name `starts with its ID`, so the new tests are named HMS-34 REQ-n."
    cites: ["context/test-strategy.md#requirement-ids"]
  - id: C3
    text: "Without PostgreSQL 16 binaries or a test database URL, `database tests are skipped`."
    cites: ["context/test-strategy.md#database-for-tests"]
  - id: C4
    text: "Each API test file gets `A fresh, migrated, empty database`, so the new migration runs before the Stale tests store STALE."
    cites: ["sample-system:db/testing/test-db.ts#L17"]
  - id: C5
    text: "The fixed test clock is `2026-10-19T02:30:00Z` (Monday 08:00 in Asia/Kolkata)."
    cites: ["sample-system:api/test/helpers.ts#L10"]
  - id: C6
    text: "Fixture doctor 1 has `slotMinutes: 15` and works Monday to Friday 09:00 to 12:00."
    cites: ["sample-system:api/test/helpers.ts#L37"]
  - id: C7
    text: "The service sweep uses the cutoff `this.clock().getTime() - DAY_MS`."
    cites: ["sample-system:api/src/services/appointmentService.ts#L136"]
  - id: C8
    text: "The sweep's actor is `SYSTEM_ACTOR`, the string system."
    cites: ["sample-system:api/src/services/appointmentService.ts#L16"]
  - id: C9
    text: "The repository marks rows `WHERE status = 'SCHEDULED' AND starts_at <= $1` and writes one STALE event per row."
    cites: ["sample-system:api/src/repositories/appointmentRepo.ts#L168"]
  - id: C10
    text: "A moved appointment is set to `status = 'SCHEDULED'`."
    cites: ["sample-system:api/src/repositories/appointmentRepo.ts#L158"]
  - id: C11
    text: "The change guard accepts `current.status !== 'STALE'` alongside Scheduled."
    cites: ["sample-system:api/src/services/appointmentService.ts#L145"]
  - id: C12
    text: "The migration re-adds `appointment_status_check` with STALE."
    cites: ["sample-system:db/migrations/20261127T104500__add_stale_status.sql#L3"]
  - id: C13
    text: "The web status label for STALE is `STALE: 'Stale'`."
    cites: ["sample-system:web/src/lib/format.ts#L41"]
  - id: C14
    text: "The appointment page labels the STALE history event `Marked stale`."
    cites: ["sample-system:web/src/pages/AppointmentDetailPage.tsx#L19"]
  - id: C15
    text: "The page shows its actions when `a.status === 'STALE'`."
    cites: ["sample-system:web/src/pages/AppointmentDetailPage.tsx#L45"]
  - id: C16
    text: "The appointment list's status filter lists every `STATUS_LABEL` entry."
    cites: ["sample-system:web/src/pages/AppointmentsPage.tsx#L34"]
  - id: C17
    text: "The server starts the hourly sweep with `setInterval(sweepStale, STALE_SWEEP_MS)` inside the listen callback of a module that starts the server on import."
    cites: ["sample-system:api/src/server.ts#L25"]
  - id: C18
    text: "Stale starts `24h after start`."
    cites: ["runs/run_01M8E7Q99RKS5X0BN20RNDJM8C/ide-answers.md#a-01"]
  - id: C19
    text: "A Stale appointment `Can be closed or rescheduled`."
    cites: ["runs/run_01M8E7Q99RKS5X0BN20RNDJM8C/ide-answers.md#a-02"]
  - id: C20
    text: "The change to Stale is recorded as `Event, actor system`."
    cites: ["runs/run_01M8E7Q99RKS5X0BN20RNDJM8C/ide-answers.md#a-03"]
  - id: C21
    text: "Staff see Stale in `Lists and page, with filter`."
    cites: ["runs/run_01M8E7Q99RKS5X0BN20RNDJM8C/ide-answers.md#a-04"]
  - id: C22
    text: "A rescheduled Stale appointment is `Scheduled again`."
    cites: ["runs/run_01M8E7Q99RKS5X0BN20RNDJM8C/ide-answers.md#a-06"]
  - id: C23
    text: "Staff may mark a Stale appointment Cancelled: `Yes, Cancelled allowed`."
    cites: ["runs/run_01M8E7Q99RKS5X0BN20RNDJM8C/ide-answers.md#a-07"]
  - id: C24
    text: "The glossary is not code: `Tech lead edits it when the PR is merged`."
    cites: ["artifacts/HMS-34/run_01M8E7Q99RKS5X0BN20RNDJM8C/impl.md#claim:C6"]
  - id: C25
    text: "`The verifier skips coverage`, so no coverage is reported."
    cites: ["context/test-strategy.md#gaps"]
trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10, REQ-11, REQ-12]
known: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10, REQ-11, REQ-12]
assumed: []
unknown:
  - {id: U-1, blocking: false}
questions:
  - id: Q-01
    blocking: false
    ask: tech lead
    text: "The hourly timer in api/src/server.ts has no unit test, because the module starts the server when it is imported. Accept that the sweep is covered through AppointmentService.markStale, or extract the timer start into a testable function in a later change?"
    why: "Every changed function should have a normal and an error test; this step may not change production code to make server.ts testable."
    evidence: ["sample-system:api/src/server.ts#L25", "sample-system:api/src/services/appointmentService.ts#L136"]
    options: ["Accept: markStale tests cover the sweep", "Extract a startStaleSweep function in a follow-up ticket"]
    default: "Accept: markStale tests cover the sweep"
code:
  - repo: sample-system
    base_sha: a395032233a0d242f4dbf9b53d8a1fc94933238b
    head_sha: e5ffdad2330cb6828378090dffa57d6238211814
    diff_sha256: aa3b95157cf61234f13eb7ac9b8ffb4773a4b86b3707f2512e63c31164e43d01
    files:
      - api/test/appointments-stale.test.ts
      - web/src/__tests__/AppointmentDetailPage.test.tsx
      - web/src/__tests__/AppointmentsPage.test.tsx
      - web/src/__tests__/format.test.ts
---
# Unit tests: HMS-34

## Summary
New tests cover the Stale status from start to finish. The API tests use Vitest and Supertest with a fixed clock (C1, C5), and each test name starts with its requirement ID (C2). They call `AppointmentService.markStale()` directly with the test clock and check the 24h cutoff (C7, C18). They check the repository's Scheduled-only update (C9), the `system` actor (C8, C20), the guard that lets staff close or reschedule a Stale appointment (C11, C19, C23), and the move back to Scheduled (C10, C22). The web tests check the Stale label, the badge, the list filter, the page actions and the `Marked stale` history label (C13 to C16, C21). The API file gets its own migrated database (C4), so it also runs the new migration (C12). Without PostgreSQL the database tests are skipped (C3).

Defaults: the stories' 1 and 2 December dates are moved to the fixture's Monday 19 and Tuesday 20 October, because the fixed clock and doctor 1's 09:00 to 12:00 hours are set there (C5, C6). Trade-off: the dates differ from the stories, but the 24h rule does not depend on the date. S-1 AC3 uses a 01:00 local start, which doctor 1 cannot be booked at (C6). So its edge, a new local day before 24h have passed, is tested at 00:30 local on the next day.

No test results or coverage are reported here. tsl records them in checks.json when it runs the verifier (H3). Coverage is skipped anyway (C25).

## Tests
- api/test/appointments-stale.test.ts: no appointment is marked before 24h after start, including at 00:30 on the next local day (S-1 AC2, AC3). Exactly 24h after start the appointment becomes Stale (S-1 AC1). Cancelled, No-show, Completed and rescheduled appointments are never marked (S-1 AC4, S-2). One STALE history entry with its time is written, with actor system, and a second sweep does not add another (S-3). The status filter returns only Stale appointments, and an unknown status still gets 400 (S-6 AC3). Staff can mark a Stale appointment Completed or No-show (S-4 AC1, AC2). Staff can cancel it with a stored reason, a cancel without a reason gets 400, and a closed appointment gets 409 (S-4 AC3). A reschedule into the past gets 400. A reschedule to a free slot makes the appointment Scheduled again, and a booking in that slot is then refused as an overlap (S-5).
- web/src/__tests__/format.test.ts: STATUS_LABEL maps STALE to Stale and keeps the other labels (REQ-10).
- web/src/__tests__/AppointmentDetailPage.test.tsx: the page shows the Stale badge (S-6 AC2) and enables reschedule, cancel, complete and no-show. The history shows Marked stale by system. In the edge case, a Completed appointment still shows as closed, with no actions.
- web/src/__tests__/AppointmentsPage.test.tsx: a Stale row shows the Stale badge (S-6 AC1). The status filter offers Stale and requests status=STALE (S-6 AC3). In the edge case, a Stale filter with no matches shows no rows.

## Known
- REQ-1: STALE is stored and returned (C4, C12), tested in appointments-stale.test.ts.
- REQ-2: the 24h cutoff and its boundary are tested (C7, C18).
- REQ-3, REQ-4: appointments that are completed or rescheduled in time stay unmarked (C9).
- REQ-5: Cancelled and No-show appointments stay unmarked (C9).
- REQ-6: a Stale appointment can be completed, marked No-show or cancelled, and is closed after that (C11, C19, C23).
- REQ-7: a rescheduled appointment is Scheduled again and counts for overlap (C10, C22).
- REQ-8, REQ-9: one STALE event per appointment, with actor system (C8, C20).
- REQ-10: the Stale label and badge show in the list and on the page (C13, C21).
- REQ-11: the Stale filter is tested in the API and the web list (C16, C21).
- REQ-12: the glossary is not code, so it has no unit test. The tech lead edits it at merge (C24).

## Assumed
None.

## Unknown
- U-1: the hourly timer in api/src/server.ts has no unit test, because the module starts the server on import (C17). This step may not change production code. The sweep logic it calls is tested through markStale. See Q-01.

## Open questions
- Q-01 (non-blocking, tech lead): accept that the markStale tests cover the sweep, or extract the timer start into a testable function in a follow-up change? Default: accept.
