---
schema: design.v1
run_id: run_01M5XZYC101HCFE34KWJ49C90V
step: design
attempt: 1
skill: design@1.4.1
inputs:
  - path: artifacts/HMS-3/run_01M5XZYC101HCFE34KWJ49C90V/impact.md
    sha256: 5d1da4d569f1ea802088c08da6c343f942b48fedbf5457c053022e868a477201
  - path: artifacts/HMS-3/run_01M5XZYC101HCFE34KWJ49C90V/split.md
    sha256: d74118fac2cec105944dfccb25b75abb2f410f0af842b8e86256f4332d8b3714
  - path: artifacts/HMS-3/run_01M5XZYC101HCFE34KWJ49C90V/req.md
    sha256: 50be375fd64aa765d6499709646b264f9c247964fabbc6dc3fdcf28aee13fa9c
  - path: context/intent-baseline.md
    sha256: 9bfd821aeb2fb9524863e346707e049e4fcfb949a6e55b90223a79194946a215
  - path: context/architecture.md
    sha256: 9faab093d10933a46f6473b25c80728f8745e597100e650cd2537bbafb2b595a
  - path: context/generated/module-map.json
    sha256: 31d9ca15805fc1b6488e8da89fdb0ee7714adbcdfda176d69581ab8f3d6482da
  - path: context/generated/api-inventory.json
    sha256: 8dfdbde2e1d64359cb29b8f710a2a467d5e19b383d90def7c856e7738d4ca104
pack_sha:
  sample-system: 415369398bdad3122ac084a00766a01a3034f900
claims:
  - id: C1
    text: "Dashboard counts are computed in `DashboardService`."
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/services/dashboardService.ts:DashboardService"]
  - id: C2
    text: "The Cancelled count selects status `CANCELLED` with starts_at from the period start up to the end of today."
    cites: ["sample-system:api/src/services/dashboardService.ts#L51"]
  - id: C3
    text: "The Completed count selects status `COMPLETED` with the same bounds."
    cites: ["sample-system:api/src/services/dashboardService.ts#L50"]
  - id: C4
    text: "The period start comes from `dayBounds` in the configured time zone."
    cites: ["sample-system:api/src/services/dashboardService.ts#L43"]
  - id: C5
    text: "The dashboard response carries `totals` with completed and cancelled counts."
    cites: ["sample-system:api/src/services/dashboardService.ts#L15"]
  - id: C6
    text: "The Cancelled tile links to `/appointments` with status CANCELLED and the period's from and to."
    cites: ["sample-system:web/src/pages/DashboardPage.tsx#L53"]
  - id: C7
    text: "The Completed tile's label adds the period name: `Completed (`."
    cites: ["sample-system:web/src/pages/DashboardPage.tsx#L46"]
  - id: C8
    text: "The appointment list accepts the status `NO_SHOW`."
    cites: ["sample-system:api/src/types.ts#L5"]
  - id: C9
    text: "The web dashboard type lists the `totals` fields."
    cites: ["sample-system:web/src/api/types.ts#L125"]
  - id: C10
    text: "The API dashboard test compares `totals` exactly."
    cites: ["sample-system:api/test/dashboard.test.ts#L32"]
  - id: C11
    text: "Each tile is drawn by `Tile`."
    cites: ["context/generated/module-map.json#sym:sample-system:web/src/pages/DashboardPage.tsx:Tile"]
  - id: C12
    text: "The dashboard data is served by `/api/dashboard`."
    cites: ["context/generated/api-inventory.json#http:GET:/api/dashboard"]
  - id: C13
    text: "The appointment list is served by `/api/appointments`."
    cites: ["context/generated/api-inventory.json#http:GET:/api/appointments"]
  - id: C14
    text: "The web app calls the API only through `client.ts`."
    cites: ["context/architecture.md#components"]
trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10]
known: [D-1, D-2, D-3, D-4, D-5]
assumed: [A-1]
unknown:
  - {id: U-1, blocking: false}
questions:
  - id: Q-01
    blocking: false
    ask: product owner
    text: "Should the tile read \"No-shows (Today)\", adding the period like the Completed and Cancelled tiles, or exactly \"No-shows\"?"
    why: "The ticket names the tile \"No-shows\"; the sibling tiles add the period name to their label (C7)."
    evidence: ["sample-system:web/src/pages/DashboardPage.tsx#L46", "artifacts/HMS-3/run_01M5XZYC101HCFE34KWJ49C90V/req.md#L41"]
    options: ["No-shows (period), like the siblings", "Exactly No-shows"]
    default: "No-shows (period), like the Completed and Cancelled tiles."
components:
  - {name: DashboardService no-show count, module: "module:sample-system:api", change: changed}
  - {name: Dashboard totals type (api), module: "module:sample-system:api", change: changed}
  - {name: dashboardRouter, module: "module:sample-system:api", change: existing}
  - {name: Dashboard totals type (web), module: "module:sample-system:web", change: changed}
  - {name: DashboardPage No-shows tile, module: "module:sample-system:web", change: changed}
  - {name: Tile, module: "module:sample-system:web", change: existing}
  - {name: Appointment list (status and from/to filters), module: "module:sample-system:api", change: existing}
  - {name: API dashboard test, module: "module:sample-system:api", change: changed}
endpoints:
  - {method: GET, path: /api/dashboard, change: changed}
  - {method: GET, path: /api/appointments, change: existing}
---
# Design: HMS-3

## Overview
Add one count and one tile; no new route, table or store. DashboardService (C1) already counts
Completed and Cancelled with one SQL statement, each by status and by starts_at from the period start
to the end of today (C2, C3), with the period start taken from dayBounds in the configured zone (C4).
The No-show count is a third sub-select in that same statement with status NO_SHOW and the same bounds,
so it is counted exactly like its siblings (REQ-4) and follows the period (REQ-5). The response's
totals (C5) gain `noShows`, and the web Dashboard type (C9) gains it too. DashboardPage adds a Tile (C11)
beside Cancelled that links to /appointments with status NO_SHOW and the period's from and to, exactly
as the Cancelled tile does (C6); the appointment list already accepts NO_SHOW (C8, C13).
Trade-off: a sub-select in the existing query instead of a repository method keeps the counts side by
side and costs one more index-assisted count per load; moving the counts to a repository would be a
larger refactor than the ticket asks.
Trade-off: the field is additive JSON, so other readers through client.ts (C14) are unaffected; but the
API test compares totals exactly (C10), so implement updates that assertion and adds a No-show case
(regression tests the design asks for).

## Components
- DashboardService no-show count (api, changed): third sub-select `status = 'NO_SHOW' AND starts_at >= period start AND starts_at < end of today`; totals.noShows = its count. Serves S-1.
- Dashboard totals type (api, changed): `noShows: number` beside completed and cancelled. Serves S-1.
- dashboardRouter (api, existing): unchanged; still serves GET /api/dashboard (C12).
- Dashboard totals type (web, changed): `noShows: number`. Serves S-1.
- DashboardPage No-shows tile (web, changed): `Tile` labelled "No-shows (<period label>)" with value totals.noShows and link `/appointments?status=NO_SHOW&from=<period.from>&to=<period.to>`, placed after Cancelled. Serves S-1 AC1 and S-2.
- Tile (web, existing): reused as is (C11).
- Appointment list (api, existing): already filters by status (NO_SHOW is a valid status, C8) and by from and to. Serves S-2.
- API dashboard test (api, changed): the exact totals assertion (C10) gains noShows, plus a case with a No-show inside and one outside the period. Regression for S-1 and S-3 AC1.
- GET /api/dashboard (changed): response totals gain noShows; nothing removed.
- GET /api/appointments (existing): link target.
- Stories: S-1 by the service count, the types and the tile; S-2 by the tile's link and the existing list; S-3 by leaving the other sub-selects, tiles and the response's other fields untouched, and by reading only the appointment table (no patient columns, so no medical history or contact details).

## Known
- D-1: the count copies the Completed and Cancelled sub-selects' status-and-bounds form (C2, C3, C4).
- D-2: the response totals gain one additive field (C5); the web type mirrors it (C9).
- D-3: the tile copies the Cancelled tile's link with status NO_SHOW (C6), and the list accepts NO_SHOW (C8).
- D-4: the API test's exact totals assertion must include the new field (C10).
- D-5: no new endpoint; GET /api/dashboard changes shape additively and GET /api/appointments is reused (C12, C13).

## Assumed
- A-1: the tile label is "No-shows (<period>)", following the Completed and Cancelled labels (C7). Basis: consistency with the sibling tiles; the ticket's "No-shows" is kept as the label's first word (Q-01).

## Unknown
- U-1 (non-blocking): whether the label should carry the period name (Q-01).

## Open questions
- Q-01 (non-blocking, product owner): "No-shows (Today)" like the siblings, or exactly "No-shows"? Default: like the siblings.
