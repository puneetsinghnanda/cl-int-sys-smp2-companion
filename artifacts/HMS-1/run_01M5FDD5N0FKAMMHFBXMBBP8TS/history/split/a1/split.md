---
schema: stories.v1
run_id: run_01M5FDD5N0FKAMMHFBXMBBP8TS
step: split
attempt: 1
skill: story-split@1.0.1
inputs:
  - path: artifacts/HMS-1/run_01M5FDD5N0FKAMMHFBXMBBP8TS/req.md
    sha256: 1ed073519e20e57dc1df2f58ae0923916091d9ef5f8df4c7ba350901e572bc05
  - path: context/glossary.md
    sha256: 9bfd556283382d3d3835bf3b12514acc4f6941dc24f21cba0c8eaa4cd658a36c
pack_sha:
  sample-system: 415369398bdad3122ac084a00766a01a3034f900
claims:
  - id: C1
    text: "A Scheduled appointment is one that is booked and still open."
    cites: ["context/glossary.md#scheduled"]
  - id: C2
    text: "Medical history is blood group, allergies, chronic conditions, current medications and notes of one patient."
    cites: ["context/glossary.md#medical-history"]
  - id: C3
    text: "An Inactive doctor or patient is kept for history but not offered for new use."
    cites: ["context/glossary.md#inactive"]
  - id: C4
    text: "The intent leaves behaviour for Inactive patients or doctors unaddressed."
    cites: ["artifacts/HMS-1/run_01M5FDD5N0FKAMMHFBXMBBP8TS/req.md#L228"]
trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10, REQ-11, REQ-12]
known: [S-1, S-3, S-4, S-5]
assumed: [S-2]
unknown: [U-1, U-2, U-3, U-4]
unknowns:
  - id: U-1
    text: "The date-time display format is not stated (intent U-1); it affects S-1 AC3."
    question: Q-01
  - id: U-2
    text: "It is not known whether an appointment page exists to link to (intent U-2); it affects S-3."
    question: Q-02
  - id: U-3
    text: "Six terms the stories use are missing from the glossary (intent U-3 to U-8)."
    question: Q-03
  - id: U-4
    text: "The intent does not say how Inactive doctors or patients are handled; it affects S-1."
    question: Q-04
questions:
  - id: Q-01
    blocking: false
    ask: product owner
    text: "In what format should the next appointment's start date and time be shown?"
    why: "S-1 AC3 can only check the time zone until the display format is fixed."
    evidence: ["artifacts/HMS-1/run_01M5FDD5N0FKAMMHFBXMBBP8TS/req.md#L145", "artifacts/HMS-1/run_01M5FDD5N0FKAMMHFBXMBBP8TS/req.md#L59"]
    options: ["Reuse the date-time format already used elsewhere in the app", "A new format chosen by the product owner"]
    default: "Reuse the app's existing date-time format, shown in Asia/Kolkata"
  - id: Q-02
    blocking: false
    ask: tech lead
    text: "Is there an existing appointment page the column can link to?"
    why: "S-3 needs a link target; if no such page exists, the scope grows."
    evidence: ["artifacts/HMS-1/run_01M5FDD5N0FKAMMHFBXMBBP8TS/req.md#L153", "artifacts/HMS-1/run_01M5FDD5N0FKAMMHFBXMBBP8TS/req.md#L83"]
    options: ["Link to the existing appointment page", "Add an appointment page as part of this ticket", "Drop the link"]
    default: "Link to the existing appointment page; impact analysis confirms it exists"
  - id: Q-03
    blocking: false
    ask: tech lead
    text: "Should 'patient list', 'appointment page', 'Cancelled', 'Completed', 'hospital time zone' and 'front-desk staff' be added to the glossary?"
    why: "The stories use these terms from the intent, and the glossary does not define them."
    evidence: ["artifacts/HMS-1/run_01M5FDD5N0FKAMMHFBXMBBP8TS/req.md#L161"]
    options: ["Add the terms", "Leave the glossary as is"]
    default: "Add the terms using their meaning in the ticket and the baseline"
  - id: Q-04
    blocking: false
    ask: product owner
    text: "Should an upcoming Scheduled appointment with an Inactive doctor, or for an Inactive patient, still be shown in the Next appointment column?"
    why: "S-1 selects by status and start time only; the glossary says Inactive records are not offered for new use, which may or may not apply to display."
    evidence: ["artifacts/HMS-1/run_01M5FDD5N0FKAMMHFBXMBBP8TS/req.md#L53", "artifacts/HMS-1/run_01M5FDD5N0FKAMMHFBXMBBP8TS/req.md#L228", "context/glossary.md#inactive"]
    options: ["Show it, because the appointment is still Scheduled", "Hide it", "Hide Inactive patients from the list entirely"]
    default: "Show it, because REQ-2 selects by Scheduled status and start time only"
stories:
  - id: S-1
    title: See each patient's next appointment in the patient list
    trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5]
    acceptance_criteria:
      - id: AC1
        text: "Given staff open the patient list, when the list is shown, then it has a column titled \"Next appointment\"."
      - id: AC2
        text: "Given a patient with more than one Scheduled appointment starting after the current time, when the patient list is shown, then the column shows the earliest of those appointments."
      - id: AC3
        text: "Given a patient with an upcoming Scheduled appointment, when the patient list is shown, then the column shows that appointment's start date and time in the hospital time zone (Asia/Kolkata)."
      - id: AC4
        text: "Given a patient with an upcoming Scheduled appointment, when the patient list is shown, then the column shows the name of that appointment's doctor."
      - id: AC5
        text: "Given a patient with appointments in status Cancelled, Completed or No-show, when the patient list is shown, then none of those appointments is shown in the column."
  - id: S-2
    title: See "None" when a patient has nothing upcoming
    trace: [REQ-6, REQ-12]
    acceptance_criteria:
      - id: AC1
        text: "Given a patient with no upcoming Scheduled appointment, when the patient list is shown, then the column shows \"None\"."
      - id: AC2
        text: "Given a patient whose column shows \"None\", when staff look at the value, then it is plain text and not a link."
  - id: S-3
    title: Open the next appointment from the patient list
    trace: [REQ-7]
    acceptance_criteria:
      - id: AC1
        text: "Given a patient whose column shows an upcoming appointment, when staff select the value, then the page of that same appointment opens."
  - id: S-4
    title: Keep the patient list's paging, search and sort
    trace: [REQ-8, REQ-9, REQ-10]
    acceptance_criteria:
      - id: AC1
        text: "Given more than 20 patients, when staff open the patient list, then each page shows 20 patients."
      - id: AC2
        text: "Given a search that staff could run before this change, when they run it now, then the list returns the same patients as before."
      - id: AC3
        text: "Given the patient list, when it is shown, then patients appear in the same order as before this change."
  - id: S-5
    title: Keep medical history out of the patient list
    trace: [REQ-11]
    acceptance_criteria:
      - id: AC1
        text: "Given a patient with medical history recorded (blood group, allergies, chronic conditions, current medications or notes), when the patient list is shown, then none of that medical history appears in the list."
---
# Stories: HMS-1

## Stories
- **S-1 See each patient's next appointment in the patient list** (traces REQ-1, REQ-2, REQ-3, REQ-4, REQ-5)
  - AC1: Given staff open the patient list, when the list is shown, then it has a column titled "Next appointment".
  - AC2: Given a patient with more than one Scheduled appointment starting after the current time, when the list is shown, then the column shows the earliest of them.
  - AC3: Given a patient with an upcoming Scheduled appointment, when the list is shown, then the column shows its start date and time in the hospital time zone (Asia/Kolkata).
  - AC4: Given a patient with an upcoming Scheduled appointment, when the list is shown, then the column shows the name of its doctor.
  - AC5: Given a patient with Cancelled, Completed or No-show appointments, when the list is shown, then none of those appointments is shown in the column.
- **S-2 See "None" when a patient has nothing upcoming** (traces REQ-6, REQ-12)
  - AC1: Given a patient with no upcoming Scheduled appointment, when the list is shown, then the column shows "None".
  - AC2: Given a patient whose column shows "None", when staff look at the value, then it is plain text and not a link.
- **S-3 Open the next appointment from the patient list** (traces REQ-7)
  - AC1: Given a patient whose column shows an upcoming appointment, when staff select the value, then the page of that same appointment opens.
- **S-4 Keep the patient list's paging, search and sort** (traces REQ-8, REQ-9, REQ-10)
  - AC1: Given more than 20 patients, when staff open the list, then each page shows 20 patients.
  - AC2: Given a search that worked before this change, when staff run it now, then the list returns the same patients as before.
  - AC3: Given the patient list, when it is shown, then patients appear in the same order as before this change.
- **S-5 Keep medical history out of the patient list** (traces REQ-11)
  - AC1: Given a patient with medical history recorded (blood group, allergies, chronic conditions, current medications or notes), when the list is shown, then none of it appears in the list.

Coverage: every known requirement (REQ-1 to REQ-11) and the assumed REQ-12 is traced by exactly one story.

## Known
- S-1: built only from known requirements REQ-1 to REQ-5; Scheduled means booked and still open (C1).
- S-3: built only from the known requirement REQ-7. Whether the link target exists is open (U-2).
- S-4: built only from the known requirements REQ-8, REQ-9 and REQ-10.
- S-5: built only from the known requirement REQ-11; the listed fields come from the glossary definition of medical history (C2).

## Assumed
- S-2: relies on the assumed requirement REQ-12, which says "None" is plain text and not a link. Basis, from the intent: only a shown appointment has a page to link to.

## Unknown
- U-1 (non-blocking): the date-time display format is not stated (intent U-1). It affects S-1 AC3. Raises Q-01.
- U-2 (non-blocking): it is not known whether an appointment page exists to link to (intent U-2). It affects S-3. Raises Q-02.
- U-3 (non-blocking): six terms the stories use are missing from the glossary (intent U-3 to U-8). Raises Q-03.
- U-4 (non-blocking): the intent does not say how Inactive doctors or patients are handled (C4); the glossary defines Inactive as not offered for new use (C3). It affects S-1. Raises Q-04.

## Open questions
- Q-01 (non-blocking, product owner): what date-time format should the column use? Proposed default: the app's existing format, in Asia/Kolkata.
- Q-02 (non-blocking, tech lead): is there an existing appointment page to link to? Proposed default: link to it, and let impact analysis confirm it exists.
- Q-03 (non-blocking, tech lead): should the six missing terms be added to the glossary? Proposed default: add them.
- Q-04 (non-blocking, product owner): should an upcoming Scheduled appointment with an Inactive doctor, or for an Inactive patient, still be shown? Proposed default: show it, because REQ-2 selects by Scheduled status and start time only.
