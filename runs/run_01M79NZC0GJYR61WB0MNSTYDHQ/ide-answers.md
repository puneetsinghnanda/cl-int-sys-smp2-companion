# IDE answers: run_01M79NZC0GJYR61WB0MNSTYDHQ

Written by the tsl post-tool hook from AskUserQuestion. Do not edit: artifacts cite these
entries as the user's own answers.

## A-01
- step: req
- attempt: 1
- asked_at: 2026-11-13T05:35:01Z
- answered_by: eng-1df5
- tool_use_id: toolu_01QAc9JhLJufZJmpSBmRzBeV
- question: Q-01: The doctor page flags only Scheduled appointments that haven't started yet (`starts_at >= now` in doctorRepo.ts). AC2 says the appointment page must flag exactly what the doctor page flags. Should a Scheduled appointment whose start time has passed (not yet closed) show the outside-availability notice?
- options: No, follow the doctor page (proposed default) | Yes, show it anyway
- answer: No, follow the doctor page (proposed default)

## A-02
- step: req
- attempt: 1
- asked_at: 2026-11-13T05:35:01Z
- answered_by: eng-1df5
- tool_use_id: toolu_01QAc9JhLJufZJmpSBmRzBeV
- question: Q-02: The doctor page checks only its first 50 upcoming Scheduled appointments (limit 50 in doctorRepo.ts), so appointments past the 50th are never flagged there. Should the appointment page apply the rule to its own appointment no matter where it falls in that list?
- options: Yes, ignore the 50 limit (proposed default) | No, match the 50 limit exactly
- answer: Use the same rule (withinAvailability on not-yet-started Scheduled appointments), but don't copy the doctor page's 50-appointment limit; that's a list size, not the rule

## A-03
- step: req
- attempt: 1
- asked_at: 2026-11-13T05:35:01Z
- answered_by: eng-1df5
- tool_use_id: toolu_01QAc9JhLJufZJmpSBmRzBeV
- question: Q-03: The doctor page shows its DOC-7 flags while the doctor is Inactive. Should the outside-availability notice also show for an Inactive doctor's appointment, together with the Inactive notice (AC4)?
- options: Yes, both notices (proposed default) | Only the Inactive notice
- answer: Yes, both notices (proposed default)

## A-04
- step: req
- attempt: 1
- asked_at: 2026-11-13T05:35:01Z
- answered_by: eng-1df5
- tool_use_id: toolu_01QAc9JhLJufZJmpSBmRzBeV
- question: REQ-6: I assumed that when both notices apply, the outside-availability notice comes first and the Inactive notice second, both above the actions (the same order as AC1 and AC3). Is that right?
- options: Yes, confirm | Inactive notice first
- answer: Yes, confirm

## A-05
- step: impact
- attempt: 1
- asked_at: 2026-11-13T06:03:04Z
- answered_by: eng-1df5
- tool_use_id: toolu_01XPkBBTHwcSVQgcMFzju8Z1
- question: Q-01: Today the appointment detail (GET /api/appointments/:id) has no doctor status and no availability result. Where should the appointment page get them?
- options: Add both to GET /:id (proposed default) | Web also calls GET /api/doctors/:id | Web re-implements the rule
- answer: Add both to GET /:id (proposed default)

## A-06
- step: impact
- attempt: 1
- asked_at: 2026-11-13T06:03:04Z
- answered_by: eng-1df5
- tool_use_id: toolu_01XPkBBTHwcSVQgcMFzju8Z1
- question: Q-02: The appointment detail is built from SUMMARY_SELECT / toSummary in appointmentRepo.ts, which the appointment list, the doctor's upcoming list, the dashboard and the patient page share. Should the new fields go on AppointmentDetail only?
- options: AppointmentDetail only (proposed default) | Add to AppointmentSummary too
- answer: AppointmentDetail only (proposed default)

## A-07
- step: design
- attempt: 1
- asked_at: 2026-11-13T06:11:16Z
- answered_by: eng-1df5
- tool_use_id: toolu_01PrRTH4yBU2MKudTGLevnE2
- question: Q-01: The appointment detail needs two new fields (per A-05/A-06, on AppointmentDetail only). What names and shape should they have?
- options: doctorStatus + outsideAvailability (proposed default) | doctor.status nested + outsideAvailability | doctorStatus + availabilityWarning
- answer: doctorStatus + outsideAvailability (proposed default)

## A-08
- step: design
- attempt: 1
- asked_at: 2026-11-13T06:11:16Z
- answered_by: eng-1df5
- tool_use_id: toolu_01PrRTH4yBU2MKudTGLevnE2
- question: Q-02: The API computes the availability flag only for Scheduled appointments that have not started (A-01). What should it return for closed or already-started appointments?
- options: false (proposed default) | null
- answer: false (proposed default)
