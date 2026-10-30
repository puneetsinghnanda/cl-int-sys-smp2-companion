---
schema: intent.v1
run_id: run_01M66D77982JFTXDREQWB5QTJV
step: req
attempt: 1
skill: requirement-review@1.0.0
inputs:
  - path: artifacts/HMS-4/run_01M66D77982JFTXDREQWB5QTJV/ticket.md
    sha256: e92810ff50ae59e26c064655adf2f5d4fbb0ae498d0cc3f335b59aed28877871
  - path: context/intent-baseline.md
    sha256: 9bfd821aeb2fb9524863e346707e049e4fcfb949a6e55b90223a79194946a215
  - path: context/glossary.md
    sha256: 7623345ad8bad33b8a14a713b452edb2a36834e8b1c2228b76f3aa154034f16e
pack_sha:
  sample-system: bd98ff08b83f8762c5e5c073ba5fadef88169b33
claims:
  - id: C1
    text: "The patient list already shows each patient's `next appointment` (HMS-1)."
    cites: ["artifacts/HMS-4/run_01M66D77982JFTXDREQWB5QTJV/ticket.md#L14"]
  - id: C2
    text: "The hospital time zone is `Asia/Kolkata`."
    cites: ["context/intent-baseline.md#constraints"]
  - id: C3
    text: "Medical history appears only on the patient's own page, never in `lists`."
    cites: ["context/intent-baseline.md#data-protection"]
  - id: C4
    text: "A Completed appointment is closed by staff once its `start time` has passed."
    cites: ["context/glossary.md#completed"]
  - id: C5
    text: "Completed, Cancelled and No-show appointments `never change`."
    cites: ["context/intent-baseline.md#closed-appointments"]
  - id: C6
    text: "In the MVP every user acts as `staff`."
    cites: ["context/glossary.md#front-desk-staff"]
  - id: C7
    text: "Medical history covers blood group, `allergies`, chronic conditions, current medications and notes."
    cites: ["context/glossary.md#medical-history"]
trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10, REQ-11, REQ-12, REQ-13, REQ-14]
known: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10, REQ-11, REQ-12]
assumed: [REQ-13, REQ-14]
unknown: [U-1, U-2, U-3, U-4]
requirements:
  - id: REQ-1
    title: Last visit column
    type: functional
    status: known
    statement: "The patient list has a column `Last visit`."
    cites: ["artifacts/HMS-4/run_01M66D77982JFTXDREQWB5QTJV/ticket.md#L16"]
  - id: REQ-2
    title: Last visit shows most recent Completed appointment
    type: functional
    status: known
    statement: "The `Last visit` column shows the date of the patient's most recent `Completed` appointment."
    cites: ["artifacts/HMS-4/run_01M66D77982JFTXDREQWB5QTJV/ticket.md#L17"]
  - id: REQ-3
    title: Last visit date in hospital time zone
    type: constraint
    status: known
    statement: "The `Last visit` date is expressed in the hospital time zone, `Asia/Kolkata`."
    cites: ["artifacts/HMS-4/run_01M66D77982JFTXDREQWB5QTJV/ticket.md#L17", "context/intent-baseline.md#constraints"]
  - id: REQ-4
    title: Cancelled appointments excluded
    type: functional
    status: known
    statement: "A `Cancelled` appointment is never shown in the Last visit column."
    cites: ["artifacts/HMS-4/run_01M66D77982JFTXDREQWB5QTJV/ticket.md#L18"]
  - id: REQ-5
    title: No-show appointments excluded
    type: functional
    status: known
    statement: "A `No-show` appointment is never shown in the Last visit column."
    cites: ["artifacts/HMS-4/run_01M66D77982JFTXDREQWB5QTJV/ticket.md#L18"]
  - id: REQ-6
    title: Scheduled appointments excluded
    type: functional
    status: known
    statement: "A `Scheduled` appointment is never shown in the Last visit column."
    cites: ["artifacts/HMS-4/run_01M66D77982JFTXDREQWB5QTJV/ticket.md#L18"]
  - id: REQ-7
    title: Placeholder when no Completed appointment
    type: functional
    status: known
    statement: "A patient with no Completed appointment shows `-` in the Last visit column."
    cites: ["artifacts/HMS-4/run_01M66D77982JFTXDREQWB5QTJV/ticket.md#L19"]
  - id: REQ-8
    title: Page size unchanged
    type: constraint
    status: known
    statement: "The patient list stays `20 per page`."
    cites: ["artifacts/HMS-4/run_01M66D77982JFTXDREQWB5QTJV/ticket.md#L20"]
  - id: REQ-9
    title: Search unchanged
    type: constraint
    status: known
    statement: "The patient list keeps its current `search` behaviour."
    cites: ["artifacts/HMS-4/run_01M66D77982JFTXDREQWB5QTJV/ticket.md#L20"]
  - id: REQ-10
    title: Sort order unchanged
    type: constraint
    status: known
    statement: "The patient list keeps its current `sort order`."
    cites: ["artifacts/HMS-4/run_01M66D77982JFTXDREQWB5QTJV/ticket.md#L20"]
  - id: REQ-11
    title: Next appointment column unchanged
    type: constraint
    status: known
    statement: "The Next appointment column is `unchanged`."
    cites: ["artifacts/HMS-4/run_01M66D77982JFTXDREQWB5QTJV/ticket.md#L21"]
  - id: REQ-12
    title: No medical history in the list
    type: constraint
    status: known
    statement: "No `medical history` appears in the patient list."
    cites: ["artifacts/HMS-4/run_01M66D77982JFTXDREQWB5QTJV/ticket.md#L22", "context/intent-baseline.md#data-protection"]
  - id: REQ-13
    title: Visit date taken from appointment start time
    type: functional
    status: assumed
    statement: "The most recent Completed appointment and its displayed date are determined by the appointment `start time`."
    basis: "A Completed appointment is closed once its start time has passed, so the start time is when the visit took place; the ticket does not say which timestamp to use."
    cites: ["context/glossary.md#completed"]
  - id: REQ-14
    title: Date format matches existing list
    type: non-functional
    status: assumed
    statement: "The Last visit date uses the same date format as the existing `next appointment` column."
    basis: "The ticket gives no format; the list already shows next appointment, so matching it keeps the list consistent."
    cites: ["artifacts/HMS-4/run_01M66D77982JFTXDREQWB5QTJV/ticket.md#L14"]
unknowns:
  - id: U-1
    text: "The term `Last visit` is not in the glossary."
    question: Q-04
  - id: U-2
    text: "The term `patient list` is not in the glossary."
    question: Q-04
  - id: U-3
    text: "The term `next appointment` is not in the glossary."
    question: Q-04
  - id: U-4
    text: "The ticket does not say whether the Last visit column can be sorted or searched."
    question: Q-03
questions:
  - id: Q-01
    blocking: false
    ask: product owner
    text: Which timestamp defines the Last visit date, the appointment start time or the time staff marked it Completed?
    why: Staff may close an appointment hours or days after it took place, which can change the date shown.
    evidence: ["artifacts/HMS-4/run_01M66D77982JFTXDREQWB5QTJV/ticket.md#L17", "context/glossary.md#completed"]
    options: [appointment start time, time marked Completed]
    default: appointment start time
  - id: Q-02
    blocking: false
    ask: product owner
    text: What date format should the Last visit column use?
    why: The ticket asks for a date but gives no format.
    evidence: ["artifacts/HMS-4/run_01M66D77982JFTXDREQWB5QTJV/ticket.md#L17"]
    options: [same format as the Next appointment column, a new format]
    default: same format as the Next appointment column
  - id: Q-03
    blocking: false
    ask: product owner
    text: Should the Last visit column be sortable or searchable?
    why: "The ticket's goal is to spot patients not seen for a long time, which sorting would help, but it also says the current search and sort order stay."
    evidence: ["artifacts/HMS-4/run_01M66D77982JFTXDREQWB5QTJV/ticket.md#L12", "artifacts/HMS-4/run_01M66D77982JFTXDREQWB5QTJV/ticket.md#L20"]
    options: [display only, sortable, sortable and filterable]
    default: display only
  - id: Q-04
    blocking: false
    ask: tech lead
    text: Should Last visit, patient list and next appointment be added to the glossary?
    why: Agreed definitions keep later steps and tests consistent.
    evidence: ["context/glossary.md#glossary"]
    options: [add all three, leave as is]
    default: "add all three; Last visit as defined in REQ-2"
glossary:
  - {term: Last visit, found: false}
  - {term: Patient list, found: false}
  - {term: Next appointment, found: false}
  - {term: Completed, found: true}
  - {term: Cancelled, found: true}
  - {term: No-show, found: true}
  - {term: Scheduled, found: true}
  - {term: Front-desk staff, found: true}
  - {term: Medical history, found: true}
---
# Requirements review: HMS-4

## Summary
HMS-4 adds a "Last visit" column to the patient list. The column shows the date of each patient's most recent Completed appointment in the hospital time zone, so front-desk staff can spot patients who have not been seen for a long time. Cancelled, No-show and Scheduled appointments never count. Patients with no Completed appointment show "-". The list's paging, search, sort order and Next appointment column must not change, and no medical history may appear. The main risks are which timestamp defines the visit date and how the date is formatted. Both have proposed defaults.

## Requirements
- REQ-1 Last visit column (known): The patient list has a column "Last visit".
- REQ-2 Most recent Completed appointment (known): The column shows the date of the patient's most recent Completed appointment.
- REQ-3 Hospital time zone (known): The date is in Asia/Kolkata.
- REQ-4 Cancelled excluded (known): Cancelled appointments are never shown.
- REQ-5 No-show excluded (known): No-show appointments are never shown.
- REQ-6 Scheduled excluded (known): Scheduled appointments are never shown.
- REQ-7 Placeholder (known): A patient with no Completed appointment shows "-".
- REQ-8 Page size (known): The list stays 20 per page.
- REQ-9 Search (known): Current search behaviour is kept.
- REQ-10 Sort order (known): Current sort order is kept.
- REQ-11 Next appointment (known): The Next appointment column is unchanged.
- REQ-12 No medical history (known): No medical history appears in the list.
- REQ-13 Visit date source (assumed): The appointment start time defines both "most recent" and the date shown.
- REQ-14 Date format (assumed): Same format as the Next appointment column.

## Known
- REQ-1 to REQ-12: each one is stated in the ticket's acceptance criteria (lines 16 to 22). REQ-3 and REQ-12 are also backed by the baseline's constraints and data-protection rules.

## Assumed
- REQ-13: The visit date is the appointment start time. Basis: a Completed appointment is closed once its start time has passed (glossary, Completed).
- REQ-14: The date format matches the existing Next appointment column. Basis: the list already shows next appointment (ticket line 14), and the ticket gives no format.

## Unknown
- U-1: "Last visit" is not in the glossary (Q-04).
- U-2: "Patient list" is not in the glossary (Q-04).
- U-3: "Next appointment" is not in the glossary (Q-04).
- U-4: Whether the new column can be sorted or searched (Q-03).

## Open questions
- Q-01 (non-blocking, product owner): Should the date come from the start time or the completion time? Default: start time.
- Q-02 (non-blocking, product owner): Which date format? Default: same as the Next appointment column.
- Q-03 (non-blocking, product owner): Should the column be sortable or searchable? Default: display only.
- Q-04 (non-blocking, tech lead): Add the three missing terms to the glossary? Default: add all three.

## Out of scope
- Sorting or filtering by Last visit: not asked for. The ticket keeps the current sort order (see Q-03).
- Changes to the Next appointment column (HMS-1): the ticket says it stays unchanged.
