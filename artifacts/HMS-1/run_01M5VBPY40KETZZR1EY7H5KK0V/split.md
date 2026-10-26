---
schema: stories.v1
run_id: run_01M5VBPY40KETZZR1EY7H5KK0V
step: split
attempt: 1
skill: story-split@1.0.1
inputs:
  - path: artifacts/HMS-1/run_01M5VBPY40KETZZR1EY7H5KK0V/req.md
    sha256: 168b14f2f9236bbbe97d4740cc4b53d5824259e81d856851c562ef78471ab680
  - path: context/glossary.md
    sha256: 9bfd556283382d3d3835bf3b12514acc4f6941dc24f21cba0c8eaa4cd658a36c
pack_sha:
  sample-system: 415369398bdad3122ac084a00766a01a3034f900
claims:
  - id: C1
    text: "A `Scheduled` appointment is booked and still open."
    cites: ["context/glossary.md#scheduled"]
  - id: C2
    text: "A `No-show` is a past appointment the patient did not attend."
    cites: ["context/glossary.md#no-show"]
  - id: C3
    text: "An Inactive doctor is kept for `history` but not offered for new use."
    cites: ["context/glossary.md#inactive"]
  - id: C4
    text: "Medical history includes `allergies` and other clinical fields of one patient."
    cites: ["context/glossary.md#medical-history"]
trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10, REQ-11, REQ-12, REQ-13]
known: [S-1, S-2, S-3, S-5]
assumed: [S-4]
unknown:
  - {id: U-1, blocking: false}
  - {id: U-2, blocking: false}
  - {id: U-3, blocking: false}
  - {id: U-4, blocking: false}
  - {id: U-5, blocking: false}
  - {id: U-6, blocking: false}
  - {id: U-7, blocking: false}
  - {id: U-8, blocking: false}
questions:
  - id: Q-01
    blocking: false
    ask: product owner
    text: "What date and time format should the Next appointment column use?"
    why: "S-1 AC3 needs an exact expected string."
    evidence: ["artifacts/HMS-1/run_01M5VBPY40KETZZR1EY7H5KK0V/req.md#L127"]
    options: ["Reuse the format already used elsewhere in the app", "Specify a new format"]
    default: "Reuse the app's existing date-time format, in Asia/Kolkata."
  - id: Q-02
    blocking: false
    ask: tech lead
    text: "Is there an existing appointment page to link to, or must one be built?"
    why: "If no page exists, S-3 grows in scope."
    evidence: ["artifacts/HMS-1/run_01M5VBPY40KETZZR1EY7H5KK0V/req.md#L92"]
    options: ["Link to the existing appointment page", "Build an appointment page in this ticket"]
    default: "Link to the existing appointment page; impact analysis confirms it."
  - id: Q-03
    blocking: false
    ask: product owner
    text: "Does PRD PAT-8 add any rule for the patient list beyond keeping medical history out of lists?"
    why: "S-5 AC4 relies on the baseline data-protection rule instead of PAT-8."
    evidence: ["artifacts/HMS-1/run_01M5VBPY40KETZZR1EY7H5KK0V/req.md#L116"]
    options: ["No, the baseline rule is enough", "Yes, share PAT-8"]
    default: "The baseline data-protection rule is enough."
  - id: Q-04
    blocking: false
    ask: product owner
    text: "If the doctor of a patient's next appointment is Inactive, should the column show the name, show it marked Inactive, or hide it?"
    why: "S-4 rests on the assumption in REQ-13 that the name is shown."
    evidence: ["artifacts/HMS-1/run_01M5VBPY40KETZZR1EY7H5KK0V/req.md#L122", "context/glossary.md#inactive"]
    options: ["Show the name", "Show the name marked Inactive", "Hide the name"]
    default: "Show the name."
  - id: Q-05
    blocking: false
    ask: tech lead
    text: "Should Cancelled, Completed, patient list and front-desk staff be added to the glossary?"
    why: "The stories use these terms with their plain meanings."
    evidence: ["artifacts/HMS-1/run_01M5VBPY40KETZZR1EY7H5KK0V/req.md#L127"]
    options: ["Add them", "Leave as plain English"]
    default: "Use the plain meanings; Cancelled and Completed are appointment statuses."
stories:
  - id: S-1
    title: See each patient's next appointment in the patient list
    trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5]
    acceptance_criteria:
      - id: AC1
        text: "Given front-desk staff open the patient list, when the list loads, then it has a column titled \"Next appointment\"."
      - id: AC2
        text: "Given a patient has several Scheduled appointments that start after now, when the patient list loads, then the column shows the earliest of them."
      - id: AC3
        text: "Given a patient has a Scheduled appointment that starts after now, when the patient list loads, then the column shows that appointment's start date and time."
      - id: AC4
        text: "Given a patient's next Scheduled appointment, when its start date and time are shown, then they are shown in the hospital time zone, Asia/Kolkata."
      - id: AC5
        text: "Given a patient's next Scheduled appointment, when the column shows it, then it also shows the name of that appointment's doctor."
  - id: S-2
    title: Show only upcoming Scheduled appointments, or "None"
    trace: [REQ-6, REQ-7, REQ-2]
    acceptance_criteria:
      - id: AC1
        text: "Given a patient's only appointments after now are Cancelled, when the patient list loads, then none of them is shown in the column."
      - id: AC2
        text: "Given a patient has a Cancelled appointment that starts before a later Scheduled one, when the patient list loads, then the column shows the Scheduled appointment."
      - id: AC3
        text: "Given a patient has Completed or No-show appointments, when the patient list loads, then none of them is shown in the column."
      - id: AC4
        text: "Given a patient has no Scheduled appointment that starts after now, when the patient list loads, then the column shows \"None\"."
  - id: S-3
    title: Open the next appointment from the patient list
    trace: [REQ-8]
    acceptance_criteria:
      - id: AC1
        text: "Given the column shows a patient's next appointment, when staff click the value, then that appointment's page opens."
  - id: S-4
    title: Keep the doctor's name when the doctor is Inactive
    trace: [REQ-13]
    acceptance_criteria:
      - id: AC1
        text: "Given a patient's next Scheduled appointment is with a doctor who is Inactive, when the patient list loads, then the column still shows that doctor's name."
  - id: S-5
    title: Keep the patient list's paging, search, sort and privacy unchanged
    trace: [REQ-9, REQ-10, REQ-11, REQ-12]
    acceptance_criteria:
      - id: AC1
        text: "Given more than 20 patients, when the patient list loads, then it shows 20 patients per page."
      - id: AC2
        text: "Given staff search the patient list, when results load, then the same patients are returned as before this change."
      - id: AC3
        text: "Given staff sort the patient list, when results load, then they are in the same order as before this change."
      - id: AC4
        text: "Given a patient has medical history recorded, when the patient list loads, then no medical history appears in the list."
---
# Stories: HMS-1

## Stories

### S-1 See each patient's next appointment in the patient list
Traces: REQ-1, REQ-2, REQ-3, REQ-4, REQ-5.
- AC1: Given front-desk staff open the patient list, when the list loads, then it has a column titled "Next appointment".
- AC2: Given a patient has several Scheduled appointments that start after now, when the patient list loads, then the column shows the earliest of them.
- AC3: Given a patient has a Scheduled appointment that starts after now, when the patient list loads, then the column shows that appointment's start date and time.
- AC4: Given a patient's next Scheduled appointment, when its start date and time are shown, then they are shown in the hospital time zone, Asia/Kolkata.
- AC5: Given a patient's next Scheduled appointment, when the column shows it, then it also shows the name of that appointment's doctor.

### S-2 Show only upcoming Scheduled appointments, or "None"
Traces: REQ-6, REQ-7, REQ-2.
- AC1: Given a patient's only appointments after now are Cancelled, when the patient list loads, then none of them is shown in the column.
- AC2: Given a patient has a Cancelled appointment that starts before a later Scheduled one, when the patient list loads, then the column shows the Scheduled appointment.
- AC3: Given a patient has Completed or No-show appointments, when the patient list loads, then none of them is shown in the column.
- AC4: Given a patient has no Scheduled appointment that starts after now, when the patient list loads, then the column shows "None".

### S-3 Open the next appointment from the patient list
Traces: REQ-8.
- AC1: Given the column shows a patient's next appointment, when staff click the value, then that appointment's page opens.

### S-4 Keep the doctor's name when the doctor is Inactive
Traces: REQ-13 (assumed).
- AC1: Given a patient's next Scheduled appointment is with a doctor who is Inactive, when the patient list loads, then the column still shows that doctor's name.

### S-5 Keep the patient list's paging, search, sort and privacy unchanged
Traces: REQ-9, REQ-10, REQ-11, REQ-12.
- AC1: Given more than 20 patients, when the patient list loads, then it shows 20 patients per page.
- AC2: Given staff search the patient list, when results load, then the same patients are returned as before this change.
- AC3: Given staff sort the patient list, when results load, then they are in the same order as before this change.
- AC4: Given a patient has medical history recorded, when the patient list loads, then no medical history appears in the list.

## Known
- S-1: built from REQ-1 to REQ-5, all known from ticket AC1 and AC2.
- S-2: built from REQ-6, REQ-7 and REQ-2, known from ticket AC2, AC3 and AC4.
- S-3: built from REQ-8, known from ticket AC5.
- S-5: built from REQ-9 to REQ-12, known from ticket AC6 and AC7 and the baseline data-protection rule.

## Assumed
- S-4: relies on REQ-13, which assumes an Inactive doctor's name is still shown. Basis: the glossary keeps Inactive records for history, and doctors with appointments are never deleted.

## Unknown
- U-1 (non-blocking): the date-time display format for S-1 AC3 is not given (Q-01).
- U-2 (non-blocking): it is not confirmed that an appointment page exists for S-3 (Q-02).
- U-3 (non-blocking): PRD PAT-8 is not among the inputs, so S-5 AC4 relies on the baseline rule (Q-03).
- U-4 (non-blocking): how an Inactive doctor's name is shown in S-4 is not settled (Q-04).
- U-5 (non-blocking): Cancelled, used in S-2, is not in the glossary (Q-05).
- U-6 (non-blocking): Completed, used in S-2, is not in the glossary (Q-05).
- U-7 (non-blocking): patient list, used in every story, is not in the glossary (Q-05).
- U-8 (non-blocking): front-desk staff, used in S-1, is not in the glossary (Q-05).

## Open questions
- Q-01 (non-blocking, product owner): which date-time format should the column use? Default: the app's existing format, in Asia/Kolkata.
- Q-02 (non-blocking, tech lead): does an appointment page exist to link to? Default: link to the existing page.
- Q-03 (non-blocking, product owner): does PAT-8 add any rule for the patient list? Default: the baseline rule is enough.
- Q-04 (non-blocking, product owner): show, mark or hide an Inactive doctor's name? Default: show the name.
- Q-05 (non-blocking, tech lead): add the missing terms to the glossary? Default: use their plain meanings.
