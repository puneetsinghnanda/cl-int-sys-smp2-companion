---
schema: intent.v1
run_id: run_01M6GDYQX07XECMVT06FY0Z6YF
step: req
attempt: 1
skill: requirement-review@1.1.0
inputs:
  - path: artifacts/HMS-4/run_01M6GDYQX07XECMVT06FY0Z6YF/ticket.md
    sha256: 3b289735078dbbf36382237d1199a3a5c9f06590126bdacdc58769151a5bcc54
  - path: context/intent-baseline.md
    sha256: 9bfd821aeb2fb9524863e346707e049e4fcfb949a6e55b90223a79194946a215
  - path: context/glossary.md
    sha256: 7623345ad8bad33b8a14a713b452edb2a36834e8b1c2228b76f3aa154034f16e
pack_sha:
  sample-system: bd98ff08b83f8762c5e5c073ba5fadef88169b33
claims:
  - id: C1
    text: "The patient list already shows each patient's `next appointment` (HMS-1)."
    cites: ["artifacts/HMS-4/run_01M6GDYQX07XECMVT06FY0Z6YF/ticket.md#L14"]
  - id: C2
    text: "The hospital time zone is `Asia/Kolkata`."
    cites: ["context/intent-baseline.md#constraints"]
  - id: C3
    text: "`Medical history` appears only on the patient's own page, never in lists."
    cites: ["context/intent-baseline.md#data-protection"]
  - id: C4
    text: "A Completed appointment is closed by staff once its `start time` has passed."
    cites: ["context/glossary.md#completed"]
  - id: C5
    text: "Completed, Cancelled and No-show appointments `never change`."
    cites: ["context/intent-baseline.md#closed-appointments"]
  - id: C6
    text: "Medical history is `Blood group`, allergies, chronic conditions, current medications and notes of one patient."
    cites: ["context/glossary.md#medical-history"]
trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10, REQ-11, REQ-12]
known: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10, REQ-11, REQ-12]
assumed: []
unknown: []
requirements:
  - id: REQ-1
    title: Last visit column on the patient list
    type: functional
    status: known
    statement: "The existing paged patient list (the one that shows Next appointment, HMS-1) has a column `Last visit`."
    cites: ["artifacts/HMS-4/run_01M6GDYQX07XECMVT06FY0Z6YF/ticket.md#L16", "runs/run_01M6GDYQX07XECMVT06FY0Z6YF/ide-answers.md#a-03"]
  - id: REQ-2
    title: Last visit is the start date of the most recent Completed appointment
    type: functional
    status: known
    statement: "Last visit shows the date of the start time of the patient's most recent `Completed` appointment."
    cites: ["artifacts/HMS-4/run_01M6GDYQX07XECMVT06FY0Z6YF/ticket.md#L17", "runs/run_01M6GDYQX07XECMVT06FY0Z6YF/ide-answers.md#a-01"]
  - id: REQ-3
    title: Date in the hospital time zone
    type: constraint
    status: known
    statement: "The Last visit date is computed in the hospital time zone, `Asia/Kolkata`."
    cites: ["artifacts/HMS-4/run_01M6GDYQX07XECMVT06FY0Z6YF/ticket.md#L17", "context/intent-baseline.md#constraints"]
  - id: REQ-4
    title: Same date format as Next appointment
    type: constraint
    status: known
    statement: "The Last visit date uses the same date format as the existing `Next appointment` column."
    cites: ["artifacts/HMS-4/run_01M6GDYQX07XECMVT06FY0Z6YF/ticket.md#L14", "runs/run_01M6GDYQX07XECMVT06FY0Z6YF/ide-answers.md#a-04"]
  - id: REQ-5
    title: Non-Completed appointments never shown
    type: functional
    status: known
    statement: "`Cancelled`, No-show and Scheduled appointments are never used for or shown in the Last visit column."
    cites: ["artifacts/HMS-4/run_01M6GDYQX07XECMVT06FY0Z6YF/ticket.md#L18"]
  - id: REQ-6
    title: Dash when no Completed appointment
    type: functional
    status: known
    statement: "A patient with no Completed appointment shows `\"-\"` in the Last visit column."
    cites: ["artifacts/HMS-4/run_01M6GDYQX07XECMVT06FY0Z6YF/ticket.md#L19"]
  - id: REQ-7
    title: Page size unchanged
    type: constraint
    status: known
    statement: "The patient list stays `20 per page`."
    cites: ["artifacts/HMS-4/run_01M6GDYQX07XECMVT06FY0Z6YF/ticket.md#L20"]
  - id: REQ-8
    title: Search unchanged
    type: constraint
    status: known
    statement: "The patient list keeps its current `search` behaviour."
    cites: ["artifacts/HMS-4/run_01M6GDYQX07XECMVT06FY0Z6YF/ticket.md#L20"]
  - id: REQ-9
    title: Sort order unchanged
    type: constraint
    status: known
    statement: "The patient list keeps its current `sort order`."
    cites: ["artifacts/HMS-4/run_01M6GDYQX07XECMVT06FY0Z6YF/ticket.md#L20"]
  - id: REQ-10
    title: Last visit is not sortable
    type: constraint
    status: known
    statement: "The Last visit column is display only; no new `sort` option is added for it."
    cites: ["artifacts/HMS-4/run_01M6GDYQX07XECMVT06FY0Z6YF/ticket.md#L20", "runs/run_01M6GDYQX07XECMVT06FY0Z6YF/ide-answers.md#a-02"]
  - id: REQ-11
    title: Next appointment column unchanged
    type: constraint
    status: known
    statement: "The Next appointment column is `unchanged`."
    cites: ["artifacts/HMS-4/run_01M6GDYQX07XECMVT06FY0Z6YF/ticket.md#L21"]
  - id: REQ-12
    title: No medical history in the list
    type: constraint
    status: known
    statement: "No `medical history` appears in the patient list."
    cites: ["artifacts/HMS-4/run_01M6GDYQX07XECMVT06FY0Z6YF/ticket.md#L22", "context/intent-baseline.md#data-protection"]
unknowns: []
questions: []
answers:
  - id: A-01
    kind: question
    about: Q-01
    answer: "Start date (proposed default)"
    requirements: [REQ-2]
  - id: A-02
    kind: question
    about: Q-02
    answer: "Not sortable (proposed default)"
    requirements: [REQ-10]
  - id: A-03
    kind: question
    about: Q-03
    answer: "Yes, that list (proposed default)"
    requirements: [REQ-1]
  - id: A-04
    kind: question
    about: Q-04
    answer: "Same as Next appointment (proposed default)"
    requirements: [REQ-4]
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
HMS-4 asks for a "Last visit" column on the existing patient list. The column shows the start date of each patient's most recent Completed appointment, in Asia/Kolkata time, and "-" when there is none. The list keeps its page size, search, sort order and Next appointment column, and must never show medical history. The main risks are counting non-Completed appointments by mistake, getting the day wrong across time zones, and slowing down or breaking the existing paging and sort query.

## Requirements
- REQ-1 Last visit column on the patient list (known): the existing paged patient list has a "Last visit" column.
- REQ-2 Start date of the most recent Completed appointment (known): the column shows the start date of the patient's most recent Completed appointment.
- REQ-3 Date in the hospital time zone (known): the date is computed in Asia/Kolkata.
- REQ-4 Same date format as Next appointment (known): it uses the Next appointment column's date format.
- REQ-5 Non-Completed appointments never shown (known): Cancelled, No-show and Scheduled appointments are never used.
- REQ-6 Dash when no Completed appointment (known): the column shows "-".
- REQ-7 Page size unchanged (known): 20 per page.
- REQ-8 Search unchanged (known): current search is kept.
- REQ-9 Sort order unchanged (known): current sort order is kept.
- REQ-10 Last visit is not sortable (known): the column is display only.
- REQ-11 Next appointment column unchanged (known).
- REQ-12 No medical history in the list (known).

## Known
- REQ-1: ticket AC1 (L16); A-03 confirmed which list.
- REQ-2: ticket AC2 (L17); A-01 chose the start date.
- REQ-3: ticket AC2 (L17) and baseline constraints (Asia/Kolkata).
- REQ-4: A-04 chose the Next appointment column's format.
- REQ-5: ticket AC3 (L18).
- REQ-6: ticket AC4 (L19).
- REQ-7, REQ-8, REQ-9: ticket AC5 (L20).
- REQ-10: ticket AC5 (L20); A-02 chose display only.
- REQ-11: ticket AC5 (L21).
- REQ-12: ticket AC6 (L22) and the baseline's data-protection rule.

## Assumed
None.

## Unknown
None. The two terms missing from the glossary ("Patient list", "Last visit") were settled by A-03 and A-01.

## Open questions
None open.

Answered in the IDE:
- A-01 (Q-01, which date counts as the last visit): "Start date (proposed default)". Answered by the user in the IDE. Decides REQ-2. No mismatch with the ticket or baseline.
- A-02 (Q-02, sortable Last visit column): "Not sortable (proposed default)". Answered by the user in the IDE. Decides REQ-10. No mismatch.
- A-03 (Q-03, which list is the patient list): "Yes, that list (proposed default)". Answered by the user in the IDE. Decides REQ-1. No mismatch.
- A-04 (Q-04, date format): "Same as Next appointment (proposed default)". Answered by the user in the IDE. Decides REQ-4. No mismatch.

## Out of scope
- The user story's goal "spot patients who have not been seen for a long time" (L12-L13) is the motivation, not a separate ask. No threshold or highlight is requested, so no requirement was written for it.
- Showing Last visit on other screens (dashboard, patient page): not asked; A-03 limits the change to the patient list.
