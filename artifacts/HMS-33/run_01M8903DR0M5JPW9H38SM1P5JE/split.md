---
schema: stories.v1
run_id: run_01M8903DR0M5JPW9H38SM1P5JE
step: split
attempt: 1
skill: story-split@1.5.0
inputs:
  - path: artifacts/HMS-33/run_01M8903DR0M5JPW9H38SM1P5JE/req.md
    sha256: be606b63b4e120ec769701f53e8d5a8a4fc7eafc2f0faf17caffd96007153b30
  - path: context/glossary.md
    sha256: 430a90e14c50f969f7f757089c61cf254eb21596ed6e60abb8bf59286822b9ff
pack_sha:
  sample-system: 7681f87749883d71abb4011d118366ca1242e1cc
claims:
  - id: C1
    text: "Long reasons are cut at `60` characters on the list."
    cites: ["artifacts/HMS-33/run_01M8903DR0M5JPW9H38SM1P5JE/req.md#item:REQ-4"]
  - id: C2
    text: "Medical history includes a patient's `allergies`."
    cites: ["context/glossary.md#medical-history"]
  - id: C3
    text: "A `Cancelled` appointment always carries a reason."
    cites: ["context/glossary.md#cancelled"]
trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10, REQ-11, REQ-12, REQ-13, REQ-14, REQ-15, REQ-16, REQ-17, REQ-18]
known: [S-1, S-2, S-3, S-4, S-5, S-6]
assumed: []
unknown: []
questions: []
stories:
  - id: S-1
    title: See the cancellation reason on the appointment list
    trace: [REQ-1, REQ-2, REQ-3, REQ-16]
    acceptance_criteria:
      - id: AC1
        text: "Given the appointment list with at least one appointment, when staff open it, then a column headed \"Cancellation reason\" appears directly after the Status column."
      - id: AC2
        text: "Given a Cancelled appointment whose reason is \"Doctor on leave\", when the list shows it, then its Cancellation reason cell shows \"Doctor on leave\"."
      - id: AC3
        text: "Given one Scheduled, one Completed and one No-show appointment on the list, when the list shows them, then the Cancellation reason cell of each of the three shows \"-\"."
      - id: AC4
        text: "Given the product code at the pinned SHA, when the change is made, then code that already meets S-1 to S-6 is kept and only missing behaviour is added."
  - id: S-2
    title: Long reasons are shortened on the list only
    trace: [REQ-4, REQ-5]
    acceptance_criteria:
      - id: AC1
        text: "Given a Cancelled appointment whose reason is one character longer than 60, when the list shows it, then the cell shows the reason's first 60 characters followed by \"…\" and not the last character."
      - id: AC2
        text: "Given a Cancelled appointment whose reason is exactly 60 characters, when the list shows it, then the cell shows the whole reason with no \"…\"."
      - id: AC3
        text: "Given the Cancelled appointment from AC1, when staff open its own page, then the full reason is shown, including its last character and with no \"…\"."
  - id: S-3
    title: The column follows the list's filter, date range and paging
    trace: [REQ-6, REQ-7, REQ-17, REQ-18]
    acceptance_criteria:
      - id: AC1
        text: "Given Cancelled and Scheduled appointments, when staff filter the list by status Scheduled, then only Scheduled rows are listed and every Cancellation reason cell shows \"-\"."
      - id: AC2
        text: "Given Cancelled, Scheduled and Completed appointments, when staff filter the list by status Cancelled, then only Cancelled rows are listed and every row shows its own reason, none shows \"-\"."
      - id: AC3
        text: "Given a Cancelled appointment inside the chosen date range and another Cancelled appointment outside it, when staff apply the range, then only the one inside is listed, with its reason."
      - id: AC4
        text: "Given more appointments than fit on one page and a Cancelled appointment that falls on the second page, when staff move to the second page, then that appointment's row shows its reason."
  - id: S-4
    title: Other appointment tables stay unchanged
    trace: [REQ-8, REQ-9, REQ-10]
    acceptance_criteria:
      - id: AC1
        text: "Given the dashboard with upcoming appointments, when staff open it, then its appointment table has the same columns as before and no Cancellation reason column."
      - id: AC2
        text: "Given the dashboard with no upcoming appointments, when staff open it, then it shows the same empty state as before and no Cancellation reason column."
      - id: AC3
        text: "Given a doctor with a Cancelled appointment, when staff open the doctor page, then its appointment table has no Cancellation reason column and the reason text is not shown."
      - id: AC4
        text: "Given a patient with a Cancelled appointment, when staff open the patient page, then its appointment table has no Cancellation reason column and the reason text is not shown."
  - id: S-5
    title: Opening, cancelling and booking work as before
    trace: [REQ-11, REQ-12, REQ-13]
    acceptance_criteria:
      - id: AC1
        text: "Given a Cancelled appointment on the list, when staff select its appointment link, then its own page opens as before."
      - id: AC2
        text: "Given a Scheduled appointment, when staff cancel it with a reason, then it is cancelled as before and the list then shows that reason in its Cancellation reason cell."
      - id: AC3
        text: "Given a free slot, when staff book an appointment in it, then it is booked as before and the list shows it as Scheduled with \"-\" in its Cancellation reason cell."
  - id: S-6
    title: No medical history or contact details on the list
    trace: [REQ-14, REQ-15]
    acceptance_criteria:
      - id: AC1
        text: "Given a patient with recorded allergies and a Cancelled appointment, when the list shows that appointment, then no medical history such as the allergies appears anywhere on the list."
      - id: AC2
        text: "Given a patient with a phone number and a Cancelled appointment, when the list shows that appointment, then none of the patient's contact details appear anywhere on the list."
---
# Stories: HMS-33

## Stories

### S-1 See the cancellation reason on the appointment list
Traces: REQ-1, REQ-2, REQ-3, REQ-16.
- AC1: Given the appointment list with at least one appointment, when staff open it, then a column headed "Cancellation reason" appears directly after the Status column.
- AC2: Given a Cancelled appointment whose reason is "Doctor on leave", when the list shows it, then its Cancellation reason cell shows "Doctor on leave".
- AC3: Given one Scheduled, one Completed and one No-show appointment on the list, when the list shows them, then the Cancellation reason cell of each of the three shows "-".
- AC4: Given the product code at the pinned SHA, when the change is made, then code that already meets S-1 to S-6 is kept and only missing behaviour is added.

### S-2 Long reasons are shortened on the list only
Traces: REQ-4, REQ-5. The cut is at `60` characters (C1).
- AC1: Given a Cancelled appointment whose reason is one character longer than 60, when the list shows it, then the cell shows the reason's first 60 characters followed by "…" and not the last character.
- AC2: Given a Cancelled appointment whose reason is exactly 60 characters, when the list shows it, then the cell shows the whole reason with no "…".
- AC3: Given the Cancelled appointment from AC1, when staff open its own page, then the full reason is shown, including its last character and with no "…".

### S-3 The column follows the list's filter, date range and paging
Traces: REQ-6, REQ-7, REQ-17, REQ-18.
- AC1: Given Cancelled and Scheduled appointments, when staff filter the list by status Scheduled, then only Scheduled rows are listed and every Cancellation reason cell shows "-".
- AC2: Given Cancelled, Scheduled and Completed appointments, when staff filter the list by status Cancelled, then only Cancelled rows are listed and every row shows its own reason, none shows "-".
- AC3: Given a Cancelled appointment inside the chosen date range and another Cancelled appointment outside it, when staff apply the range, then only the one inside is listed, with its reason.
- AC4: Given more appointments than fit on one page and a Cancelled appointment that falls on the second page, when staff move to the second page, then that appointment's row shows its reason.

### S-4 Other appointment tables stay unchanged
Traces: REQ-8, REQ-9, REQ-10.
- AC1: Given the dashboard with upcoming appointments, when staff open it, then its appointment table has the same columns as before and no Cancellation reason column.
- AC2: Given the dashboard with no upcoming appointments, when staff open it, then it shows the same empty state as before and no Cancellation reason column.
- AC3: Given a doctor with a Cancelled appointment, when staff open the doctor page, then its appointment table has no Cancellation reason column and the reason text is not shown.
- AC4: Given a patient with a Cancelled appointment, when staff open the patient page, then its appointment table has no Cancellation reason column and the reason text is not shown.

### S-5 Opening, cancelling and booking work as before
Traces: REQ-11, REQ-12, REQ-13.
- AC1: Given a Cancelled appointment on the list, when staff select its appointment link, then its own page opens as before.
- AC2: Given a Scheduled appointment, when staff cancel it with a reason, then it is cancelled as before and the list then shows that reason in its Cancellation reason cell.
- AC3: Given a free slot, when staff book an appointment in it, then it is booked as before and the list shows it as Scheduled with "-" in its Cancellation reason cell.

### S-6 No medical history or contact details on the list
Traces: REQ-14, REQ-15. Medical history includes `allergies` (C2).
- AC1: Given a patient with recorded allergies and a Cancelled appointment, when the list shows that appointment, then no medical history such as the allergies appears anywhere on the list.
- AC2: Given a patient with a phone number and a Cancelled appointment, when the list shows that appointment, then none of the patient's contact details appear anywhere on the list.

## Known
- S-1: built from known REQ-1, REQ-2, REQ-3 and REQ-16 (the user's answer in the req step).
- S-2: built from known REQ-4 and REQ-5.
- S-3: built from known REQ-6, REQ-7, REQ-17 and REQ-18.
- S-4: built from known REQ-8, REQ-9 and REQ-10.
- S-5: built from known REQ-11, REQ-12 and REQ-13.
- S-6: built from known REQ-14 and REQ-15.

## Assumed
None.

## Unknown
None.

## Open questions
None.
