---
schema: design.v1
run_id: run_01M7Q1DS00W5S7BFVX9Y70NPZG
step: design
attempt: 1
skill: design@1.9.0
inputs:
  - path: artifacts/HMS-21/run_01M7Q1DS00W5S7BFVX9Y70NPZG/impact.md
    sha256: 139225a02c4dcd31369d5b5fb12dad21413d05ed5cbb73bb6e989f0eb1c8a26e
  - path: artifacts/HMS-21/run_01M7Q1DS00W5S7BFVX9Y70NPZG/split.md
    sha256: b54499b009a5c3dc2f2280aa5d7a1ddbb5c87d032fedcf19338b3faa1e0f64a1
  - path: artifacts/HMS-21/run_01M7Q1DS00W5S7BFVX9Y70NPZG/req.md
    sha256: c5c9d980373a9426a9af4c3b4d72cca66f0eb9f6023b195e786aa8d084ba20f0
  - path: context/intent-baseline.md
    sha256: 9bfd821aeb2fb9524863e346707e049e4fcfb949a6e55b90223a79194946a215
  - path: context/architecture.md
    sha256: 9faab093d10933a46f6473b25c80728f8745e597100e650cd2537bbafb2b595a
  - path: context/generated/module-map.json
    sha256: 4158c8fcd239c22cb8f1029dd11f61d8839867ff8088bd9ab76197e2db96db4b
  - path: context/generated/api-inventory.json
    sha256: ba024ab1b4b4151d26ce1afa77bd2dab1cec645ea737ce37f98e0e8674f795e9
pack_sha:
  sample-system: 7681f87749883d71abb4011d118366ca1242e1cc
claims:
  - id: C1
    text: "Dashboard counts are computed in `DashboardService`."
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/services/dashboardService.ts:DashboardService"]
  - id: C2
    text: "The API dashboard response shape is the `Dashboard` type in the dashboard service."
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/services/dashboardService.ts:Dashboard"]
  - id: C3
    text: "The dashboard period choices are `PERIODS`; the new count does not use them."
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/services/dashboardService.ts:PERIODS"]
  - id: C4
    text: "The web app mirrors the dashboard response as `Dashboard` in its API types."
    cites: ["context/generated/module-map.json#sym:sample-system:web/src/api/types.ts:Dashboard"]
  - id: C5
    text: "The dashboard tiles are rendered by `DashboardPage`."
    cites: ["context/generated/module-map.json#sym:sample-system:web/src/pages/DashboardPage.tsx:DashboardPage"]
  - id: C6
    text: "The appointment list page is `AppointmentsPage`."
    cites: ["context/generated/module-map.json#sym:sample-system:web/src/pages/AppointmentsPage.tsx:AppointmentsPage"]
  - id: C7
    text: "The appointment list's rules live in `AppointmentService`."
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/services/appointmentService.ts:AppointmentService"]
  - id: C8
    text: "The appointment list filters are the `AppointmentFilter` interface."
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/repositories/appointmentRepo.ts:AppointmentFilter"]
  - id: C9
    text: "The appointment list query lives in `AppointmentRepo`."
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/repositories/appointmentRepo.ts:AppointmentRepo"]
  - id: C10
    text: "The appointment list route is declared in `api/src/routes/appointments.ts`."
    cites: ["context/generated/api-inventory.json#http:GET:/api/appointments"]
  - id: C11
    text: "The dashboard route is declared in `api/src/routes/dashboard.ts`."
    cites: ["context/generated/api-inventory.json#http:GET:/api/dashboard"]
  - id: C12
    text: "Web pages build query strings with the client's `query` helper."
    cites: ["context/generated/module-map.json#sym:sample-system:web/src/api/client.ts:query"]
  - id: C13
    text: "Invalid input is 400 with one message per field (`ValidationError`)."
    cites: ["context/conventions.md#errors"]
  - id: C14
    text: "Lists answer items, page, page_size and total, `20 per page`."
    cites: ["context/conventions.md#lists"]
  - id: C15
    text: "Appointment instants are stored in `UTC`."
    cites: ["context/architecture.md#time-and-slots"]
  - id: C16
    text: "The list opened from the tile uses a new filter evaluated against the server's now (`New filter, server now`)."
    cites: ["runs/run_01M7Q1DS00W5S7BFVX9Y70NPZG/ide-answers.md#a-01"]
  - id: C17
    text: "The new dashboard totals field is `awaitingOutcome`."
    cites: ["runs/run_01M7Q1DS00W5S7BFVX9Y70NPZG/ide-answers.md#a-02"]
  - id: C18
    text: "The appointment list filter is the query parameter `started=true`."
    cites: ["runs/run_01M7Q1DS00W5S7BFVX9Y70NPZG/ide-answers.md#a-03"]
  - id: C19
    text: "The API dashboard test asserts the whole totals object with `expect(res.body.totals).toEqual({`."
    cites: ["sample-system:api/test/dashboard.test.ts#L32"]
  - id: C20
    text: "The web dashboard test builds a typed totals fixture starting `totals: { patients: 200`."
    cites: ["sample-system:web/src/__tests__/DashboardPage.test.tsx#L10"]
trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10, REQ-11, REQ-12, REQ-13]
known: [S-1, S-2, S-3, S-4, S-5, S-6]
assumed: []
unknown: []
questions: []
components:
  - id: CO-1
    name: DashboardService (awaitingOutcome count)
    module: module:sample-system:api
    change: changed
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/services/dashboardService.ts:DashboardService", "runs/run_01M7Q1DS00W5S7BFVX9Y70NPZG/ide-answers.md#a-02"]
  - id: CO-2
    name: Dashboard type (API, totals.awaitingOutcome)
    module: module:sample-system:api
    change: changed
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/services/dashboardService.ts:Dashboard", "runs/run_01M7Q1DS00W5S7BFVX9Y70NPZG/ide-answers.md#a-02"]
  - id: CO-3
    name: AppointmentService list (parses started, sets server now)
    module: module:sample-system:api
    change: changed
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/services/appointmentService.ts:AppointmentService", "runs/run_01M7Q1DS00W5S7BFVX9Y70NPZG/ide-answers.md#a-01", "runs/run_01M7Q1DS00W5S7BFVX9Y70NPZG/ide-answers.md#a-03"]
  - id: CO-4
    name: AppointmentFilter and AppointmentRepo list query (start-before condition)
    module: module:sample-system:api
    change: changed
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/repositories/appointmentRepo.ts:AppointmentFilter", "context/generated/module-map.json#sym:sample-system:api/src/repositories/appointmentRepo.ts:AppointmentRepo", "runs/run_01M7Q1DS00W5S7BFVX9Y70NPZG/ide-answers.md#a-01"]
  - id: CO-5
    name: Appointments and dashboard routes
    module: module:sample-system:api
    change: existing
    cites: ["context/generated/api-inventory.json#http:GET:/api/appointments", "context/generated/api-inventory.json#http:GET:/api/dashboard"]
  - id: CO-6
    name: Dashboard type (web, totals.awaitingOutcome)
    module: module:sample-system:web
    change: changed
    cites: ["context/generated/module-map.json#sym:sample-system:web/src/api/types.ts:Dashboard", "runs/run_01M7Q1DS00W5S7BFVX9Y70NPZG/ide-answers.md#a-02"]
  - id: CO-7
    name: DashboardPage (Awaiting outcome tile and link)
    module: module:sample-system:web
    change: changed
    cites: ["context/generated/module-map.json#sym:sample-system:web/src/pages/DashboardPage.tsx:DashboardPage", "runs/run_01M7Q1DS00W5S7BFVX9Y70NPZG/ide-answers.md#a-03"]
  - id: CO-8
    name: AppointmentsPage (passes started from the URL)
    module: module:sample-system:web
    change: changed
    cites: ["context/generated/module-map.json#sym:sample-system:web/src/pages/AppointmentsPage.tsx:AppointmentsPage", "runs/run_01M7Q1DS00W5S7BFVX9Y70NPZG/ide-answers.md#a-03"]
  - id: CO-9
    name: API client query helper
    module: module:sample-system:web
    change: existing
    cites: ["context/generated/module-map.json#sym:sample-system:web/src/api/client.ts:query"]
  - id: CO-10
    name: Dashboard tests (API totals toEqual, web typed fixture and tile assertions)
    module: module:sample-system:api
    change: changed
    cites: ["sample-system:api/test/dashboard.test.ts#L32", "sample-system:web/src/__tests__/DashboardPage.test.tsx#L10"]
endpoints:
  - id: EP-1
    method: GET
    path: /api/dashboard
    change: changed
    fields:
      - {name: totals.awaitingOutcome, type: integer, nullable: false, format: "count of appointments, >= 0; not affected by the period parameter", example: 3, status: known}
    cites: ["context/generated/api-inventory.json#http:GET:/api/dashboard", "runs/run_01M7Q1DS00W5S7BFVX9Y70NPZG/ide-answers.md#a-02"]
  - id: EP-2
    method: GET
    path: /api/appointments
    change: changed
    fields:
      - {name: started (query parameter), type: string, nullable: true, format: "optional; only true; keeps starts_at < server now", example: "true", status: known}
    cites: ["context/generated/api-inventory.json#http:GET:/api/appointments", "runs/run_01M7Q1DS00W5S7BFVX9Y70NPZG/ide-answers.md#a-01", "runs/run_01M7Q1DS00W5S7BFVX9Y70NPZG/ide-answers.md#a-03"]
answers:
  - id: A-02
    kind: question
    about: Q-01
    answer: "awaitingOutcome (proposed default)"
    decides: [EP-1, CO-1, CO-2, CO-6, C17]
  - id: A-03
    kind: question
    about: Q-02
    answer: "started=true (proposed default)"
    decides: [EP-2, CO-3, CO-7, CO-8, C18]
---
# Design: HMS-21

## Overview
The dashboard API gains one more total, `awaitingOutcome` (A-02): the number of Scheduled appointments whose start instant is strictly before the server's current instant. It is computed in `DashboardService` (C1) with no `PERIODS` window (C3), so it ignores the period selector (S-3). The appointment list API gains an optional `started=true` filter (A-01, A-03). The service turns it into "start before the server's now" and the repository adds that condition to the list query (C7, C8, C9). The dashboard page adds an "Awaiting outcome" tile that links to `/appointments?status=SCHEDULED&started=true`, built with the existing `query` helper (C12). `AppointmentsPage` passes `started` from its URL to the API (C6).

Trade-offs:
- Server now, not a client instant: the list always agrees with the rule at the moment it is opened. A list opened later than the dashboard may differ from the tile by appointments that started in between, which is acceptable because both reflect the same rule.
- Strict `<` against now: an appointment starting at exactly the current instant is not yet counted, so S-1's 09:59/10:01 cases at now 10:00 are unambiguous.
- Comparing instants, not local dates: instants are stored in `UTC` (C15), so the count is the same on either side of local midnight (S-1's 00:30 local case).
- `started` accepts only `true`: a boolean flag with a single value keeps the contract small. Any other value is a 400 `ValidationError` (C13), not silently ignored.
- No new table, column, job or store: the count and filter read existing columns, so no migration is needed.

## Components
- CO-1 DashboardService — changed. Adds the `awaitingOutcome` count: status SCHEDULED and start before now. The other totals and the `next` list are untouched (S-5).
- CO-2 Dashboard type (API) — changed. `totals` gains `awaitingOutcome: number` (C2).
- CO-3 AppointmentService list — changed. It reads the optional `started` query value. Only `true` is valid (C13). When present, it sets the filter's start-before bound to the server clock's now.
- CO-4 AppointmentFilter and AppointmentRepo list query — changed. An optional start-before instant adds `starts_at < bound` to the list query, combined with the existing status and date filters. The selected columns do not change, so no contact details or medical history are added (S-6).
- CO-5 Appointments and dashboard routes — existing. They already pass the query to the services (C10, C11).
- CO-6 Dashboard type (web) — changed. `totals` gains `awaitingOutcome: number` (C4).
- CO-7 DashboardPage — changed. It adds the Awaiting outcome tile showing `totals.awaitingOutcome` (0 when none, S-1). It links to `/appointments?status=SCHEDULED&started=true`, which does not change with the period (S-3, S-4).
- CO-8 AppointmentsPage — changed. It forwards `started` from its URL to `GET /api/appointments`, alongside the existing filters.
- CO-9 API client `query` helper — existing (C12).
- CO-10 Dashboard tests — changed. The API test asserts the whole totals object (C19), and the web test's typed totals fixture (C20) must add `awaitingOutcome`. Any web tile-count assertion must expect the new tile.
- EP-1 GET /api/dashboard — changed. New field `totals.awaitingOutcome`: integer, not null, a count of 0 or more, not affected by `period`, for example 3.
- EP-2 GET /api/appointments — changed. New optional query parameter `started`:
  - Only the value `true` is accepted; any other value is 400 on field `started`.
  - When present, the list keeps appointments whose `starts_at` is strictly before the server's now, for example `started=true`.
  - Paging is unchanged at `20 per page` (C14), and `total` equals the tile count when combined with `status=SCHEDULED` (S-4).

Story coverage:
- S-1 (count, boundary at now, local midnight, zero): CO-1, CO-2, CO-6, CO-7, EP-1.
- S-2 (Completed, Cancelled and No-show excluded): CO-1 (status SCHEDULED only).
- S-3 (period ignored): CO-1 (no `PERIODS` window), CO-7 (link has no period).
- S-4 (linked list matches exactly): CO-3, CO-4, CO-7, CO-8, EP-2.
- S-5 (other tiles and next-10 list unchanged): CO-1, CO-10.
- S-6 (no medical history or contact details): CO-1 (counts only), CO-4 (no new columns).

## Known
- S-1, S-2, S-3: the count lives in `DashboardService` (C1) and `Dashboard` (C2, C4), named by A-02 (C17).
- S-4: the filter is decided by A-01 and A-03 (C16, C18), and served through `AppointmentService`, `AppointmentFilter` and `AppointmentRepo` (C7, C8, C9).
- S-5: the other totals and the `next` list keep their queries. Only the tests that read the full shape change (C19, C20).
- S-6: the dashboard returns counts only, and the list's columns do not change.

## Assumed
None.

## Unknown
None.

## Open questions
None open.

Answered in the IDE:
- A-02 (runs/run_01M7Q1DS00W5S7BFVX9Y70NPZG/ide-answers.md#a-02): what should the new totals field be called? Answer: "awaitingOutcome (proposed default)". It decides EP-1, CO-1, CO-2, CO-6 and C17. No mismatch with the inputs.
- A-03 (runs/run_01M7Q1DS00W5S7BFVX9Y70NPZG/ide-answers.md#a-03): how should the list filter be named and valued? Answer: "started=true (proposed default)". It decides EP-2, CO-3, CO-7, CO-8 and C18. No mismatch with the inputs.
- A-01 (impact step, runs/run_01M7Q1DS00W5S7BFVX9Y70NPZG/ide-answers.md#a-01): a new filter evaluated against the server's now. Reused here (C16) and not asked again.
