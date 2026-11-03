---
schema: intent.v1
run_id: run_01M6FXTGC048AXKA8X69ASS5QB
step: req
attempt: 1
skill: requirement-review@1.1.0
inputs:
  - path: artifacts/HMS-7/run_01M6FXTGC048AXKA8X69ASS5QB/ticket.md
    sha256: 237bcfa3fc921ee12e96bedb190b8e0151446cda6e4d680b20d62b5cf1fcedbc
  - path: context/intent-baseline.md
    sha256: 9bfd821aeb2fb9524863e346707e049e4fcfb949a6e55b90223a79194946a215
  - path: context/glossary.md
    sha256: 7623345ad8bad33b8a14a713b452edb2a36834e8b1c2228b76f3aa154034f16e
pack_sha:
  sample-system: bd98ff08b83f8762c5e5c073ba5fadef88169b33
claims:
  - id: C1
    text: "The hospital time zone is `Asia/Kolkata`."
    cites: ["context/intent-baseline.md#constraints"]
  - id: C2
    text: "The doctor page shows the doctor's details, weekly availability, upcoming appointments and day schedule, per PRD `DOC-3`."
    cites: ["artifacts/HMS-7/run_01M6FXTGC048AXKA8X69ASS5QB/ticket.md#L17"]
  - id: C3
    text: "A `No-show` is a past appointment the patient did not attend, closed instead of staying Scheduled."
    cites: ["context/glossary.md#no-show"]
  - id: C4
    text: "A dashboard `Tile` is one count with its label; selecting it opens the list behind the count."
    cites: ["context/glossary.md#tile"]
  - id: C5
    text: "`No-show` is allowed only after the start time, and No-show appointments never change."
    cites: ["context/intent-baseline.md#closed-appointments"]
trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8]
known: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8]
assumed: []
unknown: []
requirements:
  - id: REQ-1
    title: Show the 30-day No-show count on the doctor page
    type: functional
    status: known
    statement: "The doctor page shows a field labelled `No-shows (last 30 days)` as one more labelled field inside the doctor's existing details section."
    cites:
      - "artifacts/HMS-7/run_01M6FXTGC048AXKA8X69ASS5QB/ticket.md#L20"
      - "runs/run_01M6FXTGC048AXKA8X69ASS5QB/ide-answers.md#a-03"
      - "runs/run_01M6FXTGC048AXKA8X69ASS5QB/ide-answers.md#a-04"
  - id: REQ-2
    title: Count the doctor's No-show appointments in the window
    type: functional
    status: known
    statement: "The value counts that doctor's appointments with status `No-show` whose start time falls in the last 30 days up to the end of today."
    cites:
      - "artifacts/HMS-7/run_01M6FXTGC048AXKA8X69ASS5QB/ticket.md#L21"
  - id: REQ-3
    title: Window uses the hospital time zone
    type: constraint
    status: known
    statement: "The 30-day window and \"end of today\" are computed in the hospital time zone, `Asia/Kolkata`."
    cites:
      - "artifacts/HMS-7/run_01M6FXTGC048AXKA8X69ASS5QB/ticket.md#L22"
      - "context/intent-baseline.md#constraints"
  - id: REQ-4
    title: Same window and rule as the dashboard No-shows tile
    type: constraint
    status: known
    statement: "The count reuses exactly the window and counting rule of the existing `dashboard` No-shows tile for Last 30 days, so the two agree for the same doctor's appointments."
    cites:
      - "artifacts/HMS-7/run_01M6FXTGC048AXKA8X69ASS5QB/ticket.md#L22"
      - "runs/run_01M6FXTGC048AXKA8X69ASS5QB/ide-answers.md#a-01"
  - id: REQ-5
    title: Zero is shown as 0
    type: functional
    status: known
    statement: "A doctor with no No-show appointments in the window shows `0`."
    cites:
      - "artifacts/HMS-7/run_01M6FXTGC048AXKA8X69ASS5QB/ticket.md#L24"
  - id: REQ-6
    title: Count links to the filtered appointment list
    type: functional
    status: known
    statement: "The count links to the appointment list filtered to that doctor, status `No-show` and the same 30-day window."
    cites:
      - "artifacts/HMS-7/run_01M6FXTGC048AXKA8X69ASS5QB/ticket.md#L25"
  - id: REQ-7
    title: A count of 0 is still a link
    type: functional
    status: known
    statement: "When the count is `0` it is still a link to the filtered appointment list, which then shows no appointments."
    cites:
      - "artifacts/HMS-7/run_01M6FXTGC048AXKA8X69ASS5QB/ticket.md#L24"
      - "runs/run_01M6FXTGC048AXKA8X69ASS5QB/ide-answers.md#a-02"
  - id: REQ-8
    title: Rest of the doctor page unchanged
    type: constraint
    status: known
    statement: "Apart from the new field, the doctor page is `unchanged`: details, weekly availability, upcoming appointments and day schedule behave as before."
    cites:
      - "artifacts/HMS-7/run_01M6FXTGC048AXKA8X69ASS5QB/ticket.md#L26"
      - "runs/run_01M6FXTGC048AXKA8X69ASS5QB/ide-answers.md#a-04"
unknowns: []
questions: []
answers:
  - id: A-01
    kind: question
    about: Q-01
    answer: "Reuse dashboard tile (proposed default)"
    requirements: [REQ-4]
  - id: A-02
    kind: question
    about: Q-02
    answer: "Yes, 0 links too (proposed default)"
    requirements: [REQ-7]
  - id: A-03
    kind: assumption
    about: REQ-1
    answer: "Yes, confirm"
    requirements: [REQ-1]
  - id: A-04
    kind: question
    about: Q-03
    answer: "Use ticket meaning (proposed default)"
    requirements: [REQ-1, REQ-8]
glossary:
  - {term: No-show, found: true}
  - {term: Dashboard, found: true}
  - {term: Tile, found: true}
  - {term: Front-desk staff, found: true}
  - {term: Doctor page, found: false}
---
# Requirements review: HMS-7

## Summary
HMS-7 asks for a "No-shows (last 30 days)" count on the doctor page so front-desk staff can spot
doctors whose patients need reminder calls. The count covers the doctor's No-show appointments
starting in the last 30 days up to the end of today in Asia/Kolkata, and must match the dashboard's
No-shows tile for Last 30 days. The main risk is drift between the two counts: the user decided the
doctor page reuses the dashboard tile's window and rule rather than defining its own. The second risk
is the link target: the appointment list must accept a doctor, status and date-window filter that
yields the same set as the count. Note the context pack is 2 commits behind sample-system (E-CTX-10).

## Requirements
- REQ-1 Show the 30-day No-show count on the doctor page — known — labelled field "No-shows (last 30 days)" inside the details section.
- REQ-2 Count the doctor's No-show appointments in the window — known — No-show appointments starting in the last 30 days up to end of today.
- REQ-3 Window uses the hospital time zone — known — computed in Asia/Kolkata.
- REQ-4 Same window and rule as the dashboard No-shows tile — known — reuse the tile's Last 30 days window and rule.
- REQ-5 Zero is shown as 0 — known.
- REQ-6 Count links to the filtered appointment list — known — filtered by doctor, No-show and the 30-day window.
- REQ-7 A count of 0 is still a link — known.
- REQ-8 Rest of the doctor page unchanged — known.

## Known
- REQ-1: ticket L20, placement confirmed by A-03; "doctor page" meaning from A-04.
- REQ-2: ticket L21.
- REQ-3: ticket L22 and intent baseline constraints (Asia/Kolkata).
- REQ-4: ticket L22-23, decided by A-01.
- REQ-5: ticket L24.
- REQ-6: ticket L25.
- REQ-7: ticket L24, decided by A-02.
- REQ-8: ticket L26.

## Assumed
None.

## Unknown
None.

## Open questions
None open.

Answered in the IDE:
- A-01 (Q-01, answered by the user): How is the 30-day window defined? → "Reuse dashboard tile (proposed default)". Decides REQ-4. No mismatch.
- A-02 (Q-02, answered by the user): Should a count of 0 also be a link? → "Yes, 0 links too (proposed default)". Decides REQ-7. No mismatch.
- A-03 (REQ-1 assumption, answered by the user): The field sits inside the existing details section → "Yes, confirm". Decides REQ-1.
- A-04 (Q-03, answered by the user): "Doctor page" is not in the glossary; use the ticket's meaning (PRD DOC-3) → "Use ticket meaning (proposed default)". Decides REQ-1, REQ-8. No mismatch; the tech lead may add the term to the glossary.

## Out of scope
- The user-story framing (reminder calls, ticket L13-14) is the motivation; making reminder calls is not a requirement of this ticket.
- Changes to the dashboard No-shows tile itself: the ticket only asks to count the same way (L22-23).
