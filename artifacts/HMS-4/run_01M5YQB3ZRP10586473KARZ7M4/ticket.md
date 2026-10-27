---
schema: ticket-snapshot.v1
run_id: run_01M5YQB3ZRP10586473KARZ7M4
ticket: HMS-4
source: file
fetched_at: '2026-10-27T13:03:39Z'
title: Show each patient's last visit on the patient list
issue_type: Story
source_sha256: 7bf59ed5b1a7dbd0750788eef73d1894424c790ee35278778afaf6f747f1f8b3
---
# HMS-4 Show each patient's last visit on the patient list
As front-desk staff, I want to see when each patient last visited, so that I can spot patients
who have not been seen for a long time without opening every patient.
The patient list already shows each patient's next appointment (HMS-1).
Acceptance criteria:
1. The patient list has a column "Last visit".
2. It shows the date (hospital time zone) of the patient's most recent Completed appointment.
3. Cancelled, No-show and Scheduled appointments are never shown in that column.
4. A patient with no Completed appointment shows "-".
5. The list stays 20 per page and keeps its current search and sort order, and the Next
   appointment column is unchanged.
6. No medical history appears in the list (PRD PAT-8).
