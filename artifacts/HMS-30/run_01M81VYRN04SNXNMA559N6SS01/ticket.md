---
schema: ticket-snapshot.v1
run_id: run_01M81VYRN04SNXNMA559N6SS01
ticket: HMS-30
source: file
fetched_at: '2026-11-22T14:53:24Z'
title: Show a count per status above the appointment list
issue_type: Story
source_sha256: 215d7f2508db4fbee7cc2513e369140191541b518c0aa5b516d3b6812458ec01
---
# HMS-30 Show a count per status above the appointment list

As front-desk staff, I want to see how many of the listed appointments are in each status, so that
I can tell at a glance how many are still to happen, were attended, were cancelled or were missed,
without paging through the list.

The appointment list (PRD APT-9) shows appointments for one doctor or one patient, filtered by date
range and status, 20 per page, ordered by start time. Above the table it shows only the total, for
example "34 appointment(s)".

Acceptance criteria:
1. Above the table, next to the total, the appointment list shows one count per status in this
   order: Scheduled, Completed, Cancelled, No-show. For example "Scheduled 4 · Completed 25 ·
   Cancelled 3 · No-show 2".
2. The counts cover every appointment that matches the current doctor, patient and date range
   filters, not only the current page.
3. The counts ignore the Status filter, so all four statuses are always shown; a status with no
   appointments shows 0.
4. The four counts add up to the total shown when the Status filter is "Any status".
5. Each count links to the same list with the Status filter set to that status.
6. The table, its order, the 20-per-page paging and the existing filters are unchanged.
