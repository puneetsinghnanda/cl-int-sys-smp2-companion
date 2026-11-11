---
schema: ticket-snapshot.v1
run_id: run_01M74D9VA0FDDS3DN77YFEXK7H
ticket: HMS-9
source: file
fetched_at: '2026-11-11T04:19:20Z'
title: Show a doctor's booked and free slots for the next 7 days on the doctor page
issue_type: Story
source_sha256: 9c87c33ea91feaa7ef56ca2ed65ae407f0f57ad804bac1ef72108ed08abe5323
---
# HMS-9 Show a doctor's booked and free slots for the next 7 days on the doctor page

As front-desk staff, I want to see how full a doctor's next week is, so that I can offer
patients the day with the most free slots.

The doctor page shows the doctor's details, weekly availability, upcoming appointments, day
schedule (PRD DOC-3) and No-shows in the last 30 days (HMS-7). The day schedule shows one day's
slots, booked and free (APT-1, APT-10).

Acceptance criteria:
1. The doctor page has a section "Next 7 days" with one row per day from today to 6 days ahead
   (hospital time zone): the date, booked slots and free slots.
2. Booked and free slots are counted the same way as the day schedule shows them for that day.
   Slots that have already passed today count as neither.
3. A day with no availability shows "Not available" instead of counts.
4. Each day links to the day schedule for that date.
5. An inactive doctor shows no "Next 7 days" section.
6. The rest of the doctor page is unchanged.
