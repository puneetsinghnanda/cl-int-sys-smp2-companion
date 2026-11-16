---
schema: ticket-snapshot.v1
run_id: run_01M7HH0DM88V7JHMVZPSYX5BWE
ticket: HMS-14
source: file
fetched_at: '2026-11-16T06:34:13Z'
title: HMS-14 Book again from a cancelled or no-show appointment
source_sha256: a91a3c694e4860be5f5714a7d5a9ed8dad98dcd4f564fda4bbfdba120a98b993
---
# HMS-14 Book again from a cancelled or no-show appointment

As front-desk staff, I want to book a new appointment straight from a cancelled or no-show appointment,
so that I can rebook the patient with the same doctor without searching for both again.

The appointment page shows a Scheduled appointment's actions: reschedule (APT-5), cancel (APT-6), and mark
Completed or No-show (APT-7). Once an appointment is Cancelled or No-show it cannot be changed (APT-8), and the
page offers no next step. To rebook, staff today open the book appointment page, find the doctor and the patient
again, and retype the reason for visit. The book appointment page already accepts a doctor, patient, date and
slot from its address (as the patient page, doctor page and free-slot links use), and books by the existing
rules (APT-1 to APT-4).

Acceptance criteria:
1. The appointment page of a Cancelled or No-show appointment shows a "Book again" button. Scheduled and
   Completed appointments do not show it.
2. "Book again" opens the book appointment page with the same doctor and patient chosen and the reason for visit
   filled in from the original appointment. The date is today (hospital time zone) and no slot is chosen.
3. If the doctor or the patient is now Inactive, the button is disabled and says why: "Doctor is inactive" or
   "Patient is inactive" (PAT-7; a booking needs an Active doctor and patient).
4. Booking from there follows the existing rules unchanged, including free slots (APT-2), no overlapping
   appointments for the patient (APT-3) and "slot no longer available" (APT-4). The new appointment gets its own
   ID; the original appointment and its history stay as they were (APT-8, APT-11).
5. The reason for visit can still be edited before booking and keeps its 300-character limit.
6. The rest of the appointment page and the book appointment page work as today.
