---
schema: intent.v1
run_id: run_01M5XZYC101HCFE34KWJ49C90V
step: req
attempt: 1
skill: requirement-review@1.0.0
inputs:
  - path: artifacts/HMS-3/run_01M5XZYC101HCFE34KWJ49C90V/ticket.md
    sha256: 7778967bf9f1a7a96618d81edaef70f7df009b1bbbe21889541aee6bc64492fb
  - path: context/intent-baseline.md
    sha256: 9bfd821aeb2fb9524863e346707e049e4fcfb949a6e55b90223a79194946a215
  - path: context/glossary.md
    sha256: 9bfd556283382d3d3835bf3b12514acc4f6941dc24f21cba0c8eaa4cd658a36c
pack_sha:
  sample-system: 415369398bdad3122ac084a00766a01a3034f900
claims:
  - id: C1
    text: "A `No-show` is a past appointment the patient did not attend, closed instead of staying Scheduled."
    cites: ["context/glossary.md#no-show"]
  - id: C2
    text: "Medical history never appears on the `dashboard`."
    cites: ["context/intent-baseline.md#data-protection"]
  - id: C3
    text: "The `dashboard` shows the day in one screen."
    cites: ["context/intent-baseline.md#goals"]
  - id: C4
    text: "Completed and No-show are allowed only `after the start time`."
    cites: ["context/intent-baseline.md#closed-appointments"]
  - id: C5
    text: "The hospital time zone is `Asia/Kolkata`."
    cites: ["context/intent-baseline.md#constraints"]
trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10]
known: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10]
assumed: []
unknown: [U-1, U-2, U-3, U-4, U-5]
requirements:
  - id: REQ-1
    title: No-shows tile
    type: functional
    status: known
    statement: "The dashboard has a tile titled `No-shows`."
    cites: ["artifacts/HMS-3/run_01M5XZYC101HCFE34KWJ49C90V/ticket.md#L20"]
  - id: REQ-2
    title: What the tile counts
    type: functional
    status: known
    statement: "The tile shows the number of `No-show` appointments that start in the selected period."
    cites: ["artifacts/HMS-3/run_01M5XZYC101HCFE34KWJ49C90V/ticket.md#L21", "context/glossary.md#no-show"]
  - id: REQ-3
    title: Period choices
    type: functional
    status: known
    statement: "The selected period is one of `Today`, Last 7 days or Last 30 days."
    cites: ["artifacts/HMS-3/run_01M5XZYC101HCFE34KWJ49C90V/ticket.md#L21", "artifacts/HMS-3/run_01M5XZYC101HCFE34KWJ49C90V/ticket.md#L22"]
  - id: REQ-4
    title: Counted like Completed and Cancelled
    type: constraint
    status: known
    statement: "No-shows are counted the same way as the `Completed and Cancelled` tiles count their appointments."
    cites: ["artifacts/HMS-3/run_01M5XZYC101HCFE34KWJ49C90V/ticket.md#L22", "artifacts/HMS-3/run_01M5XZYC101HCFE34KWJ49C90V/ticket.md#L16"]
  - id: REQ-5
    title: Count follows the period
    type: functional
    status: known
    statement: "Changing the period `updates` the No-shows count, as it does for Completed and Cancelled."
    cites: ["artifacts/HMS-3/run_01M5XZYC101HCFE34KWJ49C90V/ticket.md#L23"]
  - id: REQ-6
    title: Tile links to the No-show list
    type: functional
    status: known
    statement: "The tile `links` to the appointment list filtered to No-show."
    cites: ["artifacts/HMS-3/run_01M5XZYC101HCFE34KWJ49C90V/ticket.md#L24"]
  - id: REQ-7
    title: Link keeps the period
    type: functional
    status: known
    statement: "The link filters the appointment list to the `selected period`."
    cites: ["artifacts/HMS-3/run_01M5XZYC101HCFE34KWJ49C90V/ticket.md#L24"]
  - id: REQ-8
    title: Other tiles unchanged
    type: constraint
    status: known
    statement: "The other tiles and their counts are `unchanged`."
    cites: ["artifacts/HMS-3/run_01M5XZYC101HCFE34KWJ49C90V/ticket.md#L26"]
  - id: REQ-9
    title: No medical history on the dashboard
    type: constraint
    status: known
    statement: "No `medical history` appears on the dashboard."
    cites: ["artifacts/HMS-3/run_01M5XZYC101HCFE34KWJ49C90V/ticket.md#L27", "context/intent-baseline.md#data-protection"]
  - id: REQ-10
    title: No contact details on the dashboard
    type: constraint
    status: known
    statement: "No `contact details` appear on the dashboard."
    cites: ["artifacts/HMS-3/run_01M5XZYC101HCFE34KWJ49C90V/ticket.md#L27"]
unknowns:
  - id: U-1
    text: "The ticket cites PRD DSH-5 to DSH-9, which are not among the inputs; how the existing tiles count and link is known only from the ticket."
    question: Q-01
  - id: U-2
    text: "Glossary has no entry for Completed."
    question: Q-02
  - id: U-3
    text: "Glossary has no entry for Cancelled."
    question: Q-02
  - id: U-4
    text: "Glossary has no entry for dashboard or tile."
    question: Q-02
  - id: U-5
    text: "Glossary has no entry for front-desk staff."
    question: Q-02
questions:
  - id: Q-01
    blocking: false
    ask: tech lead
    text: "Do the existing Completed and Cancelled tiles count by appointment start time in the hospital time zone, and link with the period's dates, so No-shows can copy them exactly?"
    why: "REQ-4 and REQ-7 say 'the same way' as the existing tiles; design must find that behaviour in the code, since the PRD is not an input."
    evidence: ["artifacts/HMS-3/run_01M5XZYC101HCFE34KWJ49C90V/ticket.md#L22", "artifacts/HMS-3/run_01M5XZYC101HCFE34KWJ49C90V/ticket.md#L24", "context/intent-baseline.md#constraints"]
    options: ["Copy the existing tiles' counting and link exactly", "Define the No-shows count separately"]
    default: "Copy the existing Completed and Cancelled tiles exactly; impact analysis confirms how they work."
  - id: Q-02
    blocking: false
    ask: tech lead
    text: "Should Completed, Cancelled, dashboard, tile and front-desk staff be added to the glossary?"
    why: "These terms are used by the ticket but not defined."
    evidence: ["artifacts/HMS-3/run_01M5XZYC101HCFE34KWJ49C90V/ticket.md#L16", "artifacts/HMS-3/run_01M5XZYC101HCFE34KWJ49C90V/ticket.md#L13"]
    options: ["Add them", "Leave as plain English"]
    default: "Use the plain meanings; Completed and Cancelled are appointment statuses."
glossary:
  - {term: No-show, found: true}
  - {term: Medical history, found: true}
  - {term: Completed, found: false}
  - {term: Cancelled, found: false}
  - {term: dashboard, found: false}
  - {term: tile, found: false}
  - {term: front-desk staff, found: false}
---
# Requirements review: HMS-3

## Summary
The ticket asks for a "No-shows" tile on the dashboard that counts No-show appointments starting in
the selected period, the same way the Completed and Cancelled tiles count theirs, and links to the
appointment list filtered to No-show for that period. The other tiles must not change, and the
dashboard must still show no medical history or contact details. The main risk is that "the same
way" refers to PRD rules (DSH-5 to DSH-9) that are not among the inputs, so the exact counting and
link format must come from the existing code (Q-01). No question blocks design.

## Requirements
- REQ-1 No-shows tile (known): the dashboard has a tile titled "No-shows".
- REQ-2 What the tile counts (known): the number of No-show appointments that start in the selected period.
- REQ-3 Period choices (known): Today, Last 7 days or Last 30 days.
- REQ-4 Counted like Completed and Cancelled (known): the same counting as those tiles.
- REQ-5 Count follows the period (known): changing the period updates the count.
- REQ-6 Tile links to the No-show list (known): the tile links to the appointment list filtered to No-show.
- REQ-7 Link keeps the period (known): the link filters that list to the selected period.
- REQ-8 Other tiles unchanged (known): the other tiles and their counts do not change.
- REQ-9 No medical history on the dashboard (known).
- REQ-10 No contact details on the dashboard (known).

## Known
- REQ-1, REQ-2, REQ-3: stated in acceptance criteria 1 and 2 (L20-L22); a No-show is defined in the glossary (C1).
- REQ-4: stated in acceptance criterion 2 (L22), with the existing tiles named at L16.
- REQ-5: stated in acceptance criterion 3 (L23).
- REQ-6, REQ-7: stated in acceptance criterion 4 (L24).
- REQ-8: stated in acceptance criterion 5 (L26).
- REQ-9: stated in acceptance criterion 6 (L27) and the baseline data-protection rule (C2).
- REQ-10: stated in acceptance criterion 6 (L27).

## Assumed
- None.

## Unknown
- U-1: the PRD rules the ticket cites (DSH-5 to DSH-9) are not inputs; Q-01 asks to copy the existing tiles.
- U-2: Completed is not in the glossary (Q-02).
- U-3: Cancelled is not in the glossary (Q-02).
- U-4: dashboard and tile are not in the glossary (Q-02).
- U-5: front-desk staff is not in the glossary (Q-02).

## Open questions
- Q-01 (non-blocking, tech lead): do the existing tiles count by start time in the hospital zone and link with the period's dates? Default: copy them exactly.
- Q-02 (non-blocking, tech lead): add Completed, Cancelled, dashboard, tile and front-desk staff to the glossary? Default: plain meanings.

## Out of scope
- The reason for the request (following up with patients, spotting doctors or days with many missed visits, L13-L14) is context, not a requirement: the ticket asks only for the count and link, not for per-doctor or per-day breakdowns.
