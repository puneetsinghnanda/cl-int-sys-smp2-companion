---
schema: ticket-snapshot.v1
run_id: run_01M7VTSHTGA44X1D4YYX05G0DP
ticket: HMS-26
source: file
fetched_at: '2026-11-20T06:37:38Z'
title: Show a visit summary at the top of the patient page
issue_type: Story
source_sha256: c785591baac1a27a2e1876244d16e63219ad4396a819c2041ce70b236eb260eb
---
# HMS-26 Show a visit summary at the top of the patient page

As front-desk staff, I want to see at a glance when a patient last visited, when they are due next and how
often they missed or cancelled, so that I can answer "when is my next appointment?" and spot repeated
no-shows without reading the whole appointment table.

The patient page (PRD PAT-3) shows the patient's details, medical history and a table of all their
appointments, upcoming first, then past. To find the last completed visit or count the no-shows, staff
have to scan that table row by row.

Acceptance criteria:
1. Below the patient's name and above the details, the patient page shows a "Visits" summary with:
   next appointment, last visit, and the number of Completed, Cancelled and No-show appointments.
2. Next appointment is the earliest Scheduled appointment that starts after now (hospital time zone),
   shown as its date and time and the doctor's name; the date and time link to that appointment's page.
   With none, it shows "None booked".
3. Last visit is the latest Completed appointment, shown as its date and the doctor's name. With none,
   it shows "No visits yet".
4. The counts cover all of the patient's appointments, of any date. A count of 0 is shown as 0.
5. When the No-show count is 2 or more, it is highlighted as a warning.
6. A patient with no appointments shows "None booked", "No visits yet" and three counts of 0.
7. The summary shows no medical history and no contact details, and is only on the patient's own page
   (PAT-8).
8. The rest of the patient page, and every other page, is unchanged.
