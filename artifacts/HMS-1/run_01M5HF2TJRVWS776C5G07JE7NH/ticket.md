---
schema: ticket-snapshot.v1
run_id: run_01M5HF2TJRVWS776C5G07JE7NH
ticket: HMS-1
source: file
fetched_at: '2026-10-22T09:29:11Z'
title: HMS-1 Show each patient's next appointment on the patient list
source_sha256: b5a6bde4360dc98d076795b413aadd806b5acefe1a446b1c11cc8a4bfda124b8
---
# HMS-1 Show each patient's next appointment on the patient list

As front-desk staff, I want to see each patient's next appointment in the patient list,
so that I can answer "when am I coming in next?" without opening every patient.

Acceptance criteria:
1. The patient list has a column "Next appointment".
2. It shows the start date and time (hospital time zone) and the doctor's name of the patient's
   earliest Scheduled appointment that starts after now.
3. Cancelled, Completed and No-show appointments are never shown in that column.
4. A patient with no upcoming Scheduled appointment shows "None".
5. The value links to that appointment's page.
6. The list stays 20 per page and keeps its current search and sort order.
7. No medical history appears in the list (PRD PAT-8).
