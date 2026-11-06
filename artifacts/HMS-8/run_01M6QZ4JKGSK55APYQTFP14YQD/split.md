---
schema: stories.v1
run_id: run_01M6QZ4JKGSK55APYQTFP14YQD
step: split
attempt: 1
skill: story-split@1.2.0
inputs:
  - path: artifacts/HMS-8/run_01M6QZ4JKGSK55APYQTFP14YQD/req.md
    sha256: 21ea23abcc5278cf34af72ae9fd62e5fb3032e9b40fc280a88a84a26953f2670
  - path: context/glossary.md
    sha256: 430a90e14c50f969f7f757089c61cf254eb21596ed6e60abb8bf59286822b9ff
pack_sha:
  sample-system: c2709e164850ab7e177e4bccfc42a5dd9b961fdc
claims:
  - id: C1
    text: "The hospital time zone is `Asia/Kolkata` (UTC+05:30), so 'today' on the doctors list is a hospital-local day."
    cites: ["artifacts/HMS-8/run_01M6QZ4JKGSK55APYQTFP14YQD/req.md#claim:C2"]
  - id: C2
    text: "The doctors list's Today column counts any status `except Cancelled`."
    cites: ["artifacts/HMS-8/run_01M6QZ4JKGSK55APYQTFP14YQD/req.md#claim:C4"]
  - id: C3
    text: "A `Cancelled` appointment is one called off before it took place, and its slot becomes free again."
    cites: ["context/glossary.md#cancelled"]
  - id: C4
    text: "A `No-show` is a past appointment the patient did not attend."
    cites: ["context/glossary.md#no-show"]
  - id: C5
    text: "An `Inactive` doctor is kept for history but not offered for new use."
    cites: ["context/glossary.md#inactive"]
trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10, REQ-11, REQ-12, REQ-13, REQ-14]
known: [S-1, S-2, S-3, S-4, S-5, S-6]
assumed: []
unknown:
  - {id: U-1, blocking: false}
questions:
  - id: Q-01
    blocking: false
    ask: product owner
    text: "When is the Clear All button shown: always, or only while at least one filter is set?"
    why: "A-03 makes the button Clear All but does not say when it shows. S-3 can test clearing either way, but not the button's visibility."
    evidence: ["runs/run_01M6QZ4JKGSK55APYQTFP14YQD/ide-answers.md#a-03", "artifacts/HMS-8/run_01M6QZ4JKGSK55APYQTFP14YQD/req.md#item:REQ-6"]
    options: ["Only while at least one filter is set", "Always shown"]
    default: "Only while at least one filter is set"
stories:
  - id: S-1
    title: See and pick the doctor the appointment list is filtered to
    trace: [REQ-1, REQ-2, REQ-3]
    acceptance_criteria:
      - id: AC1
        text: "Given the appointment list is opened from a link that filters it to a doctor, when the list loads, then the doctor chooser shows that doctor's name and the doctor's id is not shown."
      - id: AC2
        text: "Given the appointment list is opened with no doctor filter, when the list loads, then the doctor chooser is shown with no doctor chosen."
      - id: AC3
        text: "Given one Active and one Inactive doctor exist, when staff open the doctor chooser, then both doctors are offered."
      - id: AC4
        text: "Given the appointment list is opened from a link that filters it to an Inactive doctor, when the list loads, then the chooser shows that Inactive doctor's name."
  - id: S-2
    title: Change or clear the doctor without losing the other filters
    trace: [REQ-4, REQ-5, REQ-14]
    acceptance_criteria:
      - id: AC1
        text: "Given the list is filtered to doctor A, a patient, status Scheduled and a from and to date, when staff choose doctor B in the chooser, then the list is filtered to doctor B and the patient, status Scheduled and both dates stay as they were."
      - id: AC2
        text: "Given the list is filtered to doctor A, a patient, status Scheduled and a from and to date, when staff clear the doctor filter from the chooser, then the list is no longer filtered to a doctor and the patient, status Scheduled and both dates stay as they were."
      - id: AC3
        text: "Given the list is filtered to doctor A only, when staff clear the doctor filter from the chooser, then the list shows appointments of every doctor."
  - id: S-3
    title: Clear All filters
    trace: [REQ-6]
    acceptance_criteria:
      - id: AC1
        text: "Given the list is filtered by doctor, patient, status and a from and to date, when staff select Clear All, then no doctor, patient, status or date filter is left set."
      - id: AC2
        text: "Given the list is filtered by status and dates only, with no doctor or patient filter, when staff select Clear All, then the status and date filters are cleared too."
  - id: S-4
    title: List every status except Cancelled
    trace: [REQ-7, REQ-8]
    acceptance_criteria:
      - id: AC1
        text: "Given a doctor's day has one Scheduled, one Completed, one No-show and one Cancelled appointment, when staff choose Any except Cancelled, then the Scheduled, Completed and No-show appointments are listed and the Cancelled one is not."
      - id: AC2
        text: "Given the Cancelled appointment is the latest-starting appointment of that day, when staff choose Any except Cancelled, then it is still not listed."
      - id: AC3
        text: "Given staff choose Any except Cancelled, when the list reloads, then the list URL carries status=NOT_CANCELLED and the request to GET /api/appointments sends status=NOT_CANCELLED."
      - id: AC4
        text: "Given the list URL carries status=NOT_CANCELLED, when the page is opened from that URL, then the status filter shows Any except Cancelled."
  - id: S-5
    title: The doctors list's Today link lists what it counts
    trace: [REQ-9, REQ-10]
    acceptance_criteria:
      - id: AC1
        text: "Given a doctor has two Scheduled and one Cancelled appointment today, when staff select that doctor's Today link, then the list opens with status Any except Cancelled and lists two appointments, matching the Today count."
      - id: AC2
        text: "Given a doctor has an appointment at 00:30 hospital-local time today (Asia/Kolkata = UTC+05:30, so 19:00 UTC on the previous UTC date, just after local midnight), when staff select that doctor's Today link, then that appointment is listed and the listed number still equals the Today count."
      - id: AC3
        text: "Given a doctor's only appointment today is Cancelled, when staff select that doctor's Today link, then the list shows no appointments, matching a Today count of zero."
  - id: S-6
    title: The rest of the appointment list is unchanged
    trace: [REQ-11, REQ-12, REQ-13]
    acceptance_criteria:
      - id: AC1
        text: "Given a doctor page with a No-shows count, when staff select the No-shows link, then the list opens filtered to that doctor, status No-show and the same dates as before this change."
      - id: AC2
        text: "Given the dashboard's Appointments today tile, when staff select it, then the list opens with the same filters as before this change."
      - id: AC3
        text: "Given 21 appointments match the filters, when the list opens, then page 1 shows the first 20 per page and the 21st appointment is on page 2."
      - id: AC4
        text: "Given appointment X was booked first but starts later than appointment Y, when the list opens, then Y is listed before X because the list is ordered by start time."
---
# Stories: HMS-8

## Stories

### S-1 See and pick the doctor the appointment list is filtered to
Traces REQ-1, REQ-2, REQ-3.
- AC1: Given the appointment list is opened from a link that filters it to a doctor, when the list loads, then the doctor chooser shows that doctor's name and the doctor's id is not shown.
- AC2: Given the appointment list is opened with no doctor filter, when the list loads, then the doctor chooser is shown with no doctor chosen.
- AC3: Given one Active and one Inactive doctor exist, when staff open the doctor chooser, then both doctors are offered.
- AC4: Given the appointment list is opened from a link that filters it to an Inactive doctor, when the list loads, then the chooser shows that Inactive doctor's name.

### S-2 Change or clear the doctor without losing the other filters
Traces REQ-4, REQ-5, REQ-14.
- AC1: Given the list is filtered to doctor A, a patient, status Scheduled and a from and to date, when staff choose doctor B in the chooser, then the list is filtered to doctor B and the patient, status Scheduled and both dates stay as they were.
- AC2: Given the list is filtered to doctor A, a patient, status Scheduled and a from and to date, when staff clear the doctor filter from the chooser, then the list is no longer filtered to a doctor and the patient, status Scheduled and both dates stay as they were.
- AC3: Given the list is filtered to doctor A only, when staff clear the doctor filter from the chooser, then the list shows appointments of every doctor.

### S-3 Clear All filters
Traces REQ-6.
- AC1: Given the list is filtered by doctor, patient, status and a from and to date, when staff select Clear All, then no doctor, patient, status or date filter is left set.
- AC2: Given the list is filtered by status and dates only, with no doctor or patient filter, when staff select Clear All, then the status and date filters are cleared too.

### S-4 List every status except Cancelled
Traces REQ-7, REQ-8.
- AC1: Given a doctor's day has one Scheduled, one Completed, one No-show and one Cancelled appointment, when staff choose Any except Cancelled, then the Scheduled, Completed and No-show appointments are listed and the Cancelled one is not.
- AC2: Given the Cancelled appointment is the latest-starting appointment of that day, when staff choose Any except Cancelled, then it is still not listed.
- AC3: Given staff choose Any except Cancelled, when the list reloads, then the list URL carries status=NOT_CANCELLED and the request to GET /api/appointments sends status=NOT_CANCELLED.
- AC4: Given the list URL carries status=NOT_CANCELLED, when the page is opened from that URL, then the status filter shows Any except Cancelled.

### S-5 The doctors list's Today link lists what it counts
Traces REQ-9, REQ-10.
- AC1: Given a doctor has two Scheduled and one Cancelled appointment today, when staff select that doctor's Today link, then the list opens with status Any except Cancelled and lists two appointments, matching the Today count.
- AC2: Given a doctor has an appointment at 00:30 hospital-local time today (Asia/Kolkata = UTC+05:30, so 19:00 UTC on the previous UTC date, just after local midnight), when staff select that doctor's Today link, then that appointment is listed and the listed number still equals the Today count.
- AC3: Given a doctor's only appointment today is Cancelled, when staff select that doctor's Today link, then the list shows no appointments, matching a Today count of zero.

### S-6 The rest of the appointment list is unchanged
Traces REQ-11, REQ-12, REQ-13.
- AC1: Given a doctor page with a No-shows count, when staff select the No-shows link, then the list opens filtered to that doctor, status No-show and the same dates as before this change.
- AC2: Given the dashboard's Appointments today tile, when staff select it, then the list opens with the same filters as before this change.
- AC3: Given 21 appointments match the filters, when the list opens, then page 1 shows the first 20 per page and the 21st appointment is on page 2.
- AC4: Given appointment X was booked first but starts later than appointment Y, when the list opens, then Y is listed before X because the list is ordered by start time.

## Known
- S-1: built from known REQ-1, REQ-2 (A-02) and REQ-3 (A-01).
- S-2: built from known REQ-4, REQ-5 and REQ-14 (ticket acceptance criterion 2).
- S-3: built from known REQ-6 (A-03).
- S-4: built from known REQ-7 and REQ-8 (A-04).
- S-5: built from known REQ-9 and REQ-10. The time zone comes from the intent's claim C2 (`Asia/Kolkata`).
- S-6: built from known REQ-11, REQ-12 and REQ-13.

## Assumed
None.

## Unknown
- U-1 (non-blocking): A-03 does not say when the Clear All button is shown. S-3 tests what Clear All does, but not when it is visible. See Q-01.

## Open questions
- Q-01 (non-blocking, product owner): When is the Clear All button shown: always, or only while at least one filter is set? Proposed default: only while at least one filter is set.
