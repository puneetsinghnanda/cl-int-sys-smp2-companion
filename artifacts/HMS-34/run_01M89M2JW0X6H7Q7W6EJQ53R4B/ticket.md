---
schema: ticket-snapshot.v1
run_id: run_01M89M2JW0X6H7Q7W6EJQ53R4B
ticket: HMS-34
source: file
fetched_at: '2026-11-25T15:09:36Z'
title: Mark past appointments that were not completed or rescheduled as Stale
issue_type: Story
source_sha256: 90765490bc9948922f3672957f4f6f6dcae7d3a7f6cfd00f643c5e673459b30d
---
# HMS-34 Mark past appointments that were not completed or rescheduled as Stale
As front-desk staff, I want a scheduled appointment whose time has passed and that was neither
marked completed nor rescheduled within one day to be marked Stale, so that it is not left
looking like an upcoming Scheduled appointment.
Acceptance criteria:
1. Appointments have a new state "Stale".
2. A Scheduled appointment whose time is past, and which is not marked Completed or rescheduled
   within one day, is marked Stale.
3. A past Scheduled appointment that is marked Completed within one day is not marked Stale.
4. A past Scheduled appointment that is rescheduled within one day is not marked Stale.
