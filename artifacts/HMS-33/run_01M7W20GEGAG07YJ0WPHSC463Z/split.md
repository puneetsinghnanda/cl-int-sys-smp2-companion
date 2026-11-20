---
schema: stories.v1
run_id: run_01M7W20GEGAG07YJ0WPHSC463Z
step: split
attempt: 1
skill: story-split@1.4.0
inputs:
  - path: artifacts/HMS-33/run_01M7W20GEGAG07YJ0WPHSC463Z/req.md
    sha256: 8f8b87647a6a511b96c044fc3e961844c5b87b7ac9a1923914a691c76eba9969
  - path: context/glossary.md
    sha256: 430a90e14c50f969f7f757089c61cf254eb21596ed6e60abb8bf59286822b9ff
pack_sha:
  sample-system: 7681f87749883d71abb4011d118366ca1242e1cc
claims:
  - id: C1
    text: "A Cancelled appointment is called off `with a reason`."
    cites: ["context/glossary.md#cancelled"]
  - id: C2
    text: "Only a reason longer than `60 characters` is shortened in the list."
    cites: ["artifacts/HMS-33/run_01M7W20GEGAG07YJ0WPHSC463Z/req.md#item:REQ-5"]
  - id: C3
    text: "The list shows the reason `as staff entered it`, decided in A-01."
    cites: ["artifacts/HMS-33/run_01M7W20GEGAG07YJ0WPHSC463Z/req.md#item:REQ-17", "runs/run_01M7W20GEGAG07YJ0WPHSC463Z/ide-answers.md#a-01"]
trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10, REQ-11, REQ-12, REQ-13, REQ-14, REQ-15, REQ-16, REQ-17]
known: [S-1, S-2, S-3, S-4, S-5, S-6]
assumed: []
unknown: []
questions: []
stories:
  - id: S-1
    title: See a cancelled appointment's reason in the list
    trace: [REQ-1, REQ-2, REQ-3, REQ-6, REQ-17]
    acceptance_criteria:
      - id: AC1
        text: "Given the appointment list with at least one appointment, when staff open it, then the table has a column headed \"Cancellation reason\" placed directly after the Status column."
      - id: AC2
        text: "Given a Cancelled appointment whose reason is \"Patient travelling\", when the list shows it, then its Cancellation reason cell shows \"Patient travelling\"."
      - id: AC3
        text: "Given a Cancelled appointment whose reason is exactly 60 characters long, when the list shows it, then the cell shows the whole reason with no \"…\"."
      - id: AC4
        text: "Given a Cancelled appointment whose reason is \"Fever, will rebook\", when the list shows it, then the cell shows \"Fever, will rebook\" exactly as staff entered it, with no text removed or masked (A-01)."
  - id: S-2
    title: Long reasons are shortened in the list
    trace: [REQ-5, REQ-7, REQ-16]
    acceptance_criteria:
      - id: AC1
        text: "Given a Cancelled appointment whose reason is one character longer than 60 characters, when the list shows it, then the cell shows the first 60 characters followed by \"…\" and not the last character."
      - id: AC2
        text: "Given a Cancelled appointment whose reason is 300 characters long, when the list shows it, then the cell shows the first 60 characters followed by \"…\"."
      - id: AC3
        text: "Given that same Cancelled appointment with a 300-character reason, when staff open the appointment's own page, then the full reason is shown, not shortened."
  - id: S-3
    title: Appointments that are not cancelled show a dash
    trace: [REQ-4]
    acceptance_criteria:
      - id: AC1
        text: "Given a Scheduled appointment, when the list shows it, then its Cancellation reason cell shows \"-\"."
      - id: AC2
        text: "Given a Completed appointment, when the list shows it, then its Cancellation reason cell shows \"-\"."
      - id: AC3
        text: "Given a No-show appointment, when the list shows it, then its Cancellation reason cell shows \"-\"."
  - id: S-4
    title: The column follows filters and paging
    trace: [REQ-8, REQ-9]
    acceptance_criteria:
      - id: AC1
        text: "Given Cancelled appointments exist, when staff filter the list by status Cancelled, then every row shows a reason and no row shows \"-\"."
      - id: AC2
        text: "Given a Cancelled and a Scheduled appointment inside a date range and a Cancelled appointment outside it, when staff set that From and To date, then only the two in range are listed, the Cancelled one shows its reason and the Scheduled one shows \"-\"."
      - id: AC3
        text: "Given more appointments than fit on one page, with a Cancelled appointment on the second page, when staff move to the second page, then that appointment's row shows its own reason."
  - id: S-5
    title: Other appointment tables stay as they are
    trace: [REQ-10, REQ-11, REQ-12, REQ-14, REQ-15]
    acceptance_criteria:
      - id: AC1
        text: "Given the dashboard lists a Cancelled appointment, when staff open the dashboard, then its appointments table has no \"Cancellation reason\" column and its columns are as before."
      - id: AC2
        text: "Given a doctor with a Cancelled appointment, when staff open that doctor's page, then its appointments table has no \"Cancellation reason\" column and its columns are as before."
      - id: AC3
        text: "Given a patient with a Cancelled appointment, when staff open that patient's page, then its appointments table has no \"Cancellation reason\" column and its columns are as before."
      - id: AC4
        text: "Given a patient with medical history and contact details recorded, when the appointment list shows that patient's appointment, then no medical-history field and no contact detail appears in the list."
  - id: S-6
    title: Opening, cancelling and booking work as before
    trace: [REQ-13]
    acceptance_criteria:
      - id: AC1
        text: "Given a Cancelled appointment in the list, when staff click its appointment code, then the appointment's own page opens as before."
      - id: AC2
        text: "Given a Scheduled appointment, when staff cancel it with the reason \"Doctor unavailable\" on its own page, then it becomes Cancelled and the list then shows \"Doctor unavailable\" in its row."
      - id: AC3
        text: "Given staff book a new appointment, when the booking is saved, then it appears in the list as Scheduled with \"-\" in the Cancellation reason column."
---
# Stories: HMS-33

## Stories
- S-1 See a cancelled appointment's reason in the list (REQ-1, REQ-2, REQ-3, REQ-6, REQ-17)
  - AC1: the "Cancellation reason" column sits directly after Status.
  - AC2: a Cancelled row shows its reason "Patient travelling".
  - AC3: a reason of exactly 60 characters is shown whole, no "…" (boundary, C2).
  - AC4: a reason is shown exactly as entered, not masked (A-01, C3).
- S-2 Long reasons are shortened in the list (REQ-5, REQ-7, REQ-16)
  - AC1: one character past 60 characters shows the first 60 plus "…" (just over the limit).
  - AC2: a 300-character reason shows the first 60 plus "…".
  - AC3: the appointment's own page still shows the full 300-character reason.
- S-3 Appointments that are not cancelled show a dash (REQ-4)
  - AC1 Scheduled, AC2 Completed, AC3 No-show: each shows "-" (one case per status).
- S-4 The column follows filters and paging (REQ-8, REQ-9)
  - AC1: status filter Cancelled shows a reason on every row.
  - AC2: a date range lists only appointments in range, each with the right cell.
  - AC3: a Cancelled appointment on the second page shows its own reason.
- S-5 Other appointment tables stay as they are (REQ-10, REQ-11, REQ-12, REQ-14, REQ-15)
  - AC1 dashboard, AC2 doctor page, AC3 patient page: no new column, even when a Cancelled appointment is shown there (the most tempting case).
  - AC4: no medical-history field or contact detail appears in the list.
- S-6 Opening, cancelling and booking work as before (REQ-13)
  - AC1: opening an appointment from the list works.
  - AC2: cancelling with a reason makes the list show that reason.
  - AC3: a new booking appears as Scheduled with "-".

## Known
- S-1 to S-6: built only from known requirements of the intent; every REQ from REQ-1 to REQ-17 is traced. A Cancelled appointment always has a reason (C1).

## Assumed
None.

## Unknown
None.

## Open questions
None. The only question of this run, Q-01, was answered in A-01 (runs/run_01M7W20GEGAG07YJ0WPHSC463Z/ide-answers.md#a-01) and is applied in S-1 AC4.
