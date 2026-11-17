---
schema: design.v1
run_id: run_01M7MD2RTRSVZ65HR3QFFR9NJB
step: design
attempt: 1
skill: design@1.8.0
inputs:
  - {path: artifacts/HMS-21/run_01M7MD2RTRSVZ65HR3QFFR9NJB/impact.md, sha256: 8689e330e66d26acb467dc140698e854ec473b908b160da36b7fd6ca981c1a99}
  - {path: artifacts/HMS-21/run_01M7MD2RTRSVZ65HR3QFFR9NJB/split.md, sha256: dcc06d1291e79f057d47f662fdc9b9f93994a0b3c695173acb9df34f900df06e}
  - {path: artifacts/HMS-21/run_01M7MD2RTRSVZ65HR3QFFR9NJB/req.md, sha256: cdf917be0f8dfa8f1b3a5b08603bd0a50957b5f840e47e335b5c6a52a24591fc}
  - {path: context/intent-baseline.md, sha256: 9bfd821aeb2fb9524863e346707e049e4fcfb949a6e55b90223a79194946a215}
  - {path: context/architecture.md, sha256: 9faab093d10933a46f6473b25c80728f8745e597100e650cd2537bbafb2b595a}
  - {path: context/generated/module-map.json, sha256: 4158c8fcd239c22cb8f1029dd11f61d8839867ff8088bd9ab76197e2db96db4b}
  - {path: context/generated/api-inventory.json, sha256: ba024ab1b4b4151d26ce1afa77bd2dab1cec645ea737ce37f98e0e8674f795e9}
pack_sha: {sample-system: 7681f87749883d71abb4011d118366ca1242e1cc}
claims:
  - {id: C1, text: "Dashboard counts are computed in `DashboardService`.", cites: ["context/generated/module-map.json#sym:sample-system:api/src/services/dashboardService.ts:DashboardService"]}
  - {id: C2, text: "The API dashboard response type is `Dashboard` in dashboardService.ts.", cites: ["context/generated/module-map.json#sym:sample-system:api/src/services/dashboardService.ts:Dashboard"]}
  - {id: C3, text: "The web dashboard type declares `totals` with seven numeric counts.", cites: ["sample-system:web/src/api/types.ts#L142"]}
  - {id: C4, text: "The Upcoming count uses `starts_at > $3`, where $3 is the service clock's now.", cites: ["sample-system:api/src/services/dashboardService.ts#L52"]}
  - {id: C5, text: "The dashboard query passes `[todayBounds.start, todayBounds.end, now, periodStart]` as parameters.", cites: ["sample-system:api/src/services/dashboardService.ts#L56"]}
  - {id: C6, text: "The appointment list is served by `AppointmentService`.", cites: ["context/generated/module-map.json#sym:sample-system:api/src/services/appointmentService.ts:AppointmentService"]}
  - {id: C7, text: "The list validates status with `f.oneOf('status', APPOINTMENT_STATUSES)`.", cites: ["sample-system:api/src/services/appointmentService.ts#L35"]}
  - {id: C8, text: "The list filter type is `AppointmentFilter` in appointmentRepo.ts.", cites: ["context/generated/module-map.json#sym:sample-system:api/src/repositories/appointmentRepo.ts:AppointmentFilter"]}
  - {id: C9, text: "The repository adds the upper bound as `a.starts_at < ?`.", cites: ["sample-system:api/src/repositories/appointmentRepo.ts#L72"]}
  - {id: C10, text: "The Fields validator's `oneOf` rejects any value outside its list with a field error.", cites: ["sample-system:api/src/lib/validate.ts#L62"]}
  - {id: C11, text: "The Fields validator's `bool(name: string)` accepts only a JSON true, so it cannot read a query string.", cites: ["sample-system:api/src/lib/validate.ts#L109"]}
  - {id: C12, text: "The appointment list page whitelists `['doctorId', 'patientId', 'status', 'from', 'to', 'page']` from the URL.", cites: ["sample-system:web/src/pages/AppointmentsPage.tsx#L14"]}
  - {id: C13, text: "The dashboard page is the `DashboardPage` component.", cites: ["context/generated/module-map.json#sym:sample-system:web/src/pages/DashboardPage.tsx:DashboardPage"]}
  - {id: C14, text: "Counts are rendered by the `Tile` component.", cites: ["context/generated/module-map.json#sym:sample-system:web/src/pages/DashboardPage.tsx:Tile"]}
  - {id: C15, text: "The Upcoming tile builds its link with `query({ status: 'SCHEDULED', from: data.today })`.", cites: ["sample-system:web/src/pages/DashboardPage.tsx#L44"]}
  - {id: C16, text: "The appointment list page is the `AppointmentsPage` component.", cites: ["context/generated/module-map.json#sym:sample-system:web/src/pages/AppointmentsPage.tsx:AppointmentsPage"]}
  - {id: C17, text: "The API dashboard test compares totals with `toEqual`.", cites: ["sample-system:api/test/dashboard.test.ts#L32"]}
  - {id: C18, text: "The web dashboard test fixture is `const DATA: Dashboard`.", cites: ["sample-system:web/src/__tests__/DashboardPage.test.tsx#L7"]}
  - {id: C19, text: "Routes parse input, `services hold the rules` and repositories hold the SQL.", cites: ["context/architecture.md#components"]}
  - {id: C20, text: "The list gains a filter: `New filter on GET /api/appointments`.", cites: ["runs/run_01M7MD2RTRSVZ65HR3QFFR9NJB/ide-answers.md#a-01"]}
  - {id: C21, text: "The new totals field is named `awaitingOutcome`.", cites: ["runs/run_01M7MD2RTRSVZ65HR3QFFR9NJB/ide-answers.md#a-02"]}
  - {id: C22, text: "The list filter is `started=true` compared with the server clock.", cites: ["runs/run_01M7MD2RTRSVZ65HR3QFFR9NJB/ide-answers.md#a-03"]}
  - {id: C23, text: "List rows carry only the patient's name, `p.full_name AS patient_name`.", cites: ["sample-system:api/src/repositories/appointmentRepo.ts#L22"]}
  - {id: C24, text: "Dashboard responses carry counts only, with no contact details or medical history (`DSH-9`).", cites: ["sample-system:api/src/services/dashboardService.ts#L28"]}
trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10, REQ-11]
known: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10, REQ-11, S-1, S-2, S-3, S-4]
assumed: []
unknown: []
questions: []
components:
  - {id: CO-1, name: "DashboardService.get: awaitingOutcome count", module: "module:sample-system:api", change: changed, cites: ["context/generated/module-map.json#sym:sample-system:api/src/services/dashboardService.ts:DashboardService", "sample-system:api/src/services/dashboardService.ts#L52", "runs/run_01M7MD2RTRSVZ65HR3QFFR9NJB/ide-answers.md#a-02"]}
  - {id: CO-2, name: "Dashboard type (API)", module: "module:sample-system:api", change: changed, cites: ["context/generated/module-map.json#sym:sample-system:api/src/services/dashboardService.ts:Dashboard", "runs/run_01M7MD2RTRSVZ65HR3QFFR9NJB/ide-answers.md#a-02"]}
  - {id: CO-3, name: "AppointmentService.list: started filter", module: "module:sample-system:api", change: changed, cites: ["context/generated/module-map.json#sym:sample-system:api/src/services/appointmentService.ts:AppointmentService", "sample-system:api/src/services/appointmentService.ts#L35", "runs/run_01M7MD2RTRSVZ65HR3QFFR9NJB/ide-answers.md#a-01", "runs/run_01M7MD2RTRSVZ65HR3QFFR9NJB/ide-answers.md#a-03"]}
  - {id: CO-4, name: "AppointmentFilter and AppointmentRepo.list: startedBefore bound", module: "module:sample-system:api", change: changed, cites: ["context/generated/module-map.json#sym:sample-system:api/src/repositories/appointmentRepo.ts:AppointmentFilter", "sample-system:api/src/repositories/appointmentRepo.ts#L72", "runs/run_01M7MD2RTRSVZ65HR3QFFR9NJB/ide-answers.md#a-03"]}
  - {id: CO-5, name: "Dashboard type (web)", module: "module:sample-system:web", change: changed, cites: ["context/generated/module-map.json#sym:sample-system:web/src/api/types.ts:Dashboard", "sample-system:web/src/api/types.ts#L142", "runs/run_01M7MD2RTRSVZ65HR3QFFR9NJB/ide-answers.md#a-02"]}
  - {id: CO-6, name: "DashboardPage: Awaiting outcome tile", module: "module:sample-system:web", change: changed, cites: ["context/generated/module-map.json#sym:sample-system:web/src/pages/DashboardPage.tsx:DashboardPage", "context/generated/module-map.json#sym:sample-system:web/src/pages/DashboardPage.tsx:Tile", "runs/run_01M7MD2RTRSVZ65HR3QFFR9NJB/ide-answers.md#a-02", "runs/run_01M7MD2RTRSVZ65HR3QFFR9NJB/ide-answers.md#a-03"]}
  - {id: CO-7, name: "AppointmentsPage: pass started through", module: "module:sample-system:web", change: changed, cites: ["context/generated/module-map.json#sym:sample-system:web/src/pages/AppointmentsPage.tsx:AppointmentsPage", "sample-system:web/src/pages/AppointmentsPage.tsx#L14", "runs/run_01M7MD2RTRSVZ65HR3QFFR9NJB/ide-answers.md#a-03"]}
  - {id: CO-8, name: "Dashboard tests (api and web): add awaitingOutcome", module: "module:sample-system:api", change: changed, cites: ["sample-system:api/test/dashboard.test.ts#L32", "sample-system:web/src/__tests__/DashboardPage.test.tsx#L7"]}
  - {id: CO-9, name: "appointment table and appointment_start_idx", module: "module:sample-system:db", change: existing, cites: ["context/generated/module-map.json#module:sample-system:db", "artifacts/HMS-21/run_01M7MD2RTRSVZ65HR3QFFR9NJB/impact.md#claim:C17"]}
endpoints:
  - id: EP-1
    method: GET
    path: /api/dashboard
    change: changed
    fields:
      - {name: totals.awaitingOutcome, type: integer, nullable: false, format: "count >= 0; Scheduled appointments with starts_at strictly before the server's now; no period applied", example: 2, status: known}
    cites: ["context/generated/api-inventory.json#http:GET:/api/dashboard", "runs/run_01M7MD2RTRSVZ65HR3QFFR9NJB/ide-answers.md#a-02"]
  - id: EP-2
    method: GET
    path: /api/appointments
    change: changed
    fields:
      - {name: started (query), type: string, nullable: true, format: "only the value \"true\"; absent = no filter; any other value -> 400 field error; when \"true\", keeps rows with starts_at strictly before the server's now", example: "true", status: known}
    cites: ["context/generated/api-inventory.json#http:GET:/api/appointments", "runs/run_01M7MD2RTRSVZ65HR3QFFR9NJB/ide-answers.md#a-01", "runs/run_01M7MD2RTRSVZ65HR3QFFR9NJB/ide-answers.md#a-03"]
answers:
  - {id: A-02, kind: question, about: Q-01, answer: "awaitingOutcome (proposed default)", decides: [EP-1, CO-1, CO-2, CO-5, CO-6, C21]}
  - {id: A-03, kind: question, about: Q-02, answer: "started=true, server now (proposed default)", decides: [EP-2, CO-3, CO-4, CO-6, CO-7, C22]}
---
# Design: HMS-21

## Overview
The dashboard gains an Awaiting outcome count, and the appointment list gains a filter that shows exactly the appointments that count covers.
- **Count.** `DashboardService.get` (C1) adds one more sub-select to its existing totals query: Scheduled appointments with `starts_at < $3`. That is the same now parameter the Upcoming count already uses with `>` (C4, C5), and no period bound is applied (REQ-5). The count is returned as `totals.awaitingOutcome` (A-02, C21). An appointment starting exactly at now is in neither count.
- **List.** `GET /api/appointments` accepts `started=true` (A-01, A-03, C20, C22). The service passes the service clock's now as a new `startedBefore` bound to the repository, which adds `a.starts_at < ?` next to the existing bounds (C9). The tile links to `/appointments?status=SCHEDULED&started=true`, with no `from`/`to`, so no period applies to the list either (S-3).
- **Web.** `DashboardPage` renders a new `Tile` labelled "Awaiting outcome" (C13, C14). `AppointmentsPage` adds `started` to its URL whitelist (C12) so the link's filter reaches the API.

Trade-offs:
- Server now instead of a browser-supplied instant: the count and the list use the same clock, at the cost of the list not being reproducible from a URL at a later time (A-03).
- One more sub-select in the existing dashboard query instead of a separate query: one round trip, and the query stays in one place (C19).
- The filter is validated with `Fields.oneOf('started', ['true'])` (C10), because `Fields.bool` reads only JSON `true` (C11). An invalid value is a 400 field error, like `status` (C7).

## Components
- CO-1 DashboardService.get, changed: adds `(SELECT count(*) FROM appointment WHERE status = 'SCHEDULED' AND starts_at < $3) AS awaiting_outcome` and returns it as `totals.awaitingOutcome`. Other totals and `next` are unchanged (REQ-8, REQ-9).
- CO-2 Dashboard type (API), changed: `totals` gains `awaitingOutcome: number`.
- CO-3 AppointmentService.list, changed: reads `started` with `f.oneOf('started', ['true'])`. When it is `true`, it passes `startedBefore: this.clock()`.
- CO-4 AppointmentFilter and AppointmentRepo.list, changed: an optional `startedBefore?: Date` that adds `a.starts_at < ?`.
- CO-5 Dashboard type (web), changed: `totals` gains `awaitingOutcome: number` (C3).
- CO-6 DashboardPage, changed: a new `<Tile label="Awaiting outcome" value={data.totals.awaitingOutcome} to={`/appointments${query({ status: 'SCHEDULED', started: 'true' })}`} />`, built the same way as the Upcoming tile's link (C15). The existing tiles stay as they are.
- CO-7 AppointmentsPage, changed: its whitelist (C12) gains `started`. No new form control is added; changing any filter keeps `started`, as it keeps the others.
- CO-8 Dashboard tests, changed: `api/test/dashboard.test.ts` (C17) and `web/src/__tests__/DashboardPage.test.tsx` (C18) gain `awaitingOutcome` in their expected totals and fixture.
- CO-9 appointment table and `appointment_start_idx`, existing: read only. No migration.
- EP-1 GET /api/dashboard, changed. New field `totals.awaitingOutcome`:
  - type: integer, never null, 0 or more;
  - rule: Scheduled appointments with a start strictly before the server's now, with no period applied;
  - example: 2.
- EP-2 GET /api/appointments, changed. New optional query parameter `started`:
  - only the value "true" is allowed; when absent, there is no filter; any other value gives a 400 field error;
  - when it is "true", the list keeps only rows that start strictly before the server's now;
  - example: `?status=SCHEDULED&started=true`.

Data protection: the dashboard adds a count only (C24). List rows still carry only the patient's name (C23), so REQ-10 and REQ-11 hold.

Story map:
- S-1 (count rule, zero): CO-1, CO-2, CO-5, CO-6, EP-1.
- S-2 (no period): CO-1 and EP-1, which have no period bound.
- S-3 (list behind the tile): CO-3, CO-4, CO-6, CO-7, EP-2.
- S-4 (rest unchanged, no private data): CO-1, CO-6 and CO-8, plus C23 and C24.

## Known
- REQ-1 to REQ-11 and S-1 to S-4: served by the components above. Each change rests on a module-map fact or a product code line (C1 to C18). Each contract choice rests on A-01 to A-03.

## Assumed
None.

## Unknown
None.

## Open questions
None open.

Answered in the IDE:
- A-02 (Q-01): what to call the new totals field. Answer: "awaitingOutcome (proposed default)". It decides EP-1, CO-1, CO-2, CO-5, CO-6 and C21. No mismatch.
- A-03 (Q-02): how the tile's link expresses "started before now". Answer: "started=true, server now (proposed default)". It decides EP-2, CO-3, CO-4, CO-6, CO-7 and C22. No mismatch.
- A-01 (from impact, this run) is reused: a new filter on GET /api/appointments (C20).
