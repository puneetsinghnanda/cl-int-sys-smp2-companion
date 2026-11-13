---
schema: test-plan.v1
run_id: run_01M79P1N889PBTFM8J1EX89TBK
step: tplan
attempt: 1
skill: test-plan@1.2.0
inputs:
  - {path: artifacts/HMS-5/run_01M79P1N889PBTFM8J1EX89TBK/split.md, sha256: db8ef532a356ccb24887a9fadcb6dc8a8fbc5338ae76d37cb71b224324490de8}
  - {path: artifacts/HMS-5/run_01M79P1N889PBTFM8J1EX89TBK/design.md, sha256: 799df78b60dcfbc79dc1465cb608d0c935c43fb6ef1c5d88f90d28404b564347}
  - {path: context/test-strategy.md, sha256: f4bbc9d5d63dfc433c9fe466a7aa71a4fad0147baafe1f47db0c06e117ce6cba}
  - {path: context/generated/test-layout.json, sha256: c0752ab8543e23547679dbe34f6381f656d36b0eb5506efb2a56f94dcb729cd6}
pack_sha: {sample-system: 7681f87749883d71abb4011d118366ca1242e1cc}
claims:
  - {id: C1, text: "API tests use Vitest and `Supertest` against a throwaway PostgreSQL 16 with a fixed clock.", cites: ["context/test-strategy.md#layers"]}
  - {id: C2, text: "Web tests use Vitest with jsdom and `Testing Library`.", cites: ["context/test-strategy.md#layers"]}
  - {id: C3, text: "Every requirement has a test whose name starts with its ID, and `New requirements get new IDs and tests`.", cites: ["context/test-strategy.md#requirement-ids"]}
  - {id: C4, text: "Without PostgreSQL binaries or `HMS_TEST_DATABASE_URL`, database tests are skipped.", cites: ["context/test-strategy.md#database-for-tests"]}
  - {id: C5, text: "The project has no browser tests: `no page is tested end to end in a browser`.", cites: ["context/test-strategy.md#gaps"]}
  - {id: C6, text: "The patient detail endpoints return the new field `visitSummary` with two non-null integer counts.", cites: ["artifacts/HMS-5/run_01M79P1N889PBTFM8J1EX89TBK/design.md#item:EP-1"]}
trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9]
known: [S-1, S-2, S-3]
assumed: []
unknown: []
questions: []
tests:
  - {id: TP-1, title: "HMS-5 REQ-2/REQ-3: GET patient detail with 2 Completed and 1 No-show returns visitSummary { completed: 2, noShow: 1 }", covers: [S-1/AC1], level: integration}
  - {id: TP-2, title: "HMS-5 REQ-1/REQ-2/REQ-3: the patient page shows a Visit summary above the Appointments heading with Completed 2 and No-show 1", covers: [S-1/AC1], level: unit}
  - {id: TP-3, title: "HMS-5 REQ-3: a patient with only 3 No-show appointments returns { completed: 0, noShow: 3 }", covers: [S-1/AC2], level: integration}
  - {id: TP-4, title: "HMS-5 REQ-6: a Completed appointment that started on 2 January 2025 is still counted with the clock far later", covers: [S-1/AC3], level: integration}
  - {id: TP-5, title: "HMS-5 REQ-6: the Visit summary has no period picker and no Today, Last 7 days or Last 30 days option", covers: [S-1/AC4], level: unit}
  - {id: TP-6, title: "HMS-5 REQ-2: patient A's counts exclude patient B's appointments", covers: [S-1/AC5], level: integration}
  - {id: TP-7, title: "HMS-5 REQ-4: 1 Completed, 1 Scheduled and 1 Cancelled (the latest) returns { completed: 1, noShow: 0 }", covers: [S-2/AC1], level: integration}
  - {id: TP-8, title: "HMS-5 REQ-4/REQ-5: only Scheduled and Cancelled appointments returns 0 for each", covers: [S-2/AC2], level: integration}
  - {id: TP-9, title: "HMS-5 REQ-5: a new patient with no appointments returns 0 for each in the POST response and the detail", covers: [S-2/AC3], level: integration}
  - {id: TP-10, title: "HMS-5 REQ-5: the page shows 0 for each count when the summary is zero", covers: [S-2/AC2, S-2/AC3], level: unit}
  - {id: TP-11, title: "HMS-5 REQ-4: a past Scheduled appointment that staff have not closed is not counted", covers: [S-2/AC4], level: integration}
  - {id: TP-12, title: "HMS-5 REQ-7: the detail still returns appointments upcoming first, then past, and past-only in the same order", covers: [S-3/AC1, S-3/AC2], level: integration}
  - {id: TP-13, title: "HMS-5 REQ-7: the page renders the appointment rows in the order received, for upcoming-plus-past and past-only lists", covers: [S-3/AC1, S-3/AC2], level: unit}
  - {id: TP-14, title: "HMS-5 REQ-8: medical history with entries shows the same fields and values", covers: [S-3/AC3], level: unit}
  - {id: TP-15, title: "HMS-5 REQ-8: a patient with no medical history still shows None recorded and Not recorded", covers: [S-3/AC5], level: unit}
  - {id: TP-16, title: "HMS-5 REQ-7/REQ-9: the other sections and the appointments list are still rendered with their content", covers: [S-3/AC4], level: unit}
  - {id: TP-17, title: "HMS-5 REQ-9: compare the patient page before and after the change in a browser; only the Visit summary block differs", covers: [S-3/AC4], level: manual}
  - {id: TP-18, title: "HMS-5 REQ-2 negative: GET on an unknown patient id is still 404 and carries no visitSummary", covers: [S-1/AC1], level: integration}
  - {id: TP-19, title: "HMS-5 REQ-1 negative: PATCH on an unknown patient id is still 404", covers: [S-1/AC1], level: integration}
  - {id: TP-20, title: "HMS-5 REQ-5 negative: POST with invalid fields is still 400 with one error per field and no visitSummary", covers: [S-2/AC3], level: integration}
  - {id: TP-21, title: "HMS-5 REQ-2/REQ-3 boundary: visitSummary holds exactly the keys completed and noShow, both integers", covers: [S-1/AC1], level: integration}
---
# Test plan: HMS-5

## Scope
**In scope:**
- The new `visitSummary` field on the patient detail endpoints: GET, and the POST and PATCH responses (C6).
- The Visit summary block on the patient page.
- The parts of the page that must stay unchanged: the appointments list and its order, and the medical history.

**Levels** follow the test strategy:
- Counting rules are proven at the API with Supertest on a throwaway PostgreSQL and a fixed clock (C1), so they are integration tests. They are skipped when no database is available (C4).
- Page rendering is proven with Testing Library under jsdom (C2), so those are unit tests.
- Test names start with their HMS-5 requirement id (C3).

**Out of scope:** the dashboard and the doctor pages, which this change does not touch. Performance of the count query is also out of scope.

## Tests
- TP-1 (integration, S-1/AC1): GET detail with 2 Completed and 1 No-show returns `{ completed: 2, noShow: 1 }`.
- TP-2 (unit, S-1/AC1): the page shows the Visit summary above Appointments, with Completed 2 and No-show 1.
- TP-3 (integration, S-1/AC2): only 3 No-show gives `{ completed: 0, noShow: 3 }`, so the counts are not swapped or merged.
- TP-4 (integration, S-1/AC3): a Completed appointment from 2 January 2025 is still counted, because the counts cover all time.
- TP-5 (unit, S-1/AC4): there is no period picker.
- TP-6 (integration, S-1/AC5): patient B's appointments are not counted on patient A.
- TP-7 (integration, S-2/AC1): Scheduled and the latest Cancelled are not counted.
- TP-8 (integration, S-2/AC2): only Scheduled and Cancelled gives 0 and 0.
- TP-9 (integration, S-2/AC3): a new patient gives 0 and 0 in the POST response and the detail.
- TP-10 (unit, S-2/AC2, S-2/AC3): the page shows 0 for each count.
- TP-11 (integration, S-2/AC4): a past, unclosed Scheduled appointment is not counted.
- TP-12 (integration, S-3/AC1, S-3/AC2): the API still orders appointments upcoming first, then past, and keeps a past-only list in the same order.
- TP-13 (unit, S-3/AC1, S-3/AC2): the page renders the rows in the order it receives them.
- TP-14 (unit, S-3/AC3): medical history with entries is unchanged.
- TP-15 (unit, S-3/AC5): with no medical history, the page still shows "None recorded" and "Not recorded".
- TP-16 (unit, S-3/AC4): the other sections and the appointments list are still rendered.
- TP-17 (manual, S-3/AC4): a before-and-after visual check of the page layout. This one is manual because the project has no browser tests (C5), and jsdom cannot check layout.
- TP-18 (integration, negative, S-1/AC1): GET on an unknown id is still 404.
- TP-19 (integration, negative, S-1/AC1): PATCH on an unknown id is still 404.
- TP-20 (integration, negative, S-2/AC3): POST with invalid fields is still 400 with field errors and no `visitSummary`.
- TP-21 (integration, boundary, S-1/AC1): `visitSummary` holds only the two integer keys.

Every criterion from S-1/AC1 to S-3/AC5 is covered by at least one test.

## Known
- S-1: covered by TP-1 to TP-6, TP-18, TP-19 and TP-21.
- S-2: covered by TP-7 to TP-11 and TP-20.
- S-3: covered by TP-12 to TP-17.

## Assumed
None.

## Unknown
None.

## Open questions
None.
