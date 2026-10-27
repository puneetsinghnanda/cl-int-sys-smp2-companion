---
schema: test-plan.v1
run_id: run_01M5XZYC101HCFE34KWJ49C90V
step: tplan
attempt: 2
skill: test-plan@1.0.1
inputs:
  - path: artifacts/HMS-3/run_01M5XZYC101HCFE34KWJ49C90V/split.md
    sha256: d74118fac2cec105944dfccb25b75abb2f410f0af842b8e86256f4332d8b3714
  - path: artifacts/HMS-3/run_01M5XZYC101HCFE34KWJ49C90V/design.md
    sha256: 889e67e42166431aed6f8a95542c06b93ebc626f15e7f1c13cac95d491a6be1b
  - path: context/test-strategy.md
    sha256: 1869d3aed8521e40123c46f16539c5ea10ae5ac8c9d872ebc8a1f48b704ce150
  - path: context/generated/test-layout.json
    sha256: bd0686fe34afae476a1ec3105bcf1039ce7e5c510941d1bcdc09ab2ca8e74f3c
pack_sha:
  sample-system: 415369398bdad3122ac084a00766a01a3034f900
resolves:
  - {id: F-1, how: "TP-12 no longer asserts a status code; it is a negative test from the stories' three periods (C9). TP-15 adds a regression of the existing DSH-7 test (C10)."}
claims:
  - id: C1
    text: "API tests use Vitest and `Supertest` against a throwaway PostgreSQL with a fixed clock."
    cites: ["context/test-strategy.md#layers"]
  - id: C2
    text: "Web tests use Vitest with `jsdom` and Testing Library."
    cites: ["context/test-strategy.md#layers"]
  - id: C3
    text: "Every requirement has a test whose name starts with its `ID`."
    cites: ["context/test-strategy.md#requirement-ids"]
  - id: C4
    text: "The design counts No-shows with `starts_at >= period start AND starts_at < end of today`."
    cites: ["artifacts/HMS-3/run_01M5XZYC101HCFE34KWJ49C90V/design.md#L117"]
  - id: C5
    text: "The design's tile is titled `No-shows` and links with status NO_SHOW and the period's from and to."
    cites: ["artifacts/HMS-3/run_01M5XZYC101HCFE34KWJ49C90V/design.md#L121"]
  - id: C6
    text: "The existing API dashboard tests live in `dashboard.test.ts`."
    cites: ["context/generated/test-layout.json#test:sample-system:api/test/dashboard.test.ts"]
  - id: C7
    text: "The existing web dashboard tests live in `DashboardPage.test.tsx`."
    cites: ["context/generated/test-layout.json#test:sample-system:web/src/__tests__/DashboardPage.test.tsx"]
  - id: C8
    text: "The changed endpoint is `/api/dashboard`; no endpoint is new."
    cites: ["artifacts/HMS-3/run_01M5XZYC101HCFE34KWJ49C90V/design.md#L92"]
  - id: C9
    text: "The selected period is one of `Today`, Last 7 days or Last 30 days."
    cites: ["artifacts/HMS-3/run_01M5XZYC101HCFE34KWJ49C90V/split.md#L19"]
  - id: C10
    text: "An existing test covers the period rule: `DSH-7`."
    cites: ["context/generated/test-layout.json#test:sample-system:api/test/dashboard.test.ts"]
  - id: C11
    text: "A `No-show` is a past appointment the patient did not attend."
    cites: ["artifacts/HMS-3/run_01M5XZYC101HCFE34KWJ49C90V/split.md#L25"]
trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10]
known: [TP-1, TP-2, TP-3, TP-4, TP-5, TP-6, TP-7, TP-8, TP-9, TP-10, TP-11, TP-12, TP-13, TP-14, TP-15]
assumed: []
unknown: []
questions: []
tests:
  - {id: TP-1, title: "REQ-1 (HMS-3): DashboardPage shows a tile titled No-shows with the count from totals.noShows", covers: [S-1/AC1], level: unit}
  - {id: TP-2, title: "REQ-2/REQ-4 (HMS-3): GET /api/dashboard counts a No-show that starts in the selected period", covers: [S-1/AC2], level: integration}
  - {id: TP-3, title: "REQ-2/REQ-3 (HMS-3) boundary: a No-show that starts before the period is not counted in Today", covers: [S-1/AC3], level: integration}
  - {id: TP-4, title: "REQ-5 (HMS-3): Last 7 and 30 days count a No-show that Today does not", covers: [S-1/AC4], level: integration}
  - {id: TP-5, title: "REQ-5 (HMS-3): changing the period on DashboardPage reloads the No-shows count", covers: [S-1/AC4], level: unit}
  - {id: TP-6, title: "REQ-4 (HMS-3): the No-shows count sits beside Completed and Cancelled with the same bounds; a No-show leaves them unchanged", covers: [S-1/AC5, S-3/AC1], level: integration}
  - {id: TP-7, title: "REQ-6/REQ-7 (HMS-3): the No-shows tile links to /appointments with status NO_SHOW and the period's from and to", covers: [S-2/AC1, S-2/AC2], level: unit}
  - {id: TP-8, title: "REQ-6/REQ-7 (HMS-3): the No-show list for each period holds exactly the counted appointments", covers: [S-2/AC1, S-2/AC2], level: integration}
  - {id: TP-9, title: "REQ-8 (HMS-3): the exact dashboard totals keep every existing count and add only noShows", covers: [S-3/AC1], level: integration}
  - {id: TP-10, title: "REQ-9 (HMS-3): with a No-show counted, the dashboard carries no medical history", covers: [S-3/AC2], level: integration}
  - {id: TP-11, title: "REQ-10 (HMS-3): with a No-show counted, the dashboard carries no contact details", covers: [S-3/AC3], level: integration}
  - {id: TP-12, title: "REQ-3 (HMS-3) negative: a period other than Today, Last 7 days or Last 30 days yields no dashboard counts", covers: [S-1/AC4], level: integration}
  - {id: TP-13, title: "REQ-2 (HMS-3) edge: a period with no No-shows shows 0 on the tile", covers: [S-1/AC2], level: unit}
  - {id: TP-14, title: "REQ-2/REQ-3 (HMS-3) boundary: a No-show that starts at or after the end of the selected Today is not counted (clock set to the day before it)", covers: [S-1/AC3], level: integration}
  - {id: TP-15, title: "REQ-3 (HMS-3) regression: the existing DSH-7 period test still passes with noShows added", covers: [S-1/AC4], level: integration}
---
# Test plan: HMS-3

## Scope
In scope: the No-show count in GET /api/dashboard (C4, C8) and the No-shows tile on DashboardPage
(C5). API behaviour is proved at integration level with Vitest and Supertest against a throwaway
PostgreSQL with a fixed clock (C1). The exact totals check stays in the existing `dashboard.test.ts`
(C6). The No-show cases live in `api/test/dashboard-no-shows.test.ts`, a file with its own database:
closing an appointment as No-show and moving the clock would otherwise change the counts the shared
file's exact assertions read, so the cases would depend on running last. The tile is proved at unit
level with Vitest, jsdom and Testing Library (C2) in the existing `DashboardPage.test.tsx` (C7). Test
names start with their requirement ids (C3). Out of scope: end-to-end browser tests, the appointment
list page itself (unchanged), and how an appointment becomes No-show (unchanged route).

## Tests
- TP-1 (unit): the tile is titled No-shows and shows the count — S-1/AC1.
- TP-2 (integration): a No-show in the period is counted — S-1/AC2.
- TP-3 (integration, lower boundary): a No-show before the period start is not counted — S-1/AC3.
- TP-4 (integration): Last 7 and 30 days count what Today does not — S-1/AC4.
- TP-5 (unit): changing the period reloads the count — S-1/AC4.
- TP-6 (integration): counted like Completed and Cancelled; they stay unchanged — S-1/AC5, S-3/AC1.
- TP-7 (unit): the tile's link carries status NO_SHOW and the period — S-2/AC1, S-2/AC2.
- TP-8 (integration): the linked list matches the count for each period — S-2/AC1, S-2/AC2.
- TP-9 (integration): the exact totals keep every existing count and add only noShows — S-3/AC1.
- TP-10 (integration): no medical history on the dashboard — S-3/AC2.
- TP-11 (integration): no contact details on the dashboard — S-3/AC3.
- TP-12 (integration, negative): a period outside the three choices (C9) yields no counts — S-1/AC4.
- TP-13 (unit, edge): zero No-shows shows 0 — S-1/AC2.
- TP-14 (integration, upper boundary): with the clock on the day before a No-show, that day's Today excludes it (the bound `starts_at < end of today`, C4). A No-show is always in the past (C11), so the clock is moved, not the appointment — S-1/AC3.
- TP-15 (integration, regression): the existing DSH-7 period test (C10) still passes — S-1/AC4.

## Known
- TP-1, TP-5, TP-7, TP-13: the tile's title, count, link and period follow from the design (C5) at the web unit level the strategy uses (C2, C7).
- TP-2, TP-3, TP-4, TP-6, TP-8, TP-14: the count's bounds and the list filter follow from the design (C4, C8) at the API integration level (C1); a No-show is in the past (C11).
- TP-9, TP-15: the existing API dashboard tests (C6, C10).
- TP-10, TP-11: the response is checked for medical history and contact details with a No-show present (C1).
- TP-12: the stories allow only three periods (C9).

## Assumed
- None.

## Unknown
- None.

## Open questions
- None.
