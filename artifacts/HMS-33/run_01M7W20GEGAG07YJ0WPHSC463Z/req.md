---
schema: intent.v1
run_id: run_01M7W20GEGAG07YJ0WPHSC463Z
step: req
attempt: 1
skill: requirement-review@1.4.0
inputs:
  - path: artifacts/HMS-33/run_01M7W20GEGAG07YJ0WPHSC463Z/ticket.md
    sha256: 275be76dda6b620bf1ea4c6e30f46d1d433cb908324cd541821bcbb621b21cef
  - path: context/intent-baseline.md
    sha256: 9bfd821aeb2fb9524863e346707e049e4fcfb949a6e55b90223a79194946a215
  - path: context/glossary.md
    sha256: 430a90e14c50f969f7f757089c61cf254eb21596ed6e60abb8bf59286822b9ff
pack_sha:
  sample-system: 7681f87749883d71abb4011d118366ca1242e1cc
claims:
  - id: C1
    text: "A cancelled appointment always has a reason of up to `300 characters`."
    cites: ["artifacts/HMS-33/run_01M7W20GEGAG07YJ0WPHSC463Z/ticket.md#L14"]
  - id: C2
    text: "Today the reason is shown only on the `appointment's own page`."
    cites: ["artifacts/HMS-33/run_01M7W20GEGAG07YJ0WPHSC463Z/ticket.md#L15"]
  - id: C3
    text: "A Cancelled appointment is called off `with a reason`."
    cites: ["context/glossary.md#cancelled"]
  - id: C4
    text: "`Medical history` appears only on the patient's own page, never in lists."
    cites: ["context/intent-baseline.md#data-protection"]
  - id: C5
    text: "Only `Scheduled` appointments can change."
    cites: ["context/glossary.md#scheduled"]
trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10, REQ-11, REQ-12, REQ-13, REQ-14, REQ-15, REQ-16]
known: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10, REQ-11, REQ-12, REQ-13, REQ-14, REQ-15, REQ-16]
assumed: []
unknown: []
requirements:
  - id: REQ-1
    title: Cancellation reason column
    type: functional
    status: known
    statement: "The appointment list has a column \"Cancellation reason\"."
    cites: ["artifacts/HMS-33/run_01M7W20GEGAG07YJ0WPHSC463Z/ticket.md#L17"]
  - id: REQ-2
    title: Column position
    type: functional
    status: known
    statement: "The \"Cancellation reason\" column comes after the Status column."
    cites: ["artifacts/HMS-33/run_01M7W20GEGAG07YJ0WPHSC463Z/ticket.md#L17"]
  - id: REQ-3
    title: Reason of a Cancelled appointment
    type: functional
    status: known
    statement: "For a Cancelled appointment the column shows its cancellation reason."
    cites: ["artifacts/HMS-33/run_01M7W20GEGAG07YJ0WPHSC463Z/ticket.md#L18", "context/glossary.md#cancelled"]
  - id: REQ-4
    title: Other statuses show a dash
    type: functional
    status: known
    statement: "For Scheduled, Completed and No-show appointments the column shows \"-\"."
    cites: ["artifacts/HMS-33/run_01M7W20GEGAG07YJ0WPHSC463Z/ticket.md#L19"]
  - id: REQ-5
    title: Long reasons are shortened
    type: functional
    status: known
    statement: "A reason longer than 60 characters shows its first 60 characters followed by \"…\"."
    cites: ["artifacts/HMS-33/run_01M7W20GEGAG07YJ0WPHSC463Z/ticket.md#L20"]
  - id: REQ-6
    title: Short reasons are shown in full
    type: functional
    status: known
    statement: "A reason of 60 characters or fewer is shown in full, with no \"…\"."
    cites: ["artifacts/HMS-33/run_01M7W20GEGAG07YJ0WPHSC463Z/ticket.md#L18", "artifacts/HMS-33/run_01M7W20GEGAG07YJ0WPHSC463Z/ticket.md#L20"]
  - id: REQ-7
    title: Full reason stays on the appointment page
    type: constraint
    status: known
    statement: "The full reason is still shown on the appointment's own page."
    cites: ["artifacts/HMS-33/run_01M7W20GEGAG07YJ0WPHSC463Z/ticket.md#L20-L21"]
  - id: REQ-8
    title: Column follows filters and paging
    type: functional
    status: known
    statement: "The column follows the list's status filter, date range and paging."
    cites: ["artifacts/HMS-33/run_01M7W20GEGAG07YJ0WPHSC463Z/ticket.md#L22"]
  - id: REQ-9
    title: Cancelled filter shows a reason on every row
    type: functional
    status: known
    statement: "Filtering the list by Cancelled shows a reason on every row."
    cites: ["artifacts/HMS-33/run_01M7W20GEGAG07YJ0WPHSC463Z/ticket.md#L22-L23", "artifacts/HMS-33/run_01M7W20GEGAG07YJ0WPHSC463Z/ticket.md#L14"]
  - id: REQ-10
    title: Dashboard unchanged
    type: constraint
    status: known
    statement: "The dashboard's appointments table is unchanged and gets no new column."
    cites: ["artifacts/HMS-33/run_01M7W20GEGAG07YJ0WPHSC463Z/ticket.md#L24-L25"]
  - id: REQ-11
    title: Doctor page unchanged
    type: constraint
    status: known
    statement: "The doctor page's appointments table is unchanged and gets no new column."
    cites: ["artifacts/HMS-33/run_01M7W20GEGAG07YJ0WPHSC463Z/ticket.md#L24-L25"]
  - id: REQ-12
    title: Patient page unchanged
    type: constraint
    status: known
    statement: "The patient page's appointments table is unchanged and gets no new column."
    cites: ["artifacts/HMS-33/run_01M7W20GEGAG07YJ0WPHSC463Z/ticket.md#L24-L25"]
  - id: REQ-13
    title: Opening, cancelling and booking unchanged
    type: constraint
    status: known
    statement: "Opening, cancelling and booking appointments work as before."
    cites: ["artifacts/HMS-33/run_01M7W20GEGAG07YJ0WPHSC463Z/ticket.md#L26"]
  - id: REQ-14
    title: No medical history added
    type: constraint
    status: known
    statement: "The change adds no medical history to the appointment list."
    cites: ["artifacts/HMS-33/run_01M7W20GEGAG07YJ0WPHSC463Z/ticket.md#L27", "context/intent-baseline.md#data-protection"]
  - id: REQ-15
    title: No contact details added
    type: constraint
    status: known
    statement: "The change adds no contact details to the appointment list."
    cites: ["artifacts/HMS-33/run_01M7W20GEGAG07YJ0WPHSC463Z/ticket.md#L27"]
  - id: REQ-16
    title: Reason length limit
    type: constraint
    status: known
    statement: "A cancellation reason is at most 300 characters; the list must handle any reason up to that length."
    cites: ["artifacts/HMS-33/run_01M7W20GEGAG07YJ0WPHSC463Z/ticket.md#L14"]
unknowns: []
questions: []
answers: []
glossary:
  - {term: Front-desk staff, found: true}
  - {term: Cancelled, found: true}
  - {term: Scheduled, found: true}
  - {term: Completed, found: true}
  - {term: No-show, found: true}
  - {term: Medical history, found: true}
---
# Requirements review: HMS-33

## Summary
Front-desk staff want the appointment list to show why each Cancelled appointment was cancelled, so they do not have to open each one; today the reason is shown only on the appointment's own page (C2). Every Cancelled appointment has a reason of up to 300 characters (C1, C3), so the list shortens long reasons to 60 characters plus "…" and shows "-" for other statuses. The main risk is scope: the dashboard, doctor page and patient page show appointments in the same kind of table and must not change (REQ-10 to REQ-12). The list must also stay free of medical history and contact details (C4).

## Requirements
- REQ-1 Cancellation reason column (known): the appointment list has a column "Cancellation reason".
- REQ-2 Column position (known): the column comes after Status.
- REQ-3 Reason of a Cancelled appointment (known): a Cancelled row shows its reason.
- REQ-4 Other statuses show a dash (known): Scheduled, Completed and No-show rows show "-".
- REQ-5 Long reasons are shortened (known): over 60 characters shows the first 60 plus "…".
- REQ-6 Short reasons are shown in full (known): 60 characters or fewer, no "…".
- REQ-7 Full reason stays on the appointment page (known).
- REQ-8 Column follows filters and paging (known): status filter, date range and paging.
- REQ-9 Cancelled filter shows a reason on every row (known).
- REQ-10 Dashboard unchanged (known).
- REQ-11 Doctor page unchanged (known).
- REQ-12 Patient page unchanged (known).
- REQ-13 Opening, cancelling and booking unchanged (known).
- REQ-14 No medical history added (known).
- REQ-15 No contact details added (known).
- REQ-16 Reason length limit (known): up to 300 characters.

## Known
- REQ-1 to REQ-15: ticket acceptance criteria 1 to 8, one behaviour each.
- REQ-6: follows from criteria 2 and 4: only a reason longer than 60 characters is shortened.
- REQ-9: follows from criterion 5 and the ticket's statement that every cancelled appointment has a reason (C1).
- REQ-14: also the baseline data-protection rule (C4).
- REQ-16: the ticket's stated reason length (C1).

## Assumed
None.

## Unknown
None.

## Open questions
None. Every acceptance criterion states its behaviour and values, so no question was put to the user.

Answered in the IDE: none in this step.

## Out of scope
- A hover tooltip or expand control showing the full reason in the list. Default: none; the full reason stays on the appointment's own page (REQ-7). Trade-off: simpler list, one extra click for long reasons.
- Changing how a reason is entered, its length limit, or which statuses have one.
- Sorting or searching the list by cancellation reason.
