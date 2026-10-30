---
schema: ut-result.v1
run_id: run_01M661QPHGQW4076F0GCRQ70K1
step: ut
attempt: 1
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
    head_sha: 4645dbbaad429ea1578454a65a143b5214418484
    diff_sha256: 269f0a813d7a3826983323de1540d2cd1d5e82cc8a883be0da4a0a222852faf3
    files: [web/src/__tests__/PatientsPage.test.tsx]
claims:
  - id: C1
    text: "The patient list already has the web test `PatientsPage.test.tsx`, which the new tests extend."
    cites: ["context/generated/test-layout.json#test:sample-system:web/src/__tests__/PatientsPage.test.tsx"]
  - id: C2
    text: "The change adds the count line to `PatientsPage`."
    cites: ["artifacts/LIVE-1/run_01M661QPHGQW4076F0GCRQ70K1/impl.md#summary"]
trace: [REQ-1, REQ-3, REQ-4, REQ-5]
known: [REQ-1, REQ-3, REQ-4]
assumed: [REQ-5]
unknown:
  - id: U-1
    blocking: false
questions:
  - id: Q-01
    blocking: false
    ask: tech lead
    text: "Should a page-level test also cover the count changing when the search text changes (REQ-2), or is that left to the existing search behaviour?"
    why: "The count comes from the same response as the rows, so REQ-2 follows from the existing search refresh; no test drives a search change here."
    evidence: ["artifacts/LIVE-1/run_01M661QPHGQW4076F0GCRQ70K1/impl.md#summary"]
    options: ["Add a search-change test", "Rely on the existing search refresh"]
    default: "Rely on the existing search refresh."
---
# Unit tests: LIVE-1

## Summary
Three tests extend PatientsPage.test.tsx (C1) for the count line added by the change (C2): a total larger
than the rows shown ("12 patients"), one match ("1 patient"), and no match ("0 patients" with the empty
message kept). Tests run and coverage are recorded by tsl in checks.json when it runs the verifier (H3).

## Tests
- web/src/__tests__/PatientsPage.test.tsx: "PatientsPage match count (LIVE-1)" covers REQ-1, REQ-3, REQ-4 and REQ-5.

## Known
- REQ-1: "12 patients" and "1 patient" tests.
- REQ-3, REQ-4: the no-match test checks "0 patients" and "No patients match.".

## Assumed
- REQ-5: the "12 patients" test shows the count is the total, not the two rows on the page.

## Unknown
- U-1 (non-blocking): REQ-2 (count follows a search change) has no page-level test (Q-01).

## Open questions
- Q-01 (non-blocking, tech lead): add a search-change test? Default: rely on the existing search refresh.
