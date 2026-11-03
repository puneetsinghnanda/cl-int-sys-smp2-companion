---
schema: ticket-snapshot.v1
run_id: run_01M6FXTGC048AXKA8X69ASS5QB
ticket: HMS-7
source: file
fetched_at: '2026-11-03T05:24:00Z'
title: Show a doctor's No-shows in the last 30 days on the doctor page
issue_type: Story
source_sha256: 41f4cf799c237598d15043c6b147c64bf8475489f6652487c28053c388d19ed0
---
# HMS-7 Show a doctor's No-shows in the last 30 days on the doctor page

As front-desk staff, I want to see how many of a doctor's patients missed their appointments
recently, so that I can spot doctors whose patients need reminder calls.

The doctor page shows the doctor's details, weekly availability, upcoming appointments and day
schedule (PRD DOC-3).

Acceptance criteria:
1. The doctor page shows "No-shows (last 30 days)" with the details.
2. It counts the doctor's No-show appointments that start in the last 30 days up to the end of
   today (hospital time zone), counted the same way as the dashboard's No-shows tile for Last
   30 days.
3. A doctor with none shows 0.
4. The count links to the appointment list filtered to that doctor, No-show and those 30 days.
5. The rest of the doctor page is unchanged.
