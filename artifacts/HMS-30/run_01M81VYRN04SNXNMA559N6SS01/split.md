---
schema: stories.v1
run_id: run_01M81VYRN04SNXNMA559N6SS01
step: split
attempt: 2
skill: story-split@1.4.0
inputs:
  - path: artifacts/HMS-30/run_01M81VYRN04SNXNMA559N6SS01/req.md
    sha256: 52b4c52bad178b06dd5fc4346654ab569aa7276ac8daae7f6119485236c9cd18
  - path: context/glossary.md
    sha256: 430a90e14c50f969f7f757089c61cf254eb21596ed6e60abb8bf59286822b9ff
pack_sha:
  sample-system: 7681f87749883d71abb4011d118366ca1242e1cc
claims:
  - id: C1
    text: "The counts appear in the order Scheduled, Completed, Cancelled, `No-show`."
    cites: ["artifacts/HMS-30/run_01M81VYRN04SNXNMA559N6SS01/req.md#item:REQ-2"]
  - id: C2
    text: "The counts cover every appointment matching the doctor, patient and date range filters, `not only the current page`."
    cites: ["artifacts/HMS-30/run_01M81VYRN04SNXNMA559N6SS01/req.md#item:REQ-4"]
  - id: C3
    text: "The counts `ignore the Status filter`."
    cites: ["artifacts/HMS-30/run_01M81VYRN04SNXNMA559N6SS01/req.md#item:REQ-5"]
  - id: C4
    text: "Each count link keeps the other filters and shows `page 1`."
    cites:
      - "artifacts/HMS-30/run_01M81VYRN04SNXNMA559N6SS01/req.md#item:REQ-7"
      - "runs/run_01M81VYRN04SNXNMA559N6SS01/ide-answers.md#a-01"
  - id: C5
    text: "Under `Any status` the four counts add up to the total."
    cites: ["artifacts/HMS-30/run_01M81VYRN04SNXNMA559N6SS01/req.md#item:REQ-8"]
  - id: C6
    text: "The list keeps its `20-per-page` paging."
    cites: ["artifacts/HMS-30/run_01M81VYRN04SNXNMA559N6SS01/req.md#item:REQ-10"]
trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10, REQ-11]
known: [S-1, S-2, S-3, S-4]
assumed: []
unknown: []
questions: []
resolves:
  - {id: F-1, how: "S-2 AC4 now compares the displayed counts with the displayed total across two date ranges."}
  - {id: F-2, how: "S-2 AC2 now checks that an appointment on the last day of the range is counted."}
  - {id: F-3, how: "S-2 AC5 now checks that a patient list's counts exclude other patients' appointments."}
stories:
  - id: S-1
    title: See one count per status next to the total
    trace: [REQ-1, REQ-2, REQ-3, REQ-5, REQ-6]
    acceptance_criteria:
      - id: AC1
        text: "Given a doctor's appointment list whose matching appointments are 4 Scheduled, 25 Completed, 3 Cancelled and 2 No-show, with the Status filter set to Any status, when staff open the list, then above the table next to the total it shows \"Scheduled 4 · Completed 25 · Cancelled 3 · No-show 2\" in that order."
      - id: AC2
        text: "Given the same appointments, when staff set the Status filter to Cancelled, then the table shows only the Cancelled appointments and the counts still read \"Scheduled 4 · Completed 25 · Cancelled 3 · No-show 2\", all four statuses shown."
      - id: AC3
        text: "Given a patient's appointment list with Scheduled and Completed appointments but no Cancelled or No-show ones, when staff open the list, then the counts show \"Cancelled 0\" and \"No-show 0\" in their places rather than leaving them out."
  - id: S-2
    title: Counts cover the whole filtered list
    trace: [REQ-4, REQ-8]
    acceptance_criteria:
      - id: AC1
        text: "Given a doctor's list with 21 matching appointments, where the 21st by start time is No-show and sits on page 2, when staff view page 1, then the No-show count includes that 21st appointment."
      - id: AC2
        text: "Given a doctor who has one Completed appointment on the last day of the selected date range and another Completed appointment on the day after the range ends, when staff view the list for that range, then the Completed count includes the last-day appointment and leaves out the day-after one."
      - id: AC3
        text: "Given another doctor has Scheduled appointments in the same date range, when staff view the first doctor's list, then the Scheduled count includes only the first doctor's appointments."
      - id: AC4
        text: "Given a list with the Status filter set to Any status whose matching appointments span more than one page, when staff read the counts and then change the date range and read them again, then each time the four displayed counts add up to the displayed total."
      - id: AC5
        text: "Given another patient has No-show appointments in the same date range, when staff view the first patient's list, then the No-show count includes only the first patient's appointments."
  - id: S-3
    title: Open the list for one status from its count
    trace: [REQ-7]
    acceptance_criteria:
      - id: AC1
        text: "Given staff are on page 2 of a doctor's list with a date range set and Status Any status, when they select the Completed count, then the same doctor's list opens with the same date range, Status set to Completed and page 1 shown."
      - id: AC2
        text: "Given staff are on a patient's list with a date range set and Status Scheduled, when they select the No-show count, then the same patient's list opens with the same date range, Status set to No-show and page 1 shown."
  - id: S-4
    title: Existing table, paging and filters keep working
    trace: [REQ-9, REQ-10, REQ-11]
    acceptance_criteria:
      - id: AC1
        text: "Given a doctor's list with appointments at several start times, when staff open it, then the table rows are ordered by start time exactly as before the change."
      - id: AC2
        text: "Given a doctor's list with 21 matching appointments, when staff open page 1, then the table shows 20 rows and the 21st appointment appears on page 2."
      - id: AC3
        text: "Given a patient's list, when staff change the date range filter, then the table rows and the total change to the new range as before the change."
      - id: AC4
        text: "Given a doctor's list, when staff set the Status filter to Scheduled, then the table and the total show only Scheduled appointments as before the change."
---
# Stories: HMS-30

## Stories

**S-1 See one count per status next to the total**: traces REQ-1, REQ-2, REQ-3, REQ-5, REQ-6
- AC1: Given a doctor's list with 4 Scheduled, 25 Completed, 3 Cancelled and 2 No-show matching appointments and Status Any status, when staff open it, then next to the total it shows "Scheduled 4 · Completed 25 · Cancelled 3 · No-show 2" in that order.
- AC2: Given the same appointments, when Status is set to Cancelled, then the table shows only Cancelled rows while the counts still show all four statuses with the same values.
- AC3: Given a patient's list with no Cancelled or No-show appointments, when opened, then "Cancelled 0" and "No-show 0" are shown in place.

**S-2 Counts cover the whole filtered list**: traces REQ-4, REQ-8
- AC1: Given 21 matching appointments with the 21st (No-show) on page 2, when page 1 is shown, then the No-show count includes it.
- AC2: Given one Completed appointment on the last day of the range and one on the day after, when the list is viewed, then the Completed count includes the last-day one and excludes the day-after one.
- AC3: Given another doctor's Scheduled appointments in the same range, when the first doctor's list is viewed, then they are not counted.
- AC4: Given Any status and matches spanning more than one page, when the counts are read before and after changing the date range, then each time the four displayed counts add up to the displayed total.
- AC5: Given another patient's No-show appointments in the same range, when the first patient's list is viewed, then they are not counted.

Resolved review findings:
- F-1: S-2 AC4 now compares the displayed counts with the displayed total across two date ranges.
- F-2: S-2 AC2 now checks that an appointment on the last day of the range is counted.
- F-3: S-2 AC5 now checks that a patient list's counts exclude other patients' appointments.

**S-3 Open the list for one status from its count**: traces REQ-7
- AC1: Given page 2 of a doctor's list with a date range and Any status, when the Completed count is selected, then the same doctor and date range open with Status Completed on page 1.
- AC2: Given a patient's list with a date range and Status Scheduled, when the No-show count is selected, then the same patient and date range open with Status No-show on page 1.

**S-4 Existing table, paging and filters keep working**: traces REQ-9, REQ-10, REQ-11
- AC1: Given appointments at several start times, when the list opens, then rows are ordered by start time as before.
- AC2: Given 21 matching appointments, when page 1 opens, then it shows 20 rows and the 21st is on page 2.
- AC3: Given a patient's list, when the date range changes, then rows and total follow the new range as before.
- AC4: Given a doctor's list, when Status is set to Scheduled, then the table and total show only Scheduled appointments as before.

## Known
- S-1: built from known REQ-1, REQ-2, REQ-3, REQ-5, REQ-6 (ticket acceptance criteria 1 and 3).
- S-2: built from known REQ-4, REQ-8 (ticket acceptance criteria 2 and 4).
- S-3: built from known REQ-7, with kept filters and page 1 confirmed in runs/run_01M81VYRN04SNXNMA559N6SS01/ide-answers.md#a-01.
- S-4: built from known REQ-9, REQ-10, REQ-11 (ticket acceptance criterion 6), one case each for doctor and patient lists.

## Assumed
None

## Unknown
None

## Open questions
None
