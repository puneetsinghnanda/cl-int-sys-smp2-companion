---
schema: stories.v1
run_id: run_01M6DR2FJ8XZ1J3VWHQTB17QBH
step: split
attempt: 2
skill: story-split@1.0.1
inputs:
  - path: artifacts/HMS-4/run_01M6DR2FJ8XZ1J3VWHQTB17QBH/req.md
    sha256: a4368b1b80bb6173569256c5cf6e26985261df9a7da1faff3a4f3be7a67ec369
  - path: context/glossary.md
    sha256: 7623345ad8bad33b8a14a713b452edb2a36834e8b1c2228b76f3aa154034f16e
pack_sha:
  sample-system: bd98ff08b83f8762c5e5c073ba5fadef88169b33
claims:
  - id: C1
    text: "A `Completed` appointment is closed by staff once its start time has passed."
    cites: ["context/glossary.md#completed"]
  - id: C2
    text: "A `Scheduled` appointment is booked and still open."
    cites: ["context/glossary.md#scheduled"]
  - id: C3
    text: "`Medical history` is blood group, allergies, chronic conditions, current medications and notes of one patient."
    cites: ["context/glossary.md#medical-history"]
  - id: C4
    text: "In the MVP every user acts as `staff`."
    cites: ["context/glossary.md#front-desk-staff"]
trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10, REQ-11, REQ-12, REQ-13]
known: [S-1, S-2, S-3, S-4]
assumed: []
unknown: []
questions: []
resolves:
  - id: F-1
    how: "S-1 AC3 now uses a Completed appointment in the early morning hospital time, where the Asia/Kolkata calendar date is one day later than the UTC date, so showing the UTC date fails the check."
stories:
  - id: S-1
    title: Front-desk staff see each patient's last visit date
    trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-11]
    acceptance_criteria:
      - id: AC1
        text: "Given front-desk staff open the patient list, when the list is shown, then it has a column titled \"Last visit\"."
      - id: AC2
        text: "Given a patient with one or more Completed appointments, when the patient list is shown, then the Last visit column shows the date of the Completed appointment with the latest start time."
      - id: AC3
        text: "Given a patient whose latest Completed appointment starts in the early morning hospital time, so that its calendar date in Asia/Kolkata is one day later than its UTC date, when the patient list is shown, then the Last visit column shows the Asia/Kolkata calendar date, not the earlier UTC date."
      - id: AC4
        text: "Given a patient with a Completed appointment and a later Cancelled, No-show or Scheduled appointment, when the patient list is shown, then the Last visit column shows the Completed appointment's date and never the date of the Cancelled, No-show or Scheduled appointment."
      - id: AC5
        text: "Given a patient whose only appointments are Cancelled, No-show or Scheduled, when the patient list is shown, then the Last visit column shows none of those appointments' dates."
  - id: S-2
    title: Last visit shows only a date, or "-" when the patient has never visited
    trace: [REQ-5, REQ-13]
    acceptance_criteria:
      - id: AC1
        text: "Given a patient with no Completed appointment, when the patient list is shown, then the Last visit column shows \"-\"."
      - id: AC2
        text: "Given a patient with no appointments at all, when the patient list is shown, then the Last visit column shows \"-\"."
      - id: AC3
        text: "Given patients with old and recent last visits, when the patient list is shown, then the Last visit column shows only the date, with no highlight and no filter for patients not seen for a long time."
  - id: S-3
    title: Existing patient list behaviour is unchanged
    trace: [REQ-6, REQ-7, REQ-8, REQ-9, REQ-12]
    acceptance_criteria:
      - id: AC1
        text: "Given more than 20 patients, when front-desk staff open the patient list, then it shows 20 patients per page, as before."
      - id: AC2
        text: "Given a search term, when staff search the patient list, then the same patients are returned as before the change."
      - id: AC3
        text: "Given the patient list, when it is shown, then patients are in the same sort order as before the change."
      - id: AC4
        text: "Given a patient with a Scheduled future appointment, when the patient list is shown, then the Next appointment column shows the same value as before the change."
      - id: AC5
        text: "Given the patient list, when staff look at the sort options, then Last visit is not offered as a sort option."
  - id: S-4
    title: No medical history in the patient list
    trace: [REQ-10]
    acceptance_criteria:
      - id: AC1
        text: "Given a patient with medical history recorded, when the patient list is shown or its data is fetched, then no blood group, allergies, chronic conditions, current medications or notes appear in the list."
---
# Stories: HMS-4

## Stories

**S-1 — Front-desk staff see each patient's last visit date** (REQ-1, REQ-2, REQ-3, REQ-4, REQ-11)
- AC1: Given front-desk staff open the patient list, when the list is shown, then it has a column titled "Last visit".
- AC2: Given a patient with one or more Completed appointments, when the list is shown, then Last visit shows the date of the Completed appointment with the latest start time.
- AC3: Given a latest Completed appointment in the early morning hospital time (Asia/Kolkata date one day later than the UTC date), when the list is shown, then the Asia/Kolkata calendar date is shown, not the earlier UTC date.
- AC4: Given a Completed appointment and a later Cancelled, No-show or Scheduled one, when the list is shown, then only the Completed appointment's date is shown.
- AC5: Given only Cancelled, No-show or Scheduled appointments, when the list is shown, then none of their dates appear in Last visit.

**S-2 — Last visit shows only a date, or "-" when the patient has never visited** (REQ-5, REQ-13)
- AC1: Given no Completed appointment, when the list is shown, then Last visit shows "-".
- AC2: Given no appointments at all, when the list is shown, then Last visit shows "-".
- AC3: Given old and recent last visits, when the list is shown, then only the date is shown — no highlight, no filter.

**S-3 — Existing patient list behaviour is unchanged** (REQ-6, REQ-7, REQ-8, REQ-9, REQ-12)
- AC1: Given more than 20 patients, when the list opens, then it shows 20 per page.
- AC2: Given a search term, when staff search, then the same patients are returned as before.
- AC3: Given the list, when shown, then the sort order is the same as before.
- AC4: Given a Scheduled future appointment, when the list is shown, then Next appointment shows the same value as before.
- AC5: Given the list, when staff look at sort options, then Last visit is not offered.

**S-4 — No medical history in the patient list** (REQ-10)
- AC1: Given a patient with medical history recorded, when the list is shown or its data fetched, then no blood group, allergies, chronic conditions, current medications or notes appear.

## Known
- S-1: built only from known REQ-1, REQ-2, REQ-3, REQ-4, REQ-11.
- S-2: built only from known REQ-5, REQ-13.
- S-3: built only from known REQ-6, REQ-7, REQ-8, REQ-9, REQ-12.
- S-4: built only from known REQ-10.

## Assumed
None — every traced requirement is known.

## Unknown
None — the intent has no unknowns and every requirement is covered.

## Open questions
None.

## Resolves
- F-1: S-1 AC3 now tests an early-morning hospital-time appointment whose Asia/Kolkata date is one day later than its UTC date, so a UTC-date bug is caught.
