---
schema: intent.v1
run_id: run_01M5FB47NGKFZ6KRCT66E7X5WE
step: req
attempt: 1
skill: requirement-review@1.0.0
inputs:
  - path: artifacts/HMS-1/run_01M5FB47NGKFZ6KRCT66E7X5WE/ticket.md
    sha256: 7633b816a8b691c016b5fe8f4e848a988a8f10f9dac0e67f35ad4129d4727b21
  - path: context/intent-baseline.md
    sha256: 9bfd821aeb2fb9524863e346707e049e4fcfb949a6e55b90223a79194946a215
  - path: context/glossary.md
    sha256: 9bfd556283382d3d3835bf3b12514acc4f6941dc24f21cba0c8eaa4cd658a36c
pack_sha:
  sample-system: 415369398bdad3122ac084a00766a01a3034f900
claims:
  - id: C1
    text: "The hospital time zone is Asia/Kolkata."
    cites: ["context/intent-baseline.md#constraints"]
  - id: C2
    text: "Medical history appears only on the patient's own page, never in lists, the dashboard or logs."
    cites: ["context/intent-baseline.md#data-protection"]
  - id: C3
    text: "A Scheduled appointment is one that is booked and still open."
    cites: ["context/glossary.md#scheduled"]
  - id: C4
    text: "A No-show is a past appointment the patient did not attend, closed instead of staying Scheduled."
    cites: ["context/glossary.md#no-show"]
  - id: C5
    text: "Completed, Cancelled and No-show appointments never change."
    cites: ["context/intent-baseline.md#closed-appointments"]
  - id: C6
    text: "Medical history is blood group, allergies, chronic conditions, current medications and notes of one patient."
    cites: ["context/glossary.md#medical-history"]
  - id: C7
    text: "Staff manage doctors, patients and appointments end to end."
    cites: ["context/intent-baseline.md#goals"]
trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10, REQ-11, REQ-12]
known: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10, REQ-11]
assumed: [REQ-12]
unknown: [U-1, U-2, U-3, U-4, U-5, U-6, U-7, U-8]
requirements:
  - id: REQ-1
    title: Patient list has a Next appointment column
    type: functional
    status: known
    statement: "The patient list has a column titled \"Next appointment\"."
    cites: ["artifacts/HMS-1/run_01M5FB47NGKFZ6KRCT66E7X5WE/ticket.md#L17"]
  - id: REQ-2
    title: Column picks the earliest upcoming Scheduled appointment
    type: functional
    status: known
    statement: "For each patient, the column shows the patient's earliest appointment with status Scheduled whose start is after the current time."
    cites: ["artifacts/HMS-1/run_01M5FB47NGKFZ6KRCT66E7X5WE/ticket.md#L18", "artifacts/HMS-1/run_01M5FB47NGKFZ6KRCT66E7X5WE/ticket.md#L19", "context/glossary.md#scheduled"]
  - id: REQ-3
    title: Start date and time shown in hospital time zone
    type: functional
    status: known
    statement: "The column shows the selected appointment's start date and time in the hospital time zone (Asia/Kolkata)."
    cites: ["artifacts/HMS-1/run_01M5FB47NGKFZ6KRCT66E7X5WE/ticket.md#L18", "context/intent-baseline.md#constraints"]
  - id: REQ-4
    title: Doctor's name shown
    type: functional
    status: known
    statement: "The column shows the name of the doctor of the selected appointment."
    cites: ["artifacts/HMS-1/run_01M5FB47NGKFZ6KRCT66E7X5WE/ticket.md#L18"]
  - id: REQ-5
    title: Closed appointments never shown
    type: functional
    status: known
    statement: "Appointments with status Cancelled, Completed or No-show are never shown in the Next appointment column."
    cites: ["artifacts/HMS-1/run_01M5FB47NGKFZ6KRCT66E7X5WE/ticket.md#L20", "context/intent-baseline.md#closed-appointments"]
  - id: REQ-6
    title: None shown when nothing is upcoming
    type: functional
    status: known
    statement: "A patient with no upcoming Scheduled appointment shows \"None\" in the column."
    cites: ["artifacts/HMS-1/run_01M5FB47NGKFZ6KRCT66E7X5WE/ticket.md#L21"]
  - id: REQ-7
    title: Value links to the appointment's page
    type: functional
    status: known
    statement: "The value in the column links to the page of the selected appointment."
    cites: ["artifacts/HMS-1/run_01M5FB47NGKFZ6KRCT66E7X5WE/ticket.md#L22"]
  - id: REQ-8
    title: List stays 20 per page
    type: constraint
    status: known
    statement: "The patient list still shows 20 patients per page."
    cites: ["artifacts/HMS-1/run_01M5FB47NGKFZ6KRCT66E7X5WE/ticket.md#L23"]
  - id: REQ-9
    title: Current search keeps working
    type: constraint
    status: known
    statement: "The patient list's current search behaviour is unchanged."
    cites: ["artifacts/HMS-1/run_01M5FB47NGKFZ6KRCT66E7X5WE/ticket.md#L23"]
  - id: REQ-10
    title: Current sort order is kept
    type: constraint
    status: known
    statement: "The patient list's current sort order is unchanged."
    cites: ["artifacts/HMS-1/run_01M5FB47NGKFZ6KRCT66E7X5WE/ticket.md#L23"]
  - id: REQ-11
    title: No medical history in the list
    type: constraint
    status: known
    statement: "No medical history appears in the patient list."
    cites: ["artifacts/HMS-1/run_01M5FB47NGKFZ6KRCT66E7X5WE/ticket.md#L24", "context/intent-baseline.md#data-protection", "context/glossary.md#medical-history"]
  - id: REQ-12
    title: None is plain text, not a link
    type: functional
    status: assumed
    statement: "When the column shows \"None\", it is not a link."
    basis: "Only a shown appointment has a page to link to; \"None\" has no appointment behind it."
    cites: ["artifacts/HMS-1/run_01M5FB47NGKFZ6KRCT66E7X5WE/ticket.md#L21", "artifacts/HMS-1/run_01M5FB47NGKFZ6KRCT66E7X5WE/ticket.md#L22"]
unknowns:
  - id: U-1
    text: "The display format of the start date and time is not stated."
    question: Q-01
  - id: U-2
    text: "The inputs do not say whether an appointment page already exists or what it is."
    question: Q-02
  - id: U-3
    text: "Term 'patient list' is not defined in the glossary."
    question: Q-03
  - id: U-4
    text: "Term 'appointment page' is not defined in the glossary."
    question: Q-03
  - id: U-5
    text: "Status 'Cancelled' is not defined in the glossary."
    question: Q-03
  - id: U-6
    text: "Status 'Completed' is not defined in the glossary."
    question: Q-03
  - id: U-7
    text: "Term 'hospital time zone' is not in the glossary (the baseline constraints give its value)."
    question: Q-03
  - id: U-8
    text: "Role 'front-desk staff' is not in the glossary (the glossary only names the actor 'staff')."
    question: Q-03
questions:
  - id: Q-01
    blocking: false
    ask: product owner
    text: "In what format should the next appointment's start date and time be shown?"
    why: "Tests and the UI need an exact format; the ticket only fixes the time zone."
    evidence: ["artifacts/HMS-1/run_01M5FB47NGKFZ6KRCT66E7X5WE/ticket.md#L18", "context/intent-baseline.md#constraints"]
    options: ["Reuse the date-time format already used elsewhere in the app", "A new format chosen by the product owner"]
    default: "Reuse the app's existing date-time format, shown in Asia/Kolkata"
  - id: Q-02
    blocking: false
    ask: tech lead
    text: "Is there an existing appointment page the column can link to?"
    why: "REQ-7 needs a link target; if no such page exists, the scope grows."
    evidence: ["artifacts/HMS-1/run_01M5FB47NGKFZ6KRCT66E7X5WE/ticket.md#L22"]
    options: ["Link to the existing appointment page", "Add an appointment page as part of this ticket", "Drop the link"]
    default: "Link to the existing appointment page; impact analysis confirms it exists"
  - id: Q-03
    blocking: false
    ask: tech lead
    text: "Should 'patient list', 'appointment page', 'Cancelled', 'Completed', 'hospital time zone' and 'front-desk staff' be added to the glossary?"
    why: "The ticket uses these terms but the glossary does not define them, so later steps may read them differently."
    evidence: ["artifacts/HMS-1/run_01M5FB47NGKFZ6KRCT66E7X5WE/ticket.md#L13", "artifacts/HMS-1/run_01M5FB47NGKFZ6KRCT66E7X5WE/ticket.md#L17", "artifacts/HMS-1/run_01M5FB47NGKFZ6KRCT66E7X5WE/ticket.md#L18", "artifacts/HMS-1/run_01M5FB47NGKFZ6KRCT66E7X5WE/ticket.md#L20", "artifacts/HMS-1/run_01M5FB47NGKFZ6KRCT66E7X5WE/ticket.md#L22", "context/glossary.md#actor"]
    options: ["Add the terms", "Leave the glossary as is"]
    default: "Add the terms using their meaning in this ticket and the baseline"
glossary:
  - {term: Scheduled, found: true}
  - {term: No-show, found: true}
  - {term: Medical history, found: true}
  - {term: patient list, found: false}
  - {term: appointment page, found: false}
  - {term: Cancelled, found: false}
  - {term: Completed, found: false}
  - {term: hospital time zone, found: false}
  - {term: front-desk staff, found: false}
---
# Requirements review: HMS-1

## Summary
HMS-1 adds a "Next appointment" column to the patient list. For each patient it shows the start date and time and doctor's name of the earliest upcoming Scheduled appointment, links to that appointment's page, and shows "None" when there is nothing upcoming. The list must keep 20 per page, its search and its sort order, and must never show medical history. The ticket agrees with the baseline (Asia/Kolkata time zone, no medical history in lists), so there are no blocking questions. The open risks are the unstated date-time format, whether an appointment page already exists to link to, and six ticket terms missing from the glossary.

## Requirements
- REQ-1 Patient list has a Next appointment column. **known**: the list has a "Next appointment" column.
- REQ-2 Column picks the earliest upcoming Scheduled appointment. **known**: it shows the earliest Scheduled appointment starting after now.
- REQ-3 Start date and time shown in hospital time zone. **known**: it shows the start date and time in Asia/Kolkata.
- REQ-4 Doctor's name shown. **known**: it shows the appointment's doctor's name.
- REQ-5 Closed appointments never shown. **known**: Cancelled, Completed and No-show appointments are never shown.
- REQ-6 None shown when nothing is upcoming. **known**: a patient with no upcoming Scheduled appointment shows "None".
- REQ-7 Value links to the appointment's page. **known**: the value links to the selected appointment's page.
- REQ-8 List stays 20 per page. **known**: the list still shows 20 patients per page.
- REQ-9 Current search keeps working. **known**: search behaviour is unchanged.
- REQ-10 Current sort order is kept. **known**: sort order is unchanged.
- REQ-11 No medical history in the list. **known**: no medical history appears in the list.
- REQ-12 None is plain text, not a link. **assumed**: "None" is not a link.

## Known
- REQ-1: ticket acceptance 1 (L17).
- REQ-2: ticket acceptance 2 (L18–L19); Scheduled means booked and still open (C3).
- REQ-3: ticket acceptance 2 (L18); the baseline sets the hospital time zone to Asia/Kolkata (C1).
- REQ-4: ticket acceptance 2 (L18).
- REQ-5: ticket acceptance 3 (L20); these are the closed statuses in the baseline (C5).
- REQ-6: ticket acceptance 4 (L21).
- REQ-7: ticket acceptance 5 (L22).
- REQ-8, REQ-9, REQ-10: ticket acceptance 6 (L23).
- REQ-11: ticket acceptance 7 (L24), which matches the baseline's data-protection rule (C2, C6).

## Assumed
- REQ-12: "None" is shown as plain text, not a link. Basis: only an existing appointment has a page to link to (L21–L22).

## Unknown
- U-1: date-time display format not stated. Raises Q-01.
- U-2: whether an appointment page exists to link to. Raises Q-02.
- U-3: "patient list" is not in the glossary. Raises Q-03.
- U-4: "appointment page" is not in the glossary. Raises Q-03.
- U-5: "Cancelled" is not in the glossary. Raises Q-03.
- U-6: "Completed" is not in the glossary. Raises Q-03.
- U-7: "hospital time zone" is not in the glossary; the baseline constraints give its value. Raises Q-03.
- U-8: "front-desk staff" is not in the glossary, which only names the actor "staff". Raises Q-03.

## Open questions
- Q-01 (non-blocking, product owner): what date-time format should the column use? Proposed default: the app's existing format, in Asia/Kolkata.
- Q-02 (non-blocking, tech lead): is there an existing appointment page to link to? Proposed default: link to it, and let impact analysis confirm it exists.
- Q-03 (non-blocking, tech lead): should the six missing terms be added to the glossary? Proposed default: add them.

## Out of scope
- Sorting or filtering the patient list by the new column was not asked for; the ticket says the current sort order stays (L23).
- The ticket cites PRD PAT-8 (L24), which is not among the inputs; REQ-11 relies on the baseline's data-protection rule instead.
- Behaviour for Inactive patients or doctors is not addressed by the ticket.
