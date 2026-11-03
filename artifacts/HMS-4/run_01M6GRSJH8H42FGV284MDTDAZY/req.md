---
schema: intent.v1
run_id: run_01M6GRSJH8H42FGV284MDTDAZY
step: req
attempt: 2
skill: requirement-review@1.2.0
inputs:
- path: artifacts/HMS-4/run_01M6GRSJH8H42FGV284MDTDAZY/ticket.md
  sha256: 6eb3daef6d5b3e4d0aac977f0646c221c88e122a4d877b42040b8fa52beccf35
- path: context/intent-baseline.md
  sha256: 9bfd821aeb2fb9524863e346707e049e4fcfb949a6e55b90223a79194946a215
- path: context/glossary.md
  sha256: 7623345ad8bad33b8a14a713b452edb2a36834e8b1c2228b76f3aa154034f16e
pack_sha:
  sample-system: bd98ff08b83f8762c5e5c073ba5fadef88169b33
claims:
- id: C1
  text: "The patient list already shows each patient's `next appointment`."
  cites: ["artifacts/HMS-4/run_01M6GRSJH8H42FGV284MDTDAZY/ticket.md#L14"]
- id: C2
  text: "A Completed appointment is closed by staff once its `start time` has passed."
  cites: ["context/glossary.md#completed"]
- id: C3
  text: "The hospital time zone is `Asia/Kolkata`."
  cites: ["context/intent-baseline.md#constraints"]
- id: C4
  text: "Medical history appears only on the patient's own page, `never in lists`."
  cites: ["context/intent-baseline.md#data-protection"]
- id: C5
  text: "The list shows `20 per page` today and must keep doing so."
  cites: ["artifacts/HMS-4/run_01M6GRSJH8H42FGV284MDTDAZY/ticket.md#L20"]
- id: C6
  text: "Front-desk staff act as `staff` in the MVP."
  cites: ["context/glossary.md#front-desk-staff"]
- id: C7
  text: "Medical history covers blood group, `allergies`, chronic conditions, current medications and notes."
  cites: ["context/glossary.md#medical-history"]
trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10, REQ-11, REQ-12, REQ-13, REQ-14, REQ-15]
known: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10, REQ-11, REQ-12, REQ-13, REQ-14, REQ-15]
assumed: []
unknown: []
requirements:
- id: REQ-1
  title: Last visit column
  type: functional
  status: known
  statement: "The existing patient list, the one that already shows Next appointment, has a column \"Last visit\"."
  cites:
  - artifacts/HMS-4/run_01M6GRSJH8H42FGV284MDTDAZY/ticket.md#L16
  - artifacts/HMS-4/run_01M6GRSJH8H42FGV284MDTDAZY/ticket.md#L14
- id: REQ-2
  title: Last visit comes from the most recent Completed appointment
  type: functional
  status: known
  statement: "Last visit shows the patient's most recent Completed appointment."
  cites:
  - artifacts/HMS-4/run_01M6GRSJH8H42FGV284MDTDAZY/ticket.md#L17
- id: REQ-3
  title: Most recent means latest start time
  type: functional
  status: known
  statement: "The most recent Completed appointment is the Completed appointment with the latest start time."
  cites:
  - context/glossary.md#completed
  - runs/run_01M6GRSJH8H42FGV284MDTDAZY/ide-answers.md#a-01
- id: REQ-4
  title: The value shown is the appointment's start
  type: functional
  status: known
  statement: "The value shown in Last visit is the start of that Completed appointment."
  cites:
  - artifacts/HMS-4/run_01M6GRSJH8H42FGV284MDTDAZY/ticket.md#L17
  - runs/run_01M6GRSJH8H42FGV284MDTDAZY/ide-answers.md#a-01
- id: REQ-5
  title: Shown in the hospital time zone
  type: constraint
  status: known
  statement: "Last visit is shown in the hospital time zone, Asia/Kolkata."
  cites:
  - artifacts/HMS-4/run_01M6GRSJH8H42FGV284MDTDAZY/ticket.md#L17
  - context/intent-baseline.md#constraints
- id: REQ-6
  title: Same format as Next appointment, with time
  type: functional
  status: known
  statement: "Last visit is written in exactly the same format as the Next appointment column, the date with its hour:minute."
  cites:
  - artifacts/HMS-4/run_01M6GRSJH8H42FGV284MDTDAZY/ticket.md#L17
  - runs/run_01M6GRSJH8H42FGV284MDTDAZY/ide-answers.md#a-02
- id: REQ-7
  title: Other statuses never shown
  type: functional
  status: known
  statement: "Cancelled, No-show and Scheduled appointments are never shown in the Last visit column."
  cites:
  - artifacts/HMS-4/run_01M6GRSJH8H42FGV284MDTDAZY/ticket.md#L18
- id: REQ-8
  title: No Completed appointment shows a dash
  type: functional
  status: known
  statement: "A patient with no Completed appointment shows \"-\" in the Last visit column."
  cites:
  - artifacts/HMS-4/run_01M6GRSJH8H42FGV284MDTDAZY/ticket.md#L19
- id: REQ-9
  title: Page size unchanged
  type: constraint
  status: known
  statement: "The patient list stays at 20 per page."
  cites:
  - artifacts/HMS-4/run_01M6GRSJH8H42FGV284MDTDAZY/ticket.md#L20
- id: REQ-10
  title: Search unchanged
  type: constraint
  status: known
  statement: "The patient list keeps its current search behaviour."
  cites:
  - artifacts/HMS-4/run_01M6GRSJH8H42FGV284MDTDAZY/ticket.md#L20
- id: REQ-11
  title: Sort order unchanged
  type: constraint
  status: known
  statement: "The patient list keeps its current sort order."
  cites:
  - artifacts/HMS-4/run_01M6GRSJH8H42FGV284MDTDAZY/ticket.md#L20
- id: REQ-12
  title: Next appointment column unchanged
  type: constraint
  status: known
  statement: "The Next appointment column is unchanged."
  cites:
  - artifacts/HMS-4/run_01M6GRSJH8H42FGV284MDTDAZY/ticket.md#L20-L21
- id: REQ-13
  title: No medical history in the list
  type: constraint
  status: known
  statement: "No medical history appears in the patient list."
  cites:
  - artifacts/HMS-4/run_01M6GRSJH8H42FGV284MDTDAZY/ticket.md#L22
  - context/intent-baseline.md#data-protection
- id: REQ-14
  title: No highlight or filter for long-unseen patients
  type: constraint
  status: known
  statement: "The list only shows the Last visit value; it adds no highlight, threshold or filter for patients not seen for a long time."
  cites:
  - artifacts/HMS-4/run_01M6GRSJH8H42FGV284MDTDAZY/ticket.md#L12-L13
  - runs/run_01M6GRSJH8H42FGV284MDTDAZY/ide-answers.md#a-04
- id: REQ-15
  title: Last visit is not a sort option
  type: constraint
  status: known
  statement: "Last visit is not offered as a sort option."
  cites:
  - artifacts/HMS-4/run_01M6GRSJH8H42FGV284MDTDAZY/ticket.md#L20
  - runs/run_01M6GRSJH8H42FGV284MDTDAZY/ide-answers.md#a-03
unknowns: []
questions: []
answers:
- id: A-01
  kind: question
  about: Q-01
  answer: "Latest start, start date (proposed default)"
  requirements: [REQ-3, REQ-4]
- id: A-02
  kind: question
  about: Q-02
  answer: "Same as Next appt, with time"
  requirements: [REQ-6]
  mismatch:
    text: "Ticket L17 asks for the date of the last Completed appointment; the user decided Last visit shows the date with hour:minute, exactly like Next appointment."
    cites: ["artifacts/HMS-4/run_01M6GRSJH8H42FGV284MDTDAZY/ticket.md#L17"]
- id: A-03
  kind: question
  about: Q-03
  answer: "Not sortable (proposed default)"
  requirements: [REQ-15]
- id: A-04
  kind: question
  about: Q-04
  answer: "Only show the date (proposed default)"
  requirements: [REQ-14]
glossary:
- {term: Front-desk staff, found: true}
- {term: Completed, found: true}
- {term: Cancelled, found: true}
- {term: No-show, found: true}
- {term: Scheduled, found: true}
- {term: Medical history, found: true}
- {term: Patient list, found: false}
- {term: Last visit, found: false}
- {term: Next appointment, found: false}
---
# Requirements review: HMS-4

## Summary
HMS-4 asks for a "Last visit" column on the existing patient list, so front-desk staff can spot patients who have not been seen for a long time. The value is the start of the patient's Completed appointment with the latest start time, in the hospital time zone, written exactly like Next appointment (date and hour:minute), or "-" when there is none. Paging, search, sort order and the Next appointment column must not change, and no medical history may appear. The user's format choice differs from the ticket's wording ("the date"), which is recorded on A-02. A paged query that joins appointments naively could repeat patient rows, which design must avoid.

## Requirements
- REQ-1 Last visit column (known): The existing patient list, the one that already shows Next appointment, has a column "Last visit".
- REQ-2 Last visit comes from the most recent Completed appointment (known): Last visit shows the patient's most recent Completed appointment.
- REQ-3 Most recent means latest start time (known): The most recent Completed appointment is the Completed appointment with the latest start time.
- REQ-4 The value shown is the appointment's start (known): The value shown in Last visit is the start of that Completed appointment.
- REQ-5 Shown in the hospital time zone (known): Last visit is shown in the hospital time zone, Asia/Kolkata.
- REQ-6 Same format as Next appointment, with time (known): Last visit is written in exactly the same format as the Next appointment column, the date with its hour:minute.
- REQ-7 Other statuses never shown (known): Cancelled, No-show and Scheduled appointments are never shown in the Last visit column.
- REQ-8 No Completed appointment shows a dash (known): A patient with no Completed appointment shows "-" in the Last visit column.
- REQ-9 Page size unchanged (known): The patient list stays at 20 per page.
- REQ-10 Search unchanged (known): The patient list keeps its current search behaviour.
- REQ-11 Sort order unchanged (known): The patient list keeps its current sort order.
- REQ-12 Next appointment column unchanged (known): The Next appointment column is unchanged.
- REQ-13 No medical history in the list (known): No medical history appears in the patient list.
- REQ-14 No highlight or filter for long-unseen patients (known): The list only shows the Last visit value; it adds no highlight, threshold or filter for patients not seen for a long time.
- REQ-15 Last visit is not a sort option (known): Last visit is not offered as a sort option.

## Known
- REQ-1: ticket L16, and L14 names the list that already shows Next appointment.
- REQ-2: ticket L17.
- REQ-3: glossary "Completed" closes on start time; decided by A-01.
- REQ-4: ticket L17; decided by A-01.
- REQ-5: ticket L17 and baseline constraints (`Asia/Kolkata`).
- REQ-6: decided by A-02 (differs from ticket L17, see mismatch).
- REQ-7: ticket L18.
- REQ-8: ticket L19.
- REQ-9, REQ-10, REQ-11: ticket L20.
- REQ-12: ticket L20-L21.
- REQ-13: ticket L22 and baseline data protection.
- REQ-14: ticket L12-L13 gives no threshold; decided by A-04.
- REQ-15: decided by A-03.

## Assumed
None. Every assumption was put to the user in this run.

## Unknown
None. "Patient list", "Last visit" and "Next appointment" are not in the glossary; ticket L14 and L17 define them for this change.

## Open questions
None open.

Answered in the IDE (this run, attempt 2, by gaurav-saini-truminds):
- A-01: Q-01 which appointment and which value → "Latest start, start date (proposed default)"; decides REQ-3, REQ-4.
- A-02: Q-02 how the Last visit date looks → "Same as Next appt, with time"; decides REQ-6. Mismatch: ticket L17 says "the date"; the user decided date with hour:minute, like Next appointment. Earlier HMS-4 runs had dropped the time.
- A-03: Q-03 sortable by Last visit → "Not sortable (proposed default)"; decides REQ-15.
- A-04: Q-04 highlight long-unseen patients → "Only show the date (proposed default)"; decides REQ-14.

## Out of scope
- Highlighting, thresholds or filters for long-unseen patients (REQ-14).
- Sorting by Last visit (REQ-15).
- Where the value is computed and how it reaches the page: impact analysis and design decide this.
