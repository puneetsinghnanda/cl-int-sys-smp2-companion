# IDE answers: run_01M7T1CAT0DDAGAWYAP7TPJ3YB

Written by the tsl post-tool hook from AskUserQuestion. Do not edit: artifacts cite these
entries as the user's own answers.

## A-01
- step: req
- attempt: 1
- asked_at: 2026-11-19T13:56:36Z
- answered_by: eng-3fd6
- tool_use_id: toolu_019FdR7XktrtfVX7HVkiwoFZ
- question: Q-01: The ticket says "Last visit:" shows the start date "in the same date format as the appointment table", but the table uses `dateTime` in web/src/lib/format.ts, which prints the date AND the time (e.g. "05 Nov 2026, 10:30"). What should Last visit show?
- options: Date only (proposed default) | Date and time
- answer: Date only (proposed default)

## A-02
- step: req
- attempt: 1
- asked_at: 2026-11-19T13:56:36Z
- answered_by: eng-3fd6
- tool_use_id: toolu_019FdR7XktrtfVX7HVkiwoFZ
- question: REQ-7: I assumed all four counts are always shown, even when a count is 0 (e.g. "3 completed · 0 no-show · 0 cancelled · 1 upcoming"), using the fixed labels from the ticket's example. Is that right?
- options: Yes, confirm | Hide zero counts
- answer: Yes, confirm

## A-03
- step: req
- attempt: 1
- asked_at: 2026-11-19T13:56:36Z
- answered_by: eng-3fd6
- tool_use_id: toolu_019FdR7XktrtfVX7HVkiwoFZ
- question: REQ-9: The ticket says only the date links to the appointment, so I assumed the doctor's name in "Last visit:" is plain text (not a link to the doctor page). Is that right?
- options: Yes, confirm | Link doctor name too
- answer: Yes, confirm

## A-04
- step: design
- attempt: 1
- asked_at: 2026-11-19T14:20:16Z
- answered_by: eng-3fd6
- tool_use_id: toolu_01Uqhpni75nPmJ5f3Ski3ghJ
- question: Q-01: GET /api/patients/:id will return a new visit-summary object (four counts + last visit). PatientDetail's own fields are camelCase (medicalHistory, emergencyContact), but HMS-1 added next_appointment in snake_case on the list. Which name/case?
- options: visitSummary camelCase (proposed default) | visit_summary snake_case
- answer: visitSummary camelCase (proposed default)

## A-05
- step: design
- attempt: 1
- asked_at: 2026-11-19T14:20:16Z
- answered_by: eng-3fd6
- tool_use_id: toolu_01Uqhpni75nPmJ5f3Ski3ghJ
- question: Q-02: How should the API send the last visit's date? The UI shows date only in hospital time (A-01), and the 01:00-local-vs-UTC-previous-day test (S-3 AC2) must be checkable.
- options: Local YYYY-MM-DD (proposed default) | UTC instant startsAt
- answer: Local YYYY-MM-DD (proposed default)

## A-06
- step: design
- attempt: 1
- asked_at: 2026-11-19T14:20:16Z
- answered_by: eng-3fd6
- tool_use_id: toolu_01Uqhpni75nPmJ5f3Ski3ghJ
- question: C9: I assumed the new summary UI lives in a new component file web/src/components/VisitSummary.tsx (next to AppointmentTable.tsx) rendered by PatientDetailPage, rather than inline in the page. Is that right?
- options: Yes, confirm | Inline in the page
- answer: Yes, confirm
