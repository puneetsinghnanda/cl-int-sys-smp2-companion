---
schema: change.v1
run_id: run_01M5XZYC101HCFE34KWJ49C90V
step: impl
attempt: 1
skill: implement@1.0.1
inputs:
  - path: artifacts/HMS-3/run_01M5XZYC101HCFE34KWJ49C90V/design.md
    sha256: 889e67e42166431aed6f8a95542c06b93ebc626f15e7f1c13cac95d491a6be1b
  - path: artifacts/HMS-3/run_01M5XZYC101HCFE34KWJ49C90V/split.md
    sha256: d74118fac2cec105944dfccb25b75abb2f410f0af842b8e86256f4332d8b3714
  - path: context/conventions.md
    sha256: dc53d94ec34eb193eeb06fdd0db19eb86a24c29f7d3c434e7c212d61038ca852
  - path: context/generated/module-map.json
    sha256: 31d9ca15805fc1b6488e8da89fdb0ee7714adbcdfda176d69581ab8f3d6482da
pack_sha:
  sample-system: 415369398bdad3122ac084a00766a01a3034f900
claims:
  - id: C1
    text: "The design counts No-shows with a `third sub-select` in the dashboard statement."
    cites: ["artifacts/HMS-3/run_01M5XZYC101HCFE34KWJ49C90V/design.md#L117"]
  - id: C2
    text: "The design adds `noShows: number` to the API totals type."
    cites: ["artifacts/HMS-3/run_01M5XZYC101HCFE34KWJ49C90V/design.md#L118"]
  - id: C3
    text: "The design adds `noShows: number` to the web totals type."
    cites: ["artifacts/HMS-3/run_01M5XZYC101HCFE34KWJ49C90V/design.md#L120"]
  - id: C4
    text: "The design's tile is titled `No-shows` and links with status NO_SHOW and the period's from and to."
    cites: ["artifacts/HMS-3/run_01M5XZYC101HCFE34KWJ49C90V/design.md#L121"]
  - id: C5
    text: "The design asks the API test's exact `totals` assertion to gain noShows, plus an inside and outside case."
    cites: ["artifacts/HMS-3/run_01M5XZYC101HCFE34KWJ49C90V/design.md#L124"]
  - id: C6
    text: "The design asks the `Web DashboardPage test` to check the tile and its link."
    cites: ["artifacts/HMS-3/run_01M5XZYC101HCFE34KWJ49C90V/design.md#L125"]
  - id: C7
    text: "The dashboard rules live in `DashboardService`."
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/services/dashboardService.ts:DashboardService"]
  - id: C8
    text: "The web dashboard is the page `DashboardPage`."
    cites: ["context/generated/module-map.json#sym:sample-system:web/src/pages/DashboardPage.tsx:DashboardPage"]
trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10]
known: [CH-1, CH-2, CH-3, CH-4, CH-5]
assumed: []
unknown: []
questions: []
code:
  - repo: sample-system
    base_sha: 7e109d61d341f35f511d3f18b8b2edeb729a6401
    head_sha: 958e143e762f6afb6436dc5bcc77f19cde6e164b
    diff_sha256: 4ada92531909b574b851cccc6ea56f8ccea3de8c7442859401854e678cec4d63
    files: [api/src/services/dashboardService.ts, api/test/dashboard.test.ts, web/src/__tests__/DashboardPage.test.tsx, web/src/api/types.ts, web/src/pages/DashboardPage.tsx]
summary: "The dashboard counts No-show appointments in the selected period beside Completed and Cancelled, returns them as totals.noShows, and shows a No-shows tile linking to the No-show list for the period; both dashboard tests cover it."
---
# Change: HMS-3

## Summary
Builds the approved design. DashboardService (C7) gains a third sub-select in its counting statement,
`status = 'NO_SHOW'` with the same `$4` to `$2` bounds as Completed and Cancelled (C1), returned as
totals.noShows (C2). The web Dashboard type gains noShows (C3), and DashboardPage (C8) adds a Tile
titled "No-shows", after Cancelled, linking to /appointments with status NO_SHOW and the period's
from and to through the page's query helper (C4). The other sub-selects, tiles and response fields are
unchanged. As the design asks, the API test's exact totals assertion includes noShows and a new case
covers a No-show inside and outside the period (C5), and the web test checks the tile and link (C6).

## Changes
- `api/src/services/dashboardService.ts`: `noShows` in the Dashboard totals type; the NO_SHOW sub-select `no_shows`; `noShows: Number(r.no_shows)` in the result. Serves DashboardService no-show count and Dashboard totals type (api).
- `web/src/api/types.ts`: `noShows: number` in the dashboard totals. Serves Dashboard totals type (web).
- `web/src/pages/DashboardPage.tsx`: a `Tile` labelled "No-shows" with `data.totals.noShows` and the NO_SHOW link for the period. Serves DashboardPage No-shows tile.
- `api/test/dashboard.test.ts`: the DSH-1..6 exact totals assertion gains `noShows: 0`; new test "HMS-3 REQ-2/REQ-4/REQ-5/REQ-7" closes an appointment as No-show, checks the Today count, that the No-show list for the period matches it, and that the next day's Today excludes it while Last 7 days includes it. Serves API dashboard test.
- `web/src/__tests__/DashboardPage.test.tsx`: the fixture's totals gain `noShows: 4`; new test "HMS-3 REQ-1/REQ-6/REQ-7" checks the tile shows 4 and links to `/appointments?status=NO_SHOW&from=…&to=…`. Serves Web DashboardPage test.

## Known
- CH-1: the count copies the existing sub-selects' status and bounds (C1, C7).
- CH-2: the response and both types gain one additive number (C2, C3).
- CH-3: the tile copies the Cancelled tile's link with status NO_SHOW (C4, C8).
- CH-4: the API test's exact assertion pinned the totals shape, which the design changes, so it is updated (C5).
- CH-5: the web test fixture is typed by the totals, so it gains noShows, and the new test checks the tile (C6).

## Assumed
- None.

## Unknown
- None.

## Open questions
- None.
