---
schema: ut-result.v1
run_id: run_01M661QPHGQW4076F0GCRQ70K1
step: ut
attempt: 2
skill: unit-test@1.0.1
inputs:
  - path: artifacts/LIVE-1/run_01M661QPHGQW4076F0GCRQ70K1/impl.md
    sha256: 6f1164dd7a85ae0205169ebe51576b51fa9fe8d71f38696ad6669cf39a480b59
  - path: artifacts/LIVE-1/run_01M661QPHGQW4076F0GCRQ70K1/split.md
    sha256: b4e6622becffa36ae90e954275eb94df9ea45e61936a5f3218496527b6f429e9
  - path: context/test-strategy.md
    sha256: e7465d17d8814cd71fe160b96a859f1ba1414836ad71ef48b9aaeb166259eee8
  - path: context/generated/test-layout.json
    sha256: fd4b5d51b15f9aba1d03b5f6a0350e4e4849d862118fb3a3bda7abfa5127e179
pack_sha:
  sample-system: bd98ff08b83f8762c5e5c073ba5fadef88169b33
code:
  - repo: sample-system
    base_sha: 71c7fc8052eecc7f932e6e9999f512a4e4c3a154
    head_sha: 0671fdc6a62def86eed0c78c977232030184a2fe
    diff_sha256: e7cadbe9112c352632e43146bae17a1f0398651a34644d2497a631526a647bd1
    files: [web/src/__tests__/PatientsPage.test.tsx]
claims:
  - id: C1
    text: "The patient list already has the web test `PatientsPage.test.tsx`, which the new tests extend."
    cites: ["context/generated/test-layout.json#test:sample-system:web/src/__tests__/PatientsPage.test.tsx"]
  - id: C2
    text: "The change adds the count line to `PatientsPage`."
    cites: ["artifacts/LIVE-1/run_01M661QPHGQW4076F0GCRQ70K1/impl.md#summary"]
trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5]
known: [REQ-1, REQ-2, REQ-3, REQ-4]
assumed: [REQ-5]
unknown: []
questions: []
resolves:
  - id: F-1
    how: "Added the REQ-2 test: the first list response has total 12, a search change makes the next response total 3, and the test checks the count line moves from '12 patients' to '3 patients'."
---
# Unit tests: LIVE-1

## Summary
Five tests extend PatientsPage.test.tsx (C1) for the count line added by the change (C2):
- A total larger than the rows shown ("12 patients", and the page-row count "2 patients" is absent).
- One match ("1 patient").
- No match ("0 patients", with the empty message kept).
- The count following a change of the search text.
- A failed list request, which shows the error and no count line.

tsl records the test run and coverage in checks.json when it runs the verifier (H3).

## Tests
- web/src/__tests__/PatientsPage.test.tsx: "PatientsPage match count (LIVE-1)" covers REQ-1, REQ-2, REQ-3, REQ-4 and REQ-5, plus the error path.

## Known
- REQ-1: the "12 patients" and "1 patient" tests, and the failed-request test (no count line).
- REQ-2: the search-change test, where the count moves from 12 to 3.
- REQ-3, REQ-4: the no-match test checks "0 patients" and "No patients match.".

## Assumed
- REQ-5: the "12 patients" test shows that the count is the total, not the two rows on the page.

## Unknown
- None.

## Open questions
- None.
