---
schema: ticket-snapshot.v1
run_id: run_01M79NZC0GJYR61WB0MNSTYDHQ
ticket: HMS-13
source: file
fetched_at: '2026-11-13T05:27:06Z'
title: Warn on the appointment page when a scheduled appointment no longer fits its
  doctor
issue_type: Story
source_sha256: 605931a85bd296d1f7bc7f15493aa1bf46831ecf427d230730466c0bdc797c7a
---
# HMS-13 Warn on the appointment page when a scheduled appointment no longer fits its doctor

As front-desk staff, I want the appointment page to tell me when a scheduled appointment no longer
fits its doctor, so that I can reschedule or cancel it before the patient arrives.

The appointment page shows when, doctor, patient, reason for visit, the actions (reschedule,
cancel, mark completed or no-show) and the history (APT-5, APT-6, APT-7, APT-11). The doctor page
flags Scheduled appointments that fall outside the doctor's current weekly availability (DOC-7),
but the appointment page itself shows no warning. It also does not say when the doctor is
Inactive, although an Inactive doctor's Scheduled appointments stay and need rescheduling or
cancelling (DOC-6).

Acceptance criteria:
1. A Scheduled appointment that falls outside its doctor's current weekly availability shows the
   notice "This appointment is outside the doctor's current availability. Reschedule or cancel it."
   above the actions.
2. "Outside availability" is decided by the same rule the doctor page uses for its DOC-7 flags, so
   an appointment flagged on the doctor page is flagged on its appointment page, and one that is
   not flagged there is not flagged here.
3. A Scheduled appointment whose doctor is Inactive shows the notice "The doctor is Inactive.
   Cancel this appointment or book the patient with another doctor." above the actions.
4. When both apply, both notices are shown.
5. Completed, Cancelled and No-show appointments show neither notice.
6. The rest of the appointment page is unchanged.
