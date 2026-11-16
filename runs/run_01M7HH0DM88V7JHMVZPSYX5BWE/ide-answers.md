# IDE answers: run_01M7HH0DM88V7JHMVZPSYX5BWE

Written by the tsl post-tool hook from AskUserQuestion. Do not edit: artifacts cite these
entries as the user's own answers.

## A-01
- step: req
- attempt: 1
- asked_at: 2026-11-16T06:41:27Z
- answered_by: eng-dffd
- tool_use_id: toolu_01XMUZ7vzeQ4RBGRsqfhgQF4
- question: Q-01: Ticket says a disabled "Book again" says "Doctor is inactive" or "Patient is inactive" (ticket L27-L28), but not what it says when both are Inactive. Which applies?
- options: Show both (proposed default) | Doctor reason only | Patient reason only
- answer: Show both (proposed default)

## A-02
- step: req
- attempt: 1
- asked_at: 2026-11-16T06:41:27Z
- answered_by: eng-dffd
- tool_use_id: toolu_01XMUZ7vzeQ4RBGRsqfhgQF4
- question: Q-02: "Reason for visit" is not in the glossary. Is it the appointment's own reason-for-visit field (not the cancellation reason a Cancelled appointment also has)?
- options: Yes, visit reason (proposed default) | Include cancel reason
- answer: Yes, visit reason (proposed default)

## A-03
- step: req
- attempt: 1
- asked_at: 2026-11-16T06:41:27Z
- answered_by: eng-dffd
- tool_use_id: toolu_01XMUZ7vzeQ4RBGRsqfhgQF4
- question: REQ-18: I assumed that when the original appointment has no reason for visit, the book appointment page opens with the reason field empty, because the ticket only says it is filled in from the original (L25-L26). Is that right?
- options: Yes, confirm | Reason always present
- answer: Yes, confirm

## A-04
- step: impact
- attempt: 1
- asked_at: 2026-11-16T06:51:59Z
- answered_by: eng-dffd
- tool_use_id: toolu_01AywHbsWFNYfcdTpmi1WsEQ
- question: Q-01: The appointment page needs the doctor's and patient's Active/Inactive status to disable "Book again". The pack has no payload facts for `GET /api/appointments/:id`, and `GET /api/patients/:id` is the patient's own page data (it may carry medical history). Where should the appointment page get the two statuses?
- options: Extend appointment GET (proposed default) | Call doctor and patient GETs | Leave it to design
- answer: Extend appointment GET (proposed default)

## A-05
- step: impact
- attempt: 2
- asked_at: 2026-11-16T06:58:11Z
- answered_by: eng-dffd
- tool_use_id: toolu_01GZLDEWVZTEtHqT9nWmbK8U
- question: Q-02: The pack has no call facts, and the product code isn't readable here, so I can't see who else reads the appointment detail shape (`DetailRow` in api/src/repositories/appointmentRepo.ts, `AppointmentDetail` in web/src/api/types.ts). Should the impact treat the action routes (cancel, reschedule, complete, no-show) as possible readers too?
- options: Yes, include actions (proposed default) | Detail GET only | Leave unknown
- answer: Yes, include actions (proposed default)

## A-06
- step: design
- attempt: 1
- asked_at: 2026-11-16T07:09:22Z
- answered_by: eng-dffd
- tool_use_id: toolu_01P9D2iZtoUyynHpKPQMbu7M
- question: Q-01: The appointment detail response (`GET /api/appointments/:id`) must now carry the doctor's and patient's status (A-04). The API uses snake_case (`page_size`), and the web app already has a `RecordStatus` type. Which field names and format?
- options: doctor_status/patient_status (proposed default) | Nested objects | Booleans
- answer: doctor_status/patient_status (proposed default)

## A-07
- step: design
- attempt: 1
- asked_at: 2026-11-16T07:09:22Z
- answered_by: eng-dffd
- tool_use_id: toolu_01P9D2iZtoUyynHpKPQMbu7M
- question: Q-02: "Book again" opens the book appointment page with doctor, patient and reason for visit in its address (it already takes doctor, patient, date and slot that way). What should the new reason parameter be called?
- options: reason (proposed default) | reason_for_visit
- answer: reason (proposed default)

## A-08
- step: design
- attempt: 1
- asked_at: 2026-11-16T07:09:22Z
- answered_by: eng-dffd
- tool_use_id: toolu_01P9D2iZtoUyynHpKPQMbu7M
- question: C5: I assumed the book appointment page already picks today's date in the hospital time zone (via `todayIn` in web/src/lib/format.ts) when the address has no date, so "Book again" can simply leave date and slot out. Implement must confirm in code. Is that right?
- options: Yes, confirm | Pass today explicitly
- answer: Yes, confirm
