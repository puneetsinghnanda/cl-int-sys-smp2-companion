---
schema: ticket-snapshot.v1
run_id: run_01M8EBQFV0B46S25YWJR6HHZWG
ticket: HMS-23
source: file
fetched_at: '2026-11-27T11:19:56Z'
title: Show which doctors are on duty today on the dashboard
issue_type: Story
source_sha256: 6df6957befa3c817dd7bfd04997c98018753e6bacaf421fdbf197838f79cc5e1
---
# HMS-23 Show which doctors are on duty today on the dashboard

As front-desk staff, I want to see on the dashboard which doctors are working today and how many
free slots each has left, so that I can send a walk-in patient to a doctor who can see them today.

The dashboard (PRD §4.4) shows count tiles and the next 10 appointments. Booked and free slots per
day are shown only on the doctor page: the day schedule (PRD DOC-3) and "Next 7 days" (HMS-9).
To find a doctor with room today, staff have to open each doctor's page.

Acceptance criteria:
1. Below the tiles, the dashboard shows a section "On duty today" with one row per Active doctor who
   has availability today (hospital time zone): the doctor's name, department, booked slots, free
   slots and the time of the next free slot.
2. Booked and free slots are counted the same way as the doctor's day schedule shows them for today.
   Slots that have already passed are not free. The next free slot is the earliest free slot that
   starts after now.
3. Rows are sorted by free slots, most first; ties by doctor name.
4. A doctor with no free slots left shows "Fully booked" instead of a time.
5. The doctor's name links to that doctor's day schedule for today.
6. When no doctor is on duty today, the section shows "No doctors on duty today".
7. Inactive doctors are not listed. The section ignores the Period selector and shows no contact
   details (DSH-9).
8. The rest of the dashboard is unchanged.
