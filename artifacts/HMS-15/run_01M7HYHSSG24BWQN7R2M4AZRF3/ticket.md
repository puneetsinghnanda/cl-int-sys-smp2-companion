---
schema: ticket-snapshot.v1
run_id: run_01M7HYHSSG24BWQN7R2M4AZRF3
ticket: HMS-15
source: file
fetched_at: '2026-11-16T10:30:54Z'
title: Show the patient's next appointment on the patient page
issue_type: Story
source_sha256: aa95cdeac30f648110253c797fd70e9565fc821fdf942450b3a818686e3b8bee
---
# HMS-15 Show the patient's next appointment on the patient page

As front-desk staff, I want to see a patient's next appointment at the top of their appointments,
so that I can tell at a glance whether they already have an upcoming visit before I book another one.

The patient page (PAT-3) lists the patient's appointments in a table. To find the next visit, staff
have to read the table and skip past cancelled and closed rows.

Acceptance criteria:
1. Above the Appointments table, the patient page shows "Next appointment:" followed by the date
   and time (in the same format as the table) and the doctor's name. It uses the patient's earliest
   Scheduled appointment that starts after now.
2. When the patient has no such appointment, the page shows "No upcoming appointment".
3. Cancelled, Completed and No-show appointments, and Scheduled appointments that have already
   started, are not used as the next appointment.
4. The rest of the patient page, including the Book appointment button and the Appointments
   table, is unchanged.
