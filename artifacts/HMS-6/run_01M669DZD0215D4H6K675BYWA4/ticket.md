---
schema: ticket-snapshot.v1
run_id: run_01M669DZD0215D4H6K675BYWA4
ticket: HMS-6
source: file
fetched_at: '2026-10-30T11:34:28Z'
title: Show each doctor's appointments today on the doctors list
issue_type: Story
source_sha256: f8bfbf7dbc6ac18b4238be90292d53c2bef4d361261d5c363dc680d9d711e64a
---
# HMS-6 Show each doctor's appointments today on the doctors list

As front-desk staff, I want to see how many appointments each doctor has today, so that I can
book walk-in patients with the doctors who are least busy.

The doctors list shows code, name, specialization, department, fee and status (PRD DOC-2).

Acceptance criteria:
1. The doctors list has a column "Today".
2. It shows the number of the doctor's appointments that start today (hospital time zone),
   counted the same way as the dashboard's "Appointments today" tile: any status except
   Cancelled.
3. A doctor with no appointments today shows 0.
4. The count links to the appointment list filtered to that doctor and today.
5. The list stays 20 per page, sorted by name, with its current filters and search unchanged.
