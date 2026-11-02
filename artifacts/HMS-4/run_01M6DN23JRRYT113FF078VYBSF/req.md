---
schema: intent.v1
run_id: run_01M6DN23JRRYT113FF078VYBSF
step: req
attempt: 1
skill: requirement-review@1.1.0
inputs:
  - path: artifacts/HMS-4/run_01M6DN23JRRYT113FF078VYBSF/ticket.md
    sha256: a5a541e05b31c6ca90980b22daff90adfbaad4f41f949b8ab366c41a38dad256
  - path: context/intent-baseline.md
    sha256: 9bfd821aeb2fb9524863e346707e049e4fcfb949a6e55b90223a79194946a215
  - path: context/glossary.md
    sha256: 7623345ad8bad33b8a14a713b452edb2a36834e8b1c2228b76f3aa154034f16e
pack_sha:
  sample-system: bd98ff08b83f8762c5e5c073ba5fadef88169b33
claims:
  - id: C1
    text: "The patient list already shows each patient's `next appointment` (HMS-1)."
    cites: ["artifacts/HMS-4/run_01M6DN23JRRYT113FF078VYBSF/ticket.md#L14"]
  - id: C2
    text: "The hospital time zone is `Asia/Kolkata`."
    cites: ["context/intent-baseline.md#constraints"]
  - id: C3
    text: "`Medical history` appears only on the patient's own page, never in lists."
    cites: ["context/intent-baseline.md#data-protection"]
  - id: C4
    text: "A `Completed` appointment is closed by staff once its start time has passed."
    cites: ["context/glossary.md#completed"]
  - id: C5
    text: "`Medical history` means blood group, allergies, chronic conditions, current medications and notes of one patient."
    cites: ["context/glossary.md#medical-history"]
  - id: C6
    text: "`Front-desk staff` register patients and book, move and close appointments."
    cites: ["context/glossary.md#front-desk-staff"]
trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10, REQ-11]
known: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10, REQ-11]
assumed: []
unknown: []
requirements:
  - id: REQ-1
    title: Last visit column
    type: functional
    status: known
    statement: "The patient list has a column titled `Last visit`."
    cites: ["artifacts/HMS-4/run_01M6DN23JRRYT113FF078VYBSF/ticket.md#L16"]
  - id: REQ-2
    title: Last visit value is the most recent Completed appointment
    type: functional
    status: known
    statement: "The `Last visit` column shows the date of the patient's `Completed` appointment with the latest start time."
    cites: ["artifacts/HMS-4/run_01M6DN23JRRYT113FF078VYBSF/ticket.md#L17", "context/glossary.md#completed", "runs/run_01M6DN23JRRYT113FF078VYBSF/ide-answers.md#a-02", "runs/run_01M6DN23JRRYT113FF078VYBSF/ide-answers.md#a-04"]
  - id: REQ-3
    title: Date in hospital time zone
    type: constraint
    status: known
    statement: "The Last visit date is the calendar date in the hospital time zone `Asia/Kolkata`."
    cites: ["artifacts/HMS-4/run_01M6DN23JRRYT113FF078VYBSF/ticket.md#L17", "context/intent-baseline.md#constraints"]
  - id: REQ-4
    title: Date display format
    type: functional
    status: known
    statement: "The Last visit date uses the same date format as the existing `Next appointment` column, without the time."
    cites: ["artifacts/HMS-4/run_01M6DN23JRRYT113FF078VYBSF/ticket.md#L14", "runs/run_01M6DN23JRRYT113FF078VYBSF/ide-answers.md#a-01"]
  - id: REQ-5
    title: Non-Completed appointments excluded
    type: functional
    status: known
    statement: "`Cancelled`, `No-show` and `Scheduled` appointments are never shown in the Last visit column."
    cites: ["artifacts/HMS-4/run_01M6DN23JRRYT113FF078VYBSF/ticket.md#L18"]
  - id: REQ-6
    title: No Completed appointment shows a dash
    type: functional
    status: known
    statement: "A patient with no Completed appointment shows `-` in the Last visit column."
    cites: ["artifacts/HMS-4/run_01M6DN23JRRYT113FF078VYBSF/ticket.md#L19"]
  - id: REQ-7
    title: Page size unchanged
    type: constraint
    status: known
    statement: "The patient list stays at `20 per page`."
    cites: ["artifacts/HMS-4/run_01M6DN23JRRYT113FF078VYBSF/ticket.md#L20"]
  - id: REQ-8
    title: Sort order unchanged; Last visit display-only
    type: constraint
    status: known
    statement: "The patient list keeps its current `sort order`; Last visit is display-only and is not added as a sort option."
    cites: ["artifacts/HMS-4/run_01M6DN23JRRYT113FF078VYBSF/ticket.md#L20", "runs/run_01M6DN23JRRYT113FF078VYBSF/ide-answers.md#a-03"]
  - id: REQ-9
    title: Search unchanged
    type: constraint
    status: known
    statement: "The patient list keeps its current `search` behaviour."
    cites: ["artifacts/HMS-4/run_01M6DN23JRRYT113FF078VYBSF/ticket.md#L20"]
  - id: REQ-10
    title: Next appointment column unchanged
    type: constraint
    status: known
    statement: "The `Next appointment` column of the patient list is unchanged."
    cites: ["artifacts/HMS-4/run_01M6DN23JRRYT113FF078VYBSF/ticket.md#L20", "artifacts/HMS-4/run_01M6DN23JRRYT113FF078VYBSF/ticket.md#L21"]
  - id: REQ-11
    title: No medical history in the list
    type: constraint
    status: known
    statement: "No `medical history` appears in the patient list (PRD PAT-8)."
    cites: ["artifacts/HMS-4/run_01M6DN23JRRYT113FF078VYBSF/ticket.md#L22", "context/intent-baseline.md#data-protection", "context/glossary.md#medical-history"]
unknowns: []
questions: []
answers:
  - id: A-01
    kind: question
    about: Q-01
    answer: "Match Next appointment (proposed default)"
    requirements: [REQ-4]
  - id: A-02
    kind: assumption
    about: REQ-2
    answer: "Yes, confirm"
    requirements: [REQ-2]
  - id: A-03
    kind: assumption
    about: REQ-8
    answer: "Yes, confirm"
    requirements: [REQ-8]
  - id: A-04
    kind: question
    about: Q-02
    answer: "Yes, use that (proposed default)"
    requirements: [REQ-2]
glossary:
  - {term: Completed, found: true}
  - {term: Cancelled, found: true}
  - {term: No-show, found: true}
  - {term: Scheduled, found: true}
  - {term: Front-desk staff, found: true}
  - {term: Medical history, found: true}
  - {term: Patient list, found: false}
  - {term: Last visit, found: false}
---
# Requirements review: HMS-4

## Summary
HMS-4 asks for a read-only "Last visit" column on the patient list, so front-desk staff can spot patients who have not been seen for a long time. The value is the hospital-time-zone date of the patient's latest Completed appointment, or "-" when there is none. The list must otherwise stay as it is: 20 per page, the same search and sort, and the Next appointment column unchanged. The main risks are leaking non-Completed appointments into the column, getting the date wrong at the time-zone boundary (Asia/Kolkata), and slowing down or breaking the existing paging and sort when the value is added to the list query. Medical history must still never appear in the list.

## Requirements
- REQ-1 Last visit column — known — The patient list has a column titled "Last visit".
- REQ-2 Last visit value — known — Shows the date of the patient's Completed appointment with the latest start time.
- REQ-3 Date in hospital time zone — known — The date is the calendar date in Asia/Kolkata.
- REQ-4 Date display format — known — Same date format as the Next appointment column, without the time.
- REQ-5 Non-Completed excluded — known — Cancelled, No-show and Scheduled appointments are never shown in the column.
- REQ-6 No Completed → "-" — known — A patient with no Completed appointment shows "-".
- REQ-7 Page size unchanged — known — The list stays at 20 per page.
- REQ-8 Sort unchanged — known — Current sort order kept; Last visit is display-only, not a sort option.
- REQ-9 Search unchanged — known — Current search behaviour kept.
- REQ-10 Next appointment unchanged — known — The Next appointment column is unchanged.
- REQ-11 No medical history — known — No medical history appears in the list.

## Known
- REQ-1, REQ-5, REQ-6, REQ-7, REQ-9, REQ-10: stated in the ticket's acceptance criteria (L16, L18–L21).
- REQ-2: ticket L17; the user confirmed "most recent" means the latest start time (A-02) and the Last visit definition (A-04).
- REQ-3: ticket L17 plus the baseline constraint that the hospital time zone is Asia/Kolkata.
- REQ-4: the user chose to match the Next appointment column's date format (A-01).
- REQ-8: ticket L20; the user confirmed Last visit is display-only (A-03).
- REQ-11: ticket L22 and the baseline data-protection rule.

## Assumed
None. The user confirmed every assumption.

## Unknown
None. Every question was answered.

## Open questions
None open.

Answered in the IDE:
- A-01 (Q-01, date display format): "Match Next appointment (proposed default)". Answered by the user in the IDE. Decides REQ-4. No mismatch.
- A-02 (REQ-2, most recent = latest start time): "Yes, confirm". Answered by the user in the IDE. Decides REQ-2. No mismatch.
- A-03 (REQ-8, Last visit display-only, not sortable): "Yes, confirm". Answered by the user in the IDE. Decides REQ-8. No mismatch.
- A-04 (Q-02, glossary definition of Last visit): "Yes, use that (proposed default)". Answered by the user in the IDE. Decides REQ-2. No mismatch. The tech lead should add "Last visit" (and "Patient list") to the glossary.

## Out of scope
- Highlighting or filtering patients who have not visited "for a long time" (ticket L12–L13): this is the user's goal, not an acceptance criterion, and no threshold is given.
- Adding "Last visit" and "Patient list" to context/glossary.md: a follow-up for the glossary owner. This step edits no context files.
