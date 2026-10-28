---
schema: ticket-snapshot.v1
run_id: run_01M60VHZHR79G15R3AKPB0VYDK
ticket: HMS-5
source: file
fetched_at: '2026-10-28T08:55:47Z'
title: Show a visit summary on the patient page
issue_type: Story
source_sha256: 8fcb9c88f14acec63218023b30032363ac2510caa9710b0d031297c421a8af5d
---
# HMS-5 Show a visit summary on the patient page
As front-desk staff, I want to see at a glance how often a patient has attended or missed
appointments, so that I can follow up with patients who often miss them.
The patient page already lists the patient's appointments, upcoming first, then past (PRD PAT-3).
Acceptance criteria:
1. The patient page shows a "Visit summary" above the appointments list.
2. It shows the number of the patient's Completed appointments and the number of their No-show
   appointments.
3. Scheduled and Cancelled appointments are not counted in either number.
4. A patient with no Completed or No-show appointments shows 0 for each.
5. The rest of the patient page, including the appointments list and medical history, is
   unchanged.
