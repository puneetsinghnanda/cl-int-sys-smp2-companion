---
schema: intent.v1
run_id: run_01M820V4J0VSJHKNZZ5VK0BT2Y
step: req
attempt: 1
skill: requirement-review@1.4.0
inputs:
  - path: artifacts/HMS-31/run_01M820V4J0VSJHKNZZ5VK0BT2Y/ticket.md
    sha256: 92382abe29e3a985c6f639e9e27748c1c59ed95f6ae24a56e1fdf4b6183d3eec
  - path: context/intent-baseline.md
    sha256: 9bfd821aeb2fb9524863e346707e049e4fcfb949a6e55b90223a79194946a215
  - path: context/glossary.md
    sha256: 430a90e14c50f969f7f757089c61cf254eb21596ed6e60abb8bf59286822b9ff
pack_sha:
  sample-system: 7681f87749883d71abb4011d118366ca1242e1cc
claims:
  - id: C1
    text: "The appointment page today shows the appointment's time, doctor name, patient name, reason for visit, `cancellation reason` and history."
    cites: ["artifacts/HMS-31/run_01M820V4J0VSJHKNZZ5VK0BT2Y/ticket.md#L17-L18"]
  - id: C2
    text: "Today the doctor's specialization, department and fee are shown only on the `doctors list` and the doctor page."
    cites: ["artifacts/HMS-31/run_01M820V4J0VSJHKNZZ5VK0BT2Y/ticket.md#L18-L19"]
  - id: C3
    text: "A doctor has one specialization, one department and a fee stored in `integer paise`."
    cites: ["context/data-model.md#doctors"]
  - id: C4
    text: "A department or specialization used by a doctor is set `Inactive` instead of deleted."
    cites: ["context/data-model.md#reference-lists"]
  - id: C5
    text: "Medical history appears only on the `patient's own page`, never in lists, the dashboard or logs."
    cites: ["context/intent-baseline.md#data-protection"]
  - id: C6
    text: "The hospital currency is `INR`."
    cites: ["context/intent-baseline.md#constraints"]
  - id: C7
    text: "Completed, Cancelled and No-show appointments `never change`."
    cites: ["context/intent-baseline.md#closed-appointments"]
trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10, REQ-11]
known: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10, REQ-11]
assumed: []
unknown: [U-1]
requirements:
  - id: REQ-1
    title: Show the doctor's specialization
    type: functional
    status: known
    statement: "Below \"Doctor\", the appointment page shows `Specialization` with the specialization of the appointment's doctor."
    cites: ["artifacts/HMS-31/run_01M820V4J0VSJHKNZZ5VK0BT2Y/ticket.md#L22-L23"]
  - id: REQ-2
    title: Show the doctor's department
    type: functional
    status: known
    statement: "Below \"Doctor\", the appointment page shows `Department` with the department of the appointment's doctor."
    cites: ["artifacts/HMS-31/run_01M820V4J0VSJHKNZZ5VK0BT2Y/ticket.md#L22-L23"]
  - id: REQ-3
    title: Show the doctor's consultation fee
    type: functional
    status: known
    statement: "Below \"Doctor\", the appointment page shows `Consultation fee` with the fee of the appointment's doctor."
    cites: ["artifacts/HMS-31/run_01M820V4J0VSJHKNZZ5VK0BT2Y/ticket.md#L22-L23"]
  - id: REQ-4
    title: Fee format matches the doctors list
    type: constraint
    status: known
    statement: "The fee is shown the same way as on the doctors list: `₹` with two decimals."
    cites: ["artifacts/HMS-31/run_01M820V4J0VSJHKNZZ5VK0BT2Y/ticket.md#L24", "context/intent-baseline.md#constraints"]
  - id: REQ-5
    title: Current details for closed appointments
    type: functional
    status: known
    statement: "The shown values are the doctor's current details, also for `Completed`, Cancelled and No-show appointments."
    cites: ["artifacts/HMS-31/run_01M820V4J0VSJHKNZZ5VK0BT2Y/ticket.md#L25-L26"]
  - id: REQ-6
    title: Current details for an Inactive doctor
    type: functional
    status: known
    statement: "The shown values are the doctor's current details also when the doctor is `Inactive`."
    cites: ["artifacts/HMS-31/run_01M820V4J0VSJHKNZZ5VK0BT2Y/ticket.md#L25-L26"]
  - id: REQ-7
    title: Mark an Inactive specialization or department
    type: functional
    status: known
    statement: "When the doctor's specialization or department is Inactive, the appointment page shows its name followed by `(Inactive)`."
    cites: ["runs/run_01M820V4J0VSJHKNZZ5VK0BT2Y/ide-answers.md#a-01", "context/data-model.md#reference-lists"]
  - id: REQ-8
    title: Doctor name still links to the doctor page
    type: constraint
    status: known
    statement: "The doctor's name on the appointment page still `links to the doctor page`."
    cites: ["artifacts/HMS-31/run_01M820V4J0VSJHKNZZ5VK0BT2Y/ticket.md#L27"]
  - id: REQ-9
    title: No doctor contact details added
    type: constraint
    status: known
    statement: "No `contact details` of the doctor are added to the appointment page."
    cites: ["artifacts/HMS-31/run_01M820V4J0VSJHKNZZ5VK0BT2Y/ticket.md#L28"]
  - id: REQ-10
    title: No patient medical history added
    type: constraint
    status: known
    statement: "No `medical history` of the patient is added to the appointment page."
    cites: ["artifacts/HMS-31/run_01M820V4J0VSJHKNZZ5VK0BT2Y/ticket.md#L28", "context/intent-baseline.md#data-protection"]
  - id: REQ-11
    title: Rest of the appointment page unchanged
    type: constraint
    status: known
    statement: "The rest of the appointment page, including its `actions` and history, is unchanged."
    cites: ["artifacts/HMS-31/run_01M820V4J0VSJHKNZZ5VK0BT2Y/ticket.md#L29"]
unknowns:
  - id: U-1
    text: "The glossary has no entry for `Specialization`, Department or Consultation fee; the data model describes the first two as reference lists and the fee as integer paise."
    question: Q-02
questions:
  - id: Q-02
    blocking: false
    ask: tech lead
    text: "Should the glossary get entries for Specialization, Department and Consultation fee?"
    why: "The terms are used on the appointment page but are not defined in the glossary."
    evidence: ["context/glossary.md#glossary", "context/data-model.md#reference-lists", "context/data-model.md#doctors"]
    options: ["Add an entry from the data model's reference-lists description", "Leave the glossary as it is"]
    default: "Add an entry from the data model's reference-lists description"
answers:
  - id: A-01
    kind: question
    about: Q-01
    answer: "Name plus '(Inactive)'"
    requirements: [REQ-7]
    mismatch:
      text: "The proposed default and the ticket's 'current details' would show the name as is; the user decided to append '(Inactive)' when the specialization or department is Inactive."
      cites: ["artifacts/HMS-31/run_01M820V4J0VSJHKNZZ5VK0BT2Y/ticket.md#L25-L26"]
glossary:
  - {term: Front-desk staff, found: true}
  - {term: Specialization, found: false}
  - {term: Department, found: false}
  - {term: Consultation fee, found: false}
  - {term: Completed, found: true}
  - {term: Cancelled, found: true}
  - {term: No-show, found: true}
  - {term: Inactive, found: true}
  - {term: Medical history, found: true}
---
# Requirements review: HMS-31

## Summary
HMS-31 asks that the appointment page show the doctor's specialization, department and
consultation fee below "Doctor", so that front-desk staff do not have to open the doctor page.
The fee must look the same as on the doctors list (₹ with two decimals), and the values are
always the doctor's current details, also for closed appointments and Inactive doctors. The main
risks are leaking extra data onto the page (doctor contact details, patient medical history) and
changing other parts of the page by accident. In the IDE the user decided that an Inactive
specialization or department is shown with an "(Inactive)" marker.

## Requirements
- REQ-1 Show the doctor's specialization — known — Below "Doctor", the page shows "Specialization".
- REQ-2 Show the doctor's department — known — Below "Doctor", the page shows "Department".
- REQ-3 Show the doctor's consultation fee — known — Below "Doctor", the page shows "Consultation fee".
- REQ-4 Fee format matches the doctors list — known — ₹ with two decimals.
- REQ-5 Current details for closed appointments — known — also for Completed, Cancelled and No-show.
- REQ-6 Current details for an Inactive doctor — known — also when the doctor is Inactive.
- REQ-7 Mark an Inactive specialization or department — known — name followed by "(Inactive)".
- REQ-8 Doctor name still links to the doctor page — known.
- REQ-9 No doctor contact details added — known.
- REQ-10 No patient medical history added — known.
- REQ-11 Rest of the appointment page unchanged — known — including actions and history.

## Known
- REQ-1, REQ-2, REQ-3: acceptance criterion 1 of the ticket.
- REQ-4: acceptance criterion 2; currency INR per the baseline constraints.
- REQ-5, REQ-6: acceptance criterion 3.
- REQ-7: decided by the user in A-01; reference lists can be Inactive per the data model.
- REQ-8: acceptance criterion 4.
- REQ-9, REQ-10: acceptance criterion 5; medical history rule in the baseline data protection.
- REQ-11: acceptance criterion 6.

## Assumed
None

## Unknown
- U-1: the glossary has no entry for Specialization, Department or Consultation fee (Q-02).

## Open questions
- Q-02 (non-blocking, tech lead): should the glossary get entries for Specialization, Department
  and Consultation fee? Proposed default: add them from the data model's descriptions.

Answered in the IDE:
- A-01 (Q-01, answered by the user): what to show when the doctor's specialization or department
  is Inactive → "Name plus '(Inactive)'". Decides REQ-7. Mismatch: the ticket asks only for the
  current details and the proposed default was the name as is; the user chose to add the marker.

## Out of scope
- Changing what the doctors list or doctor page show: the ticket only names the appointment page.
- Showing the doctor's details as they were at booking time: the ticket asks for current details.
