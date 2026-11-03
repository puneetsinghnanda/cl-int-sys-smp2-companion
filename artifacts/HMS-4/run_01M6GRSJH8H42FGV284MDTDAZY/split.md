---
schema: stories.v1
run_id: run_01M6GRSJH8H42FGV284MDTDAZY
step: split
attempt: 1
skill: story-split@1.2.0
inputs:
- path: artifacts/HMS-4/run_01M6GRSJH8H42FGV284MDTDAZY/req.md
  sha256: 46b4592f9dc5abdb3a91db9291a7078ecdbd2594a7c01a3cb7a5228916e0da85
- path: context/glossary.md
  sha256: 7623345ad8bad33b8a14a713b452edb2a36834e8b1c2228b76f3aa154034f16e
pack_sha:
  sample-system: bd98ff08b83f8762c5e5c073ba5fadef88169b33
claims:
- id: C1
  text: "A Completed appointment is closed by staff once its `start time` has passed."
  cites: ["context/glossary.md#completed"]
- id: C2
  text: "A Scheduled appointment is booked and `still open`, so its start time can be in the past before staff close it."
  cites: ["context/glossary.md#scheduled"]
- id: C3
  text: "Medical history includes `Blood group`, allergies, chronic conditions, current medications and notes."
  cites: ["context/glossary.md#medical-history"]
- id: C4
  text: "A Cancelled appointment is called off before it took place, `with a reason`."
  cites: ["context/glossary.md#cancelled"]
trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10, REQ-11, REQ-12, REQ-13, REQ-14, REQ-15]
known: [S-1, S-2, S-3, S-4, S-5]
assumed: []
unknown: []
questions: []
stories:
- id: S-1
  title: Front-desk staff see each patient's last visit on the patient list
  trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-14]
  acceptance_criteria:
  - id: AC1
    text: "Given a patient with two Completed appointments, one starting at 10:00 Asia/Kolkata on 3 March and one starting at 11:30 Asia/Kolkata on 17 March, where staff closed the 17 March one before the 3 March one, when front-desk staff open the patient list, then the patient's row has a column \"Last visit\" showing 17 March at 11:30."
  - id: AC2
    text: "Given a patient whose only Completed appointment starts at 01:00 Asia/Kolkata on 10 March (Asia/Kolkata = UTC+05:30, so this is 19:30 UTC on 9 March, on the local side after midnight), when staff open the patient list, then Last visit shows 10 March at 01:00, not 9 March and not 19:30."
  - id: AC3
    text: "Given a patient whose only Completed appointment starts at 23:45 Asia/Kolkata on 10 March (Asia/Kolkata = UTC+05:30, so this is 18:15 UTC on 10 March, the same date on both sides), when staff open the patient list, then Last visit shows 10 March at 23:45."
  - id: AC4
    text: "Given a patient with a Completed appointment and an upcoming Scheduled appointment, when staff open the patient list, then Last visit is written in exactly the same format as the Next appointment column in that row, with both the date and the hour:minute."
  - id: AC5
    text: "Given one patient whose last Completed appointment started on 3 March of the previous year and one whose last Completed appointment started yesterday, when staff open the patient list, then both rows look alike apart from the Last visit value, with no highlight, and the list offers no new filter or threshold."
- id: S-2
  title: Only Completed appointments count as a visit, and patients without one show a dash
  trace: [REQ-7, REQ-8]
  acceptance_criteria:
  - id: AC1
    text: "Given a patient with a Completed appointment starting at 10:00 Asia/Kolkata on 3 March and a No-show appointment starting on 17 March, when staff open the patient list, then Last visit shows 3 March at 10:00."
  - id: AC2
    text: "Given a patient with a Completed appointment starting at 10:00 Asia/Kolkata on 3 March and a Cancelled appointment that was due to start on 17 March, when staff open the patient list, then Last visit shows 3 March at 10:00."
  - id: AC3
    text: "Given a patient with a Completed appointment starting at 10:00 Asia/Kolkata on 3 March and a Scheduled appointment that started on 17 March but has not been closed yet, when staff open the patient list, then Last visit shows 3 March at 10:00."
  - id: AC4
    text: "Given a patient whose appointments are only Cancelled, No-show or Scheduled (none Completed), when staff open the patient list, then Last visit shows \"-\"."
  - id: AC5
    text: "Given a patient with no appointments at all, when staff open the patient list, then Last visit shows \"-\"."
- id: S-3
  title: Paging, search and sort of the patient list keep working as before
  trace: [REQ-9, REQ-10, REQ-11, REQ-15]
  acceptance_criteria:
  - id: AC1
    text: "Given the system holds 21 patients and, in the list's current sort order, the 1st and the 20th patient each have two Completed appointments plus a Cancelled and a No-show appointment, when staff open the first and then the second page of the patient list, then the list shows 20 per page, every patient appears in exactly one row, page one holds the 1st to the 20th patient, and the 21st patient is the only row on page two."
  - id: AC2
    text: "Given patients whose names match a search term and patients whose names do not, when staff search the patient list with that term, then it returns the same patients as before this change, each row with a Last visit value."
  - id: AC3
    text: "Given patients whose last visits are in a different order from the list's current sort order, when staff open the patient list, then the rows appear in the current sort order, not ordered by Last visit."
  - id: AC4
    text: "Given the patient list is open, when staff look at the sort options and select the Last visit column header, then the options are the same as before this change, none sorts by Last visit, and the row order does not change."
- id: S-4
  title: The Next appointment column is unchanged
  trace: [REQ-12]
  acceptance_criteria:
  - id: AC1
    text: "Given a patient with a Completed appointment and an upcoming Scheduled appointment, when staff open the patient list, then the Next appointment column shows that upcoming appointment exactly as it did before this change."
  - id: AC2
    text: "Given a patient with a Completed appointment and no upcoming Scheduled appointment, when staff open the patient list, then the Next appointment column shows the same value as before this change, and the Completed appointment appears only under Last visit."
- id: S-5
  title: No medical history appears in the patient list
  trace: [REQ-13]
  acceptance_criteria:
  - id: AC1
    text: "Given a patient with a recorded blood group, allergies, chronic conditions, current medications and notes and a Completed appointment, when staff open the patient list, then none of the medical history values appear in the list, including in the Last visit column."
---
# Stories: HMS-4

## Stories

### S-1 Front-desk staff see each patient's last visit on the patient list
Traces REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-14.
- AC1: Given two Completed appointments (3 March 10:00 and 17 March 11:30), where the 17 March one was closed first, then the "Last visit" column shows 17 March at 11:30 (latest start, not latest closed).
- AC2: Given a Completed appointment at 01:00 Asia/Kolkata on 10 March (19:30 UTC on 9 March), then Last visit shows 10 March at 01:00, not 9 March and not 19:30.
- AC3: Given a Completed appointment at 23:45 Asia/Kolkata on 10 March (18:15 UTC on 10 March), then Last visit shows 10 March at 23:45.
- AC4: Given a Completed appointment and an upcoming Scheduled one, then Last visit uses exactly the Next appointment format, date and hour:minute.
- AC5: Given a patient last seen on 3 March of the previous year and one seen yesterday, then neither row is highlighted and no new filter or threshold is offered.

### S-2 Only Completed appointments count as a visit, and patients without one show a dash
Traces REQ-7, REQ-8.
- AC1: Given a later No-show, then Last visit still shows the earlier Completed appointment.
- AC2: Given a later Cancelled appointment, then Last visit still shows the earlier Completed appointment.
- AC3: Given a later Scheduled appointment whose start has passed but which is not closed, then Last visit still shows the earlier Completed appointment.
- AC4: Given only Cancelled, No-show or Scheduled appointments, then Last visit shows "-".
- AC5: Given no appointments, then Last visit shows "-".

### S-3 Paging, search and sort of the patient list keep working as before
Traces REQ-9, REQ-10, REQ-11, REQ-15.
- AC1: Given 21 patients, where the 1st and the 20th each have several Completed, Cancelled and No-show appointments, then each patient appears exactly once, the list shows 20 per page, and the 21st patient is the only row on page two.
- AC2: Given a search term, then the search returns the same patients as before.
- AC3: Given last visits in a different order from the current sort, then rows keep the current sort order.
- AC4: The sort options are unchanged, none sorts by Last visit, and selecting the Last visit header does not reorder rows.

### S-4 The Next appointment column is unchanged
Traces REQ-12.
- AC1: With an upcoming Scheduled appointment, Next appointment shows it as before.
- AC2: Without an upcoming Scheduled appointment, Next appointment shows the same value as before, and the Completed appointment appears only under Last visit.

### S-5 No medical history appears in the patient list
Traces REQ-13.
- AC1: Given recorded blood group, allergies, chronic conditions, current medications and notes, then none of them appear in the list.

## Known
- S-1: built from known REQ-1 to REQ-6 and REQ-14 (column, latest-start Completed appointment, its start, Asia/Kolkata, same format as Next appointment with time, no highlight or filter).
- S-2: built from known REQ-7 and REQ-8 (Cancelled, No-show and Scheduled excluded; "-" when there is no Completed appointment).
- S-3: built from known REQ-9, REQ-10, REQ-11 and REQ-15 (20 per page, search, sort order, Last visit not sortable).
- S-4: built from known REQ-12 (Next appointment column unchanged).
- S-5: built from known REQ-13 (no medical history in the list).

## Assumed
None.

## Unknown
None.

## Open questions
None.
