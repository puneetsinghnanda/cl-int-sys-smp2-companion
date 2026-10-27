---
schema: ticket-snapshot.v1
run_id: run_01M5XZYC101HCFE34KWJ49C90V
ticket: HMS-3
source: file
fetched_at: '2026-10-27T06:14:44Z'
title: Show a No-shows tile on the dashboard
issue_type: Story
source_sha256: ed35b34aa8bbd8284f2e19fd81ccf5711bd6f97c7f385b2a2926e2b775640513
---
# HMS-3 Show a No-shows tile on the dashboard

As front-desk staff, I want to see how many patients did not turn up for their appointments,
so that I can follow up with them and spot doctors or days with many missed visits.

The dashboard already shows Completed and Cancelled appointments for the selected period
(PRD DSH-5, DSH-6, DSH-7). No-show appointments are not shown anywhere on the dashboard.

Acceptance criteria:
1. The dashboard has a tile "No-shows".
2. It shows the number of No-show appointments that start in the selected period (Today, Last 7
   days or Last 30 days), counted the same way as the Completed and Cancelled tiles.
3. Changing the period updates the No-shows count, as it does for Completed and Cancelled.
4. The tile links to the appointment list filtered to No-show in the selected period (as DSH-8
   does for the other tiles).
5. The other tiles and their counts are unchanged.
6. No medical history or contact details appear on the dashboard (PRD DSH-9).
