---
schema: ticket-snapshot.v1
run_id: run_01M7MD2RTRSVZ65HR3QFFR9NJB
ticket: HMS-21
source: file
fetched_at: '2026-11-17T09:23:19Z'
title: Show appointments awaiting an outcome on the dashboard
issue_type: Story
source_sha256: ddb39e4da5245bc7ba09128879b0b507b55e531ff72943f80f085ef38f998711
---
# HMS-21 Show appointments awaiting an outcome on the dashboard
As front-desk staff, I want to see how many past appointments are still Scheduled, so that I can
mark them Completed or No-show and keep the counts accurate.
An appointment whose start time has passed stays Scheduled until staff mark it (PRD APT-7).
Acceptance criteria:
1. The dashboard has a tile "Awaiting outcome".
2. It shows the number of Scheduled appointments whose start time is before now.
3. Completed, Cancelled and No-show appointments, and Scheduled appointments that have not
   started yet, are never counted.
4. The count does not change with the period selector (PRD DSH-7).
5. The tile links to the appointment list showing exactly the appointments it counts (PRD DSH-8).
6. When there are no such appointments the tile shows 0.
7. The other tiles and the list of the next 10 upcoming appointments are unchanged.
8. No medical history and no contact details appear (PRD DSH-9).
