---
schema: stories.v1
run_id: run_01M5YQB3ZRP10586473KARZ7M4
step: split
attempt: 1
skill: story-split@1.0.1
inputs:
  - path: artifacts/HMS-4/run_01M5YQB3ZRP10586473KARZ7M4/req.md
    sha256: 3e7adfbb0c88fcca0bb2ac95ec1a1b58c07a9930dc1a1c6e56af91e5475f7321
  - path: context/glossary.md
    sha256: 7623345ad8bad33b8a14a713b452edb2a36834e8b1c2228b76f3aa154034f16e
pack_sha:
  sample-system: bd98ff08b83f8762c5e5c073ba5fadef88169b33
claims:
  - id: C1
    text: "The patient list gets a column titled `Last visit`."
    cites: ["artifacts/HMS-4/run_01M5YQB3ZRP10586473KARZ7M4/req.md#L47"]
  - id: C2
    text: "The Last visit date is shown in the hospital time zone `Asia/Kolkata`."
    cites: ["artifacts/HMS-4/run_01M5YQB3ZRP10586473KARZ7M4/req.md#L59"]
  - id: C3
    text: "\"Most recent\" is assumed to mean the latest `start time`."
    cites: ["artifacts/HMS-4/run_01M5YQB3ZRP10586473KARZ7M4/req.md#L65"]
  - id: C4
    text: "A patient with `no Completed appointment` shows \"-\" in the Last visit column."
    cites: ["artifacts/HMS-4/run_01M5YQB3ZRP10586473KARZ7M4/req.md#L90"]
  - id: C5
    text: "The patient list stays at `20 per page`."
    cites: ["artifacts/HMS-4/run_01M5YQB3ZRP10586473KARZ7M4/req.md#L96"]
  - id: C6
    text: "Medical history covers `Blood group`, allergies, chronic conditions, current medications and notes."
    cites: ["context/glossary.md#medical-history"]
trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10, REQ-11, REQ-12, REQ-13]
known: [S-2, S-3, S-4]
assumed: [S-1]
unknown: [U-1, U-2, U-3, U-4, U-5]
unknowns:
  - {id: U-1, blocking: false, text: "Display format of the Last visit date is not fixed.", question: Q-01}
  - {id: U-2, blocking: false, text: "Whether the Last visit column is sortable or filterable is not stated.", question: Q-02}
  - {id: U-3, blocking: false, text: "Term `Last visit` is not in the glossary.", question: Q-03}
  - {id: U-4, blocking: false, text: "Term `patient list` is not in the glossary.", question: Q-03}
  - {id: U-5, blocking: false, text: "Term `Next appointment` is not in the glossary.", question: Q-03}
questions:
  - id: Q-01
    blocking: false
    ask: product owner
    text: "Which date format should the Last visit column use? (intent Q-01, U-1)"
    why: "Acceptance tests need one exact format; the intent only fixes the time zone."
    evidence: ["artifacts/HMS-4/run_01M5YQB3ZRP10586473KARZ7M4/req.md#L59"]
    options: ["Same date format as the existing Next appointment column", "A different, specified format"]
    default: "Same date format as the existing Next appointment column, in Asia/Kolkata"
  - id: Q-02
    blocking: false
    ask: product owner
    text: "Should staff be able to sort or filter the list by Last visit? (intent Q-02, U-2)"
    why: "The user goal is spotting long-unseen patients, but the list must keep its current sort order."
    evidence: ["artifacts/HMS-4/run_01M5YQB3ZRP10586473KARZ7M4/req.md#L108"]
    options: ["Display only; no new sort or filter", "Add sort by Last visit (separate ticket)"]
    default: "Display only; no new sort or filter in HMS-4"
  - id: Q-03
    blocking: false
    ask: tech lead
    text: "Should 'Last visit', 'patient list' and 'Next appointment' be added to the glossary? (intent Q-03, U-3 to U-5)"
    why: "Agreed terms keep later stories and tests consistent."
    evidence: ["context/glossary.md#glossary"]
    options: ["Add the three terms", "Leave as is"]
    default: "Add: Last visit = date of the patient's most recent Completed appointment in hospital time"
stories:
  - id: S-1
    title: See each patient's last visit date on the patient list
    trace: [REQ-1, REQ-2, REQ-3, REQ-4]
    acceptance_criteria:
      - id: AC1
        text: "Given front-desk staff on the patient list, when the list is shown, then it has a column titled \"Last visit\"."
      - id: AC2
        text: "Given a patient with one Completed appointment, when the patient list is shown, then that patient's Last visit shows the date of that appointment."
      - id: AC3
        text: "Given a patient with two Completed appointments, when the patient list is shown, then Last visit shows the date of the one with the latest start time (assumed REQ-4)."
      - id: AC4
        text: "Given a Completed appointment whose start time falls on a different calendar date in Asia/Kolkata than in the server's time zone, when the patient list is shown, then Last visit shows the Asia/Kolkata date."
  - id: S-2
    title: Only Completed appointments count as a visit
    trace: [REQ-5, REQ-6, REQ-7, REQ-8]
    acceptance_criteria:
      - id: AC1
        text: "Given a patient with an earlier Completed appointment and a later Cancelled appointment, when the patient list is shown, then Last visit shows the Completed appointment's date."
      - id: AC2
        text: "Given a patient with an earlier Completed appointment and a later No-show appointment, when the patient list is shown, then Last visit shows the Completed appointment's date."
      - id: AC3
        text: "Given a patient with a Completed appointment and a Scheduled appointment, when the patient list is shown, then Last visit shows the Completed appointment's date, never the Scheduled one."
      - id: AC4
        text: "Given a patient whose appointments are only Cancelled, No-show or Scheduled, when the patient list is shown, then Last visit shows \"-\"."
      - id: AC5
        text: "Given a patient with no appointments at all, when the patient list is shown, then Last visit shows \"-\"."
  - id: S-3
    title: Patient list keeps its paging, search, sort and Next appointment column
    trace: [REQ-9, REQ-10, REQ-11, REQ-12]
    acceptance_criteria:
      - id: AC1
        text: "Given more than 20 patients, when staff open the patient list, then each page shows 20 patients."
      - id: AC2
        text: "Given a search term that matched a set of patients before this change, when staff search the patient list, then the same patients are returned."
      - id: AC3
        text: "Given the patient list before this change, when staff open it after the change, then patients appear in the same sort order."
      - id: AC4
        text: "Given a patient with a next appointment, when the patient list is shown, then the Next appointment column shows the same value as before this change."
  - id: S-4
    title: Patient list shows no medical history
    trace: [REQ-13]
    acceptance_criteria:
      - id: AC1
        text: "Given a patient with medical history recorded (blood group, allergies, chronic conditions, current medications, notes), when the patient list is shown, then none of those values appear in the list, including the Last visit column."
---
# Stories: HMS-4

## Stories

**S-1 See each patient's last visit date on the patient list** (REQ-1, REQ-2, REQ-3, REQ-4)
- AC1: Given front-desk staff on the patient list, when the list is shown, then it has a column titled "Last visit".
- AC2: Given a patient with one Completed appointment, when the list is shown, then Last visit shows that appointment's date.
- AC3: Given a patient with two Completed appointments, when the list is shown, then Last visit shows the date of the one with the latest start time (assumed REQ-4).
- AC4: Given a Completed appointment that falls on a different calendar date in Asia/Kolkata than in the server's time zone, when the list is shown, then Last visit shows the Asia/Kolkata date.

**S-2 Only Completed appointments count as a visit** (REQ-5, REQ-6, REQ-7, REQ-8)
- AC1: A later Cancelled appointment is ignored; the earlier Completed date is shown.
- AC2: A later No-show appointment is ignored; the earlier Completed date is shown.
- AC3: A Scheduled appointment is never shown; the Completed date is shown.
- AC4: A patient with only Cancelled, No-show or Scheduled appointments shows "-".
- AC5: A patient with no appointments shows "-".

**S-3 Patient list keeps its paging, search, sort and Next appointment column** (REQ-9, REQ-10, REQ-11, REQ-12)
- AC1: Each page still shows 20 patients.
- AC2: A search returns the same patients as before the change.
- AC3: Patients appear in the same sort order as before the change.
- AC4: The Next appointment column shows the same values as before the change.

**S-4 Patient list shows no medical history** (REQ-13)
- AC1: No medical history values (blood group, allergies, chronic conditions, current medications, notes) appear anywhere in the list, including the Last visit column.

## Known
- S-2: built from known REQ-5 to REQ-8 (ticket acceptance criteria 3 and 4).
- S-3: built from known REQ-9 to REQ-12 (ticket acceptance criterion 5).
- S-4: built from known REQ-13 (ticket acceptance criterion 6).

## Assumed
- S-1: relies on assumed REQ-4, that "most recent" means the Completed appointment with the latest start time. Basis: the glossary defines Completed by its start time having passed. REQ-1 to REQ-3 in the same story are known.

## Unknown
- U-1 (non-blocking): the display format of the Last visit date is not fixed (Q-01). S-1 checks the date value and time zone, not its format.
- U-2 (non-blocking): whether Last visit is sortable or filterable (Q-02). No story adds sorting; S-3 keeps the current order.
- U-3, U-4, U-5 (non-blocking): "Last visit", "patient list" and "Next appointment" are not glossary terms (Q-03). Stories use the intent's wording.

## Open questions
- Q-01 (non-blocking, product owner): date format of Last visit; default is the Next appointment column's format, in Asia/Kolkata.
- Q-02 (non-blocking, product owner): sort or filter by Last visit; default is display only.
- Q-03 (non-blocking, tech lead): add the three terms to the glossary.
