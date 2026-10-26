---
schema: ticket-snapshot.v1
run_id: run_01M5W1YTCRV6NKT7JB4DNZ753K
ticket: HMS-2
source: file
fetched_at: '2026-10-26T12:11:27Z'
title: Patient list shows "None" instead of "-" when there is no next appointment
issue_type: Bug
source_sha256: acbf4499dc6442d64f4365d0a323a0b33a39b1e8ad1e9be749036cd9e088c7ab
---
# HMS-2 Patient list shows "None" instead of "-" when there is no next appointment

On the patient list, the "Next appointment" column shows the word "None" for a patient who has
no upcoming Scheduled appointment. It should show a hyphen ("-").

Steps to reproduce:
1. Open the patient list.
2. Find a patient with no upcoming Scheduled appointment.

Actual: the "Next appointment" column shows "None".
Expected: the "Next appointment" column shows "-".

Acceptance criteria:
1. A patient with no upcoming Scheduled appointment shows "-" in the "Next appointment" column.
2. Patients with an upcoming Scheduled appointment are unchanged: they still show its start and
   doctor, linked to the appointment.
