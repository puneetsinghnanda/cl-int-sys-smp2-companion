---
schema: stories.v1
run_id: run_01M820V4J0VSJHKNZZ5VK0BT2Y
step: split
attempt: 2
skill: story-split@1.4.0
inputs:
  - path: artifacts/HMS-31/run_01M820V4J0VSJHKNZZ5VK0BT2Y/req.md
    sha256: d970576e048aa037a9ebd47efc879fe8e0b4bac02f0aa652abd98bc55edb9e2c
  - path: context/glossary.md
    sha256: 430a90e14c50f969f7f757089c61cf254eb21596ed6e60abb8bf59286822b9ff
pack_sha:
  sample-system: 7681f87749883d71abb4011d118366ca1242e1cc
claims:
  - id: C1
    text: "The fee is shown the same way as on the doctors list: `₹` with two decimals."
    cites: ["artifacts/HMS-31/run_01M820V4J0VSJHKNZZ5VK0BT2Y/req.md#item:REQ-4"]
  - id: C2
    text: "Fees are stored and sent as integer `paise`, one hundredth of a rupee."
    cites: ["context/glossary.md#paise"]
  - id: C3
    text: "An Inactive specialization or department is shown with its name followed by `(Inactive)`."
    cites: ["artifacts/HMS-31/run_01M820V4J0VSJHKNZZ5VK0BT2Y/req.md#item:REQ-7", "runs/run_01M820V4J0VSJHKNZZ5VK0BT2Y/ide-answers.md#a-01"]
  - id: C4
    text: "Medical history is blood group, `allergies`, chronic conditions, current medications and notes."
    cites: ["context/glossary.md#medical-history"]
  - id: C5
    text: "An `Inactive` doctor, department or specialization is kept for history but not offered for new use."
    cites: ["context/glossary.md#inactive"]
  - id: C6
    text: "Only `Scheduled` appointments can change."
    cites: ["context/glossary.md#scheduled"]
resolves:
  - id: F-1
    how: "S-4 checks that the rest of the page is unchanged for Scheduled, Completed, Cancelled and No-show appointments."
  - id: F-3
    how: "S-3 AC2 and AC3 also check that the appointment data the page receives holds no doctor contact details or patient medical history."
trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10, REQ-11]
known: [S-1, S-2, S-3, S-4]
assumed: []
unknown:
  - {id: U-1, blocking: false}
questions: []
stories:
  - id: S-1
    title: See the doctor's specialization, department and consultation fee on the appointment page
    trace: [REQ-1, REQ-2, REQ-3, REQ-4]
    acceptance_criteria:
      - id: AC1
        text: "Given an appointment whose doctor has a specialization, when front-desk staff open the appointment page, then below \"Doctor\" it shows \"Specialization\" with that doctor's specialization name."
      - id: AC2
        text: "Given an appointment whose doctor has a department, when front-desk staff open the appointment page, then below \"Doctor\" it shows \"Department\" with that doctor's department name."
      - id: AC3
        text: "Given an appointment whose doctor's fee is 50000 paise (one paisa is one hundredth of a rupee, so this is 500 rupees), when front-desk staff open the appointment page, then below \"Doctor\" it shows \"Consultation fee\" as ₹500.00."
      - id: AC4
        text: "Given a doctor whose fee is 12345 paise (123 rupees and 45 paise), when front-desk staff open an appointment of that doctor and the doctors list, then both show the fee as ₹123.45, with ₹ and two decimals."
  - id: S-2
    title: Doctor details are always current, also for closed appointments and Inactive entries
    trace: [REQ-5, REQ-6, REQ-7]
    acceptance_criteria:
      - id: AC1
        text: "Given a Completed appointment whose doctor's fee and department were changed after the appointment, when front-desk staff open the appointment page, then it shows the doctor's current fee and current department, not the earlier ones."
      - id: AC2
        text: "Given a Cancelled appointment and a No-show appointment whose doctor's specialization was changed after booking, when front-desk staff open each appointment page, then both show the doctor's current specialization."
      - id: AC3
        text: "Given an appointment whose doctor is Inactive, when front-desk staff open the appointment page, then it still shows the doctor's current specialization, department and consultation fee."
      - id: AC4
        text: "Given an appointment whose doctor's specialization is Inactive and department is active, when front-desk staff open the appointment page, then \"Specialization\" shows the name followed by \"(Inactive)\" and \"Department\" shows the name without \"(Inactive)\"."
      - id: AC5
        text: "Given an appointment whose doctor's department is Inactive and specialization is active, when front-desk staff open the appointment page, then \"Department\" shows the name followed by \"(Inactive)\" and \"Specialization\" shows the name without \"(Inactive)\"."
  - id: S-3
    title: The doctor link stays and no extra personal data reaches the appointment page
    trace: [REQ-8, REQ-9, REQ-10]
    acceptance_criteria:
      - id: AC1
        text: "Given an appointment, when front-desk staff select the doctor's name on the appointment page, then the doctor page of that doctor opens."
      - id: AC2
        text: "Given an appointment whose doctor has contact details recorded, when front-desk staff open the appointment page, then none of the doctor's contact details are shown and the appointment data the page receives holds none of them."
      - id: AC3
        text: "Given an appointment whose patient has allergies and current medications recorded, when front-desk staff open the appointment page, then no part of the patient's medical history is shown and the appointment data the page receives holds none of it."
  - id: S-4
    title: The rest of the appointment page is unchanged for every appointment status
    trace: [REQ-11]
    acceptance_criteria:
      - id: AC1
        text: "Given a Scheduled appointment with history entries, when front-desk staff open the appointment page, then its time, doctor name, patient name, reason for visit, actions and history are the same as before this change."
      - id: AC2
        text: "Given a Completed appointment with history entries, when front-desk staff open the appointment page, then its time, doctor name, patient name, reason for visit, actions and history are the same as before this change."
      - id: AC3
        text: "Given a Cancelled appointment with a cancellation reason, when front-desk staff open the appointment page, then the cancellation reason, actions and history are the same as before this change."
      - id: AC4
        text: "Given a No-show appointment with history entries, when front-desk staff open the appointment page, then its time, doctor name, patient name, reason for visit, actions and history are the same as before this change."
---
# Stories: HMS-31

## Stories

### S-1 See the doctor's specialization, department and consultation fee on the appointment page
Traces REQ-1, REQ-2, REQ-3, REQ-4.
- AC1: Given an appointment whose doctor has a specialization, when front-desk staff open the appointment page, then below "Doctor" it shows "Specialization" with that doctor's specialization name.
- AC2: Given an appointment whose doctor has a department, when front-desk staff open the appointment page, then below "Doctor" it shows "Department" with that doctor's department name.
- AC3: Given an appointment whose doctor's fee is 50000 paise (one paisa is one hundredth of a rupee, so this is 500 rupees), when front-desk staff open the appointment page, then below "Doctor" it shows "Consultation fee" as ₹500.00.
- AC4: Given a doctor whose fee is 12345 paise (123 rupees and 45 paise), when front-desk staff open an appointment of that doctor and the doctors list, then both show the fee as ₹123.45, with ₹ and two decimals.

### S-2 Doctor details are always current, also for closed appointments and Inactive entries
Traces REQ-5, REQ-6, REQ-7.
- AC1: Given a Completed appointment whose doctor's fee and department were changed after the appointment, when front-desk staff open the appointment page, then it shows the doctor's current fee and current department, not the earlier ones.
- AC2: Given a Cancelled appointment and a No-show appointment whose doctor's specialization was changed after booking, when front-desk staff open each appointment page, then both show the doctor's current specialization.
- AC3: Given an appointment whose doctor is Inactive, when front-desk staff open the appointment page, then it still shows the doctor's current specialization, department and consultation fee.
- AC4: Given an appointment whose doctor's specialization is Inactive and department is active, when front-desk staff open the appointment page, then "Specialization" shows the name followed by "(Inactive)" and "Department" shows the name without "(Inactive)".
- AC5: Given an appointment whose doctor's department is Inactive and specialization is active, when front-desk staff open the appointment page, then "Department" shows the name followed by "(Inactive)" and "Specialization" shows the name without "(Inactive)".

### S-3 The doctor link stays and no extra personal data reaches the appointment page
Traces REQ-8, REQ-9, REQ-10.
- AC1: Given an appointment, when front-desk staff select the doctor's name on the appointment page, then the doctor page of that doctor opens.
- AC2: Given an appointment whose doctor has contact details recorded, when front-desk staff open the appointment page, then none of the doctor's contact details are shown and the appointment data the page receives holds none of them.
- AC3: Given an appointment whose patient has allergies and current medications recorded, when front-desk staff open the appointment page, then no part of the patient's medical history is shown and the appointment data the page receives holds none of it.

### S-4 The rest of the appointment page is unchanged for every appointment status
Traces REQ-11. Each appointment status is a separate partition, because only Scheduled appointments can change and so the page offers different actions per status.
- AC1: Given a Scheduled appointment with history entries, when front-desk staff open the appointment page, then its time, doctor name, patient name, reason for visit, actions and history are the same as before this change.
- AC2: Given a Completed appointment with history entries, when front-desk staff open the appointment page, then its time, doctor name, patient name, reason for visit, actions and history are the same as before this change.
- AC3: Given a Cancelled appointment with a cancellation reason, when front-desk staff open the appointment page, then the cancellation reason, actions and history are the same as before this change.
- AC4: Given a No-show appointment with history entries, when front-desk staff open the appointment page, then its time, doctor name, patient name, reason for visit, actions and history are the same as before this change.

### Resolved review findings
- F-1: S-4 checks that the rest of the page is unchanged for Scheduled, Completed, Cancelled and No-show appointments.
- F-3: S-3 AC2 and AC3 also check that the appointment data the page receives holds no doctor contact details or patient medical history.

## Known
- S-1: built from known REQ-1 to REQ-4 (ticket acceptance criteria 1 and 2).
- S-2: built from known REQ-5, REQ-6 and REQ-7 (ticket acceptance criterion 3 and answer A-01).
- S-3: built from known REQ-8, REQ-9 and REQ-10 (ticket acceptance criteria 4 and 5).
- S-4: built from known REQ-11 (ticket acceptance criterion 6).

## Assumed
None.

## Unknown
- U-1 (non-blocking): the glossary has no entry for Specialization, Department or Consultation fee. The question is already open as runs/run_01M820V4J0VSJHKNZZ5VK0BT2Y/open-questions.md#q-02; the stories use the intent's wording meanwhile.

## Open questions
None new. The open glossary question is runs/run_01M820V4J0VSJHKNZZ5VK0BT2Y/open-questions.md#q-02 (non-blocking, tech lead).
