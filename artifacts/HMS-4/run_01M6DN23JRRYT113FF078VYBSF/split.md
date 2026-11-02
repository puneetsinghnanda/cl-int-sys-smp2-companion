---
schema: stories.v1
run_id: run_01M6DN23JRRYT113FF078VYBSF
step: split
attempt: 1
skill: story-split@1.0.1
inputs:
  - path: artifacts/HMS-4/run_01M6DN23JRRYT113FF078VYBSF/req.md
    sha256: df068dbe9b4a2785e85a805729d855f730bfda32f9cae55f731603a04e2a8bca
  - path: context/glossary.md
    sha256: 7623345ad8bad33b8a14a713b452edb2a36834e8b1c2228b76f3aa154034f16e
pack_sha:
  sample-system: bd98ff08b83f8762c5e5c073ba5fadef88169b33
claims:
  - id: C1
    text: "A `Completed` appointment is a past appointment the patient attended, closed by staff once its start time has passed."
    cites: ["context/glossary.md#completed"]
  - id: C2
    text: "A `Scheduled` appointment is booked and still open."
    cites: ["context/glossary.md#scheduled"]
  - id: C3
    text: "`Medical history` is blood group, allergies, chronic conditions, current medications and notes of one patient."
    cites: ["context/glossary.md#medical-history"]
  - id: C4
    text: "The Last visit date is the calendar date in the hospital time zone `Asia/Kolkata`."
    cites: ["artifacts/HMS-4/run_01M6DN23JRRYT113FF078VYBSF/req.md#requirements"]
  - id: C5
    text: "The patient list stays at `20 per page`."
    cites: ["artifacts/HMS-4/run_01M6DN23JRRYT113FF078VYBSF/req.md#requirements"]
trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10, REQ-11]
known: [S-1, S-2, S-3, S-4]
assumed: []
unknown: []
questions: []
stories:
  - id: S-1
    title: See each patient's last visit date on the patient list
    trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5]
    acceptance_criteria:
      - id: AC1
        text: "Given front-desk staff open the patient list, when the list loads, then it has a column titled \"Last visit\"."
      - id: AC2
        text: "Given a patient has several Completed appointments, when staff view the patient list, then Last visit shows the date of the Completed appointment with the latest start time."
      - id: AC3
        text: "Given a patient's latest Completed appointment starts on a date in Asia/Kolkata that differs from its UTC date, when staff view the patient list, then Last visit shows the Asia/Kolkata date."
      - id: AC4
        text: "Given a patient has a Completed appointment, when staff view the patient list, then the Last visit date uses the same date format as the Next appointment column, without the time."
      - id: AC5
        text: "Given a patient has a Cancelled, No-show or Scheduled appointment later than their latest Completed appointment, when staff view the patient list, then Last visit shows the Completed appointment's date and never the Cancelled, No-show or Scheduled one."
  - id: S-2
    title: Patients never seen show a dash
    trace: [REQ-6, REQ-5]
    acceptance_criteria:
      - id: AC1
        text: "Given a patient has no appointments, when staff view the patient list, then Last visit shows \"-\"."
      - id: AC2
        text: "Given a patient has only Cancelled, No-show or Scheduled appointments, when staff view the patient list, then Last visit shows \"-\"."
  - id: S-3
    title: Existing patient list behaviour stays the same
    trace: [REQ-7, REQ-8, REQ-9, REQ-10]
    acceptance_criteria:
      - id: AC1
        text: "Given more patients exist than fit on one page, when staff page through the patient list, then each page shows 20 per page as before."
      - id: AC2
        text: "Given staff use the patient list's current sort order, when the list loads with the Last visit column, then rows appear in the same order as before the change."
      - id: AC3
        text: "Given staff look at the patient list's sort options, when they open them, then Last visit is not offered as a sort option."
      - id: AC4
        text: "Given staff search the patient list, when results load, then the same patients match as before the change."
      - id: AC5
        text: "Given a patient has a Scheduled future appointment, when staff view the patient list, then the Next appointment column shows the same value as before the change."
  - id: S-4
    title: Patient list never shows medical history
    trace: [REQ-11]
    acceptance_criteria:
      - id: AC1
        text: "Given a patient has medical history recorded, when staff view the patient list, then no medical history (blood group, allergies, chronic conditions, current medications or notes) appears in any column, including Last visit."
---
# Stories: HMS-4

## Stories

**S-1: See each patient's last visit date on the patient list.** Traces REQ-1, REQ-2, REQ-3, REQ-4, REQ-5.
- AC1: Given front-desk staff open the patient list, when the list loads, then it has a column titled "Last visit".
- AC2: Given a patient has several Completed appointments, when staff view the list, then Last visit shows the date of the Completed appointment with the latest start time.
- AC3: Given the latest Completed appointment's Asia/Kolkata date differs from its UTC date, when staff view the list, then Last visit shows the Asia/Kolkata date.
- AC4: Given a patient has a Completed appointment, when staff view the list, then the date uses the same format as the Next appointment column, without the time.
- AC5: Given a later Cancelled, No-show or Scheduled appointment exists, when staff view the list, then Last visit still shows the latest Completed appointment's date.

**S-2: Patients never seen show a dash.** Traces REQ-6, REQ-5.
- AC1: Given a patient has no appointments, when staff view the list, then Last visit shows "-".
- AC2: Given a patient has only Cancelled, No-show or Scheduled appointments, when staff view the list, then Last visit shows "-".

**S-3: Existing patient list behaviour stays the same.** Traces REQ-7, REQ-8, REQ-9, REQ-10.
- AC1: Paging still shows 20 per page.
- AC2: Rows keep the current sort order.
- AC3: Last visit is not offered as a sort option.
- AC4: Search returns the same patients as before.
- AC5: The Next appointment column shows the same value as before.

**S-4: Patient list never shows medical history.** Traces REQ-11.
- AC1: Given a patient has medical history recorded, when staff view the list, then no medical history appears in any column, including Last visit.

## Known
- S-1: built from known requirements REQ-1 to REQ-5 (ticket criteria 1–3, plus IDE answers A-01, A-02 and A-04).
- S-2: built from known requirements REQ-6 and REQ-5 (ticket criteria 3 and 4).
- S-3: built from known requirements REQ-7 to REQ-10 (ticket criterion 5, plus IDE answer A-03).
- S-4: built from known requirement REQ-11 (ticket criterion 6 and the baseline data-protection rule).

## Assumed
None. Every traced requirement is known.

## Unknown
None. The intent has no unknown or blocking items.

## Open questions
None.
