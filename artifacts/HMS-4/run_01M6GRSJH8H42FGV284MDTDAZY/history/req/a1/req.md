---
schema: intent.v1
run_id: run_01M6GRSJH8H42FGV284MDTDAZY
step: req
attempt: 1
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
trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10, REQ-11, REQ-12, REQ-13, REQ-14]
known: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10, REQ-11, REQ-12, REQ-13, REQ-14]
assumed: []
unknown: []
requirements:
- id: REQ-1
  title: Last visit column
  type: functional
  status: known
  statement: "The existing paged patient list, the one that already shows Next appointment, has a column \"Last visit\"."
  cites:
  - artifacts/HMS-4/run_01M6GRSJH8H42FGV284MDTDAZY/ticket.md#L16
  - artifacts/HMS-4/run_01M6GRSJH8H42FGV284MDTDAZY/ticket.md#L14
  - runs/run_01M6GDYQX07XECMVT06FY0Z6YF/ide-answers.md#a-03
- id: REQ-2
  title: Last visit comes from the most recent Completed appointment
  type: functional
  status: known
  statement: "Last visit shows the date of the patient's most recent Completed appointment."
  cites:
  - artifacts/HMS-4/run_01M6GRSJH8H42FGV284MDTDAZY/ticket.md#L17
  - runs/run_01M6DR2FJ8XZ1J3VWHQTB17QBH/ide-answers.md#a-03
- id: REQ-3
  title: Most recent means latest start time
  type: functional
  status: known
  statement: "The most recent Completed appointment is the Completed appointment with the latest start time."
  cites:
  - context/glossary.md#completed
  - runs/run_01M6DR2FJ8XZ1J3VWHQTB17QBH/ide-answers.md#a-04
  - runs/run_01M6DN23JRRYT113FF078VYBSF/ide-answers.md#a-02
- id: REQ-4
  title: The date shown is the appointment's start date
  type: functional
  status: known
  statement: "The date shown in Last visit is the start date of that Completed appointment."
  cites:
  - artifacts/HMS-4/run_01M6GRSJH8H42FGV284MDTDAZY/ticket.md#L17
  - runs/run_01M6GDYQX07XECMVT06FY0Z6YF/ide-answers.md#a-01
- id: REQ-5
  title: Date in the hospital time zone
  type: constraint
  status: known
  statement: "The Last visit date is the calendar date in the hospital time zone, Asia/Kolkata."
  cites:
  - artifacts/HMS-4/run_01M6GRSJH8H42FGV284MDTDAZY/ticket.md#L17
  - context/intent-baseline.md#constraints
- id: REQ-6
  title: Date format matches Next appointment, date only
  type: functional
  status: known
  statement: "Last visit shows only a date, with no time of day, written in the same date style as the Next appointment column."
  cites:
  - artifacts/HMS-4/run_01M6GRSJH8H42FGV284MDTDAZY/ticket.md#L17
  - runs/run_01M6GDYQX07XECMVT06FY0Z6YF/ide-answers.md#a-04
  - runs/run_01M6DN23JRRYT113FF078VYBSF/ide-answers.md#a-01
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
  title: Sort order unchanged, Last visit not sortable
  type: constraint
  status: known
  statement: "The patient list keeps its current sort order, and Last visit is not offered as a sort option."
  cites:
  - artifacts/HMS-4/run_01M6GRSJH8H42FGV284MDTDAZY/ticket.md#L20
  - runs/run_01M6GDYQX07XECMVT06FY0Z6YF/ide-answers.md#a-02
  - runs/run_01M6DR2FJ8XZ1J3VWHQTB17QBH/ide-answers.md#a-01
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
  statement: "The list only shows the Last visit date; it adds no highlight, threshold or filter for patients not seen for a long time."
  cites:
  - artifacts/HMS-4/run_01M6GRSJH8H42FGV284MDTDAZY/ticket.md#L12-L13
  - runs/run_01M6DR2FJ8XZ1J3VWHQTB17QBH/ide-answers.md#a-02
unknowns: []
questions: []
answers: []
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
HMS-4 asks for a "Last visit" column on the existing patient list, so front-desk staff can spot patients who have not been seen for a long time. The value is the start date, in the hospital time zone, of the patient's most recent Completed appointment, or "-" when there is none. Paging, search, sort order and the Next appointment column must not change, and no medical history may appear. The main risks are formatting (Next appointment shows a date and a time, while Last visit is a date only) and a paged query that repeats patient rows if appointments are joined naively. Every open decision was already answered in earlier HMS-4 runs, so this review reuses those answers and asks nothing new.

## Requirements
- REQ-1 Last visit column (known): The existing paged patient list, the one that already shows Next appointment, has a column "Last visit".
- REQ-2 Last visit comes from the most recent Completed appointment (known): Last visit shows the date of the patient's most recent Completed appointment.
- REQ-3 Most recent means latest start time (known): The most recent Completed appointment is the Completed appointment with the latest start time.
- REQ-4 The date shown is the appointment's start date (known): The date shown in Last visit is the start date of that Completed appointment.
- REQ-5 Date in the hospital time zone (known): The Last visit date is the calendar date in the hospital time zone, Asia/Kolkata.
- REQ-6 Date format matches Next appointment, date only (known): Last visit shows only a date, with no time of day, written in the same date style as the Next appointment column.
- REQ-7 Other statuses never shown (known): Cancelled, No-show and Scheduled appointments are never shown in the Last visit column.
- REQ-8 No Completed appointment shows a dash (known): A patient with no Completed appointment shows "-" in the Last visit column.
- REQ-9 Page size unchanged (known): The patient list stays at 20 per page.
- REQ-10 Search unchanged (known): The patient list keeps its current search behaviour.
- REQ-11 Sort order unchanged, Last visit not sortable (known): The patient list keeps its current sort order, and Last visit is not offered as a sort option.
- REQ-12 Next appointment column unchanged (known): The Next appointment column is unchanged.
- REQ-13 No medical history in the list (known): No medical history appears in the patient list.
- REQ-14 No highlight or filter for long-unseen patients (known): The list only shows the Last visit date; it adds no highlight, threshold or filter for patients not seen for a long time.

## Known
- REQ-1: ticket L16 and L14; "patient list" is the existing paged list (earlier answer, run_01M6GDYQX07XECMVT06FY0Z6YF A-03).
- REQ-2: ticket L17; the user agreed to use the ticket's definition (run_01M6DR2FJ8XZ1J3VWHQTB17QBH A-03).
- REQ-3: glossary "Completed" closes on start time; confirmed twice (run_01M6DR2FJ8XZ1J3VWHQTB17QBH A-04, run_01M6DN23JRRYT113FF078VYBSF A-02).
- REQ-4: ticket L17; start date chosen (run_01M6GDYQX07XECMVT06FY0Z6YF A-01).
- REQ-5: ticket L17 and baseline constraints (`Asia/Kolkata`).
- REQ-6: ticket L17 asks for "the date"; same style as Next appointment (run_01M6GDYQX07XECMVT06FY0Z6YF A-04, run_01M6DN23JRRYT113FF078VYBSF A-01).
- REQ-7: ticket L18.
- REQ-8: ticket L19.
- REQ-9, REQ-10: ticket L20.
- REQ-11: ticket L20; not sortable (run_01M6GDYQX07XECMVT06FY0Z6YF A-02, run_01M6DR2FJ8XZ1J3VWHQTB17QBH A-01).
- REQ-12: ticket L20-L21.
- REQ-13: ticket L22 and baseline data protection.
- REQ-14: ticket L12-L13 gives no threshold; only the date is shown (run_01M6DR2FJ8XZ1J3VWHQTB17QBH A-02).

## Assumed
None. Every earlier assumption was confirmed by the user in an earlier HMS-4 run.

## Unknown
None. "Patient list", "Last visit" and "Next appointment" are not in the glossary, but the user already settled their meaning (run_01M6GDYQX07XECMVT06FY0Z6YF A-03, run_01M6DR2FJ8XZ1J3VWHQTB17QBH A-03).

## Open questions
None open.

Answered in the IDE: no new questions this run. Earlier HMS-4 answers reused (all by gaurav-saini-truminds):
- run_01M6GDYQX07XECMVT06FY0Z6YF A-01: which date for Last visit → "Start date (proposed default)"; decides REQ-4.
- run_01M6GDYQX07XECMVT06FY0Z6YF A-02: sortable by Last visit → "Not sortable (proposed default)"; decides REQ-11.
- run_01M6GDYQX07XECMVT06FY0Z6YF A-03: which list → "Yes, that list (proposed default)"; decides REQ-1.
- run_01M6GDYQX07XECMVT06FY0Z6YF A-04: date format → "Same as Next appointment (proposed default)"; decides REQ-6. Mismatch: Next appointment shows a time as well; the ticket (L17) asks for a date, so Last visit uses the same date style without a time.
- run_01M6DR2FJ8XZ1J3VWHQTB17QBH A-01: sort by Last visit → "No sorting (proposed default)"; decides REQ-11.
- run_01M6DR2FJ8XZ1J3VWHQTB17QBH A-02: highlight long-unseen patients → "No, only show the date (proposed default)"; decides REQ-14.
- run_01M6DR2FJ8XZ1J3VWHQTB17QBH A-03: meaning of Last visit → "Yes, use ticket L17 (proposed default)"; decides REQ-2.
- run_01M6DR2FJ8XZ1J3VWHQTB17QBH A-04: most recent = latest start time → "Yes, confirm"; decides REQ-3.
- run_01M6DN23JRRYT113FF078VYBSF A-01: date format → "Match Next appointment (proposed default)"; decides REQ-6.
- run_01M6DN23JRRYT113FF078VYBSF A-02: most recent = latest start time → "Yes, confirm"; decides REQ-3.

## Out of scope
- Highlighting, thresholds or filters for long-unseen patients: the user chose to show only the date (REQ-14).
- Sorting by Last visit (REQ-11).
- Where the value is computed and how it reaches the page: impact analysis and design decide this.
