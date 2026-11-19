---
schema: ticket-snapshot.v1
run_id: run_01M7T1CAT0DDAGAWYAP7TPJ3YB
ticket: HMS-24
source: file
fetched_at: '2026-11-19T13:54:16Z'
title: Show a visit summary on the patient page
issue_type: Story
source_sha256: 8bdff5529150f940941997f991a8f20cd8e6d5c958e09922636fc9a8b3560e0b
---
# HMS-24 Show a visit summary on the patient page

As front-desk staff, I want to see at a glance how often a patient has visited and when they last
came in, so that I can answer "when was I last here?" and spot patients who often miss appointments
without reading the whole appointment table.

The patient page (PRD PAT-3) lists the patient's appointments, upcoming first, then past. To count
visits or find the last one, staff have to read every row.

Acceptance criteria:
1. On the patient page, directly above the Appointments table, a "Visit summary" line shows the
   number of the patient's Completed, No-show and Cancelled appointments, and the number of
   Scheduled appointments that start after now, for example
   "4 completed · 1 no-show · 2 cancelled · 1 upcoming".
2. The counts cover all of the patient's appointments, with any doctor and on any date.
3. Below the counts, "Last visit:" shows the start date (hospital time zone, same date format as the
   appointment table) and the doctor's name of the patient's most recent Completed appointment. The
   date links to that appointment.
4. A patient with no Completed appointment shows "Last visit: none yet".
5. A patient with no appointments at all shows "No visits yet" instead of the counts and last visit.
6. The summary shows no reason for visit, no cancellation reason and no medical history details.
7. The rest of the patient page and every other page are unchanged.
