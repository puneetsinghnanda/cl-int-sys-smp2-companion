---
schema: change.v1
run_id: run_01M7Q1DS00W5S7BFVX9Y70NPZG
step: impl
attempt: 1
skill: implement@1.3.0
inputs:
  - path: artifacts/HMS-21/run_01M7Q1DS00W5S7BFVX9Y70NPZG/design.md
    sha256: 67fec0fca1d7cdefdee99ee1eeed978a3cefdfdb2b3ca102126f07401d052b77
  - path: artifacts/HMS-21/run_01M7Q1DS00W5S7BFVX9Y70NPZG/split.md
    sha256: b54499b009a5c3dc2f2280aa5d7a1ddbb5c87d032fedcf19338b3faa1e0f64a1
  - path: context/conventions.md
    sha256: dc53d94ec34eb193eeb06fdd0db19eb86a24c29f7d3c434e7c212d61038ca852
  - path: context/generated/module-map.json
    sha256: 4158c8fcd239c22cb8f1029dd11f61d8839867ff8088bd9ab76197e2db96db4b
pack_sha:
  sample-system: 7681f87749883d71abb4011d118366ca1242e1cc
claims:
  - id: C1
    text: "The dashboard count is built in DashboardService as `awaitingOutcome`."
    cites: ["artifacts/HMS-21/run_01M7Q1DS00W5S7BFVX9Y70NPZG/design.md#item:CO-1"]
  - id: C2
    text: "The list filter is the optional query parameter `started`, validated in AppointmentService."
    cites: ["artifacts/HMS-21/run_01M7Q1DS00W5S7BFVX9Y70NPZG/design.md#item:CO-3"]
  - id: C3
    text: "The repository adds a start-before condition to the `AppointmentFilter` list query."
    cites: ["artifacts/HMS-21/run_01M7Q1DS00W5S7BFVX9Y70NPZG/design.md#item:CO-4"]
  - id: C4
    text: "The dashboard page adds the `Awaiting outcome` tile and link."
    cites: ["artifacts/HMS-21/run_01M7Q1DS00W5S7BFVX9Y70NPZG/design.md#item:CO-7"]
  - id: C5
    text: "`AppointmentsPage` forwards started from the URL."
    cites: ["artifacts/HMS-21/run_01M7Q1DS00W5S7BFVX9Y70NPZG/design.md#item:CO-8"]
  - id: C6
    text: "Invalid input is 400 with one message per field (`ValidationError`)."
    cites: ["context/conventions.md#errors"]
  - id: C7
    text: "SQL lives in `api/src/repositories`; rules in api/src/services."
    cites: ["context/conventions.md#code-layout"]
  - id: C8
    text: "The existing API test asserts the whole totals object with `expect(res.body.totals).toEqual({`."
    cites: ["sample-system:api/test/dashboard.test.ts#L32"]
  - id: C9
    text: "The web test's typed totals fixture starts `totals: { patients: 200`."
    cites: ["sample-system:web/src/__tests__/DashboardPage.test.tsx#L10"]
  - id: C10
    text: "The API dashboard test's clock is set to `09:20` on Monday, before the 09:30 Scheduled booking."
    cites: ["sample-system:api/test/dashboard.test.ts#L21"]
trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10, REQ-11, REQ-12, REQ-13]
known: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10, REQ-11, REQ-12, REQ-13]
assumed: []
unknown: []
questions: []
code:
  - repo: sample-system
    base_sha: 7681f87749883d71abb4011d118366ca1242e1cc
    head_sha: 6898d50a8bd9a9518ae29c87cf12ceee37241edb
    diff_sha256: 8a7e28c01968d7a3669357c951250635f3583a3a827b64c75433445116fead94
    files:
      - api/src/repositories/appointmentRepo.ts
      - api/src/services/appointmentService.ts
      - api/src/services/dashboardService.ts
      - api/test/dashboard.test.ts
      - web/src/__tests__/DashboardPage.test.tsx
      - web/src/api/types.ts
      - web/src/pages/AppointmentsPage.tsx
      - web/src/pages/DashboardPage.tsx
summary: "Adds totals.awaitingOutcome to GET /api/dashboard (Scheduled appointments starting before the server's now, no period window), an optional started=true filter on GET /api/appointments bound to the server clock, and an Awaiting outcome tile linking to /appointments?status=SCHEDULED&started=true."
---
# Change: HMS-21

## Summary
The change follows the design components one for one, in 8 files, well under the change limits, and touches no sensitive path:
- **CO-1, CO-2:** `DashboardService` adds `awaitingOutcome` to the totals (C1). It counts appointments with status SCHEDULED whose `starts_at` is before the server's now (`$3`, the same parameter `upcoming` uses), with no period window. So the count ignores the selector, and an appointment starting exactly now is not counted.
- **CO-3:** `AppointmentService.list` validates `started` with the same validator as `status` (C2). Only `true` is accepted; anything else is a 400 field error (C6). When `started=true`, the bound passed to the repository is the server clock, never a request value.
- **CO-4:** `AppointmentFilter` gains `startedBefore`, and the list query adds `a.starts_at < $n` through the existing bound-parameter helper (C3, C7). Columns and paging are unchanged.
- **CO-6, CO-7, CO-8:** the web `Dashboard` type gains the field. `DashboardPage` adds the "Awaiting outcome" tile (C4), linking to `/appointments?status=SCHEDULED&started=true` through the `query` helper. `AppointmentsPage` forwards `started` from its URL (C5).
- **CO-10, CO-11:** the two existing tests that pin the totals shape are updated (C8, C9).

## Changes
- `api/src/services/dashboardService.ts` — CO-1, CO-2. Adds the `awaitingOutcome` type field, the subquery and the mapping.
- `api/src/services/appointmentService.ts` — CO-3. Validates `started` and sets `startedBefore` from the server clock.
- `api/src/repositories/appointmentRepo.ts` — CO-4. Adds `startedBefore` to `AppointmentFilter` and its parameterised condition.
- `web/src/api/types.ts` — CO-6. Adds `awaitingOutcome: number` to the totals.
- `web/src/pages/DashboardPage.tsx` — CO-7. Adds the Awaiting outcome tile and its fixed link.
- `web/src/pages/AppointmentsPage.tsx` — CO-8. Adds `started` to the forwarded URL filters.
- `api/test/dashboard.test.ts` — CO-10. The totals `toEqual` (C8) now expects `awaitingOutcome: 0` (HMS-21 REQ-6). At the test clock of 09:20 (C10), the 09:30 Scheduled appointment has not started, and the 09:00 and 09:15 appointments are Completed and Cancelled.
- `web/src/__tests__/DashboardPage.test.tsx` — CO-11. The typed fixture (C9) gains `awaitingOutcome: 5`, as the type now requires. This file has no tile-count assertion, so nothing else changes.

New behaviour tests are not added here (rule 10): the boundary at now, the excluded statuses, the period being ignored, the list matching the count, the 400 for a bad `started`, and no contact data in the started list. They are left to the unit-test step, together with the no-personal-data test that the threat model names as a control.

## Known
- REQ-1 to REQ-13: each is served by a design component built here (C1 to C5). Their tests are left to the unit-test step.

## Assumed
None.

## Unknown
None.

## Open questions
None.
