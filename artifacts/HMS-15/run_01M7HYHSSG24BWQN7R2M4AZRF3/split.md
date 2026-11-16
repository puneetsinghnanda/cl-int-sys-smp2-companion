---
schema: stories.v1
run_id: run_01M7HYHSSG24BWQN7R2M4AZRF3
step: split
attempt: 1
skill: story-split@1.3.0
inputs:
  - path: artifacts/HMS-15/run_01M7HYHSSG24BWQN7R2M4AZRF3/req.md
    sha256: 3054686c1ee1ec1e6f05993d831044fb058e06c509ab0d9300f743d21b2ac834
  - path: context/glossary.md
    sha256: 430a90e14c50f969f7f757089c61cf254eb21596ed6e60abb8bf59286822b9ff
pack_sha:
  sample-system: 7681f87749883d71abb4011d118366ca1242e1cc
claims:
  - id: C1
    text: "A Completed appointment is closed only once its `start time` has passed, so it can never start after now."
    cites: ["context/glossary.md#completed"]
  - id: C2
    text: "A No-show is a `past appointment`, so it can never start after now."
    cites: ["context/glossary.md#no-show"]
  - id: C3
    text: "Now and start times are compared in the hospital time zone `Asia/Kolkata`."
    cites: ["artifacts/HMS-15/run_01M7HYHSSG24BWQN7R2M4AZRF3/req.md#item:REQ-9"]
  - id: C4
    text: "The next appointment links to the appointment detail page `/appointments/:id`."
    cites: ["artifacts/HMS-15/run_01M7HYHSSG24BWQN7R2M4AZRF3/req.md#item:REQ-11", "runs/run_01M7HYHSSG24BWQN7R2M4AZRF3/ide-answers.md#a-01"]
  - id: C5
    text: "Now is taken when the patient page loads, with no live refresh (`Yes, confirm`)."
    cites: ["runs/run_01M7HYHSSG24BWQN7R2M4AZRF3/ide-answers.md#a-02"]
  - id: C6
    text: "A Cancelled appointment is one `called off before it took place`, so it can still have a start time after now."
    cites: ["context/glossary.md#cancelled"]
trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10, REQ-11]
known: [S-1, S-2, S-3, S-4]
assumed: []
unknown: []
questions: []
stories:
  - id: S-1
    title: Staff see the patient's next appointment above the Appointments table
    trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-9, REQ-10]
    acceptance_criteria:
      - id: AC1
        text: "Given a patient has two Scheduled appointments that start after now, where the one booked first starts on 20 March 11:00 and the one booked second starts on 18 March 10:00 with a different doctor, when front-desk staff open the patient page, then above the Appointments table the page shows \"Next appointment:\" followed by the 18 March 10:00 appointment's date and time, in the same format as the table, and that appointment's doctor's name."
      - id: AC2
        text: "Given now is 15 March 00:30 in Asia/Kolkata (Asia/Kolkata = UTC+05:30, so this is 14 March 19:00 UTC, local side just after midnight), the patient has a Scheduled appointment that started on 15 March 00:15 local (14 March 18:45 UTC) and another Scheduled appointment on 15 March 10:00 local, when staff open the patient page, then the line shows the 15 March 10:00 appointment, dated 15 March, not the 15 March 00:15 one."
      - id: AC3
        text: "Given now is 14 March 23:50 in Asia/Kolkata (14 March 18:20 UTC) and the patient's only upcoming Scheduled appointment starts on 15 March 00:10 local (14 March 18:40 UTC, a local date different from its UTC date), when staff open the patient page, then the line shows that appointment dated 15 March, as the table shows it."
      - id: AC4
        text: "Given the patient page is open and shows a next appointment, and that appointment's start time passes while the page stays open, when staff look at the page without reloading, then the line still shows the same appointment; and when staff reload the page, then the line shows the patient's next Scheduled appointment that starts after the reload, or \"No upcoming appointment\" if there is none."
  - id: S-2
    title: Closed and started appointments are skipped, with an empty state
    trace: [REQ-5, REQ-6, REQ-7]
    acceptance_criteria:
      - id: AC1
        text: "Given the patient's earliest appointment that starts after now is Cancelled and a later appointment is Scheduled, when staff open the patient page, then \"Next appointment:\" shows the later Scheduled appointment, not the Cancelled one."
      - id: AC2
        text: "Given the patient's most recent appointments are one Completed and one No-show and the patient has no Scheduled appointment that starts after now, when staff open the patient page, then the page shows \"No upcoming appointment\" and no \"Next appointment:\" line."
      - id: AC3
        text: "Given now is 09:10 local and the patient's only Scheduled appointment started at 09:00 local today, when staff open the patient page, then the page shows \"No upcoming appointment\"."
      - id: AC4
        text: "Given the patient's only appointment that starts after now is Cancelled, when staff open the patient page, then the page shows \"No upcoming appointment\"."
      - id: AC5
        text: "Given the patient has no appointments at all, when staff open the patient page, then the page shows \"No upcoming appointment\"."
  - id: S-3
    title: Staff open the next appointment from the line
    trace: [REQ-11]
    acceptance_criteria:
      - id: AC1
        text: "Given the patient page shows \"Next appointment:\" for an appointment, when staff select that next appointment on the line, then the appointment detail page /appointments/:id opens for that same appointment."
      - id: AC2
        text: "Given the patient page shows \"No upcoming appointment\", when staff look at that text, then it is not a link to any appointment detail page."
  - id: S-4
    title: The rest of the patient page is unchanged
    trace: [REQ-8]
    acceptance_criteria:
      - id: AC1
        text: "Given a patient with a Scheduled appointment that starts after now, when staff open the patient page, then the Book appointment button and the Appointments table (rows, order, columns and values) are the same as before this change."
      - id: AC2
        text: "Given a patient with no Scheduled appointment that starts after now, when staff open the patient page, then the Book appointment button and the Appointments table (rows, order, columns and values) are the same as before this change."
---
# Stories: HMS-15

## Stories

### S-1 Staff see the patient's next appointment above the Appointments table
Traces REQ-1, REQ-2, REQ-3, REQ-4, REQ-9, REQ-10.
- AC1: Given two upcoming Scheduled appointments where the one booked first starts later (20 March 11:00) and the one booked second starts earlier (18 March 10:00, different doctor), when staff open the patient page, then "Next appointment:" above the Appointments table shows the 18 March 10:00 appointment's date and time in the table's format and its doctor's name.
- AC2: Given now is 15 March 00:30 Asia/Kolkata (14 March 19:00 UTC; Asia/Kolkata = UTC+05:30), one Scheduled appointment started at 15 March 00:15 local and another is at 15 March 10:00 local, when staff open the page, then the line shows the 15 March 10:00 appointment, dated 15 March.
- AC3: Given now is 14 March 23:50 local (14 March 18:20 UTC) and the only upcoming Scheduled appointment is at 15 March 00:10 local (14 March 18:40 UTC), when staff open the page, then the line shows it dated 15 March, as the table does.
- AC4: Given the page is open and the shown appointment's start time passes, when staff do not reload, then the line is unchanged; after a reload it shows the next Scheduled appointment after the reload, or "No upcoming appointment".

### S-2 Closed and started appointments are skipped, with an empty state
Traces REQ-5, REQ-6, REQ-7.
- AC1: An earlier Cancelled appointment after now is skipped; the later Scheduled one is shown.
- AC2: With only Completed and No-show appointments (they are always past, C1, C2), the page shows "No upcoming appointment".
- AC3: A Scheduled appointment that started at 09:00 local, with now 09:10 local, is not used; the page shows "No upcoming appointment".
- AC4: When the only appointment after now is Cancelled, the page shows "No upcoming appointment".
- AC5: A patient with no appointments sees "No upcoming appointment".

### S-3 Staff open the next appointment from the line
Traces REQ-11.
- AC1: Selecting the next appointment on the line opens /appointments/:id for that appointment.
- AC2: "No upcoming appointment" is not a link.

### S-4 The rest of the patient page is unchanged
Traces REQ-8.
- AC1: With an upcoming Scheduled appointment, the Book appointment button and Appointments table are as before.
- AC2: With no upcoming Scheduled appointment, the Book appointment button and Appointments table are as before.

## Known
- S-1: built from known REQ-1, REQ-2, REQ-3, REQ-4, REQ-9 and REQ-10 (REQ-10 confirmed in A-02).
- S-2: built from known REQ-5, REQ-6 and REQ-7.
- S-3: built from known REQ-11 (decided in A-01).
- S-4: built from known REQ-8.

## Assumed
None.

## Unknown
None.

## Open questions
None.
