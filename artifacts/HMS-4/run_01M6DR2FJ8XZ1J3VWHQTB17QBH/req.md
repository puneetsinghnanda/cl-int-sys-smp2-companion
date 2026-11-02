---
schema: intent.v1
run_id: run_01M6DR2FJ8XZ1J3VWHQTB17QBH
step: req
attempt: 1
skill: requirement-review@1.1.0
inputs:
  - path: artifacts/HMS-4/run_01M6DR2FJ8XZ1J3VWHQTB17QBH/ticket.md
    sha256: cb9d1fac2d833b7e52e4f04026d323655358bc48061cd36bad09c1f7e2f4f6c1
  - path: context/intent-baseline.md
    sha256: 9bfd821aeb2fb9524863e346707e049e4fcfb949a6e55b90223a79194946a215
  - path: context/glossary.md
    sha256: 7623345ad8bad33b8a14a713b452edb2a36834e8b1c2228b76f3aa154034f16e
pack_sha:
  sample-system: bd98ff08b83f8762c5e5c073ba5fadef88169b33
claims:
  - id: C1
    text: "The patient list already shows each patient's `next appointment` (HMS-1)."
    cites: ["artifacts/HMS-4/run_01M6DR2FJ8XZ1J3VWHQTB17QBH/ticket.md#L14"]
  - id: C2
    text: "The hospital time zone is `Asia/Kolkata`."
    cites: ["context/intent-baseline.md#constraints"]
  - id: C3
    text: "Medical history appears only on the patient's own page, never in `lists`."
    cites: ["context/intent-baseline.md#data-protection"]
  - id: C4
    text: "A `Completed` appointment is closed by staff once its `start time` has passed."
    cites: ["context/glossary.md#completed"]
  - id: C5
    text: "`Medical history` is blood group, allergies, chronic conditions, current medications and notes of one patient."
    cites: ["context/glossary.md#medical-history"]
  - id: C6
    text: "In the MVP every user acts as `staff`."
    cites: ["context/glossary.md#front-desk-staff"]
trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10, REQ-11, REQ-12, REQ-13]
known: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10, REQ-11, REQ-12, REQ-13]
assumed: []
unknown: []
requirements:
  - id: REQ-1
    title: Last visit column
    type: functional
    status: known
    statement: "The patient list has a column titled `Last visit`."
    cites: ["artifacts/HMS-4/run_01M6DR2FJ8XZ1J3VWHQTB17QBH/ticket.md#L16"]
  - id: REQ-2
    title: Shows date of most recent Completed appointment
    type: functional
    status: known
    statement: "The `Last visit` column shows the date of the patient's most recent `Completed` appointment."
    cites: ["artifacts/HMS-4/run_01M6DR2FJ8XZ1J3VWHQTB17QBH/ticket.md#L17", "runs/run_01M6DR2FJ8XZ1J3VWHQTB17QBH/ide-answers.md#a-03"]
  - id: REQ-3
    title: Date in hospital time zone
    type: constraint
    status: known
    statement: "The Last visit date is the calendar date in the hospital time zone `Asia/Kolkata`."
    cites: ["artifacts/HMS-4/run_01M6DR2FJ8XZ1J3VWHQTB17QBH/ticket.md#L17", "context/intent-baseline.md#constraints"]
  - id: REQ-4
    title: Other statuses never shown
    type: functional
    status: known
    statement: "`Cancelled`, `No-show` and `Scheduled` appointments are never used for the Last visit column."
    cites: ["artifacts/HMS-4/run_01M6DR2FJ8XZ1J3VWHQTB17QBH/ticket.md#L18"]
  - id: REQ-5
    title: Placeholder when no Completed appointment
    type: functional
    status: known
    statement: "A patient with no `Completed` appointment shows `-` in the Last visit column."
    cites: ["artifacts/HMS-4/run_01M6DR2FJ8XZ1J3VWHQTB17QBH/ticket.md#L19"]
  - id: REQ-6
    title: Page size unchanged
    type: constraint
    status: known
    statement: "The patient list stays `20` per page."
    cites: ["artifacts/HMS-4/run_01M6DR2FJ8XZ1J3VWHQTB17QBH/ticket.md#L20"]
  - id: REQ-7
    title: Search unchanged
    type: constraint
    status: known
    statement: "The patient list keeps its current `search` behaviour."
    cites: ["artifacts/HMS-4/run_01M6DR2FJ8XZ1J3VWHQTB17QBH/ticket.md#L20"]
  - id: REQ-8
    title: Sort order unchanged
    type: constraint
    status: known
    statement: "The patient list keeps its current `sort order`."
    cites: ["artifacts/HMS-4/run_01M6DR2FJ8XZ1J3VWHQTB17QBH/ticket.md#L20"]
  - id: REQ-9
    title: Next appointment column unchanged
    type: constraint
    status: known
    statement: "The existing `Next` appointment column is unchanged."
    cites: ["artifacts/HMS-4/run_01M6DR2FJ8XZ1J3VWHQTB17QBH/ticket.md#L20", "artifacts/HMS-4/run_01M6DR2FJ8XZ1J3VWHQTB17QBH/ticket.md#L14"]
  - id: REQ-10
    title: No medical history in the list
    type: constraint
    status: known
    statement: "No `medical history` appears in the patient list (PRD PAT-8)."
    cites: ["artifacts/HMS-4/run_01M6DR2FJ8XZ1J3VWHQTB17QBH/ticket.md#L22", "context/intent-baseline.md#data-protection"]
  - id: REQ-11
    title: Most recent means latest start time
    type: functional
    status: known
    statement: "The most recent Completed appointment is the one with the latest `start time`."
    cites: ["context/glossary.md#completed", "runs/run_01M6DR2FJ8XZ1J3VWHQTB17QBH/ide-answers.md#a-04"]
  - id: REQ-12
    title: Last visit not sortable
    type: functional
    status: known
    statement: "The Last visit column is display-only and is not added as a sort option (`No sorting`)."
    cites: ["artifacts/HMS-4/run_01M6DR2FJ8XZ1J3VWHQTB17QBH/ticket.md#L20", "runs/run_01M6DR2FJ8XZ1J3VWHQTB17QBH/ide-answers.md#a-01"]
  - id: REQ-13
    title: No highlight or filter for long-unseen patients
    type: functional
    status: known
    statement: "The list only shows the Last visit `date`; it adds no highlight or filter for patients not seen for a long time."
    cites: ["artifacts/HMS-4/run_01M6DR2FJ8XZ1J3VWHQTB17QBH/ticket.md#L12", "runs/run_01M6DR2FJ8XZ1J3VWHQTB17QBH/ide-answers.md#a-02"]
unknowns: []
questions: []
answers:
  - id: A-01
    kind: question
    about: Q-01
    answer: "No sorting (proposed default)"
    requirements: [REQ-12]
  - id: A-02
    kind: question
    about: Q-02
    answer: "No, only show the date (proposed default)"
    requirements: [REQ-13]
  - id: A-03
    kind: question
    about: Q-03
    answer: "Yes, use ticket L17 (proposed default)"
    requirements: [REQ-2]
  - id: A-04
    kind: assumption
    about: REQ-11
    answer: "Yes, confirm"
    requirements: [REQ-11]
glossary:
  - {term: Completed, found: true}
  - {term: Cancelled, found: true}
  - {term: No-show, found: true}
  - {term: Scheduled, found: true}
  - {term: Front-desk staff, found: true}
  - {term: Medical history, found: true}
  - {term: Last visit, found: false}
  - {term: Patient list, found: false}
  - {term: Next appointment, found: false}
---
# Requirements review: HMS-4

## Summary
HMS-4 asks for a new "Last visit" column on the patient list so front-desk staff can spot patients who have not been seen for a long time. The column shows the hospital-time-zone date of each patient's most recent Completed appointment, or "-" when there is none. The list's page size, search, sort order and the existing Next appointment column must not change, and no medical history may appear. Main risks: regressing the existing list behaviour (paging, search, sort, Next appointment from HMS-1), date-boundary errors from the Asia/Kolkata time zone, and leaking non-Completed or medical data into a list view.

## Requirements
- REQ-1 Last visit column — known — The patient list has a column titled "Last visit".
- REQ-2 Shows date of most recent Completed appointment — known — The column shows the date of the patient's most recent Completed appointment.
- REQ-3 Date in hospital time zone — known — The date is the calendar date in Asia/Kolkata.
- REQ-4 Other statuses never shown — known — Cancelled, No-show and Scheduled appointments are never used for the column.
- REQ-5 Placeholder when no Completed appointment — known — Shows "-".
- REQ-6 Page size unchanged — known — The list stays 20 per page.
- REQ-7 Search unchanged — known — The list keeps its current search behaviour.
- REQ-8 Sort order unchanged — known — The list keeps its current sort order.
- REQ-9 Next appointment column unchanged — known — The existing Next appointment column is unchanged.
- REQ-10 No medical history in the list — known — No medical history appears in the list.
- REQ-11 Most recent means latest start time — known — Most recent = latest appointment start time.
- REQ-12 Last visit not sortable — known — The column is display-only, not a sort option.
- REQ-13 No highlight or filter — known — Only the date is shown; no long-unseen highlight or filter.

## Known
- REQ-1, REQ-2, REQ-4, REQ-5: ticket acceptance criteria 1–4 (L16–L19); REQ-2 definition confirmed by A-03.
- REQ-3: ticket L17 plus baseline constraint (Asia/Kolkata).
- REQ-6, REQ-7, REQ-8, REQ-9: ticket acceptance criterion 5 (L20–L21).
- REQ-10: ticket L22 and baseline data-protection rule.
- REQ-11: assumption confirmed by the user (A-04).
- REQ-12: decided by the user (A-01).
- REQ-13: decided by the user (A-02).

## Assumed
None — the user confirmed the only assumption (REQ-11).

## Unknown
None — all questions were answered in the IDE.

## Open questions
None open.

Answered in the IDE:
- A-01 (Q-01: should Last visit be sortable?) → "No sorting (proposed default)", answered by the user; decides REQ-12. No mismatch.
- A-02 (Q-02: highlight/filter long-unseen patients?) → "No, only show the date (proposed default)", answered by the user; decides REQ-13. No mismatch.
- A-03 (Q-03: "Last visit" not in glossary — use ticket L17 definition?) → "Yes, use ticket L17 (proposed default)", answered by the user; decides REQ-2. No mismatch. Glossary may later add "Last visit", "Patient list" and "Next appointment".
- A-04 (REQ-11: most recent = latest start time?) → "Yes, confirm", answered by the user; decides REQ-11. No mismatch.

## Out of scope
- Highlighting, filtering or alerting on patients not seen for a long time (user story motive only; excluded by A-02).
- Sorting by Last visit (excluded by A-01).
- Changes to the patient's own page, dashboard or Next appointment logic (not asked by the ticket).
