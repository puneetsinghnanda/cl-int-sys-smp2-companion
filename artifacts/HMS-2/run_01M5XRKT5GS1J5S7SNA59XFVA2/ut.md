---
schema: ut-result.v1
run_id: run_01M5XRKT5GS1J5S7SNA59XFVA2
step: ut
attempt: 1
skill: unit-test@1.0.1
inputs:
  - path: artifacts/HMS-2/run_01M5XRKT5GS1J5S7SNA59XFVA2/impl.md
    sha256: e67aefd7dd1565a585efb3ea2a7066985f8d05f557a46645679ad8990cc0a92a
  - path: context/test-strategy.md
    sha256: 1869d3aed8521e40123c46f16539c5ea10ae5ac8c9d872ebc8a1f48b704ce150
  - path: context/generated/test-layout.json
    sha256: bd0686fe34afae476a1ec3105bcf1039ce7e5c510941d1bcdc09ab2ca8e74f3c
pack_sha:
  sample-system: 415369398bdad3122ac084a00766a01a3034f900
claims:
  - id: C1
    text: "Web tests use Vitest with `jsdom` and Testing Library."
    cites: ["context/test-strategy.md#layers"]
  - id: C2
    text: "Test names start with the requirement `ID` they cover."
    cites: ["context/test-strategy.md#requirement-ids"]
  - id: C3
    text: "The change makes the `empty branch` of the Next appointment cell render '-'."
    cites: ["artifacts/HMS-2/run_01M5XRKT5GS1J5S7SNA59XFVA2/impl.md#L47"]
  - id: C4
    text: "Before this change, `pages` had no end-to-end web tests."
    cites: ["context/test-strategy.md#gaps"]
trace: [HMS-2]
known: [T-1, T-2]
assumed: []
unknown: []
questions: []
code:
  - repo: sample-system
    base_sha: fcbae730d0545661b11d9df17d365da5cf4b6a4f
    head_sha: 947748e39abbcfccb46b33b8bb1e10770d195d55
    diff_sha256: ee1a83fb739a25b63ff4bd85fd1232c05d7c142eaf363cd2cda7fd42afc578db
    files: [web/src/__tests__/PatientsPage.test.tsx]
---
# Unit tests: HMS-2

## Summary
Adds the first page-level web test, `web/src/__tests__/PatientsPage.test.tsx`, with Vitest, jsdom
and Testing Library (C1). It renders the patient list with a stubbed API response and checks the
Next appointment cell for both branches of the change (C3). Test names start with HMS-2 (C2). Tests
run and coverage come from the verifier (H3); tsl records them, so no numbers are written here.

## Tests
- `web/src/__tests__/PatientsPage.test.tsx`:
  - T-1 (edge path, the fix): a patient with `next_appointment: null` shows exactly "-" in the Next
    appointment cell, with no link.
  - T-2 (normal path, unchanged): a patient with an upcoming appointment still shows a link to
    `/appointments/7` with the doctor's name, not "-".

## Known
- T-1: proves the changed empty branch renders "-" (C3).
- T-2: proves the non-empty branch is unchanged, so the fix did not touch it (C3).

## Assumed
- None.

## Unknown
- None.

## Open questions
- None.
