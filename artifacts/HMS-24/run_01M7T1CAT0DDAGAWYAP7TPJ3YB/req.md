---
schema: intent.v1
run_id: run_01M7T1CAT0DDAGAWYAP7TPJ3YB
step: req
attempt: 1
skill: requirement-review@1.4.0
inputs:
  - path: artifacts/HMS-24/run_01M7T1CAT0DDAGAWYAP7TPJ3YB/ticket.md
    sha256: 97b2137ee0bc01f0c2f93b1ee4ea698d00a86e66700e4b65eb0a78122df35c63
  - path: context/intent-baseline.md
    sha256: 9bfd821aeb2fb9524863e346707e049e4fcfb949a6e55b90223a79194946a215
  - path: context/glossary.md
    sha256: 430a90e14c50f969f7f757089c61cf254eb21596ed6e60abb8bf59286822b9ff
pack_sha:
  sample-system: 7681f87749883d71abb4011d118366ca1242e1cc
claims:
  - id: C1
    text: "The patient page (PRD PAT-3) `lists the patient's appointments`, upcoming first, then past."
    cites: ["artifacts/HMS-24/run_01M7T1CAT0DDAGAWYAP7TPJ3YB/ticket.md#L17"]
  - id: C2
    text: "Today staff have to `read every row` of the appointment table to count visits or find the last one."
    cites: ["artifacts/HMS-24/run_01M7T1CAT0DDAGAWYAP7TPJ3YB/ticket.md#L18"]
  - id: C3
    text: "The hospital time zone is `Asia/Kolkata`."
    cites: ["context/intent-baseline.md#constraints"]
  - id: C4
    text: "`Medical history` appears only on the patient's own page, never in lists, the dashboard or logs."
    cites: ["context/intent-baseline.md#data-protection"]
  - id: C5
    text: "A Completed appointment is a past appointment the patient `attended`."
    cites: ["context/glossary.md#completed"]
  - id: C6
    text: "A Scheduled appointment is booked and `still open`."
    cites: ["context/glossary.md#scheduled"]
  - id: C7
    text: "A Cancelled appointment is called off `with a reason`."
    cites: ["context/glossary.md#cancelled"]
  - id: C8
    text: "Completed and No-show are allowed only `after the start time`."
    cites: ["context/intent-baseline.md#closed-appointments"]
trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10, REQ-11, REQ-12, REQ-13, REQ-14]
known: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10, REQ-11, REQ-12, REQ-13, REQ-14]
assumed: []
unknown: []
requirements:
  - id: REQ-1
    title: Visit summary placement
    type: functional
    status: known
    statement: "On the patient page, a `Visit summary` line is shown directly above the Appointments table."
    cites: ["artifacts/HMS-24/run_01M7T1CAT0DDAGAWYAP7TPJ3YB/ticket.md#L21"]
  - id: REQ-2
    title: Completed count
    type: functional
    status: known
    statement: "The visit summary shows the number of the patient's `Completed` appointments."
    cites: ["artifacts/HMS-24/run_01M7T1CAT0DDAGAWYAP7TPJ3YB/ticket.md#L22"]
  - id: REQ-3
    title: No-show count
    type: functional
    status: known
    statement: "The visit summary shows the number of the patient's `No-show` appointments."
    cites: ["artifacts/HMS-24/run_01M7T1CAT0DDAGAWYAP7TPJ3YB/ticket.md#L22"]
  - id: REQ-4
    title: Cancelled count
    type: functional
    status: known
    statement: "The visit summary shows the number of the patient's `Cancelled` appointments."
    cites: ["artifacts/HMS-24/run_01M7T1CAT0DDAGAWYAP7TPJ3YB/ticket.md#L22"]
  - id: REQ-5
    title: Upcoming count
    type: functional
    status: known
    statement: "The visit summary shows the number of the patient's `Scheduled appointments that start after now` as the upcoming count."
    cites: ["artifacts/HMS-24/run_01M7T1CAT0DDAGAWYAP7TPJ3YB/ticket.md#L23"]
  - id: REQ-6
    title: Counts cover all appointments
    type: functional
    status: known
    statement: "The counts cover all of the patient's appointments, with `any doctor` and on any date."
    cites: ["artifacts/HMS-24/run_01M7T1CAT0DDAGAWYAP7TPJ3YB/ticket.md#L25"]
  - id: REQ-7
    title: Counts line format
    type: functional
    status: known
    statement: "The counts read like `4 completed · 1 no-show · 2 cancelled · 1 upcoming`, with these fixed labels, and all four counts are always shown, including any count of 0."
    cites: ["artifacts/HMS-24/run_01M7T1CAT0DDAGAWYAP7TPJ3YB/ticket.md#L24", "runs/run_01M7T1CAT0DDAGAWYAP7TPJ3YB/ide-answers.md#a-02"]
  - id: REQ-8
    title: Last visit date
    type: functional
    status: known
    statement: "Below the counts, `Last visit:` shows the start date of the patient's most recent Completed appointment in the hospital time zone, as a date only (day, short month, year as in the appointment table, without the time)."
    cites: ["artifacts/HMS-24/run_01M7T1CAT0DDAGAWYAP7TPJ3YB/ticket.md#L26", "runs/run_01M7T1CAT0DDAGAWYAP7TPJ3YB/ide-answers.md#a-01", "context/intent-baseline.md#constraints"]
  - id: REQ-9
    title: Last visit doctor
    type: functional
    status: known
    statement: "After the date, Last visit shows the `doctor's name` of that most recent Completed appointment as plain text, not a link."
    cites: ["artifacts/HMS-24/run_01M7T1CAT0DDAGAWYAP7TPJ3YB/ticket.md#L26-L28", "runs/run_01M7T1CAT0DDAGAWYAP7TPJ3YB/ide-answers.md#a-03"]
  - id: REQ-10
    title: Last visit date link
    type: functional
    status: known
    statement: "The Last visit date `links to that appointment`'s page."
    cites: ["artifacts/HMS-24/run_01M7T1CAT0DDAGAWYAP7TPJ3YB/ticket.md#L28"]
  - id: REQ-11
    title: No completed appointment
    type: functional
    status: known
    statement: "A patient with no Completed appointment shows `Last visit: none yet`."
    cites: ["artifacts/HMS-24/run_01M7T1CAT0DDAGAWYAP7TPJ3YB/ticket.md#L29"]
  - id: REQ-12
    title: No appointments at all
    type: functional
    status: known
    statement: "A patient with no appointments at all shows `No visits yet` instead of the counts and the last visit."
    cites: ["artifacts/HMS-24/run_01M7T1CAT0DDAGAWYAP7TPJ3YB/ticket.md#L30"]
  - id: REQ-13
    title: No sensitive details in the summary
    type: constraint
    status: known
    statement: "The summary shows no `reason for visit`, no cancellation reason and no medical history details."
    cites: ["artifacts/HMS-24/run_01M7T1CAT0DDAGAWYAP7TPJ3YB/ticket.md#L31"]
  - id: REQ-14
    title: Nothing else changes
    type: constraint
    status: known
    statement: "The rest of the patient page and every other page are `unchanged`."
    cites: ["artifacts/HMS-24/run_01M7T1CAT0DDAGAWYAP7TPJ3YB/ticket.md#L32"]
unknowns: []
questions: []
answers:
  - id: A-01
    kind: question
    about: Q-01
    answer: "Date only (proposed default)"
    requirements: [REQ-8]
    mismatch:
      text: "The ticket asks for the appointment table's date format, and that format also prints the time; the user decided Last visit shows the date only."
      cites: ["artifacts/HMS-24/run_01M7T1CAT0DDAGAWYAP7TPJ3YB/ticket.md#L26"]
  - id: A-02
    kind: assumption
    about: REQ-7
    answer: "Yes, confirm"
    requirements: [REQ-7]
  - id: A-03
    kind: assumption
    about: REQ-9
    answer: "Yes, confirm"
    requirements: [REQ-9]
glossary:
  - {term: Completed, found: true}
  - {term: No-show, found: true}
  - {term: Cancelled, found: true}
  - {term: Scheduled, found: true}
  - {term: Front-desk staff, found: true}
  - {term: Medical history, found: true}
---
# Requirements review: HMS-24

## Summary
HMS-24 adds a read-only "Visit summary" line above the Appointments table on the patient page
(C1, C2). It shows counts of Completed, No-show, Cancelled and upcoming appointments, plus the date
and doctor of the patient's last Completed visit, with empty states for patients with no visits.
The main risk is completeness: the counts must cover every appointment of the patient (REQ-6), even
when the page itself loads only some of them. The table's date format also prints a time, so the user
decided Last visit shows the date only (A-01).

## Requirements
- REQ-1 Visit summary placement (known): a `Visit summary` line directly above the Appointments table.
- REQ-2 Completed count (known): shows the number of `Completed` appointments.
- REQ-3 No-show count (known): shows the number of `No-show` appointments.
- REQ-4 Cancelled count (known): shows the number of `Cancelled` appointments.
- REQ-5 Upcoming count (known): shows the number of `Scheduled appointments that start after now`.
- REQ-6 Counts cover all appointments (known): with `any doctor` and on any date.
- REQ-7 Counts line format (known): reads like `4 completed · 1 no-show · 2 cancelled · 1 upcoming`; all four counts always shown, zeros included.
- REQ-8 Last visit date (known): `Last visit:` shows the start date of the most recent Completed appointment, hospital time zone, date only.
- REQ-9 Last visit doctor (known): the `doctor's name` of that appointment, as plain text.
- REQ-10 Last visit date link (known): the date `links to that appointment`'s page.
- REQ-11 No completed appointment (known): shows `Last visit: none yet`.
- REQ-12 No appointments at all (known): shows `No visits yet` instead of the counts and last visit.
- REQ-13 No sensitive details in the summary (known): no `reason for visit`, cancellation reason or medical history details.
- REQ-14 Nothing else changes (known): the rest of the patient page and every other page are `unchanged`.

## Known
- REQ-1 to REQ-6, REQ-10 to REQ-14: stated in ticket acceptance criteria 1 to 7.
- REQ-7: example format from the ticket; the user confirmed that all four counts are always shown (A-02).
- REQ-8: ticket criterion 3, with the date-only format the user chose (A-01) and the hospital time zone `Asia/Kolkata` (C3).
- REQ-9: ticket criterion 3; the user confirmed the doctor's name is plain text (A-03).

## Assumed
None. The user confirmed every assumption.

## Unknown
None. Every question was answered.

## Open questions
None open.

Answered in the IDE:
- A-01 (Q-01, answered by eng-3fd6): Last visit format, date only or date and time? Answer: "Date only (proposed default)". Decides REQ-8. Mismatch: the ticket asks for the appointment table's format, which also prints the time. The user decided date only.
- A-02 (REQ-7, answered by eng-3fd6): are all four counts always shown, zeros included? Answer: "Yes, confirm". Decides REQ-7.
- A-03 (REQ-9, answered by eng-3fd6): is the doctor's name plain text, not a link? Answer: "Yes, confirm". Decides REQ-9.

## Out of scope
- How and where the counts are computed (API field or new endpoint versus the web app) is left to design. Note for design: the patient page's appointment list is loaded with a row limit, so counting only the loaded rows may not satisfy REQ-6.
- Showing the summary anywhere other than the patient page: the ticket limits it to the patient page, and REQ-14 keeps every other page unchanged.
- The user story's goal of spotting patients who often miss appointments is served by the No-show count. The ticket asks for no flag or threshold, so none is added.
