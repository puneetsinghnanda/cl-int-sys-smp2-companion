---
schema: test-plan.v1
run_id: run_01M7MD2RTRSVZ65HR3QFFR9NJB
step: tplan
attempt: 1
skill: test-plan@1.2.0
inputs:
  - {path: artifacts/HMS-21/run_01M7MD2RTRSVZ65HR3QFFR9NJB/split.md, sha256: dcc06d1291e79f057d47f662fdc9b9f93994a0b3c695173acb9df34f900df06e}
  - {path: artifacts/HMS-21/run_01M7MD2RTRSVZ65HR3QFFR9NJB/design.md, sha256: 7af71278d91675313b29a7418b6bbc21ac67eec00434e34a677eb231824f5a7d}
  - {path: context/test-strategy.md, sha256: f4bbc9d5d63dfc433c9fe466a7aa71a4fad0147baafe1f47db0c06e117ce6cba}
  - {path: context/generated/test-layout.json, sha256: c0752ab8543e23547679dbe34f6381f656d36b0eb5506efb2a56f94dcb729cd6}
pack_sha: {sample-system: 7681f87749883d71abb4011d118366ca1242e1cc}
claims:
  - {id: C1, text: "API tests use Vitest and Supertest against a throwaway PostgreSQL 16 with a `fixed clock`.", cites: ["context/test-strategy.md#layers"]}
  - {id: C2, text: "Web tests use Vitest with `jsdom and Testing Library`.", cites: ["context/test-strategy.md#layers"]}
  - {id: C3, text: "`no page is tested end to end in a browser`, so the project has no e2e level today.", cites: ["context/test-strategy.md#gaps"]}
  - {id: C4, text: "Every requirement has a test whose name `starts with its ID`.", cites: ["context/test-strategy.md#requirement-ids"]}
  - {id: C5, text: "Web tests already cover `the dashboard tiles and links`.", cites: ["context/test-strategy.md#gaps"]}
  - {id: C6, text: "The design changes `DashboardService.get: awaitingOutcome count` (CO-1).", cites: ["artifacts/HMS-21/run_01M7MD2RTRSVZ65HR3QFFR9NJB/design.md#item:CO-1"]}
  - {id: C7, text: "The design changes `AppointmentService.list: started filter` (CO-3).", cites: ["artifacts/HMS-21/run_01M7MD2RTRSVZ65HR3QFFR9NJB/design.md#item:CO-3"]}
  - {id: C8, text: "The design changes `DashboardPage: Awaiting outcome tile` (CO-6).", cites: ["artifacts/HMS-21/run_01M7MD2RTRSVZ65HR3QFFR9NJB/design.md#item:CO-6"]}
  - {id: C9, text: "The design changes `AppointmentsPage: pass started through` (CO-7).", cites: ["artifacts/HMS-21/run_01M7MD2RTRSVZ65HR3QFFR9NJB/design.md#item:CO-7"]}
  - {id: C10, text: "The list filter is `started=true` compared with the server clock.", cites: ["artifacts/HMS-21/run_01M7MD2RTRSVZ65HR3QFFR9NJB/design.md#claim:C22"]}
  - {id: C11, text: "The existing HMS-3 tile test keeps `Its own database` so other tests do not move its counts.", cites: ["sample-system:api/test/dashboard-no-shows.test.ts#L7"]}
trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10, REQ-11]
known: [S-1, S-2, S-3, S-4, TP-1, TP-2, TP-3, TP-4, TP-5, TP-6, TP-7, TP-8, TP-9, TP-10, TP-11, TP-12, TP-13, TP-14, TP-15, TP-16, TP-17]
assumed: []
unknown: []
questions: []
tests:
  - {id: TP-1, title: "API: counts Scheduled appointments started at 09:00 and 09:30 when it is 10:00", covers: [S-1/AC1], level: integration}
  - {id: TP-2, title: "API: Completed, No-show and Cancelled (latest) are not counted", covers: [S-1/AC2], level: integration}
  - {id: TP-3, title: "API boundary: a start exactly at now and a later start are not counted", covers: [S-1/AC3], level: integration}
  - {id: TP-4, title: "API: count is 0 when only future Scheduled appointments remain", covers: [S-1/AC4], level: integration}
  - {id: TP-5, title: "API: completing a counted appointment drops the count by one", covers: [S-1/AC5], level: integration}
  - {id: TP-6, title: "API: a 1 August 2026 appointment counts for Today, 7d and 30d alike", covers: [S-2/AC1], level: integration}
  - {id: TP-7, title: "API: started=true list holds exactly the counted appointments in start order", covers: [S-3/AC1], level: integration}
  - {id: TP-8, title: "API: started=true list includes the 1 August 2026 appointment (no period)", covers: [S-3/AC2], level: integration}
  - {id: TP-9, title: "API: with a count of 0 the started=true list is empty", covers: [S-3/AC3], level: integration}
  - {id: TP-10, title: "API negative: started values other than true return 400 with a started field error", covers: [S-3/AC1], level: integration}
  - {id: TP-11, title: "API edge: no started or an empty started leaves the list unfiltered", covers: [S-3/AC1], level: integration}
  - {id: TP-12, title: "API regression: other totals for each period are unchanged", covers: [S-4/AC1], level: integration}
  - {id: TP-13, title: "API regression: the next list shows 10:30 and not 09:00; same when nothing is upcoming", covers: [S-4/AC2, S-4/AC3], level: integration}
  - {id: TP-14, title: "API privacy: no allergy or note values of a counted patient appear", covers: [S-4/AC4], level: integration}
  - {id: TP-15, title: "API privacy: a counted patient's phone and email values do not appear", covers: [S-4/AC5], level: integration}
  - {id: TP-16, title: "Web: Awaiting outcome tile count, period-free link and unchanged other tiles", covers: [S-1/AC1, S-1/AC4, S-2/AC1, S-3/AC1, S-4/AC1], level: unit}
  - {id: TP-17, title: "Web: AppointmentsPage passes started from the URL to the API", covers: [S-3/AC1, S-3/AC2], level: unit}
---
# Test plan: HMS-21

## Scope
**In scope** are the components the design changes:
- the dashboard count, CO-1 (C6);
- the `started=true` list filter, CO-3 (C7, C10);
- the dashboard tile, CO-6 (C8);
- the list page passing the filter through, CO-7 (C9).

Each of the 16 acceptance criteria of S-1 to S-4 is covered by at least one test.

Levels follow the test strategy:
- **API rules → integration.** API rules are proved with Vitest and Supertest against a throwaway PostgreSQL 16 with a fixed clock (C1). The API tests run in their own database, as the existing tile test does (C11).
- **Web rendering → unit.** Web rendering and links are proved with Vitest, jsdom and Testing Library (C2). These extend the existing dashboard tile tests (C5).
- **No e2e or manual tests.** The project has no browser end-to-end level (C3), and every criterion can be proved at the two levels above.

Every test name starts with HMS-21 and its REQ id (C4).

**Out of scope** are load or query-plan tests for the unbounded count. Those are accepted risks, tracked as the secure review's Q-01.

## Tests
- **TP-1 (integration):** with the clock at 10:00, Scheduled appointments at 09:00 and 09:30 give a count of 2. Covers S-1/AC1.
- **TP-2 (integration):** Completed, No-show and Cancelled appointments are not counted. The Cancelled one is the most recent. Covers S-1/AC2.
- **TP-3 (integration, boundary):** a start at exactly 10:00 and one at 10:30 are not counted. Covers S-1/AC3.
- **TP-4 (integration):** when only future Scheduled appointments remain, the count is 0. Covers S-1/AC4.
- **TP-5 (integration):** completing one counted appointment lowers the count by 1. Covers S-1/AC5.
- **TP-6 (integration):** a 1 August 2026 appointment counts the same for Today, 7d and 30d. Covers S-2/AC1.
- **TP-7 (integration):** the `started=true` list holds exactly the counted appointments, in start order. Covers S-3/AC1.
- **TP-8 (integration):** the list includes 1 August 2026, because no period applies. Covers S-3/AC2.
- **TP-9 (integration):** with a count of 0, the list is empty. Covers S-3/AC3.
- **TP-10 (integration, negative):** `started` set to `false`, `yes` or `1` gives a 400 field error on `started`. Covers S-3/AC1. This is the negative test for the new input field.
- **TP-11 (integration, edge):** an absent or empty `started` leaves the list unfiltered. Covers S-3/AC1.
- **TP-12 (integration, regression):** the other totals for each period are unchanged, and the existing dashboard totals test still passes. Covers S-4/AC1.
- **TP-13 (integration, regression):** the next list shows 10:30 and not 09:00, and it is empty when nothing is upcoming. Covers S-4/AC2 and S-4/AC3.
- **TP-14 (integration, privacy):** no allergy or note values of a counted patient appear on the dashboard or the list. Covers S-4/AC4.
- **TP-15 (integration, privacy):** a counted patient is given a phone and an email. Neither the values nor the keys appear in the dashboard JSON. Covers S-4/AC5. This closes the gap the unit-test review noted, where the old check only searched for the key names.
- **TP-16 (unit, web):** the tile shows its count, including 0. It links to `/appointments?status=SCHEDULED&started=true` with no period, and the other tiles keep their counts and links. Covers S-1/AC1, S-1/AC4, S-2/AC1, S-3/AC1 and S-4/AC1.
- **TP-17 (unit, web):** the list page sends `started` from the URL to the API, and leaves it out when absent. Covers S-3/AC1 and S-3/AC2.

## Known
- **S-1 to S-4:** every acceptance criterion appears in at least one `covers` list (TP-1 to TP-17).
- **TP-1 to TP-15:** API integration tests at the level the strategy names (C1). Most already exist in `api/test/dashboard-awaiting-outcome.test.ts` from the unit-test step. TP-4, TP-13 (the "nothing upcoming" case) and TP-15 (a recorded email) are new.
- **TP-16 and TP-17:** web unit tests at the level the strategy names (C2, C5). They exist in `web/src/__tests__/`.

## Assumed
None.

## Unknown
None.

## Open questions
None.
