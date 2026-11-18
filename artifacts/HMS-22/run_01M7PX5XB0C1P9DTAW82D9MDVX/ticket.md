---
schema: ticket-snapshot.v1
run_id: run_01M7PX5XB0C1P9DTAW82D9MDVX
ticket: HMS-22
source: file
fetched_at: '2026-11-18T08:43:08Z'
title: Show new patient registrations on the dashboard
issue_type: Story
source_sha256: afd7f35d5da2c32c1368cd57149a9a2ad13c088f529d734f8f2339995c83ced4
---
# HMS-22 Show new patient registrations on the dashboard
As front-desk staff, I want to see how many patients were registered in the selected period, so
that I can track registration workload.
The dashboard already shows the total number of Active patients (PRD DSH-1) and has a period
selector of Today, Last 7 days or Last 30 days (PRD DSH-7).
Acceptance criteria:
1. The dashboard has a tile "New patients".
2. It shows the number of patients registered in the selected period, using the hospital time
   zone.
3. Active and Inactive patients registered in the period are both counted.
4. Changing the period updates the count, as it does for the Completed and Cancelled tiles.
5. The tile links to the patient list filtered to the patients registered in that period
   (PRD DSH-8).
6. The filtered patient list stays 20 per page and keeps its current search, status filter and
   sort order.
7. When no patient was registered in the period the tile shows 0.
8. The Total patients tile and the rest of the dashboard are unchanged.
9. No medical history and no contact details appear on the dashboard (PRD DSH-9).
