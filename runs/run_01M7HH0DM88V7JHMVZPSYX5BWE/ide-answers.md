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
