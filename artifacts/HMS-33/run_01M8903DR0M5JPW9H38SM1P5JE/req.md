---
schema: intent.v1
run_id: run_01M8903DR0M5JPW9H38SM1P5JE
step: req
attempt: 1
skill: requirement-review@1.5.0
inputs:
  - path: artifacts/HMS-33/run_01M8903DR0M5JPW9H38SM1P5JE/ticket.md
    sha256: 08fcf94ef9d7b17ffab138d632012bc2f9528aab51bf5f100c294ad5ee2dcb39
  - path: context/intent-baseline.md
    sha256: 9bfd821aeb2fb9524863e346707e049e4fcfb949a6e55b90223a79194946a215
  - path: context/glossary.md
    sha256: 430a90e14c50f969f7f757089c61cf254eb21596ed6e60abb8bf59286822b9ff
pack_sha:
  sample-system: 7681f87749883d71abb4011d118366ca1242e1cc
claims:
  - id: C1
    text: "A cancelled appointment's reason is limited to `300` characters by the ticket."
    cites: ["artifacts/HMS-33/run_01M8903DR0M5JPW9H38SM1P5JE/ticket.md#L14"]
  - id: C7
    text: "The dashboard renders the shared table as `<AppointmentTable items={data.next} />` without the column."
    cites: ["sample-system:web/src/pages/DashboardPage.tsx#L62"]
  - id: C8
    text: "The patient page renders the shared table with `hide=\"patient\"` and without the column."
    cites: ["sample-system:web/src/pages/PatientDetailPage.tsx#L76"]
  - id: C9
    text: "A `Cancelled` appointment is called off with a reason."
    cites: ["context/glossary.md#cancelled"]
  - id: C10
    text: "`Medical history` never appears in lists."
    cites: ["context/intent-baseline.md#data-protection"]
trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10, REQ-11, REQ-12, REQ-13, REQ-14, REQ-15, REQ-16]
known: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10, REQ-11, REQ-12, REQ-13, REQ-14, REQ-15, REQ-16]
assumed: []
unknown: []
requirements:
  - id: REQ-1
    title: Cancellation reason column
    type: functional
    status: known
    statement: "The appointment list has a column `Cancellation reason` placed after the Status column."
    cites: ["artifacts/HMS-33/run_01M8903DR0M5JPW9H38SM1P5JE/ticket.md#L17"]
  - id: REQ-2
    title: Reason shown for Cancelled
    type: functional
    status: known
    statement: "For a `Cancelled` appointment the column shows its cancellation reason."
    cites: ["artifacts/HMS-33/run_01M8903DR0M5JPW9H38SM1P5JE/ticket.md#L18"]
  - id: REQ-3
    title: Dash for other statuses
    type: functional
    status: known
    statement: "For Scheduled, Completed and `No-show` appointments the column shows \"-\"."
    cites: ["artifacts/HMS-33/run_01M8903DR0M5JPW9H38SM1P5JE/ticket.md#L19"]
  - id: REQ-4
    title: Long reasons shortened
    type: functional
    status: known
    statement: "A reason longer than `60` characters shows its first 60 characters followed by \"…\"."
    cites: ["artifacts/HMS-33/run_01M8903DR0M5JPW9H38SM1P5JE/ticket.md#L20"]
  - id: REQ-5
    title: Full reason on the appointment page
    type: functional
    status: known
    statement: "The full reason is still shown on the appointment's `own page`."
    cites: ["artifacts/HMS-33/run_01M8903DR0M5JPW9H38SM1P5JE/ticket.md#L20-L21"]
  - id: REQ-6
    title: Column follows filters and paging
    type: functional
    status: known
    statement: "The column follows the list's `status filter`, date range and paging."
    cites: ["artifacts/HMS-33/run_01M8903DR0M5JPW9H38SM1P5JE/ticket.md#L22"]
  - id: REQ-7
    title: Cancelled filter shows reasons
    type: functional
    status: known
    statement: "Filtering by `Cancelled` shows a reason on every row."
    cites: ["artifacts/HMS-33/run_01M8903DR0M5JPW9H38SM1P5JE/ticket.md#L22-L23"]
  - id: REQ-8
    title: Dashboard unchanged
    type: constraint
    status: known
    statement: "The `dashboard` appointment table is unchanged and gets no new column."
    cites: ["artifacts/HMS-33/run_01M8903DR0M5JPW9H38SM1P5JE/ticket.md#L24-L25"]
  - id: REQ-9
    title: Doctor page unchanged
    type: constraint
    status: known
    statement: "The `doctor page` appointment table is unchanged and gets no new column."
    cites: ["artifacts/HMS-33/run_01M8903DR0M5JPW9H38SM1P5JE/ticket.md#L24-L25"]
  - id: REQ-10
    title: Patient page unchanged
    type: constraint
    status: known
    statement: "The `patient page` appointment table is unchanged and gets no new column."
    cites: ["artifacts/HMS-33/run_01M8903DR0M5JPW9H38SM1P5JE/ticket.md#L24-L25"]
  - id: REQ-11
    title: Opening unchanged
    type: constraint
    status: known
    statement: "`Opening` an appointment works as before."
    cites: ["artifacts/HMS-33/run_01M8903DR0M5JPW9H38SM1P5JE/ticket.md#L26"]
  - id: REQ-12
    title: Cancelling unchanged
    type: constraint
    status: known
    statement: "`cancelling` an appointment works as before."
    cites: ["artifacts/HMS-33/run_01M8903DR0M5JPW9H38SM1P5JE/ticket.md#L26"]
  - id: REQ-13
    title: Booking unchanged
    type: constraint
    status: known
    statement: "`booking` an appointment works as before."
    cites: ["artifacts/HMS-33/run_01M8903DR0M5JPW9H38SM1P5JE/ticket.md#L26"]
  - id: REQ-14
    title: No medical history in the list
    type: constraint
    status: known
    statement: "No `medical history` is added to the list."
    cites: ["artifacts/HMS-33/run_01M8903DR0M5JPW9H38SM1P5JE/ticket.md#L27", "context/intent-baseline.md#data-protection"]
  - id: REQ-15
    title: No contact details in the list
    type: constraint
    status: known
    statement: "No `contact details` are added to the list."
    cites: ["artifacts/HMS-33/run_01M8903DR0M5JPW9H38SM1P5JE/ticket.md#L27"]
  - id: REQ-16
    title: Verify the existing implementation
    type: constraint
    status: known
    statement: "`Verify existing` code against the other requirements and change only what is missing."
    cites: ["runs/run_01M8903DR0M5JPW9H38SM1P5JE/ide-answers.md#a-01"]
unknowns: []
questions: []
answers:
  - id: A-01
    kind: question
    about: Q-01
    answer: "Verify existing (proposed default)"
    requirements: [REQ-16]
glossary:
  - {term: Cancelled, found: true}
  - {term: Scheduled, found: true}
  - {term: Completed, found: true}
  - {term: No-show, found: true}
  - {term: Dashboard, found: true}
  - {term: Front-desk staff, found: true}
  - {term: Medical history, found: true}
---
# Requirements review: HMS-33

## Summary
HMS-33 asks for a "Cancellation reason" column on the appointment list only. Cancelled rows show the reason, shortened past 60 characters; every other status shows "-". The dashboard, doctor page and patient page must stay unchanged, and no medical history or contact details may be added. The dashboard and patient page already use the same shared table without such a column (C7, C8), so the column must stay opt-in. A local checkout appeared to contain an implementation, and the user chose to verify existing code and change only what is missing (A-01); that code is not at the pinned SHA, so the later steps decide what is actually missing. The main risk is the column leaking into the other pages that share the table.

## Requirements
- REQ-1 Cancellation reason column — known — The appointment list has a column `Cancellation reason` after Status.
- REQ-2 Reason shown for Cancelled — known — A `Cancelled` appointment shows its cancellation reason.
- REQ-3 Dash for other statuses — known — Scheduled, Completed and `No-show` show "-".
- REQ-4 Long reasons shortened — known — Over `60` characters: first 60 characters plus "…".
- REQ-5 Full reason on the appointment page — known — The full reason stays on the appointment's `own page`.
- REQ-6 Column follows filters and paging — known — It follows the `status filter`, date range and paging.
- REQ-7 Cancelled filter shows reasons — known — Filtering by `Cancelled` shows a reason on every row.
- REQ-8 Dashboard unchanged — known — The `dashboard` table gets no new column.
- REQ-9 Doctor page unchanged — known — The `doctor page` table gets no new column.
- REQ-10 Patient page unchanged — known — The `patient page` table gets no new column.
- REQ-11 Opening unchanged — known — `Opening` an appointment works as before.
- REQ-12 Cancelling unchanged — known — `cancelling` works as before.
- REQ-13 Booking unchanged — known — `booking` works as before.
- REQ-14 No medical history in the list — known — No `medical history` is added.
- REQ-15 No contact details in the list — known — No `contact details` are added.
- REQ-16 Verify the existing implementation — known — `Verify existing` code against the other requirements and change only what is missing.

## Known
- REQ-1 to REQ-7: acceptance criteria 1 to 5 of the ticket.
- REQ-8 to REQ-10: acceptance criterion 6, split per page.
- REQ-11 to REQ-13: acceptance criterion 7, split per action.
- REQ-14: acceptance criterion 8, backed by the baseline's data protection rule (C10).
- REQ-15: acceptance criterion 8.
- REQ-16: decided by the user in the IDE (A-01).

## Assumed
None.

## Unknown
None.

## Open questions
None open.

Answered in the IDE:
- A-01 (Q-01): HMS-33 looks already implemented at the pinned SHA; how should the run treat it? Answer: "Verify existing (proposed default)", from the user in the IDE. Decides REQ-16. No mismatch with the ticket or baseline.

## Out of scope
- The reason's `300`-character limit (C1) is set when cancelling; the ticket asks for no change to it.
- Changing the appointment's own page: the ticket only requires that it keeps showing the full reason (REQ-5).
