---
schema: ut-result.v1
run_id: run_01M8DTDH90WA96KAFT09500VSZ
step: ut
attempt: 1
skill: unit-test@1.4.0
inputs:
  - path: artifacts/HMS-29/run_01M8DTDH90WA96KAFT09500VSZ/impl.md
    sha256: 1071d8f70af8c5b0b6f2db524634df3c9f76a9a7ebf2a6669cdbe99127208150
  - path: artifacts/HMS-29/run_01M8DTDH90WA96KAFT09500VSZ/split.md
    sha256: e3d898f637b4c06aca5f52dd5b2aa2e8805aa36cce0d6c292f70756d736be215
  - path: context/test-strategy.md
    sha256: f4bbc9d5d63dfc433c9fe466a7aa71a4fad0147baafe1f47db0c06e117ce6cba
  - path: context/generated/test-layout.json
    sha256: c0752ab8543e23547679dbe34f6381f656d36b0eb5506efb2a56f94dcb729cd6
pack_sha:
  sample-system: 7681f87749883d71abb4011d118366ca1242e1cc
claims:
  - id: C1
    text: "Web tests use Vitest with `jsdom` and Testing Library."
    cites: ["context/test-strategy.md#layers"]
  - id: C2
    text: "Every requirement has at least one test whose name `starts with its ID`."
    cites: ["context/test-strategy.md#requirement-ids"]
  - id: C3
    text: "The change modified `RefList`, the component that renders the Settings tables."
    cites: ["artifacts/HMS-29/run_01M8DTDH90WA96KAFT09500VSZ/impl.md#claim:C1"]
  - id: C4
    text: "The change added the helper `doctorsLinkLabel` for the link's accessible name."
    cites: ["artifacts/HMS-29/run_01M8DTDH90WA96KAFT09500VSZ/impl.md#claim:C2"]
  - id: C5
    text: "A department count links with the doctor list parameter `departmentId`."
    cites: ["artifacts/HMS-29/run_01M8DTDH90WA96KAFT09500VSZ/impl.md#claim:C4"]
  - id: C6
    text: "A specialization count links with the doctor list parameter `specializationId`."
    cites: ["artifacts/HMS-29/run_01M8DTDH90WA96KAFT09500VSZ/impl.md#claim:C5"]
  - id: C7
    text: "The accessible name follows `3 doctors in Cardiology` for both kinds, singular for a count of 1."
    cites:
      - "artifacts/HMS-29/run_01M8DTDH90WA96KAFT09500VSZ/impl.md#claim:C6"
      - "runs/run_01M8DTDH90WA96KAFT09500VSZ/ide-answers.md#a-01"
  - id: C8
    text: "The new test follows the neighbouring web test `web/src/__tests__/DoctorsPage.test.tsx`."
    cites: ["context/generated/test-layout.json#test:sample-system:web/src/__tests__/DoctorsPage.test.tsx"]
  - id: C9
    text: "An entry used by any doctor `cannot be` deleted, so an in-use row has no Delete action."
    cites: ["artifacts/HMS-29/run_01M8DTDH90WA96KAFT09500VSZ/split.md#claim:C9"]
  - id: C10
    text: "The verifier skips coverage because the project has no `coverage script`."
    cites: ["context/test-strategy.md#gaps"]
trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8]
known: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, S-1, S-2, S-3, S-4]
assumed: []
unknown: []
questions: []
code:
  - repo: sample-system
    base_sha: 7fe6f1e63a41e73a07151c0af7e456bcad2ede8b
    head_sha: 4a58830b9f87b5d7d65c6f4478e3b663da578425
    diff_sha256: d95a4064d50a076b1a20d97beff3c198fdefffa537da72d74795a617b55de682
    files: ["web/src/__tests__/SettingsPage.test.tsx"]
---
# Unit tests: HMS-29

## Summary
This step adds one web test file, `web/src/__tests__/SettingsPage.test.tsx`, with 13 tests. It uses
Vitest, jsdom and Testing Library as the test strategy says (C1), and every test name starts with
its requirement ID (C2). It covers the changed `RefList` Doctors cell (C3) and the new
`doctorsLinkLabel` helper (C4). It checks the normal path for both kinds (C5, C6), the edge paths
(a count of 0, a count of 1 (C7), an Inactive entry) and an error path (a failed department list).
It also covers the journey from a Settings count link into the doctor list, with that list's
filters. All data is invented ("Test Department 002", "Test Doctor 001", a dummy phone number).
"Cardiology" is used only because the acceptance criteria name it. Every network call is stubbed
with `fetch` mocks, as in the neighbouring web tests (C8). No production code changed.

Tests and coverage come from the verifier, not from this step. tsl records them in checks.json when
it runs the verifier (H3). Coverage is not reported because the project has no coverage script
(C10).

Default: the file is named after the page (`SettingsPage.test.tsx`), like `DoctorsPage.test.tsx`
(C8). Trade-off: the link-into-doctor-list tests live here and not in `DoctorsPage.test.tsx`, which
keeps each HMS-29 test in one file.

Observation outside this ticket's scope: `RefList` does not render its list load error, so a failed
department list shows an empty table with no message. HMS-29 did not change this (REQ-7 keeps the
rest of the page as it is). The error test only checks that the page fails safely, with no links
and with the other table still working.

## Tests
- web/src/__tests__/SettingsPage.test.tsx, "SettingsPage Doctors count link (HMS-29)":
  - HMS-29 REQ-1, REQ-3: a department count links to `/doctors?departmentId=1`, with no status parameter (C5).
  - HMS-29 REQ-2, REQ-3: a specialization count links to `/doctors?specializationId=11`, with no status parameter (C6).
  - HMS-29 REQ-4 edge (S-1 AC3, S-2 AC3): a count of 0 is plain text with no link, for both kinds.
  - HMS-29 REQ-5 (S-1 AC4, S-2 AC4): Inactive department and specialization rows link and are labelled the same way as Active ones.
  - HMS-29 REQ-6 (S-3 AC1, AC2): the accessible names are "3 doctors in Cardiology" and "3 doctors in Test Specialization 011" (C7).
  - HMS-29 REQ-6 edge (S-3 AC3): a count of 1 reads "1 doctor in Test Department 004" (C7).
  - HMS-29 REQ-7 (S-4 AC1): an Active row in use keeps its name, status, Rename and Deactivate, and has no Delete (C9).
  - HMS-29 REQ-7 (S-4 AC2): an unused Inactive row keeps Rename, Activate and Delete. Both add forms and the column headers are unchanged.
  - HMS-29 REQ-1 error: a failed department list renders no count links, and the specializations table still links.
- web/src/__tests__/SettingsPage.test.tsx, "Settings count link opens the doctor list (HMS-29)":
  - HMS-29 REQ-1, REQ-3 (S-1 AC1, AC2): clicking the Cardiology link opens the doctor list. The API is asked for `departmentId=1` with no status. The Department filter shows Cardiology and the Status filter shows "Any status". All 3 doctors are listed, the Inactive one included.
  - HMS-29 REQ-2, REQ-3 (S-2 AC1, AC2): clicking a specialization link opens the list on `specializationId=11`, with the department and status filters empty.
  - HMS-29 REQ-8 (S-4 AC3): the doctor list opened directly shows "All departments", "All specializations" and "Any status", and sends no filters.
  - HMS-29 REQ-8 edge (S-4 AC4): after the link, choosing "All departments" sends a new request without `departmentId`.

## Known
- REQ-1: department link href and the journey into the filtered doctor list are tested (C5).
- REQ-2: specialization link href and the journey into the filtered doctor list are tested (C6).
- REQ-3: tests check that neither the link nor the doctor list request carries a status, and that the Status filter shows "Any status".
- REQ-4: a count of 0 is tested as plain text for both kinds.
- REQ-5: Inactive rows are tested to link and label like Active ones.
- REQ-6: the plural and singular accessible names are tested for both kinds (C7).
- REQ-7: the row actions, add forms and headers are tested as unchanged (C9).
- REQ-8: the doctor list's default filters and clearing the department filter are tested.
- S-1: AC1 to AC4 are covered by the department tests above.
- S-2: AC1 to AC4 are covered by the specialization tests above.
- S-3: AC1 to AC3 are covered by the accessible-name tests.
- S-4: AC1 to AC4 are covered by the REQ-7 and REQ-8 tests.

## Assumed
None.

## Unknown
None.

## Open questions
None.
