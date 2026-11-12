---
schema: ticket-snapshot.v1
run_id: run_01M77B1NEGA2SK4TJPT0CAK6MX
ticket: HMS-11
source: file
fetched_at: '2026-11-12T07:37:38Z'
title: Find the earliest free slot by specialization on the book appointment page
issue_type: Story
source_sha256: db60818c9a610c8c7ca0fbbdb201112a37f31ced4469aefddb39af83bc358def
---
# HMS-11 Find the earliest free slot by specialization on the book appointment page

As front-desk staff, I want to see which doctor of a specialization can see a patient soonest,
so that I can book the earliest appointment without checking every doctor and every day by hand.

The book appointment page lets staff choose a doctor, a patient, a date and one of that doctor's
free slots for that day (APT-1). To find the soonest appointment for, say, Cardiology, staff
today have to pick each cardiologist in turn and step through the dates one by one. Free slots
follow APT-2, and the day schedule shows them per doctor and day (APT-10).

Acceptance criteria:
1. The book appointment page has a section "Earliest free slot" with a specialization picker
   that lists only Active specializations.
2. After a specialization is chosen, the section lists each Active doctor with that
   specialization and their earliest free slot from now until the end of the 7th day after
   today (hospital time zone): doctor name, department, date and start time. The list is ordered
   by that slot, earliest first; ties are ordered by doctor name.
3. A slot counts as free exactly when the day schedule shows it as free for that doctor and date
   (APT-2, APT-10). Slots that have already started are never shown.
4. A doctor with no free slot in that window is listed last with "No free slot in the next 7 days".
   A specialization with no Active doctors shows "No active doctors".
5. Choosing a listed slot fills the booking form with that doctor, date and slot. Staff still
   choose the patient and confirm; booking follows the existing rules, including "slot no longer
   available" if someone else took it first (APT-4).
6. The rest of the book appointment page, and booking a chosen doctor and date as today, is
   unchanged.
