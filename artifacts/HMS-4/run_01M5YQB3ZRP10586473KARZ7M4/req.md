---
schema: intent.v1
run_id: run_01M5YQB3ZRP10586473KARZ7M4
step: req
attempt: 1
skill: requirement-review@1.0.0
inputs:
  - path: artifacts/HMS-4/run_01M5YQB3ZRP10586473KARZ7M4/ticket.md
    sha256: aedc297677ffde07ae0a285a107f77286d06c9320979a9af9c4325fffcfbea84
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
    text: "Medical history appears only on the patient's own page, `never in lists`."
    cites: ["context/intent-baseline.md#data-protection"]
  - id: C3
    text: "Medical history means `Blood group`, allergies, chronic conditions, current medications and notes of one patient."
    cites: ["context/glossary.md#medical-history"]
  - id: C4
    text: "A Completed appointment is one the patient attended, closed by staff once its `start time` has passed."
    cites: ["context/glossary.md#completed"]
  - id: C5
    text: "The patient list already shows each patient's `next appointment` (HMS-1)."
    cites: ["artifacts/HMS-4/run_01M5YQB3ZRP10586473KARZ7M4/ticket.md#L14"]
  - id: C6
    text: "In the MVP every user acts as `staff`."
    cites: ["context/glossary.md#front-desk-staff"]
  - id: C7
    text: "Completed, Cancelled and No-show appointments `never change`."
    cites: ["context/intent-baseline.md#closed-appointments"]
trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10, REQ-11, REQ-12, REQ-13]
known: [REQ-1, REQ-2, REQ-3, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10, REQ-11, REQ-12, REQ-13]
assumed: [REQ-4]
unknown: [U-1, U-2, U-3, U-4, U-5]
requirements:
  - id: REQ-1
    title: Last visit column
    type: functional
    status: known
    statement: "The patient list has a column titled `Last visit`."
    cites: ["artifacts/HMS-4/run_01M5YQB3ZRP10586473KARZ7M4/ticket.md#L16"]
  - id: REQ-2
    title: Last visit shows most recent Completed appointment
    type: functional
    status: known
    statement: "For each patient, the Last visit column shows the date of that patient's most recent `Completed` appointment."
    cites: ["artifacts/HMS-4/run_01M5YQB3ZRP10586473KARZ7M4/ticket.md#L17"]
  - id: REQ-3
    title: Date in hospital time zone
    type: constraint
    status: known
    statement: "The Last visit date is the appointment's date in the hospital time zone, `Asia/Kolkata`."
    cites: ["artifacts/HMS-4/run_01M5YQB3ZRP10586473KARZ7M4/ticket.md#L17", "context/intent-baseline.md#constraints"]
  - id: REQ-4
    title: Most recent ordered by start time
    type: functional
    status: assumed
    statement: "\"Most recent\" Completed appointment is the Completed appointment with the latest `start time`."
    basis: "The ticket does not say which timestamp orders appointments; the glossary defines Completed by its start time having passed, so start time is the natural ordering key."
    cites: ["context/glossary.md#completed", "artifacts/HMS-4/run_01M5YQB3ZRP10586473KARZ7M4/ticket.md#L17"]
  - id: REQ-5
    title: Cancelled appointments excluded
    type: functional
    status: known
    statement: "A `Cancelled` appointment is never shown in the Last visit column."
    cites: ["artifacts/HMS-4/run_01M5YQB3ZRP10586473KARZ7M4/ticket.md#L18"]
  - id: REQ-6
    title: No-show appointments excluded
    type: functional
    status: known
    statement: "A `No-show` appointment is never shown in the Last visit column."
    cites: ["artifacts/HMS-4/run_01M5YQB3ZRP10586473KARZ7M4/ticket.md#L18"]
  - id: REQ-7
    title: Scheduled appointments excluded
    type: functional
    status: known
    statement: "A `Scheduled` appointment is never shown in the Last visit column."
    cites: ["artifacts/HMS-4/run_01M5YQB3ZRP10586473KARZ7M4/ticket.md#L18"]
  - id: REQ-8
    title: Placeholder when no Completed appointment
    type: functional
    status: known
    statement: "A patient with no Completed appointment shows `\"-\"` in the Last visit column."
    cites: ["artifacts/HMS-4/run_01M5YQB3ZRP10586473KARZ7M4/ticket.md#L19"]
  - id: REQ-9
    title: Page size unchanged
    type: constraint
    status: known
    statement: "The patient list stays at `20 per page`."
    cites: ["artifacts/HMS-4/run_01M5YQB3ZRP10586473KARZ7M4/ticket.md#L20"]
  - id: REQ-10
    title: Search unchanged
    type: constraint
    status: known
    statement: "The patient list keeps its `current search` behaviour."
    cites: ["artifacts/HMS-4/run_01M5YQB3ZRP10586473KARZ7M4/ticket.md#L20"]
  - id: REQ-11
    title: Sort order unchanged
    type: constraint
    status: known
    statement: "The patient list keeps its current `sort order`."
    cites: ["artifacts/HMS-4/run_01M5YQB3ZRP10586473KARZ7M4/ticket.md#L20"]
  - id: REQ-12
    title: Next appointment column unchanged
    type: constraint
    status: known
    statement: "The existing Next appointment column (HMS-1) is unchanged: `appointment column is unchanged`."
    cites: ["artifacts/HMS-4/run_01M5YQB3ZRP10586473KARZ7M4/ticket.md#L21", "artifacts/HMS-4/run_01M5YQB3ZRP10586473KARZ7M4/ticket.md#L14"]
  - id: REQ-13
    title: No medical history in the list
    type: constraint
    status: known
    statement: "No `medical history` appears in the patient list (PRD PAT-8); medical history appears only on the patient's own page, never in lists."
    cites: ["artifacts/HMS-4/run_01M5YQB3ZRP10586473KARZ7M4/ticket.md#L22", "context/intent-baseline.md#data-protection"]
unknowns:
  - id: U-1
    text: "The display format of the Last visit date is not given in the ticket or baseline."
    question: Q-01
  - id: U-2
    text: "The ticket does not say whether the Last visit column is itself sortable or searchable."
    question: Q-02
  - id: U-3
    text: "Term `Last visit` is not in the glossary."
    question: Q-03
  - id: U-4
    text: "Term `patient list` is not in the glossary."
    question: Q-03
  - id: U-5
    text: "Term `Next appointment` is not in the glossary."
    question: Q-03
questions:
  - id: Q-01
    blocking: false
    ask: product owner
    text: "Which date format should the Last visit column use?"
    why: "Tests and UI need one exact format; the ticket only says 'the date (hospital time zone)'."
    evidence: ["artifacts/HMS-4/run_01M5YQB3ZRP10586473KARZ7M4/ticket.md#L17", "artifacts/HMS-4/run_01M5YQB3ZRP10586473KARZ7M4/ticket.md#L14"]
    options: ["Same date format as the existing Next appointment column", "A different, specified format"]
    default: "Same date format as the existing Next appointment column, in Asia/Kolkata"
  - id: Q-02
    blocking: false
    ask: product owner
    text: "Should staff be able to sort or filter the list by Last visit (to spot patients not seen for a long time)?"
    why: "The user goal is spotting long-unseen patients, but acceptance criterion 5 keeps the current sort order and adds no sort."
    evidence: ["artifacts/HMS-4/run_01M5YQB3ZRP10586473KARZ7M4/ticket.md#L12", "artifacts/HMS-4/run_01M5YQB3ZRP10586473KARZ7M4/ticket.md#L20"]
    options: ["Display only; no new sort or filter", "Add sort by Last visit (separate ticket)"]
    default: "Display only; no new sort or filter in HMS-4"
  - id: Q-03
    blocking: false
    ask: tech lead
    text: "Should 'Last visit', 'patient list' and 'Next appointment' be added to the glossary?"
    why: "Agreed terms keep later stories and tests consistent."
    evidence: ["context/glossary.md#glossary", "artifacts/HMS-4/run_01M5YQB3ZRP10586473KARZ7M4/ticket.md#L16"]
    options: ["Add the three terms", "Leave as is"]
    default: "Add: Last visit = date of the patient's most recent Completed appointment in hospital time"
glossary:
  - {term: Completed, found: true}
  - {term: Cancelled, found: true}
  - {term: No-show, found: true}
  - {term: Scheduled, found: true}
  - {term: Front-desk staff, found: true}
  - {term: Medical history, found: true}
  - {term: Last visit, found: false}
  - {term: patient list, found: false}
  - {term: Next appointment, found: false}
---
# Requirements review: HMS-4

## Summary
HMS-4 asks for a "Last visit" column on the patient list so front-desk staff can spot patients who have not been seen for a long time. The column shows the hospital-time date of the patient's most recent Completed appointment, ignores every other status, and shows "-" when the patient has none. The list's paging, search, sort order and Next appointment column must not change, and medical history must stay out of the list. The main risks are breaking the existing list query (paging, sort, HMS-1 column) and ordering appointments by the wrong timestamp. Two non-blocking questions remain: the date format, and whether staff need to sort by the new column.

## Requirements
- REQ-1 Last visit column — known — The patient list has a column "Last visit".
- REQ-2 Most recent Completed appointment — known — The column shows the date of the patient's most recent Completed appointment.
- REQ-3 Date in hospital time zone — known — The date is in Asia/Kolkata.
- REQ-4 Most recent by start time — assumed — "Most recent" means the latest start time.
- REQ-5 Cancelled excluded — known — Cancelled appointments are never shown.
- REQ-6 No-show excluded — known — No-show appointments are never shown.
- REQ-7 Scheduled excluded — known — Scheduled appointments are never shown.
- REQ-8 Placeholder — known — No Completed appointment shows "-".
- REQ-9 Page size unchanged — known — 20 per page.
- REQ-10 Search unchanged — known — Current search kept.
- REQ-11 Sort order unchanged — known — Current sort order kept.
- REQ-12 Next appointment column unchanged — known — HMS-1 column unchanged.
- REQ-13 No medical history — known — No medical history in the list.

## Known
- REQ-1, REQ-2, REQ-5 to REQ-12: stated directly in the ticket's acceptance criteria (ticket L16–L21).
- REQ-3: ticket L17 asks for hospital time zone; baseline constraints set it to Asia/Kolkata.
- REQ-13: ticket L22 and baseline data protection (medical history never in lists).

## Assumed
- REQ-4: the most recent Completed appointment is ordered by appointment start time. Basis: the glossary defines Completed by its start time having passed; the ticket names no other timestamp.

## Unknown
- U-1: date display format not given (Q-01).
- U-2: whether Last visit is sortable/searchable (Q-02).
- U-3, U-4, U-5: "Last visit", "patient list" and "Next appointment" are not glossary terms (Q-03).

## Open questions
- Q-01 (non-blocking, product owner): date format; default is the Next appointment column's format.
- Q-02 (non-blocking, product owner): sort or filter by Last visit; default is display only.
- Q-03 (non-blocking, tech lead): add the three terms to the glossary.

## Out of scope
- Flagging or highlighting patients "not seen for a long time": the ticket states this as the motivation, gives no threshold and no acceptance criterion, so no requirement is written (see Q-02).
- PRD PAT-8 itself is not among the inputs; REQ-13 relies on the ticket line and the baseline data-protection rule instead.
