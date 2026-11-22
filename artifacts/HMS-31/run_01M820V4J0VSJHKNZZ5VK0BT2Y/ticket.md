---
schema: ticket-snapshot.v1
run_id: run_01M820V4J0VSJHKNZZ5VK0BT2Y
ticket: HMS-31
source: file
fetched_at: '2026-11-22T16:18:48Z'
title: Show the doctor's specialization, department and fee on the appointment page
issue_type: Story
source_sha256: c010a4f494875724e5bb574ded46160c5d9abcedeb80ae7d966d08cdf443ad55
---
# HMS-31 Show the doctor's specialization, department and fee on the appointment page

As front-desk staff, I want to see the doctor's specialization, department and consultation fee on
the appointment page, so that I can answer a patient's questions about their appointment without
opening the doctor's page.

The appointment page shows the appointment's time, doctor name, patient name, reason for visit,
cancellation reason (when cancelled) and history (PRD APT-11). The doctor's specialization,
department and fee are shown only on the doctors list and the doctor page (PRD DOC-2, DOC-3).

Acceptance criteria:
1. Below "Doctor", the appointment page shows "Specialization", "Department" and "Consultation fee"
   for the appointment's doctor.
2. The fee is shown the same way as on the doctors list: ₹ with two decimals (PRD §4.1).
3. The values are the doctor's current details, also for Completed, Cancelled and No-show
   appointments, and also when the doctor is Inactive.
4. The doctor's name still links to the doctor page.
5. No contact details of the doctor and no medical history of the patient are added (PRD PAT-8).
6. The rest of the appointment page, including its actions and history, is unchanged.
