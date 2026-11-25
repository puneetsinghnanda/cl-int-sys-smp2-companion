---
schema: test-plan.v1
run_id: run_01M7Q1DS00W5S7BFVX9Y70NPZG
step: tplan
attempt: 1
skill: test-plan@1.3.0
inputs:
  - path: artifacts/HMS-21/run_01M7Q1DS00W5S7BFVX9Y70NPZG/split.md
    sha256: b54499b009a5c3dc2f2280aa5d7a1ddbb5c87d032fedcf19338b3faa1e0f64a1
  - path: artifacts/HMS-21/run_01M7Q1DS00W5S7BFVX9Y70NPZG/design.md
    sha256: 67fec0fca1d7cdefdee99ee1eeed978a3cefdfdb2b3ca102126f07401d052b77
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
    text: "Web tests use Vitest with `jsdom` and Testing Library."
    cites: ["context/test-strategy.md#layers"]
  - id: C3
    text: "In this system `no page is tested end to end in a browser`."
    cites: ["context/test-strategy.md#gaps"]
  - id: C4
    text: "Without a PostgreSQL binary or `HMS_TEST_DATABASE_URL`, database tests are skipped."
    cites: ["context/test-strategy.md#database-for-tests"]
  - id: C5
    text: "Every test name starts with its requirement ID (`DOC-1, APT-4`)."
    cites: ["context/test-strategy.md#requirement-ids"]
  - id: C6
    text: "Dashboard API tests with their own database follow `api/test/dashboard-no-shows.test.ts`."
    cites: ["context/generated/test-layout.json#test:sample-system:api/test/dashboard-no-shows.test.ts"]
  - id: C7
    text: "Dashboard web tests live in `web/src/__tests__/DashboardPage.test.tsx`."
    cites: ["context/generated/test-layout.json#test:sample-system:web/src/__tests__/DashboardPage.test.tsx"]
  - id: C8
    text: "The list filter is named and valued `started=true`."
    cites: ["runs/run_01M7Q1DS00W5S7BFVX9Y70NPZG/ide-answers.md#a-03"]
  - id: C9
    text: "The invented test doctors are available only `Mon–Fri 09:00–12:00` (doctor 1)."
    cites: ["sample-system:api/test/helpers.ts#L23"]
  - id: C10
    text: "Medical history is blood group, allergies, chronic conditions, current medications and notes (`Medical history`)."
    cites: ["artifacts/HMS-21/run_01M7Q1DS00W5S7BFVX9Y70NPZG/split.md#claim:C8"]
trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10, REQ-11, REQ-12, REQ-13]
known: [TP-1, TP-2, TP-3, TP-4, TP-5, TP-6, TP-7, TP-8, TP-9, TP-10, TP-11, TP-12, TP-13, TP-14, TP-15, TP-16, TP-17, TP-18, TP-19, TP-20]
assumed: []
unknown: []
questions: []
tests:
  - {id: TP-1, title: "Dashboard shows a tile labelled Awaiting outcome with its count", covers: [S-1/AC1], level: unit}
  - {id: TP-2, title: "Count includes an appointment started one minute before now and excludes one starting one minute after", covers: [S-1/AC2], level: integration}
  - {id: TP-3, title: "Count includes an appointment at 00:30 local (previous UTC day) and excludes one later today", covers: [S-1/AC3], level: integration}
  - {id: TP-4, title: "Count includes a never-marked appointment that started weeks earlier", covers: [S-1/AC4, S-3/AC2], level: integration}
  - {id: TP-5, title: "Count is 0 when every Scheduled appointment starts after now; tile shows 0", covers: [S-1/AC5, S-5/AC2], level: integration}
  - {id: TP-6, title: "Completed, Cancelled and No-show appointments that started are not counted, including the most recent", covers: [S-2/AC1, S-2/AC2, S-2/AC3], level: integration}
  - {id: TP-7, title: "Marking a counted appointment Completed removes it from the count and the list", covers: [S-2/AC4], level: integration}
  - {id: TP-8, title: "Count is the same for Today, Last 7 days and Last 30 days with an appointment outside Today", covers: [S-3/AC1, S-3/AC2], level: integration}
  - {id: TP-9, title: "Tile count and link stay the same when the period selector changes", covers: [S-3/AC1], level: unit}
  - {id: TP-10, title: "Tile links to the Scheduled list with started=true", covers: [S-4/AC1], level: unit}
  - {id: TP-11, title: "Appointments page forwards started=true to the API and omits it when absent", covers: [S-4/AC1], level: unit}
  - {id: TP-12, title: "started=true list holds exactly the counted appointments at the boundary; total equals the count", covers: [S-4/AC1], level: integration}
  - {id: TP-13, title: "started=true list includes the 00:30 local and weeks-old appointments", covers: [S-4/AC2], level: integration}
  - {id: TP-14, title: "started=true list excludes a Completed appointment that started", covers: [S-4/AC3], level: integration}
  - {id: TP-15, title: "Negative: started values other than true (false, 1, TRUE) return 400 on the started field", covers: [S-4/AC1], level: integration}
  - {id: TP-16, title: "All other totals keep their values with an appointment awaiting an outcome", covers: [S-5/AC1], level: integration}
  - {id: TP-17, title: "Existing tiles keep their counts and links in order", covers: [S-5/AC1, S-5/AC2], level: unit}
  - {id: TP-18, title: "Next upcoming list shows the 10 earliest of 11 future appointments and never a started one", covers: [S-5/AC3, S-5/AC4], level: integration}
  - {id: TP-19, title: "Dashboard response and page carry no contact details or medical history of a fully filled patient", covers: [S-6/AC1], level: integration}
  - {id: TP-20, title: "started=true list response carries no contact details or medical history of a fully filled patient", covers: [S-6/AC2], level: integration}
---
# Test plan: HMS-21

## Scope
In scope: the Awaiting outcome count in `GET /api/dashboard`, the `started=true` filter on `GET /api/appointments` (C8), the dashboard tile and its link, the Appointments page forwarding `started`, and the regressions on the other tiles and the next list.

Levels follow the strategy:
- **Integration:** API tests with Vitest, Supertest and a throwaway PostgreSQL with a fixed clock (C1), in a file of their own like the No-shows tests (C6). They run only where a PostgreSQL binary or test database URL is available (C4).
- **Unit:** web tests with jsdom and Testing Library (C2), next to the existing DashboardPage tests (C7).
- **No e2e and no manual tests:** the system has no browser end-to-end tests (C3), and every criterion can be proven at a lower level.

Every test name starts with `HMS-21 REQ-n` (C5).

Test data notes:
- **TP-3 and TP-13** need an appointment at 00:30 local. The test doctors are available only from 09:00 (C9), so booking through the API is refused. These tests insert the row directly into the test database, or add a test doctor with early availability.
- **TP-19 and TP-20** seed an invented patient with every contact field and every medical-history field (C10). They then assert none of those values appear.
- **TP-18** books 11 future appointments.

Out of scope: performance of the count on large data, and the existing period, list and paging behaviour beyond the regressions above.

## Tests
- TP-1 (unit): the dashboard shows the Awaiting outcome tile with its count. Covers S-1/AC1.
- TP-2 (integration): the count includes an appointment that started one minute before now and excludes one starting one minute after. Covers S-1/AC2.
- TP-3 (integration): the count includes an appointment at 00:30 local (19:00 UTC the day before) and excludes one later today. Covers S-1/AC3.
- TP-4 (integration): the count includes a never-marked appointment that started weeks earlier. Covers S-1/AC4 and S-3/AC2.
- TP-5 (integration): the count is 0 when every Scheduled appointment starts after now. Covers S-1/AC5 and S-5/AC2.
- TP-6 (integration): Completed, Cancelled and No-show appointments that started are not counted, even the most recent one. Covers S-2/AC1, S-2/AC2 and S-2/AC3.
- TP-7 (integration): marking a counted appointment Completed removes it from the count and the list. Covers S-2/AC4.
- TP-8 (integration): the count is the same for Today, Last 7 days and Last 30 days, with an appointment outside Today. Covers S-3/AC1 and S-3/AC2.
- TP-9 (unit): the tile's count and link stay the same when the period changes. Covers S-3/AC1.
- TP-10 (unit): the tile links to `/appointments?status=SCHEDULED&started=true`. Covers S-4/AC1.
- TP-11 (unit): the Appointments page forwards `started=true` to the API, and omits it when the URL has none. Covers S-4/AC1.
- TP-12 (integration): at the boundary, the `started=true` list holds exactly the counted appointments, and its total equals the count. Covers S-4/AC1.
- TP-13 (integration): the `started=true` list includes the 00:30 local and the weeks-old appointments. Covers S-4/AC2.
- TP-14 (integration): the `started=true` list excludes a Completed appointment that started. Covers S-4/AC3.
- TP-15 (integration, negative): `started` set to `false`, `1` or `TRUE` returns 400 on the `started` field. Covers S-4/AC1.
- TP-16 (integration): with an appointment awaiting an outcome, every other total keeps its value. Covers S-5/AC1.
- TP-17 (unit): the existing tiles keep their counts and links, in order. Covers S-5/AC1 and S-5/AC2.
- TP-18 (integration): the next list shows the 10 earliest of 11 future appointments, and never one that started. Covers S-5/AC3 and S-5/AC4.
- TP-19 (integration): the dashboard response carries no contact details or medical history of a fully filled patient. Covers S-6/AC1.
- TP-20 (integration): the `started=true` list response carries no contact details or medical history of a fully filled patient. Covers S-6/AC2.

## Known
- TP-1 to TP-20: each is placed at the level the strategy names for its layer (C1, C2), and together they cover every acceptance criterion S-1 to S-6.

## Assumed
None.

## Unknown
None.

## Open questions
None.
