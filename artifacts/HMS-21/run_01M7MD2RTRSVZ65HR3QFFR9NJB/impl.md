---
schema: change.v1
run_id: run_01M7MD2RTRSVZ65HR3QFFR9NJB
step: impl
attempt: 1
skill: implement@1.2.0
inputs:
  - {path: artifacts/HMS-21/run_01M7MD2RTRSVZ65HR3QFFR9NJB/design.md, sha256: 7af71278d91675313b29a7418b6bbc21ac67eec00434e34a677eb231824f5a7d}
  - {path: artifacts/HMS-21/run_01M7MD2RTRSVZ65HR3QFFR9NJB/split.md, sha256: dcc06d1291e79f057d47f662fdc9b9f93994a0b3c695173acb9df34f900df06e}
  - {path: context/conventions.md, sha256: dc53d94ec34eb193eeb06fdd0db19eb86a24c29f7d3c434e7c212d61038ca852}
  - {path: context/generated/module-map.json, sha256: 4158c8fcd239c22cb8f1029dd11f61d8839867ff8088bd9ab76197e2db96db4b}
pack_sha: {sample-system: 7681f87749883d71abb4011d118366ca1242e1cc}
code:
  - repo: sample-system
    base_sha: 7681f87749883d71abb4011d118366ca1242e1cc
    head_sha: 118d6ca844070e96ca87772869b2160ed40c7cac
    diff_sha256: 8d283e2bdf2ecec0d669a9ce07d56cc0433fd23bc7c6ec90b5647e5487ca291d
    files: [api/src/repositories/appointmentRepo.ts, api/src/services/appointmentService.ts, api/src/services/dashboardService.ts, api/test/dashboard.test.ts, web/src/__tests__/DashboardPage.test.tsx, web/src/api/types.ts, web/src/pages/AppointmentsPage.tsx, web/src/pages/DashboardPage.tsx]
claims:
  - {id: C1, text: "The design names the new totals field `awaitingOutcome`.", cites: ["artifacts/HMS-21/run_01M7MD2RTRSVZ65HR3QFFR9NJB/design.md#claim:C21"]}
  - {id: C2, text: "The design's list filter is `started=true` compared with the server clock.", cites: ["artifacts/HMS-21/run_01M7MD2RTRSVZ65HR3QFFR9NJB/design.md#claim:C22"]}
  - {id: C3, text: "The Upcoming count uses `starts_at > $3`, the mirror of the new count.", cites: ["sample-system:api/src/services/dashboardService.ts#L52"]}
  - {id: C4, text: "The repository binds filter values as numbered parameters with `params.push(value)`.", cites: ["sample-system:api/src/repositories/appointmentRepo.ts#L65"]}
  - {id: C5, text: "The list validates enumerated query values with `f.oneOf`.", cites: ["sample-system:api/src/services/appointmentService.ts#L35"]}
  - {id: C6, text: "The list page reads a whitelist of filters (`'doctorId', 'patientId', 'status', 'from', 'to'`) from the URL.", cites: ["sample-system:web/src/pages/AppointmentsPage.tsx#L14"]}
  - {id: C7, text: "The API dashboard test compares totals with `toEqual`.", cites: ["sample-system:api/test/dashboard.test.ts#L32"]}
  - {id: C8, text: "The web dashboard test fixture is `const DATA: Dashboard`.", cites: ["sample-system:web/src/__tests__/DashboardPage.test.tsx#L7"]}
  - {id: C9, text: "In the API dashboard test the clock is set to `at(MONDAY, '09:20')` before the assertions.", cites: ["sample-system:api/test/dashboard.test.ts#L21"]}
  - {id: C10, text: "Invalid input is 400 with one message per field (`ValidationError`).", cites: ["context/conventions.md#errors"]}
  - {id: C11, text: "Logs carry method, route and error message only, `never request bodies`.", cites: ["context/conventions.md#logging-and-data"]}
trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10, REQ-11]
known: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10, REQ-11]
assumed: []
unknown: []
questions: []
summary: "Adds totals.awaitingOutcome (Scheduled, starts_at < server now, no period) to GET /api/dashboard, a started=true filter on GET /api/appointments that bounds starts_at < server now, an Awaiting outcome tile linking to /appointments?status=SCHEDULED&started=true, passes started through the list page, and updates the two dashboard tests that pin the totals shape."
---
# Change: HMS-21

## Summary
The change builds the design's components and nothing else.
- **Count (CO-1, CO-2).** `DashboardService.get` gains one sub-select in its existing totals query: `status = 'SCHEDULED' AND starts_at < $3`, where `$3` is the same now the Upcoming count uses with `>` (C3). It is returned as `totals.awaitingOutcome` (C1). No period bound is applied, and an appointment starting exactly at now is not counted. The API `Dashboard` type gains the field.
- **List filter (CO-3, CO-4).** `AppointmentService.list` reads `started` with `f.oneOf('started', ['true'])` (C5), so any other value is a 400 field error (C10). When the value is `true`, it passes `startedBefore: this.clock()`, the server's now and never a client instant (C2). `AppointmentFilter` gains an optional `startedBefore`. The repository adds `a.starts_at < ?` through the existing parameterised `add` helper (C4).
- **Web (CO-5, CO-6, CO-7).** The web `Dashboard` type gains `awaitingOutcome: number`. `DashboardPage` renders an "Awaiting outcome" `Tile` linking to `/appointments?status=SCHEDULED&started=true`, built with the same `query()` helper as the other tiles. `AppointmentsPage` adds `started` to its URL whitelist (C6), so the filter reaches the API.
- **Tests (CO-8).** Only the two tests that pin the changed totals shape are updated (C7, C8).

Nothing new is logged (C11). There is no migration (CO-9).

## Changes
- `api/src/services/dashboardService.ts`: CO-1 and CO-2. One new sub-select (`awaiting_outcome`), the `awaitingOutcome` field on the `Dashboard` type, and its mapping in `totals`.
- `api/src/services/appointmentService.ts`: CO-3. Reads and validates `started`, then passes `startedBefore: this.clock()`.
- `api/src/repositories/appointmentRepo.ts`: CO-4. `AppointmentFilter.startedBefore?: Date` and its `a.starts_at < ?` bound.
- `web/src/api/types.ts`: CO-5. `awaitingOutcome: number` in `Dashboard.totals`.
- `web/src/pages/DashboardPage.tsx`: CO-6. The new "Awaiting outcome" tile and link. The existing tiles are unchanged.
- `web/src/pages/AppointmentsPage.tsx`: CO-7. `started` added to the URL whitelist.
- `api/test/dashboard.test.ts`: CO-8, REQ-2. The expected totals gain `awaitingOutcome: 0`. With the clock at 09:20 (C9), the only Scheduled appointments start at 09:30 Monday and 10:00 Tuesday, so none has started.
- `web/src/__tests__/DashboardPage.test.tsx`: CO-8, REQ-1. The typed fixture gains `awaitingOutcome: 3`.

## Known
- REQ-1, REQ-7: CO-6 tile and the CO-1 count, which is 0 when there are no matches.
- REQ-2, REQ-3, REQ-4: CO-1 counts only Scheduled appointments with a start strictly before now (C3).
- REQ-5: no period bound in CO-1, and no `from`/`to` in the tile link.
- REQ-6: CO-3, CO-4, CO-6 and CO-7 give the list the same rule as the count (C2).
- REQ-8, REQ-9: the other totals and the `next` query are untouched, and the existing test still pins them (C7).
- REQ-10, REQ-11: the change adds a count and a filter only, and list rows are unchanged.

## Assumed
None.

## Unknown
None.

## Open questions
None.
