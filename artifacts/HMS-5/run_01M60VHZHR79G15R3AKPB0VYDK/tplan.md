---
schema: test-plan.v1
run_id: run_01M60VHZHR79G15R3AKPB0VYDK
step: tplan
attempt: 1
skill: test-plan@1.0.1
inputs:
  - path: artifacts/HMS-5/run_01M60VHZHR79G15R3AKPB0VYDK/split.md
    sha256: 70a37e2163c9ffeee2153255649beda54907325e5980a65864572ae50d997b4a
  - path: artifacts/HMS-5/run_01M60VHZHR79G15R3AKPB0VYDK/design.md
    sha256: 8deafa866532fab9ef893732597031376c69447af56a8d73760a0458cf6e701a
  - path: context/test-strategy.md
    sha256: e7465d17d8814cd71fe160b96a859f1ba1414836ad71ef48b9aaeb166259eee8
  - path: context/generated/test-layout.json
    sha256: fd4b5d51b15f9aba1d03b5f6a0350e4e4849d862118fb3a3bda7abfa5127e179
pack_sha:
  sample-system: bd98ff08b83f8762c5e5c073ba5fadef88169b33
claims:
  - id: C1
    text: "API tests use `Supertest` against a throwaway PostgreSQL 16 with a fixed clock."
    cites: ["context/test-strategy.md#layers"]
  - id: C2
    text: "Web tests use Vitest with jsdom and `Testing Library`."
    cites: ["context/test-strategy.md#layers"]
  - id: C3
    text: "Every requirement has a test whose name `starts with its ID`."
    cites: ["context/test-strategy.md#requirement-ids"]
  - id: C4
    text: "No page is tested `end to end in a browser`."
    cites: ["context/test-strategy.md#gaps"]
  - id: C5
    text: "The design adds `visit_summary` to the existing patient response."
    cites: ["artifacts/HMS-5/run_01M60VHZHR79G15R3AKPB0VYDK/design.md#overview"]
  - id: C6
    text: "The design adds `No new endpoint`, table, migration or infrastructure."
    cites: ["artifacts/HMS-5/run_01M60VHZHR79G15R3AKPB0VYDK/design.md#overview"]
trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9]
known: [REQ-1, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8]
assumed: [REQ-2, REQ-3, REQ-9]
unknown: []
questions: []
tests:
  - {id: TP-1, title: "Visit summary heading renders above the Appointments heading", covers: [S-1/AC1], level: unit}
  - {id: TP-2, title: "GET patient returns the number of Completed appointments", covers: [S-1/AC2], level: integration}
  - {id: TP-3, title: "GET patient returns the number of No-show appointments", covers: [S-1/AC3], level: integration}
  - {id: TP-4, title: "Completed and No-show appointments weeks apart are both counted, before and after the clock moves", covers: [S-1/AC4], level: integration}
  - {id: TP-5, title: "Scheduled appointments are counted in neither number", covers: [S-2/AC1], level: integration}
  - {id: TP-6, title: "Cancelled appointments are counted in neither number", covers: [S-2/AC2], level: integration}
  - {id: TP-7, title: "Patient with no Completed or No-show appointments returns 0 and 0", covers: [S-2/AC3], level: integration}
  - {id: TP-8, title: "Page renders 0 for both counts", covers: [S-2/AC3], level: unit}
  - {id: TP-9, title: "Completed-only patient returns 0 No-shows", covers: [S-2/AC4], level: integration}
  - {id: TP-10, title: "No-show-only patient returns 0 Completed", covers: [S-2/AC5], level: integration}
  - {id: TP-11, title: "Page renders 0 Completed next to a non-zero No-show count", covers: [S-2/AC5], level: unit}
  - {id: TP-12, title: "Appointments list keeps the same appointments in the same order, upcoming first", covers: [S-3/AC1], level: integration}
  - {id: TP-13, title: "Medical history is returned by the API and rendered on the page as before", covers: [S-3/AC2], level: unit}
  - {id: TP-14, title: "Patient response keeps every existing top-level field; only visit_summary is added", covers: [S-3/AC3], level: integration}
  - {id: TP-15, title: "Visual check that every patient page section other than the Visit summary looks as before", covers: [S-3/AC3], level: manual}
  - {id: TP-16, title: "Negative: unknown patient still returns 404 with no visit_summary", covers: [S-1/AC2, S-1/AC3], level: integration}
  - {id: TP-17, title: "Negative: visit_summary sent in a PATCH is ignored while the real edit applies", covers: [S-1/AC2, S-1/AC3], level: integration}
  - {id: TP-18, title: "Each patient gets only their own counts (three patients with different mixes)", covers: [S-1/AC2, S-1/AC3], level: integration}
---
# Test plan: HMS-5

## Scope
**In scope:**
- The Completed and No-show counts that `GET /api/patients/:id` returns as `visit_summary` (C5).
- The Visit summary block on the patient page.
- A check that the rest of the patient response and page is unchanged.

The design adds `No new endpoint` and no new input field (C6). The negative tests therefore target the
changed endpoint: an unknown id, and the read-only field sent back in an edit.

**Levels:**
- **integration:** API tests through Supertest against a throwaway PostgreSQL with a fixed clock (C1).
- **unit:** web component tests with Testing Library and jsdom (C2).
- **e2e:** none. The project has no browser end-to-end tests (C4).
- **manual:** one visual check, TP-15, for the whole-page regression of S-3/AC3. With no browser
  end-to-end tooling, a manual pass is the only way to check every rendered section at once. TP-14
  automates the data side of the same criterion.

Every test name starts with `HMS-5` and its REQ ids (C3).

**Out of scope:** load and performance of the per-read count query (threat-model Q-01), and the MVP
grant-all permission (security review SF-1). Neither is an acceptance criterion of this ticket.

## Tests
- TP-1 (unit): Visit summary heading renders above Appointments. Covers S-1/AC1.
- TP-2 (integration): Completed count returned. Covers S-1/AC2.
- TP-3 (integration): No-show count returned. Covers S-1/AC3.
- TP-4 (integration): closed appointments weeks apart are both counted, with the clock moved. Covers S-1/AC4.
- TP-5 (integration): Scheduled not counted. Covers S-2/AC1.
- TP-6 (integration): Cancelled not counted. Covers S-2/AC2.
- TP-7 (integration): none of either status returns 0 and 0. Covers S-2/AC3.
- TP-8 (unit): page renders 0 and 0. Covers S-2/AC3.
- TP-9 (integration): Completed-only patient has 0 No-shows. Covers S-2/AC4.
- TP-10 (integration): No-show-only patient has 0 Completed. Covers S-2/AC5.
- TP-11 (unit): page renders 0 Completed next to a non-zero No-show count. Covers S-2/AC5.
- TP-12 (integration): exact appointment order, upcoming first. Covers S-3/AC1.
- TP-13 (unit): medical history rendered as before. Covers S-3/AC2.
- TP-14 (integration): all existing top-level response fields are kept. Covers S-3/AC3.
- TP-15 (manual): visual pass over the patient page sections. Covers S-3/AC3. Manual because there is no browser e2e (C4).
- TP-16 (integration, negative): unknown patient returns 404 with no visit_summary. Covers S-1/AC2 and S-1/AC3.
- TP-17 (integration, negative): visit_summary sent in a PATCH is ignored and the real edit applies. Covers S-1/AC2 and S-1/AC3.
- TP-18 (integration): per-patient isolation across three patients. Covers S-1/AC2 and S-1/AC3.

All 12 criteria are covered: S-1/AC1 to AC4, S-2/AC1 to AC5 and S-3/AC1 to AC3. The unit-test step
already automates TP-1 to TP-13 and TP-16 to TP-18. TP-14 (the full top-level key set) is not
automated yet; it is planned as an addition to `api/test/patients-visit-summary.test.ts`.

## Known
- REQ-1: TP-1.
- REQ-4, REQ-5: TP-5 and TP-6.
- REQ-6: TP-7 to TP-11.
- REQ-7: TP-12.
- REQ-8: TP-13 to TP-15.

## Assumed
- REQ-2, REQ-3: TP-2, TP-3 and TP-16 to TP-18 assert the field names `visit_summary.completed` and `visit_summary.no_show` (C5). They change if the naming question (change Q-01) moves to camelCase.
- REQ-9: TP-4 asserts all-time counts, as designed. It changes if the product owner picks a recent period instead.

## Unknown
No item is unknown. Every acceptance criterion has at least one planned test.

## Open questions
None new. The naming question (change Q-01) and the all-time question (intent Q-03) may change the expected values in TP-2 to TP-4 and TP-16 to TP-18.
