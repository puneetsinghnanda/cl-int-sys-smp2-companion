---
schema: test-plan.v1
run_id: run_01M5VBPY40KETZZR1EY7H5KK0V
step: tplan
attempt: 1
skill: test-plan@1.0.1
inputs:
  - path: artifacts/HMS-1/run_01M5VBPY40KETZZR1EY7H5KK0V/split.md
    sha256: 8193194ed9f5168ece5dab22f4c694ae9fe98e787bfd5dd0a55ae234091fed5e
  - path: artifacts/HMS-1/run_01M5VBPY40KETZZR1EY7H5KK0V/design.md
    sha256: b35d0758b7a70010b1e5bf16befa8a356a6c9c2b09beb353e093f34384cad393
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
    text: "Web tests today cover the dashboard links, the slot picker and formatting only; `pages` are not tested end to end."
    cites: ["context/test-strategy.md#gaps"]
  - id: C5
    text: "The design adds `next_appointment` to each GET /api/patients list item."
    cites: ["artifacts/HMS-1/run_01M5VBPY40KETZZR1EY7H5KK0V/design.md#L174"]
trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10, REQ-11, REQ-12, REQ-13]
known: [TP-2, TP-3, TP-5, TP-6, TP-7, TP-8, TP-9, TP-11, TP-12, TP-13, TP-14, TP-15, TP-16]
assumed: [TP-1, TP-4, TP-10, TP-17, TP-18]
unknown:
  - {id: U-1, blocking: false}
questions:
  - id: Q-01
    blocking: false
    ask: tech lead
    text: "Should the web tests for PatientsPage (column, time zone, link, None) be added now, given that pages have no web tests today?"
    why: "TP-1, TP-4, TP-10, TP-17 and TP-18 need a PatientsPage test with a mocked API client; the strategy lists pages as a known gap."
    evidence: ["context/test-strategy.md#gaps", "context/test-strategy.md#layers"]
    options: ["Add PatientsPage web tests in this ticket", "Cover the page manually for now"]
    default: "Add them as unit-level web tests with a mocked api.patients."
tests:
  - {id: TP-1, title: "HMS-1 REQ-1: PatientsPage shows a Next appointment column header", covers: [S-1/AC1], level: unit}
  - {id: TP-2, title: "HMS-1 REQ-2: GET /api/patients returns the earliest Scheduled appointment after now", covers: [S-1/AC2], level: integration}
  - {id: TP-3, title: "HMS-1 REQ-3: next_appointment.starts_at is the appointment start as a UTC instant", covers: [S-1/AC3], level: integration}
  - {id: TP-4, title: "HMS-1 REQ-4: PatientsPage formats the start in the settings time zone", covers: [S-1/AC4], level: unit}
  - {id: TP-5, title: "HMS-1 REQ-5: next_appointment.doctor_name is the appointment's doctor", covers: [S-1/AC5], level: integration}
  - {id: TP-6, title: "HMS-1 REQ-6: only Cancelled appointments after now give null", covers: [S-2/AC1], level: integration}
  - {id: TP-7, title: "HMS-1 REQ-6: a Cancelled appointment before a later Scheduled one is skipped", covers: [S-2/AC2], level: integration}
  - {id: TP-8, title: "HMS-1 REQ-6: Completed and No-show appointments are skipped", covers: [S-2/AC3], level: integration}
  - {id: TP-9, title: "HMS-1 REQ-7: a patient with no upcoming Scheduled appointment gets null", covers: [S-2/AC4], level: integration}
  - {id: TP-10, title: "HMS-1 REQ-8: the column value links to /appointments/:id", covers: [S-3/AC1], level: unit}
  - {id: TP-11, title: "HMS-1 REQ-13: an Inactive doctor's name is still returned", covers: [S-4/AC1], level: integration}
  - {id: TP-12, title: "HMS-1 REQ-9: with more than 20 patients the list returns 20 per page and a second page", covers: [S-5/AC1], level: integration}
  - {id: TP-13, title: "HMS-1 REQ-10: search by name, code and phone returns the same patients", covers: [S-5/AC2], level: integration}
  - {id: TP-14, title: "HMS-1 REQ-11: the list stays newest first", covers: [S-5/AC3], level: integration}
  - {id: TP-15, title: "HMS-1 REQ-12 / PAT-8: list items carry no medical history and next_appointment has only id, starts_at, doctor_name", covers: [S-5/AC4], level: integration}
  - {id: TP-16, title: "HMS-1 REQ-10 negative: an invalid list filter is still rejected with a field error", covers: [S-5/AC2], level: integration}
  - {id: TP-17, title: "HMS-1 REQ-4 negative: an invalid or missing time zone falls back to the default zone", covers: [S-1/AC4], level: unit}
  - {id: TP-18, title: "HMS-1 REQ-7: PatientsPage shows None as plain text when next_appointment is null", covers: [S-2/AC4], level: unit}
---
# Test plan: HMS-1

## Scope
In scope: the widened GET /api/patients response (`next_appointment`, C5) and the new PatientsPage column. API behaviour is proved at integration level with Vitest and Supertest against a throwaway PostgreSQL with a fixed clock (C1); page rendering at unit level with Vitest, jsdom and Testing Library and a mocked API client (C2). Test names start with their requirement id (C3). Out of scope: end-to-end browser tests, and the appointment page itself, which is unchanged.

## Tests
- TP-1 (unit): PatientsPage shows a Next appointment column header — S-1/AC1.
- TP-2 (integration): earliest Scheduled appointment after now is returned — S-1/AC2.
- TP-3 (integration): starts_at is the start as a UTC instant — S-1/AC3.
- TP-4 (unit): the page formats the start in the settings time zone — S-1/AC4.
- TP-5 (integration): doctor_name is the appointment's doctor — S-1/AC5.
- TP-6 (integration): only Cancelled appointments after now give null — S-2/AC1.
- TP-7 (integration): a Cancelled earlier appointment is skipped for a later Scheduled one — S-2/AC2.
- TP-8 (integration): Completed and No-show appointments are skipped — S-2/AC3.
- TP-9 (integration): no upcoming Scheduled appointment gives null — S-2/AC4.
- TP-10 (unit): the value links to /appointments/:id — S-3/AC1.
- TP-11 (integration): an Inactive doctor's name is still returned — S-4/AC1.
- TP-12 (integration): 20 per page with a second page when there are more than 20 patients — S-5/AC1.
- TP-13 (integration): search by name, code and phone is unchanged — S-5/AC2.
- TP-14 (integration): newest-first order is unchanged — S-5/AC3.
- TP-15 (integration): no medical history in list items; next_appointment keys are exact — S-5/AC4.
- TP-16 (integration, negative): an invalid list filter is still rejected — S-5/AC2.
- TP-17 (unit, negative): an invalid or missing zone falls back to the default — S-1/AC4.
- TP-18 (unit): None is shown as plain text when next_appointment is null — S-2/AC4.

## Known
- TP-2, TP-3, TP-5, TP-6, TP-7, TP-8, TP-9, TP-11, TP-12, TP-13, TP-14, TP-15, TP-16: API behaviour at the integration level the strategy uses (C1); most already exist in api/test/patients-next-appointment.test.ts and patients.test.ts (PAT-8), and TP-6 and TP-12 are new.

## Assumed
- TP-1, TP-4, TP-10, TP-17, TP-18: page tests at unit level with jsdom and a mocked API client (C2). Basis: the strategy's web layer; pages have no web tests yet (C4), so these are new (Q-01).

## Unknown
- U-1 (non-blocking): whether the PatientsPage web tests are added in this ticket (Q-01).

## Open questions
- Q-01 (non-blocking, tech lead): add PatientsPage web tests now? Default: add them as unit-level web tests with a mocked api.patients.
