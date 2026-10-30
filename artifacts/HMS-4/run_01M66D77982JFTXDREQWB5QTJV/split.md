---
schema: stories.v1
run_id: run_01M66D77982JFTXDREQWB5QTJV
step: split
attempt: 2
skill: story-split@1.0.1
inputs:
  - path: artifacts/HMS-4/run_01M66D77982JFTXDREQWB5QTJV/req.md
    sha256: a33bb8ac0e7a965160b9d6ba639feb8f4086518ad90e0374e053ffbfacdfaf35
  - path: context/glossary.md
    sha256: 7623345ad8bad33b8a14a713b452edb2a36834e8b1c2228b76f3aa154034f16e
pack_sha:
  sample-system: bd98ff08b83f8762c5e5c073ba5fadef88169b33
claims:
  - id: C1
    text: "A Completed appointment is closed by staff once its `start time` has passed."
    cites: ["context/glossary.md#completed"]
  - id: C2
    text: "A Scheduled appointment is `booked and still open`."
    cites: ["context/glossary.md#scheduled"]
  - id: C3
    text: "In the MVP every user acts as `staff`."
    cites: ["context/glossary.md#front-desk-staff"]
  - id: C4
    text: "Medical history covers blood group, `allergies`, chronic conditions, current medications and notes."
    cites: ["context/glossary.md#medical-history"]
trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10, REQ-11, REQ-12, REQ-13, REQ-14, REQ-15]
known: [S-2, S-3, S-4]
assumed: [S-1]
unknown:
  - {id: U-1, blocking: false}
  - {id: U-2, blocking: false}
  - {id: U-3, blocking: false}
  - {id: U-4, blocking: false}
questions:
  - id: Q-01
    blocking: false
    ask: product owner
    text: Which timestamp defines the Last visit date, the appointment start time or the time staff marked it Completed?
    why: Staff may close an appointment after it took place, which can change the date shown; S-1 assumes start time (REQ-13, REQ-15).
    evidence: ["artifacts/HMS-4/run_01M66D77982JFTXDREQWB5QTJV/req.md#L115", "context/glossary.md#completed"]
    options: [appointment start time, time marked Completed]
    default: appointment start time
  - id: Q-02
    blocking: false
    ask: product owner
    text: What date format should the Last visit column use?
    why: S-1 AC4 assumes the Next appointment column's format (REQ-14).
    evidence: ["artifacts/HMS-4/run_01M66D77982JFTXDREQWB5QTJV/req.md#L122"]
    options: [same format as the Next appointment column, a new format]
    default: same format as the Next appointment column
  - id: Q-03
    blocking: false
    ask: product owner
    text: Should the Last visit column be sortable or searchable?
    why: No story covers sorting or searching by Last visit; S-3 keeps the current search and sort order.
    evidence: ["artifacts/HMS-4/run_01M66D77982JFTXDREQWB5QTJV/req.md#L97"]
    options: [display only, sortable, sortable and filterable]
    default: display only
  - id: Q-04
    blocking: false
    ask: tech lead
    text: Should Last visit, patient list and next appointment be added to the glossary?
    why: Agreed definitions keep tests and later steps consistent.
    evidence: ["context/glossary.md#glossary"]
    options: [add all three, leave as is]
    default: "add all three; Last visit as defined in REQ-2"
stories:
  - id: S-1
    title: See each patient's last visit date on the patient list
    trace: [REQ-1, REQ-2, REQ-3, REQ-13, REQ-14, REQ-15]
    acceptance_criteria:
      - id: AC1
        text: "Given front-desk staff open the patient list, when the list loads, then it has a column `Last visit`."
      - id: AC2
        text: "Given a patient with two Completed appointments, when staff view the patient list, then the Last visit column shows the date of the Completed appointment with the later start time."
      - id: AC3
        text: "Given a patient whose most recent Completed appointment starts at a time whose calendar date differs between UTC and Asia/Kolkata, when staff view the patient list, then the Last visit column shows the start date in `Asia/Kolkata`."
      - id: AC4
        text: "Given a patient with a Completed appointment, when staff view the patient list, then the Last visit date uses the same date format as the Next appointment column."
  - id: S-2
    title: Only Completed appointments count as a visit
    trace: [REQ-4, REQ-5, REQ-6, REQ-7]
    acceptance_criteria:
      - id: AC1
        text: "Given a patient whose most recent appointment is `Cancelled` and who has an earlier Completed appointment, when staff view the patient list, then the Last visit column shows the earlier Completed appointment's date."
      - id: AC2
        text: "Given a patient whose most recent past appointment is `No-show` and who has an earlier Completed appointment, when staff view the patient list, then the Last visit column shows the earlier Completed appointment's date."
      - id: AC3
        text: "Given a patient with a `Scheduled` appointment and an earlier Completed appointment, when staff view the patient list, then the Last visit column shows the Completed appointment's date, not the Scheduled one."
      - id: AC4
        text: "Given a patient with no Completed appointment, when staff view the patient list, then the Last visit column shows `-`."
  - id: S-3
    title: Existing patient list behaviour stays the same
    trace: [REQ-8, REQ-9, REQ-10, REQ-11]
    acceptance_criteria:
      - id: AC1
        text: "Given more patients than fit on one page, when staff open the patient list, then it shows `20 per page`."
      - id: AC2
        text: "Given staff search the patient list, when results load, then the same patients are returned as before the change."
      - id: AC3
        text: "Given staff open the patient list, when it loads, then patients appear in the same sort order as before the change."
      - id: AC4
        text: "Given a patient with a next appointment, when staff view the patient list, then the Next appointment column shows the same value as before the change."
  - id: S-4
    title: No medical history on the patient list
    trace: [REQ-12]
    acceptance_criteria:
      - id: AC1
        text: "Given a patient with recorded medical history, when staff view the patient list, then no `medical history` field appears in the list or its response."
---
# Stories: HMS-4

## Stories

### S-1 See each patient's last visit date on the patient list
Traces: REQ-1, REQ-2, REQ-3, REQ-13, REQ-14, REQ-15
- AC1: Given front-desk staff open the patient list, when the list loads, then it has a column "Last visit".
- AC2: Given a patient with two Completed appointments, when staff view the list, then the column shows the date of the one with the later start time.
- AC3: Given a patient whose most recent Completed appointment falls on different calendar dates in UTC and Asia/Kolkata, when staff view the list, then the column shows the Asia/Kolkata start date.
- AC4: Given a patient with a Completed appointment, when staff view the list, then the date uses the same format as the Next appointment column.

### S-2 Only Completed appointments count as a visit
Traces: REQ-4, REQ-5, REQ-6, REQ-7
- AC1: A later Cancelled appointment never replaces an earlier Completed one.
- AC2: A later No-show appointment never replaces an earlier Completed one.
- AC3: A Scheduled appointment is never shown; the Completed one is shown instead.
- AC4: A patient with no Completed appointment shows "-".

### S-3 Existing patient list behaviour stays the same
Traces: REQ-8, REQ-9, REQ-10, REQ-11
- AC1: The list shows 20 per page.
- AC2: Search returns the same patients as before the change.
- AC3: The sort order is the same as before the change.
- AC4: The Next appointment column shows the same value as before the change.

### S-4 No medical history on the patient list
Traces: REQ-12
- AC1: No medical history field appears in the list or its response.

## Known
- S-2: built only from known REQ-4 to REQ-7 (ticket acceptance criteria 3 and 4).
- S-3: built only from known REQ-8 to REQ-11 (ticket acceptance criterion 5).
- S-4: built only from known REQ-12 (ticket acceptance criterion 6 and the baseline data-protection rule).

## Assumed
- S-1: relies on assumed REQ-13 and REQ-15 (the appointment start time decides both the ranking and the date shown, based on the glossary entry for Completed) and REQ-14 (the date format matches the Next appointment column). Q-01 and Q-02 can overturn these.

## Unknown
- U-1 (non-blocking): "Last visit" is not in the glossary (Q-04).
- U-2 (non-blocking): "Patient list" is not in the glossary (Q-04).
- U-3 (non-blocking): "Next appointment" is not in the glossary (Q-04).
- U-4 (non-blocking): Whether the Last visit column can be sorted or searched; no story covers it (Q-03).

## Open questions
- Q-01 (non-blocking, product owner): Should the date come from the start time or the completion time? Default: start time.
- Q-02 (non-blocking, product owner): Which date format? Default: same as the Next appointment column.
- Q-03 (non-blocking, product owner): Should the column be sortable or searchable? Default: display only.
- Q-04 (non-blocking, tech lead): Add the three missing terms to the glossary? Default: add all three.
