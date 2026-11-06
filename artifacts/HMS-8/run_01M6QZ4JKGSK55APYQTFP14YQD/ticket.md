---
schema: ticket-snapshot.v1
run_id: run_01M6QZ4JKGSK55APYQTFP14YQD
ticket: HMS-8
source: file
fetched_at: '2026-11-06T08:20:54Z'
title: Show and change the doctor filter on the appointment list, and leave out Cancelled
  from the Today link
issue_type: Story
source_sha256: f5fe9df318ad1b9c3539a9c2dba087d93cff4441ec38204b503d81bc0f373db0
---
# HMS-8 Show and change the doctor filter on the appointment list, and leave out Cancelled from the Today link

As front-desk staff, I want to see which doctor the appointment list is filtered to and change
it, and I want the doctors list's "Today" link to show the same appointments it counts, so that
I don't book walk-ins from a wrong list.

The appointment list can be filtered by doctor or patient, status and date range (PRD APT-9).
Today a doctor filter set by a link is not shown, and the only way to change it is "Clear doctor /
patient filter", which also clears the status and dates. The doctors list's "Today" column
counts any status except Cancelled (HMS-6), but its link opens the list with "Any status", so
Cancelled appointments are listed too.

Acceptance criteria:
1. When the appointment list is filtered to a doctor, it shows that doctor's name (not the id).
2. Staff can choose a different doctor or clear the doctor filter from the list, and the status,
   dates and patient filter stay as they were.
3. The status filter has an option "Any except Cancelled", which lists every status except
   Cancelled.
4. The doctors list's "Today" link opens the list with "Any except Cancelled", so the number of
   appointments listed equals the "Today" count.
5. Other links to the appointment list (such as the doctor page's No-shows link) and the rest of
   the list (20 per page, ordered by start time) are unchanged.
