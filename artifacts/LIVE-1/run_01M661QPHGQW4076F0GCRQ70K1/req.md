---
schema: intent.v1
run_id: run_01M661QPHGQW4076F0GCRQ70K1
step: req
attempt: 1
skill: requirement-review@1.0.0
inputs:
  - path: artifacts/LIVE-1/run_01M661QPHGQW4076F0GCRQ70K1/ticket.md
    sha256: 1fdcf1c03e7301a68b705604e90d282f2f7db7e021b61535b45416762742fb08
  - path: context/intent-baseline.md
    sha256: 9bfd821aeb2fb9524863e346707e049e4fcfb949a6e55b90223a79194946a215
  - path: context/glossary.md
    sha256: 7623345ad8bad33b8a14a713b452edb2a36834e8b1c2228b76f3aa154034f16e
pack_sha:
  sample-system: bd98ff08b83f8762c5e5c073ba5fadef88169b33
claims:
  - id: C1
    text: "The patient list already supports `search` and sort."
    cites: ["artifacts/LIVE-1/run_01M661QPHGQW4076F0GCRQ70K1/ticket.md#L16"]
  - id: C2
    text: "The users of the patient list are `Front-desk staff`, who register patients and book appointments."
    cites: ["context/glossary.md#front-desk-staff"]
  - id: C3
    text: "A system goal is that staff manage `patients` end to end."
    cites: ["context/intent-baseline.md#goals"]
trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5]
known: [REQ-1, REQ-2, REQ-3, REQ-4]
assumed: [REQ-5]
unknown: [U-1, U-2, U-3]
requirements:
  - id: REQ-1
    title: Show the number of matching patients
    type: functional
    status: known
    statement: "The patient list shows the number of patients that match the current search, in the form `12 patients`."
    cites: ["artifacts/LIVE-1/run_01M661QPHGQW4076F0GCRQ70K1/ticket.md#L19", "artifacts/LIVE-1/run_01M661QPHGQW4076F0GCRQ70K1/ticket.md#L20"]
  - id: REQ-2
    title: Update the number when the search changes
    type: functional
    status: known
    statement: "The number of patients shown updates when the `search text` changes."
    cites: ["artifacts/LIVE-1/run_01M661QPHGQW4076F0GCRQ70K1/ticket.md#L21"]
  - id: REQ-3
    title: Show zero when nothing matches
    type: functional
    status: known
    statement: "When no patient matches the search, the list shows `0 patients`."
    cites: ["artifacts/LIVE-1/run_01M661QPHGQW4076F0GCRQ70K1/ticket.md#L22"]
  - id: REQ-4
    title: Keep the existing empty message
    type: constraint
    status: known
    statement: "When no patient matches, the list keeps its existing `empty message` next to the zero count."
    cites: ["artifacts/LIVE-1/run_01M661QPHGQW4076F0GCRQ70K1/ticket.md#L22"]
  - id: REQ-5
    title: Count all matches, not only the rows on screen
    type: functional
    status: assumed
    statement: "The number counts every patient that `match` the current search, even if the list shows them over more than one page."
    basis: "The acceptance criterion counts patients that match the current search, while the story speaks of the patients the list is showing; the acceptance criterion is the more precise wording."
    cites: ["artifacts/LIVE-1/run_01M661QPHGQW4076F0GCRQ70K1/ticket.md#L19", "artifacts/LIVE-1/run_01M661QPHGQW4076F0GCRQ70K1/ticket.md#L13"]
unknowns:
  - id: U-1
    text: "If the patient list is paged, the ticket does not say whether the number is all matches or only the rows on the current page."
    question: Q-01
  - id: U-2
    text: "The ticket gives the plural form only; the wording for exactly one match is not stated."
    question: Q-02
  - id: U-3
    text: "\"Patient list\" is not a glossary term."
    question: Q-03
questions:
  - id: Q-01
    blocking: false
    ask: product owner
    text: "Should the number count every matching patient, or only the patients on the current page?"
    why: "The story and the acceptance criterion read differently when the list is paged."
    evidence: ["artifacts/LIVE-1/run_01M661QPHGQW4076F0GCRQ70K1/ticket.md#L13", "artifacts/LIVE-1/run_01M661QPHGQW4076F0GCRQ70K1/ticket.md#L19"]
    options: ["Every matching patient", "Only the patients on the current page"]
    default: "Every matching patient, as the acceptance criterion says."
  - id: Q-02
    blocking: false
    ask: product owner
    text: "What should the list say when exactly one patient matches?"
    why: "The ticket shows only the plural form."
    evidence: ["artifacts/LIVE-1/run_01M661QPHGQW4076F0GCRQ70K1/ticket.md#L20"]
    options: ["1 patient", "1 patients"]
    default: "1 patient"
  - id: Q-03
    blocking: false
    ask: tech lead
    text: "Should \"patient list\" be added to the glossary?"
    why: "The ticket names the screen, but the glossary does not define it."
    evidence: ["context/glossary.md#glossary"]
    options: ["Add it", "Leave it out"]
    default: "Leave it out; the ticket's own wording is clear enough for this change."
glossary:
  - term: front-desk staff
    found: true
  - term: patient list
    found: false
---
# Requirements review: LIVE-1

## Summary
LIVE-1 asks the patient list to show how many patients match the current search, for example "12 patients".
The number must follow the search text, and with no match it reads "0 patients" while the existing empty
message stays. The main risk is paging: if the list is paged, "patients showing" and "patients matching"
differ, so REQ-5 assumes the count is of every match (Q-01).

## Requirements
- REQ-1 Show the number of matching patients (known): the list shows the number of matching patients, as "12 patients".
- REQ-2 Update the number when the search changes (known): the number updates when the search text changes.
- REQ-3 Show zero when nothing matches (known): with no match, the list shows "0 patients".
- REQ-4 Keep the existing empty message (known): with no match, the existing empty message stays.
- REQ-5 Count all matches, not only the rows on screen (assumed): the number counts every matching patient.

## Known
- REQ-1: stated in the ticket's first acceptance criterion.
- REQ-2: stated in the ticket's second acceptance criterion.
- REQ-3: stated in the ticket's third acceptance criterion.
- REQ-4: stated in the ticket's third acceptance criterion.

## Assumed
- REQ-5: the count covers every match, not one page. Basis: the acceptance criterion says "match the current search".

## Unknown
- U-1: whether the number is all matches or one page when the list is paged (Q-01).
- U-2: the wording for exactly one match (Q-02).
- U-3: "patient list" is not in the glossary (Q-03).

## Open questions
- Q-01 (non-blocking, product owner): count every match or only the current page? Default: every match.
- Q-02 (non-blocking, product owner): wording for one match? Default: "1 patient".
- Q-03 (non-blocking, tech lead): add "patient list" to the glossary? Default: leave it out.

## Out of scope
- How the count looks (position, style): the ticket does not ask for a layout, so the design step decides.
