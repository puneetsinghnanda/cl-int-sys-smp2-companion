---
schema: intent.v1
run_id: run_01M6QZ4JKGSK55APYQTFP14YQD
step: req
attempt: 1
skill: requirement-review@1.2.0
inputs:
  - path: artifacts/HMS-8/run_01M6QZ4JKGSK55APYQTFP14YQD/ticket.md
    sha256: fea95abccb47c099a79ebf13a7a1703c6aa7610d237d21de878e16130a080472
  - path: context/intent-baseline.md
    sha256: 9bfd821aeb2fb9524863e346707e049e4fcfb949a6e55b90223a79194946a215
  - path: context/glossary.md
    sha256: 430a90e14c50f969f7f757089c61cf254eb21596ed6e60abb8bf59286822b9ff
pack_sha:
  sample-system: c2709e164850ab7e177e4bccfc42a5dd9b961fdc
claims:
  - id: C1
    text: "The baseline goal is that staff manage doctors, patients and `appointments end to end`."
    cites: ["context/intent-baseline.md#goals"]
  - id: C2
    text: "The hospital time zone is `Asia/Kolkata`."
    cites: ["context/intent-baseline.md#constraints"]
  - id: C3
    text: "Today the only way to change a doctor filter is the `Clear doctor /` patient filter control, which also clears the status and dates."
    cites: ["artifacts/HMS-8/run_01M6QZ4JKGSK55APYQTFP14YQD/ticket.md#L19-L20"]
  - id: C4
    text: "The doctors list's Today column counts any status `except Cancelled`, but its link opens the list with Any status."
    cites: ["artifacts/HMS-8/run_01M6QZ4JKGSK55APYQTFP14YQD/ticket.md#L20-L21"]
  - id: C5
    text: "An Inactive doctor is kept for history but `not offered for new use`; filtering past appointments is not new use."
    cites: ["context/glossary.md#inactive"]
trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10, REQ-11, REQ-12, REQ-13]
known: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10, REQ-11, REQ-12, REQ-13]
assumed: []
unknown: []
requirements:
  - id: REQ-1
    title: Show the filtered doctor's name
    type: functional
    status: known
    statement: "When the appointment list is filtered to a doctor, it `shows that doctor's name` and not the id."
    cites: ["artifacts/HMS-8/run_01M6QZ4JKGSK55APYQTFP14YQD/ticket.md#L25"]
  - id: REQ-2
    title: Doctor chooser always shown
    type: functional
    status: known
    statement: "The appointment list always shows a doctor chooser (`Always shown`), whether or not a doctor filter is set, and the chooser lets staff clear the doctor filter."
    cites: ["runs/run_01M6QZ4JKGSK55APYQTFP14YQD/ide-answers.md#a-02", "artifacts/HMS-8/run_01M6QZ4JKGSK55APYQTFP14YQD/ticket.md#L26"]
  - id: REQ-3
    title: Chooser offers all doctors
    type: functional
    status: known
    statement: "The doctor chooser offers `All doctors`, Active and Inactive."
    cites: ["runs/run_01M6QZ4JKGSK55APYQTFP14YQD/ide-answers.md#a-01"]
  - id: REQ-4
    title: Changing the doctor keeps other filters
    type: functional
    status: known
    statement: "When staff `choose a different doctor`, the status, dates and patient filter stay as they were."
    cites: ["artifacts/HMS-8/run_01M6QZ4JKGSK55APYQTFP14YQD/ticket.md#L26-L27"]
  - id: REQ-5
    title: Clearing the doctor keeps other filters
    type: functional
    status: known
    statement: "When staff `clear the doctor filter` from the chooser, the status, dates and patient filter stay as they were."
    cites: ["artifacts/HMS-8/run_01M6QZ4JKGSK55APYQTFP14YQD/ticket.md#L26-L27"]
  - id: REQ-6
    title: Clear All button
    type: functional
    status: known
    statement: "The existing clear button becomes a `Clear All` button that clears all the filters."
    cites: ["runs/run_01M6QZ4JKGSK55APYQTFP14YQD/ide-answers.md#a-03"]
  - id: REQ-7
    title: Any except Cancelled status option
    type: functional
    status: known
    statement: "The status filter has an option `Any except Cancelled`, which lists every status except Cancelled."
    cites: ["artifacts/HMS-8/run_01M6QZ4JKGSK55APYQTFP14YQD/ticket.md#L28-L29"]
  - id: REQ-8
    title: URL and API value for Any except Cancelled
    type: constraint
    status: known
    statement: "'Any except Cancelled' is written as `status=NOT_CANCELLED` in the list URL and sent the same way to GET /api/appointments."
    cites: ["runs/run_01M6QZ4JKGSK55APYQTFP14YQD/ide-answers.md#a-04"]
  - id: REQ-9
    title: Today link uses Any except Cancelled
    type: functional
    status: known
    statement: "The doctors list's \"Today\" link `opens the list with \"Any except Cancelled\"`."
    cites: ["artifacts/HMS-8/run_01M6QZ4JKGSK55APYQTFP14YQD/ticket.md#L30"]
  - id: REQ-10
    title: Listed count equals Today count
    type: functional
    status: known
    statement: "After the Today link opens it, the number of appointments listed `equals the \"Today\" count`."
    cites: ["artifacts/HMS-8/run_01M6QZ4JKGSK55APYQTFP14YQD/ticket.md#L31"]
  - id: REQ-11
    title: Other links unchanged
    type: constraint
    status: known
    statement: "Other links to the appointment list, such as the doctor page's `No-shows link`, are unchanged."
    cites: ["artifacts/HMS-8/run_01M6QZ4JKGSK55APYQTFP14YQD/ticket.md#L32"]
  - id: REQ-12
    title: Page size unchanged
    type: constraint
    status: known
    statement: "The appointment list still shows `20 per page`."
    cites: ["artifacts/HMS-8/run_01M6QZ4JKGSK55APYQTFP14YQD/ticket.md#L33"]
  - id: REQ-13
    title: Order unchanged
    type: constraint
    status: known
    statement: "The appointment list stays `ordered by start time`."
    cites: ["artifacts/HMS-8/run_01M6QZ4JKGSK55APYQTFP14YQD/ticket.md#L33"]
unknowns: []
questions: []
answers:
  - id: A-01
    kind: question
    about: Q-01
    answer: "All doctors (proposed default)"
    requirements: [REQ-3]
  - id: A-02
    kind: question
    about: Q-02
    answer: "Always shown (proposed default)"
    requirements: [REQ-2]
  - id: A-03
    kind: question
    about: Q-03
    answer: "Make it a Clear All button to clear all the filters."
    requirements: [REQ-6]
  - id: A-04
    kind: question
    about: Q-04
    answer: "status=NOT_CANCELLED (proposed default)"
    requirements: [REQ-8]
glossary:
  - {term: Front-desk staff, found: true}
  - {term: Cancelled, found: true}
  - {term: Inactive, found: true}
  - {term: Period, found: true}
---
# Requirements review: HMS-8

## Summary
HMS-8 asks for the appointment list to show the name of the doctor it is filtered to and to let
front-desk staff change or clear that doctor without losing the status, dates and patient filter
(C3). It also adds an "Any except Cancelled" status option and makes the doctors list's "Today"
link use it, so the listed appointments match the Today count (C4). The main risks: REQ-8 adds a new
value, `status=NOT_CANCELLED`, to the status filter's contract. The clear button also changes
meaning to Clear All (REQ-6), and every other link to the list must stay unchanged (REQ-11).

## Requirements
- REQ-1 Show the filtered doctor's name (known): when filtered to a doctor, the list shows the doctor's name, not the id.
- REQ-2 Doctor chooser always shown (known): the list always shows a doctor chooser, and the chooser can clear the doctor filter.
- REQ-3 Chooser offers all doctors (known): Active and Inactive doctors are offered.
- REQ-4 Changing the doctor keeps other filters (known): status, dates and patient filter stay.
- REQ-5 Clearing the doctor keeps other filters (known): status, dates and patient filter stay.
- REQ-6 Clear All button (known): the existing clear button becomes Clear All and clears all the filters.
- REQ-7 Any except Cancelled status option (known): lists every status except Cancelled.
- REQ-8 URL and API value (known): written as `status=NOT_CANCELLED` in the URL and sent the same way to GET /api/appointments.
- REQ-9 Today link uses Any except Cancelled (known).
- REQ-10 Listed count equals Today count (known).
- REQ-11 Other links unchanged (known): for example the doctor page's No-shows link.
- REQ-12 Page size unchanged (known): 20 per page.
- REQ-13 Order unchanged (known): ordered by start time.

## Known
- REQ-1, REQ-4, REQ-5, REQ-7, REQ-9, REQ-10, REQ-11, REQ-12, REQ-13: stated in the ticket's acceptance criteria (L25–L33).
- REQ-2: ticket L26, decided by A-02 (always shown).
- REQ-3: decided by A-01 (all doctors).
- REQ-6: decided by A-03 (Clear All).
- REQ-8: decided by A-04 (`status=NOT_CANCELLED`).

## Assumed
None.

## Unknown
None.

## Open questions
None open.

Answered in the IDE:
- A-01 (Q-01, answered by eng-3c51): which doctors the chooser offers. Answer: All doctors (proposed default). Decides REQ-3. No mismatch.
- A-02 (Q-02, answered by eng-3c51): chooser always shown or only when filtered. Answer: Always shown (proposed default). Decides REQ-2. No mismatch.
- A-03 (Q-03, answered by eng-3c51): what happens to "Clear doctor / patient filter". Answer: Make it a Clear All button to clear all the filters. Decides REQ-6. No mismatch: the ticket only describes today's button (C3), and AC 2 is met by the chooser (REQ-4, REQ-5).
- A-04 (Q-04, answered by eng-3c51): how "Any except Cancelled" is written. Answer: status=NOT_CANCELLED (proposed default). Decides REQ-8. No mismatch.

## Out of scope
- A chooser or name display for the patient filter: the ticket asks only for the doctor (L25–L27). The patient filter is only kept as it is.
- When the Clear All button shows (always, or only while a filter is set): the answer did not say, so the design step should state it.
- The dashboard's "Appointments today" tile link: covered only by "other links unchanged" (REQ-11).
- No instruction-like text, credentials or personal data found in the inputs.
