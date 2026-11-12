---
schema: ticket-snapshot.v1
run_id: run_01M77BM48R93EMY13TZXCDN7PZ
ticket: HMS-11
source: file
fetched_at: '2026-11-12T07:47:43Z'
title: API to find each doctor's earliest free slot for a specialization
issue_type: Story
source_sha256: cad07cf9efb3dc6838660d307e831d810e1100517743bd9df3fb3164c432c116
---
# HMS-11 API to find each doctor's earliest free slot for a specialization

As front-desk staff, I want the system to tell me which doctor of a specialization can see a patient
soonest, so that I do not have to check every doctor and every day by hand.

Today the only way to find free slots is the day schedule, one doctor and one date at a time
(`GET /api/doctors/:id/slots?date=`, APT-10). Free slots follow APT-2. This story adds the API only;
the booking page section that uses it is a later story.

Acceptance criteria:
1. A new endpoint `GET /api/specializations/:id/earliest-slots` returns, for that specialization,
   one entry per Active doctor with that specialization.
2. The window runs from now until the end of the 6th day after today in the hospital time zone,
   which is 7 calendar days (today to today+6), the same days as the doctor page's next-7-days summary (HMS-9).
3. Each entry has the doctor's id, code, full name and department name, and their earliest free slot
   in the window as date (YYYY-MM-DD, hospital time zone), start (UTC ISO instant) and label (HH:MM),
   or `slot: null` when the doctor has no free slot in the window.
4. A slot counts as free exactly when the day schedule shows it as FREE for that doctor and date
   (APT-2, APT-10). Slots that have already started are never returned.
5. Entries are ordered by slot start, earliest first; ties by doctor name. Doctors with `slot: null`
   come last, ordered by doctor name.
6. The response is `{ items: [...] }` with every Active doctor of the specialization and no paging.
   A specialization with no Active doctors returns `{ items: [] }`.
7. An unknown specialization id returns 404. An Inactive specialization returns 400 with the message
   "specialization is Inactive".
8. Entries carry no contact details and no medical history.
9. No other endpoint, page or booking rule changes.
