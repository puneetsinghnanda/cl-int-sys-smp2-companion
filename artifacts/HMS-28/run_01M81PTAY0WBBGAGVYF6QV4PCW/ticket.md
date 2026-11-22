---
schema: ticket-snapshot.v1
run_id: run_01M81PTAY0WBBGAGVYF6QV4PCW
ticket: HMS-28
source: file
fetched_at: '2026-11-22T13:23:36Z'
title: Show and filter doctors by the days they work on the doctor list
issue_type: Story
source_sha256: 4ffe99e428148f64c364aea22f304d5515188c9b63276fbbbe309f6e23712f50
---
# HMS-28 Show and filter doctors by the days they work on the doctor list

As front-desk staff, I want to see on the doctor list which weekdays each doctor works and to list
only the doctors who work on a given day, so that I can tell a patient which doctors they can book
on, for example, a Saturday.

The doctor list (PRD DOC-2) shows code, name, specialization, department, fee and status, and can
be filtered by department, specialization and status. A doctor's weekly availability (blocks of
day-of-week, start time and end time) is shown only on the doctor's own page (DOC-3), so staff
have to open each doctor to find out who works on a given day.

Acceptance criteria:
1. The doctor list shows a column "Works on" with the weekdays on which the doctor has at least one
   availability block, as short names in the order Mon, Tue, Wed, Thu, Fri, Sat, Sun, separated by
   commas (for example "Mon, Wed, Sat").
2. A doctor with no availability blocks shows "No availability" in that column.
3. The filters have a new "Works on" choice: "Any day" (the default) or one weekday, Monday to
   Sunday. Choosing a weekday lists only the doctors who have at least one availability block on
   that weekday.
4. The "Works on" filter combines with search and the department, specialization and status
   filters (DOC-2). The list stays sorted by name, 20 per page, and the page count reflects the
   filter.
5. Like the other filters, the chosen weekday is kept in the page address, so going back or
   opening a copied link shows the same filtered list.
6. Inactive doctors show their weekdays too; the status filter works as before.
7. The rest of the doctor list is unchanged.
