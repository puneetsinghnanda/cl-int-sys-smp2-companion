---
schema: change.v1
run_id: run_01M661QPHGQW4076F0GCRQ70K1
step: impl
attempt: 1
skill: implement@1.0.1
inputs:
  - path: artifacts/LIVE-1/run_01M661QPHGQW4076F0GCRQ70K1/design.md
    sha256: 352b347ebaf9c4496e3c68531a5185b15d70f3e30799ca9cca5f6d5ce2f71d58
  - path: artifacts/LIVE-1/run_01M661QPHGQW4076F0GCRQ70K1/split.md
    sha256: b4e6622becffa36ae90e954275eb94df9ea45e61936a5f3218496527b6f429e9
  - path: context/conventions.md
    sha256: dc53d94ec34eb193eeb06fdd0db19eb86a24c29f7d3c434e7c212d61038ca852
  - path: context/generated/module-map.json
    sha256: 04f84ff09b4ad6ffaba59ec846716d25f03c8aae22292958a47ebb9247632345
pack_sha:
  sample-system: bd98ff08b83f8762c5e5c073ba5fadef88169b33
code:
  - repo: sample-system
    base_sha: bd98ff08b83f8762c5e5c073ba5fadef88169b33
    head_sha: ab275efb2b582ea32ea7fe1023bd801ab48824a1
    diff_sha256: 167920cd5dc2d0c4e15c5e535c3f19ac038d76c8c1f52fc639c6d388f8bcabe0
    files: [web/src/pages/PatientsPage.tsx]
claims:
  - id: C1
    text: "List routes answer with `total` next to the items of one page."
    cites: ["context/conventions.md#lists"]
  - id: C2
    text: "The patient list is the `PatientsPage` component of the web module."
    cites: ["context/generated/module-map.json#sym:sample-system:web/src/pages/PatientsPage.tsx:PatientsPage"]
trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5]
known: [REQ-1, REQ-2, REQ-3, REQ-4]
assumed: [REQ-5]
unknown: []
questions: []
summary: "PatientsPage shows '<total> patients' (or '1 patient') above the table from the list response's total; the empty message stays."
---
# Change: LIVE-1

## Summary
PatientsPage (design component, changed) now shows a count line above the patient table, built from the
`total` the existing list response already carries (C1). The count comes with the same response as the
rows, so it follows the search text, counts every match rather than one page (REQ-5), reads "0 patients"
with no match while the existing "No patients match." message stays, and reads "1 patient" for one match.
The API is unchanged, as the design says.

## Changes
- web/src/pages/PatientsPage.tsx: count line from list.data.total (PatientsPage component).

## Known
- REQ-1, REQ-2, REQ-3, REQ-4: served by the count line and the kept empty message.

## Assumed
- REQ-5: the count is of every match (intent Q-01 default), which the list total gives (C1).

## Unknown
- None.

## Open questions
- None.
