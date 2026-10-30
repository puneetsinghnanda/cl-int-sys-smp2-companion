---
schema: test-plan.v1
run_id: run_01M661QPHGQW4076F0GCRQ70K1
step: tplan
attempt: 1
skill: test-plan@1.0.1
inputs:
  - path: artifacts/LIVE-1/run_01M661QPHGQW4076F0GCRQ70K1/split.md
    sha256: b4e6622becffa36ae90e954275eb94df9ea45e61936a5f3218496527b6f429e9
  - path: artifacts/LIVE-1/run_01M661QPHGQW4076F0GCRQ70K1/design.md
    sha256: 352b347ebaf9c4496e3c68531a5185b15d70f3e30799ca9cca5f6d5ce2f71d58
  - path: context/test-strategy.md
    sha256: e7465d17d8814cd71fe160b96a859f1ba1414836ad71ef48b9aaeb166259eee8
  - path: context/generated/test-layout.json
    sha256: fd4b5d51b15f9aba1d03b5f6a0350e4e4849d862118fb3a3bda7abfa5127e179
pack_sha:
  sample-system: bd98ff08b83f8762c5e5c073ba5fadef88169b33
claims:
  - id: C1
    text: "Web tests use Vitest with jsdom and `Testing Library`."
    cites: ["context/test-strategy.md#layers"]
  - id: C2
    text: "The patient list already has the web test `PatientsPage.test.tsx`, which these tests extend."
    cites: ["context/generated/test-layout.json#test:sample-system:web/src/__tests__/PatientsPage.test.tsx"]
trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5]
known: [TP-1, TP-2, TP-4, TP-5, TP-6]
assumed: [TP-3]
unknown: []
questions: []
tests:
  - id: TP-1
    title: "A list response whose total is 12 shows '12 patients'"
    covers: [S-1/AC1]
    level: unit
  - id: TP-2
    title: "Changing the search text refetches and the count follows the new total"
    covers: [S-1/AC2]
    level: unit
  - id: TP-3
    title: "A total larger than the rows on the page is shown, not the row count"
    covers: [S-1/AC3]
    level: unit
  - id: TP-4
    title: "A total of 0 shows '0 patients'"
    covers: [S-2/AC1]
    level: unit
  - id: TP-5
    title: "With no match, the existing empty message is still shown under the count"
    covers: [S-2/AC2]
    level: unit
  - id: TP-6
    title: "Negative: a failed list request shows the error and no count line"
    covers: [S-1/AC1]
    level: unit
---
# Test plan: LIVE-1

## Scope
In scope: the count line on the patient list, tested as web unit tests in the existing PatientsPage test
file (C2) with Vitest and Testing Library (C1). The list API is unchanged in the design, so no new endpoint
or input field needs a negative API test. The API's existing list tests stay as they are.
Out of scope: changes to the list API and to pagination.

## Tests
- TP-1 (unit): a total of 12 shows "12 patients". Covers S-1/AC1.
- TP-2 (unit): a search change refetches, and the count follows the new total. Covers S-1/AC2.
- TP-3 (unit): a total larger than the rows on the page is shown, not the row count. Covers S-1/AC3.
- TP-4 (unit): a total of 0 shows "0 patients". Covers S-2/AC1.
- TP-5 (unit): with no match, the empty message is still shown. Covers S-2/AC2.
- TP-6 (unit, negative): a failed request shows the error and no count line. Covers S-1/AC1.

## Known
- TP-1, TP-2, TP-6: prove S-1/AC1 and S-1/AC2, from the known REQ-1 and REQ-2.
- TP-4, TP-5: prove S-2/AC1 and S-2/AC2, from the known REQ-3 and REQ-4.

## Assumed
- TP-3: proves S-1/AC3, which relies on the assumed REQ-5 (count every match, not one page), as the stories state.

## Unknown
- None.

## Open questions
- None.
