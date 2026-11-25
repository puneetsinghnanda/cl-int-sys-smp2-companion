# IDE answers: run_01M89BPMN0XE2ZPMFG17PNG5CP

Written by the tsl post-tool hook from AskUserQuestion. Do not edit: artifacts cite these
entries as the user's own answers.

## A-01
- step: req
- attempt: 1
- asked_at: 2026-11-25T12:44:28Z
- answered_at: 2026-11-25T12:50:46Z
- answered_by: eng-dffd
- tool_use_id: toolu_018zQj9U7nbYFiz6qzaDXm4h
- question: Q-01: The ticket says "last visit" but the glossary doesn't define a visit. Appointments can be SCHEDULED, COMPLETED, CANCELLED or NO_SHOW. Which appointment counts as the patient's last visit?
- options: Latest Completed (proposed default) | Completed or No-show | Any past, not cancelled
- answer: Latest Completed (proposed default)

## A-02
- step: req
- attempt: 1
- asked_at: 2026-11-25T12:44:28Z
- answered_at: 2026-11-25T12:50:46Z
- answered_by: eng-dffd
- tool_use_id: toolu_018zQj9U7nbYFiz6qzaDXm4h
- question: Q-02: Should patients who have never visited (shown as "Never") be included when the "Not seen for a year" filter is on?
- options: No, exclude (proposed default) | Yes, include them
- answer: No, exclude (proposed default)

## A-03
- step: req
- attempt: 1
- asked_at: 2026-11-25T12:44:28Z
- answered_at: 2026-11-25T12:50:46Z
- answered_by: eng-dffd
- tool_use_id: toolu_018zQj9U7nbYFiz6qzaDXm4h
- question: Q-03: How should "more than 12 months ago" be measured?
- options: 12 calendar months, IST (proposed default) | 365 days
- answer: 12 calendar months, IST (proposed default)

## A-04
- step: req
- attempt: 1
- asked_at: 2026-11-25T12:44:28Z
- answered_at: 2026-11-25T12:50:46Z
- answered_by: eng-dffd
- tool_use_id: toolu_018zQj9U7nbYFiz6qzaDXm4h
- question: REQ-3: I assumed the "Last visit" column shows only the date (no time), as a hospital-local date in Asia/Kolkata, because the baseline sets the hospital time zone to Asia/Kolkata. Is that right?
- options: Yes, confirm | Date and time
- answer: Yes, confirm

## A-05
- step: design
- attempt: 1
- asked_at: 2026-11-25T13:03:12Z
- answered_at: 2026-11-25T13:10:56Z
- answered_by: eng-dffd
- tool_use_id: toolu_018r9sbvHT9tvxopaYC7KBsP
- question: Q-01: The stories test the last-visit date and doctor on each patient row. What shape should GET /api/patients return for it?
- options: Nested object (proposed default) | Two flat fields
- answer: Nested object (proposed default)

## A-06
- step: design
- attempt: 1
- asked_at: 2026-11-25T13:03:12Z
- answered_at: 2026-11-25T13:10:56Z
- answered_by: eng-dffd
- tool_use_id: toolu_018r9sbvHT9tvxopaYC7KBsP
- question: Q-02: What query parameter should turn on the "Not seen for a year" filter on GET /api/patients?
- options: not_seen=true (proposed default) | last_visit_before=YYYY-MM-DD
- answer: not_seen=true (proposed default)

## A-07
- step: design
- attempt: 1
- asked_at: 2026-11-25T13:03:12Z
- answered_at: 2026-11-25T13:10:56Z
- answered_by: eng-dffd
- tool_use_id: toolu_018r9sbvHT9tvxopaYC7KBsP
- question: Q-03: How should the web page display the last-visit date? Note that the existing dateTime helper prints a time as well, so it doesn't fit a date-only value.
- options: New date-only helper (proposed default) | Show YYYY-MM-DD as is
- answer: New date-only helper (proposed default)

## A-08
- step: design
- attempt: 1
- asked_at: 2026-11-25T13:03:12Z
- answered_at: 2026-11-25T13:10:56Z
- answered_by: eng-dffd
- tool_use_id: toolu_018r9sbvHT9tvxopaYC7KBsP
- question: C14: I assumed the web patient list request goes through `api` in web/src/api/client.ts, which builds query strings with `query`, so the new filter flag is added there. Is that right? (implement must confirm the exact method)
- options: Yes, confirm | PatientsPage calls it
- answer: Yes, confirm
