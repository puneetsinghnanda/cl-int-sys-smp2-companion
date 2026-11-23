---
schema: stories.v1
run_id: run_01M83R9SM0CVRW4RSHWJJBEGQM
step: split
attempt: 1
skill: story-split@1.4.0
inputs:
  - path: artifacts/HMS-33/run_01M83R9SM0CVRW4RSHWJJBEGQM/req.md
    sha256: e9dd632615c723b4de366bffe1da1e75d2fc4833df8ec163ed535ecc8a733fb0
  - path: context/glossary.md
    sha256: 430a90e14c50f969f7f757089c61cf254eb21596ed6e60abb8bf59286822b9ff
pack_sha:
  sample-system: 7681f87749883d71abb4011d118366ca1242e1cc
claims:
  - id: C1
    text: "The appointment list gets a `Cancellation reason` column directly after Status."
    cites: ["artifacts/HMS-33/run_01M83R9SM0CVRW4RSHWJJBEGQM/req.md#item:REQ-1"]
  - id: C2
    text: "The reason lives in the nullable column `cancellation_reason`."
    cites: ["context/generated/db-schema.json#column:appointment.cancellation_reason"]
  - id: C3
    text: "A Cancelled row with no stored reason shows \"-\", as decided in the IDE (`proposed default`)."
    cites: ["runs/run_01M83R9SM0CVRW4RSHWJJBEGQM/ide-answers.md#a-01"]
  - id: C4
    text: "The list shows `No tooltip` with the full reason."
    cites: ["runs/run_01M83R9SM0CVRW4RSHWJJBEGQM/ide-answers.md#a-02"]
  - id: C5
    text: "A reason longer than `60 characters` shows its first 60 characters followed by \"…\"."
    cites: ["artifacts/HMS-33/run_01M83R9SM0CVRW4RSHWJJBEGQM/req.md#item:REQ-5"]
  - id: C6
    text: "The dashboard, the `doctor page` and the patient page get no new column."
    cites: ["artifacts/HMS-33/run_01M83R9SM0CVRW4RSHWJJBEGQM/req.md#item:REQ-10"]
  - id: C7
    text: "`Medical history` is blood group, allergies, chronic conditions, current medications and notes of one patient."
    cites: ["context/glossary.md#medical-history"]
trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10, REQ-11, REQ-12, REQ-13, REQ-14, REQ-15]
known: [S-1, S-2, S-3, S-4, S-5, S-6]
assumed: []
unknown: []
questions: []
stories:
  - id: S-1
    title: See the cancellation reason in its own column
    trace: [REQ-1, REQ-2, REQ-3, REQ-4]
    acceptance_criteria:
      - id: AC1
        text: "Given the appointment list is open, when staff look at the table header, then a column \"Cancellation reason\" appears directly after the Status column."
      - id: AC2
        text: "Given a Cancelled appointment with the reason \"Doctor on leave\", when staff open the appointment list, then its row shows \"Doctor on leave\" in the Cancellation reason column."
      - id: AC3
        text: "Given a Scheduled appointment, when staff open the appointment list, then its row shows \"-\" in the Cancellation reason column."
      - id: AC4
        text: "Given one Completed and one No-show appointment, when staff open the appointment list, then both rows show \"-\" in the Cancellation reason column."
      - id: AC5
        text: "Given a Cancelled appointment whose stored cancellation_reason is NULL, when staff open the appointment list, then its row shows \"-\" in the Cancellation reason column, not an empty cell or an error (A-01)."
  - id: S-2
    title: Long reasons are cut in the list and read in full on the appointment page
    trace: [REQ-5, REQ-6, REQ-7]
    acceptance_criteria:
      - id: AC1
        text: "Given a Cancelled appointment whose reason is one character longer than 60 characters (the 61st character is \"Z\"), when staff open the appointment list, then the cell shows exactly the first 60 characters followed by \"…\" and no \"Z\"."
      - id: AC2
        text: "Given a Cancelled appointment whose reason is exactly 60 characters long, when staff open the appointment list, then the cell shows the whole reason with no \"…\"."
      - id: AC3
        text: "Given the Cancelled appointment from AC1, when staff open that appointment's own page, then the full reason including the 61st character \"Z\" is shown."
      - id: AC4
        text: "Given the Cancelled appointment from AC1, when staff hover over its cut reason in the list, then no tooltip or hover text with the full reason appears (A-02)."
  - id: S-3
    title: The reason column follows the list's filter, date range and paging
    trace: [REQ-8, REQ-9, REQ-14, REQ-15]
    acceptance_criteria:
      - id: AC1
        text: "Given Scheduled and Cancelled appointments exist, when staff filter the list by status Scheduled, then every shown row shows \"-\" and no cancellation reason appears."
      - id: AC2
        text: "Given two Cancelled appointments, one with the reason \"Patient travelling\" and one whose stored reason is NULL, when staff filter the list by Cancelled, then the first row shows \"Patient travelling\" and the second shows \"-\"."
      - id: AC3
        text: "Given a Cancelled appointment inside the chosen date range and another Cancelled appointment outside it, when staff apply that date range, then only the appointment inside the range is shown, with its own reason."
      - id: AC4
        text: "Given enough appointments to fill more than one page and a Cancelled appointment that falls on the second page, when staff move to the second page, then that row shows its own reason and every other row on that page shows its own reason or \"-\"."
  - id: S-4
    title: Other appointment tables keep their columns
    trace: [REQ-10]
    acceptance_criteria:
      - id: AC1
        text: "Given a Cancelled appointment among the next upcoming appointments shown on the dashboard, when staff open the dashboard, then its appointment table has the same columns as before and no Cancellation reason column."
      - id: AC2
        text: "Given a doctor with a Cancelled appointment, when staff open that doctor page, then its appointment table has the same columns as before and no Cancellation reason column."
      - id: AC3
        text: "Given a patient with a Cancelled appointment, when staff open that patient page, then its appointment table has the same columns as before and no Cancellation reason column."
      - id: AC4
        text: "Given a doctor and a patient with no Cancelled appointments, when staff open their pages, then their appointment tables have the same columns as before and no Cancellation reason column."
  - id: S-5
    title: Opening, cancelling and booking appointments work as before
    trace: [REQ-11]
    acceptance_criteria:
      - id: AC1
        text: "Given the appointment list with the new column, when staff select an appointment row, then that appointment's own page opens as before."
      - id: AC2
        text: "Given a Scheduled appointment, when staff cancel it with the reason \"Clinic closed\", then the cancellation succeeds as before and the list row shows status Cancelled and \"Clinic closed\"."
      - id: AC3
        text: "Given a free slot, when staff book a new appointment, then the booking succeeds as before and the new row shows status Scheduled and \"-\" in the Cancellation reason column."
  - id: S-6
    title: The list shows no medical history or contact details
    trace: [REQ-12, REQ-13]
    acceptance_criteria:
      - id: AC1
        text: "Given a patient with recorded allergies and current medications who has a Cancelled appointment, when staff open the appointment list, then no medical history value of that patient appears in any column."
      - id: AC2
        text: "Given a patient with a phone number and address on record who has a Cancelled appointment, when staff open the appointment list, then no contact details of that patient appear in any column."
---
# Stories: HMS-33

## Stories

**S-1: See the cancellation reason in its own column** (traces REQ-1, REQ-2, REQ-3, REQ-4)
- AC1: Given the appointment list is open, when staff look at the header, then "Cancellation reason" appears directly after Status.
- AC2: Given a Cancelled appointment with the reason "Doctor on leave", when staff open the list, then its row shows "Doctor on leave".
- AC3: Given a Scheduled appointment, when staff open the list, then its row shows "-".
- AC4: Given one Completed and one No-show appointment, when staff open the list, then both rows show "-".
- AC5: Given a Cancelled appointment whose stored cancellation_reason is NULL, when staff open the list, then its row shows "-" (A-01).

**S-2: Long reasons are cut in the list and read in full on the appointment page** (traces REQ-5, REQ-6, REQ-7)
- AC1: Given a reason one character longer than 60 characters (61st character "Z"), when staff open the list, then the cell shows the first 60 characters followed by "…" and no "Z".
- AC2: Given a reason exactly 60 characters long, when staff open the list, then the cell shows the whole reason with no "…".
- AC3: Given the AC1 appointment, when staff open its own page, then the full reason including "Z" is shown.
- AC4: Given the AC1 appointment, when staff hover over the cut reason, then no tooltip appears (A-02).

**S-3: The reason column follows the list's filter, date range and paging** (traces REQ-8, REQ-9, REQ-14, REQ-15)
- AC1: Given Scheduled and Cancelled appointments, when staff filter by Scheduled, then every row shows "-".
- AC2: Given a Cancelled appointment with the reason "Patient travelling" and one with a NULL reason, when staff filter by Cancelled, then they show "Patient travelling" and "-".
- AC3: Given Cancelled appointments inside and outside a date range, when staff apply it, then only the one inside is shown, with its own reason.
- AC4: Given a Cancelled appointment on the second page, when staff move to the second page, then its row shows its own reason.

**S-4: Other appointment tables keep their columns** (traces REQ-10)
- AC1: Given a Cancelled appointment among the dashboard's upcoming appointments, when staff open the dashboard, then there is no Cancellation reason column.
- AC2: Given a doctor with a Cancelled appointment, when staff open the doctor page, then there is no Cancellation reason column.
- AC3: Given a patient with a Cancelled appointment, when staff open the patient page, then there is no Cancellation reason column.
- AC4: Given a doctor and a patient with no Cancelled appointments, when staff open their pages, then the columns are unchanged.

**S-5: Opening, cancelling and booking appointments work as before** (traces REQ-11)
- AC1: Given the list with the new column, when staff select a row, then the appointment's own page opens.
- AC2: Given a Scheduled appointment, when staff cancel it with the reason "Clinic closed", then the cancellation succeeds and the row shows Cancelled and "Clinic closed".
- AC3: Given a free slot, when staff book an appointment, then the booking succeeds and the new row shows Scheduled and "-".

**S-6: The list shows no medical history or contact details** (traces REQ-12, REQ-13)
- AC1: Given a patient with recorded allergies and medications who has a Cancelled appointment, when staff open the list, then no medical history appears.
- AC2: Given a patient with a phone number and address on record, when staff open the list, then no contact details appear.

## Known
- S-1: built from known REQ-1 to REQ-4 (REQ-4 decided by A-01).
- S-2: built from known REQ-5 to REQ-7 (REQ-7 decided by A-02).
- S-3: built from known REQ-8, REQ-9, REQ-14 and REQ-15.
- S-4: built from known REQ-10.
- S-5: built from known REQ-11.
- S-6: built from known REQ-12 and REQ-13.

## Assumed
None

## Unknown
None

## Open questions
None
