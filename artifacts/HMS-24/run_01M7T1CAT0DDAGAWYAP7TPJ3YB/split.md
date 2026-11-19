---
schema: stories.v1
run_id: run_01M7T1CAT0DDAGAWYAP7TPJ3YB
step: split
attempt: 1
skill: story-split@1.4.0
inputs:
  - path: artifacts/HMS-24/run_01M7T1CAT0DDAGAWYAP7TPJ3YB/req.md
    sha256: 13b81d643b95fe75f614a4a6b02720f9fa3b75987ebe1a8b922ae11f0929beec
  - path: context/glossary.md
    sha256: 430a90e14c50f969f7f757089c61cf254eb21596ed6e60abb8bf59286822b9ff
pack_sha:
  sample-system: 7681f87749883d71abb4011d118366ca1242e1cc
claims:
  - id: C1
    text: "The hospital time zone is `Asia/Kolkata`."
    cites: ["artifacts/HMS-24/run_01M7T1CAT0DDAGAWYAP7TPJ3YB/req.md#claim:C3"]
  - id: C2
    text: "The patient page `lists the patient's appointments`, upcoming first, then past."
    cites: ["artifacts/HMS-24/run_01M7T1CAT0DDAGAWYAP7TPJ3YB/req.md#claim:C1"]
  - id: C3
    text: "A No-show is a past appointment the patient `did not attend`."
    cites: ["context/glossary.md#no-show"]
  - id: C4
    text: "A Cancelled appointment is called off `with a reason`."
    cites: ["context/glossary.md#cancelled"]
  - id: C5
    text: "Completed and No-show are allowed only `after the start time`, so a Scheduled appointment whose start has passed can still exist until staff close it."
    cites: ["artifacts/HMS-24/run_01M7T1CAT0DDAGAWYAP7TPJ3YB/req.md#claim:C8"]
  - id: C6
    text: "`Medical history` appears only on the patient's own page."
    cites: ["artifacts/HMS-24/run_01M7T1CAT0DDAGAWYAP7TPJ3YB/req.md#claim:C4"]
trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10, REQ-11, REQ-12, REQ-13, REQ-14]
known: [S-1, S-2, S-3, S-4, S-5]
assumed: []
unknown: []
questions: []
stories:
  - id: S-1
    title: See visit counts on the patient page
    trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-7]
    acceptance_criteria:
      - id: AC1
        text: "Given a patient with 4 Completed, 1 No-show, 2 Cancelled and 1 Scheduled appointment that starts after now, when staff open the patient page, then a Visit summary line directly above the Appointments table reads \"4 completed · 1 no-show · 2 cancelled · 1 upcoming\"."
      - id: AC2
        text: "Given a patient with 3 Completed appointments, 1 Scheduled appointment that starts after now and no No-show or Cancelled appointments, when staff open the patient page, then the Visit summary reads \"3 completed · 0 no-show · 0 cancelled · 1 upcoming\", with every zero count shown."
      - id: AC3
        text: "Given a patient with 1 Completed appointment and 1 Scheduled appointment whose start time has already passed and that staff have not closed yet, when staff open the patient page, then the Visit summary reads \"1 completed · 0 no-show · 0 cancelled · 0 upcoming\" (the past Scheduled appointment is not upcoming and is not counted as completed, no-show or cancelled)."
      - id: AC4
        text: "Given a patient with 1 Completed appointment and 1 Scheduled appointment that starts one minute after now, when staff open the patient page, then the Visit summary shows \"1 upcoming\"."
  - id: S-2
    title: Counts cover every appointment of the patient
    trace: [REQ-6]
    acceptance_criteria:
      - id: AC1
        text: "Given a patient whose Completed appointments are with two different doctors, one on a date more than a year before today and one last week, when staff open the patient page, then the completed count includes both appointments."
      - id: AC2
        text: "Given a patient with more appointments than the patient page's Appointments table loads, when staff open the patient page, then the four counts equal the totals over all of the patient's appointments, not only the rows the table shows."
  - id: S-3
    title: See the patient's last visit
    trace: [REQ-8, REQ-9, REQ-10, REQ-11]
    acceptance_criteria:
      - id: AC1
        text: "Given a patient whose most recent Completed appointment is with Dr A and started on 05 Nov 2026 at 10:30 hospital time, when staff open the patient page, then below the counts the line reads \"Last visit: 05 Nov 2026\" followed by Dr A's name as plain text, with no time shown and no link on the doctor's name."
      - id: AC2
        text: "Given a patient whose only Completed appointment starts at 19:30 UTC on 9 March 2026 (Asia/Kolkata = UTC+05:30, so 01:00 local on 10 March 2026, after local midnight), when staff open the patient page, then Last visit shows 10 Mar 2026, not 9 Mar 2026."
      - id: AC3
        text: "Given a patient whose last Completed appointment is on 01 Nov 2026 and who has a later No-show on 10 Nov 2026, a later Cancelled appointment on 12 Nov 2026 and a Scheduled appointment that starts after now, when staff open the patient page, then Last visit shows 01 Nov 2026 and that appointment's doctor."
      - id: AC4
        text: "Given a patient with a Completed appointment, when staff select the Last visit date, then the page of that appointment opens."
      - id: AC5
        text: "Given a patient with 1 No-show and 1 Scheduled appointment that starts after now and no Completed appointment, when staff open the patient page, then the counts are shown and the line reads \"Last visit: none yet\"."
  - id: S-4
    title: Empty state for a patient with no appointments
    trace: [REQ-12]
    acceptance_criteria:
      - id: AC1
        text: "Given a patient with no appointments at all, when staff open the patient page, then the Visit summary shows \"No visits yet\" and shows no counts and no Last visit line."
  - id: S-5
    title: Summary stays free of sensitive details and nothing else changes
    trace: [REQ-13, REQ-14]
    acceptance_criteria:
      - id: AC1
        text: "Given a patient whose most recent appointment is Cancelled with a cancellation reason and whose most recent Completed appointment has a reason for visit, when staff open the patient page, then neither the cancellation reason text nor the reason for visit text appears in the Visit summary."
      - id: AC2
        text: "Given a patient with allergies and current medications recorded, when staff open the patient page, then no medical history value appears in the Visit summary, and the Medical history section still shows them as before."
      - id: AC3
        text: "Given a patient with both upcoming and past appointments, when staff open the patient page, then the Appointments table lists the same rows in the same order as before (upcoming first, then past) and the patient details and Medical history sections are as before."
      - id: AC4
        text: "Given a patient with no appointments, when staff open the patient page, then apart from the \"No visits yet\" line the page is the same as before, including the Appointments table's empty state."
      - id: AC5
        text: "Given patients with and without appointments, when staff open the patient list, the appointment list, an appointment's page, a doctor's page and the dashboard, then each looks and behaves as before and shows no Visit summary."
---
# Stories: HMS-24

## Stories

### S-1 See visit counts on the patient page
Traces: REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-7.
- AC1: Given a patient with 4 Completed, 1 No-show, 2 Cancelled and 1 Scheduled appointment that starts after now, when staff open the patient page, then a Visit summary line directly above the Appointments table reads "4 completed · 1 no-show · 2 cancelled · 1 upcoming".
- AC2: Given a patient with 3 Completed appointments, 1 Scheduled appointment that starts after now and no No-show or Cancelled appointments, when staff open the patient page, then the Visit summary reads "3 completed · 0 no-show · 0 cancelled · 1 upcoming", with every zero count shown.
- AC3: Given a patient with 1 Completed appointment and 1 Scheduled appointment whose start time has already passed and that staff have not closed yet, when staff open the patient page, then the Visit summary reads "1 completed · 0 no-show · 0 cancelled · 0 upcoming". The past Scheduled appointment is not upcoming (C5) and is not counted as completed, no-show or cancelled.
- AC4: Given a patient with 1 Completed appointment and 1 Scheduled appointment that starts one minute after now, when staff open the patient page, then the Visit summary shows "1 upcoming".

### S-2 Counts cover every appointment of the patient
Traces: REQ-6.
- AC1: Given a patient whose Completed appointments are with two different doctors, one on a date more than a year before today and one last week, when staff open the patient page, then the completed count includes both appointments.
- AC2: Given a patient with more appointments than the patient page's Appointments table loads, when staff open the patient page, then the four counts equal the totals over all of the patient's appointments, not only the rows the table shows.

### S-3 See the patient's last visit
Traces: REQ-8, REQ-9, REQ-10, REQ-11.
- AC1: Given a patient whose most recent Completed appointment is with Dr A and started on 05 Nov 2026 at 10:30 hospital time, when staff open the patient page, then below the counts the line reads "Last visit: 05 Nov 2026" followed by Dr A's name as plain text. No time is shown and the doctor's name has no link.
- AC2: Given a patient whose only Completed appointment starts at 19:30 UTC on 9 March 2026 (Asia/Kolkata = UTC+05:30, so 01:00 local on 10 March 2026, after local midnight), when staff open the patient page, then Last visit shows 10 Mar 2026, not 9 Mar 2026 (C1).
- AC3: Given a patient whose last Completed appointment is on 01 Nov 2026 and who has a later No-show on 10 Nov 2026, a later Cancelled appointment on 12 Nov 2026 and a Scheduled appointment that starts after now, when staff open the patient page, then Last visit shows 01 Nov 2026 and that appointment's doctor (C3, C4).
- AC4: Given a patient with a Completed appointment, when staff select the Last visit date, then the page of that appointment opens.
- AC5: Given a patient with 1 No-show and 1 Scheduled appointment that starts after now and no Completed appointment, when staff open the patient page, then the counts are shown and the line reads "Last visit: none yet".

### S-4 Empty state for a patient with no appointments
Traces: REQ-12.
- AC1: Given a patient with no appointments at all, when staff open the patient page, then the Visit summary shows "No visits yet" and shows no counts and no Last visit line.

### S-5 Summary stays free of sensitive details and nothing else changes
Traces: REQ-13, REQ-14.
- AC1: Given a patient whose most recent appointment is Cancelled with a cancellation reason and whose most recent Completed appointment has a reason for visit, when staff open the patient page, then neither the cancellation reason text nor the reason for visit text appears in the Visit summary (C4).
- AC2: Given a patient with allergies and current medications recorded, when staff open the patient page, then no medical history value appears in the Visit summary, and the Medical history section still shows them as before (C6).
- AC3: Given a patient with both upcoming and past appointments, when staff open the patient page, then the Appointments table lists the same rows in the same order as before (upcoming first, then past; C2) and the patient details and Medical history sections are as before.
- AC4: Given a patient with no appointments, when staff open the patient page, then apart from the "No visits yet" line the page is the same as before, including the Appointments table's empty state.
- AC5: Given patients with and without appointments, when staff open the patient list, the appointment list, an appointment's page, a doctor's page and the dashboard, then each looks and behaves as before and shows no Visit summary.

## Known
- S-1: built from REQ-1 to REQ-5 and REQ-7, all known in the intent; the zero-count rule was confirmed in the req step (runs/run_01M7T1CAT0DDAGAWYAP7TPJ3YB/ide-answers.md#a-02).
- S-2: built from REQ-6, known.
- S-3: built from REQ-8 to REQ-11, all known. The date-only format and the plain-text doctor name were decided in the req step (runs/run_01M7T1CAT0DDAGAWYAP7TPJ3YB/ide-answers.md#a-01, runs/run_01M7T1CAT0DDAGAWYAP7TPJ3YB/ide-answers.md#a-03).
- S-4: built from REQ-12, known.
- S-5: built from REQ-13 and REQ-14, known constraints.

## Assumed
None.

## Unknown
None. Every requirement in the intent is known and covered.

## Open questions
None.
