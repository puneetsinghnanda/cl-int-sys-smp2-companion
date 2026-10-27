---
schema: ut-result.v1
run_id: run_01M5XZYC101HCFE34KWJ49C90V
step: ut
attempt: 1
skill: unit-test@1.0.1
inputs:
  - path: artifacts/HMS-3/run_01M5XZYC101HCFE34KWJ49C90V/impl.md
    sha256: 494b207ab62d6a2b4728b9c8b2f3c794f95f70ea156045649073f55375b2ba54
  - path: artifacts/HMS-3/run_01M5XZYC101HCFE34KWJ49C90V/split.md
    sha256: d74118fac2cec105944dfccb25b75abb2f410f0af842b8e86256f4332d8b3714
  - path: context/test-strategy.md
    sha256: 1869d3aed8521e40123c46f16539c5ea10ae5ac8c9d872ebc8a1f48b704ce150
  - path: context/generated/test-layout.json
    sha256: bd0686fe34afae476a1ec3105bcf1039ce7e5c510941d1bcdc09ab2ca8e74f3c
pack_sha:
  sample-system: 415369398bdad3122ac084a00766a01a3034f900
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
    text: "Without a test database, database tests are `skipped`."
    cites: ["context/test-strategy.md#database-for-tests"]
  - id: C5
    text: "The change returns the No-show count as `totals.noShows`."
    cites: ["artifacts/HMS-3/run_01M5XZYC101HCFE34KWJ49C90V/impl.md#L54"]
  - id: C6
    text: "The implement step's No-show case lived in `dashboard.test.ts` and moved the shared clock."
    cites: ["artifacts/HMS-3/run_01M5XZYC101HCFE34KWJ49C90V/impl.md#L71"]
  - id: C7
    text: "The existing web dashboard tests live in `DashboardPage.test.tsx`."
    cites: ["context/generated/test-layout.json#test:sample-system:web/src/__tests__/DashboardPage.test.tsx"]
trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10]
known: [T-1, T-2, T-3, T-4]
assumed: []
unknown: []
questions: []
code:
  - repo: sample-system
    base_sha: 524c0ae2931364d0b6580a5a914822089524400a
    head_sha: 1640a6e9509c269bc9d079a8eb56e2eaf7011132
    diff_sha256: ff21d357873f59820baa4ebc5e45e64ee0a9040dc3ff527aaff721020b24c9f5
    files: [api/test/dashboard-no-shows.test.ts, api/test/dashboard.test.ts, web/src/__tests__/DashboardPage.test.tsx]
---
# Unit tests: HMS-3

## Summary
Tests the No-show count (C5) at the API with Supertest against a throwaway PostgreSQL (C1), and the
No-shows tile in the web app with jsdom and Testing Library (C2). The implement step's API case changed
the shared clock and state of `dashboard.test.ts` and had to run last (C6); it now lives in its own
file with its own fresh database, so no test's order matters. Every test name starts with HMS-3 and the
REQ ids it covers (C3). The verifier (H3) runs the suites; tsl records tests run and coverage in
checks.json, so no numbers are written here. Database tests are skipped where no test database is set
up (C4); the verifier provides one.

## Tests
- `api/test/dashboard-no-shows.test.ts` (new, own database): T-1 normal path, a No-show today is counted (REQ-2, REQ-4) and leaves Completed, Cancelled and upcoming as they are (REQ-8); the tile's No-show list matches the count for Today, Last 7 and 30 days (REQ-6, REQ-7). T-2 edges and errors: the next day's Today excludes Monday's No-show while Last 7 and 30 days include it (REQ-5); an unknown period is still rejected with 400 (REQ-3); no contact details or medical history with a No-show counted (REQ-9, REQ-10).
- `api/test/dashboard.test.ts`: keeps the exact totals assertion with `noShows: 0` from implement; the stateful No-show case moved out (T-3).
- `web/src/__tests__/DashboardPage.test.tsx` (C7): T-4 changing the period reloads the count and moves the tile's link to that period (REQ-5, REQ-7); a period with no No-shows shows 0 (REQ-2 edge); implement's tile and link test stays (REQ-1, REQ-6).

## Known
- T-1: the normal path of the changed service and the tile's list (C1, C5).
- T-2: edge and error paths of the same endpoint (C1).
- T-3: the move removes the order dependence in the shared file (C6).
- T-4: the tile follows the period on the page (C2, C7).

## Assumed
- None.

## Unknown
- None.

## Open questions
- None.
