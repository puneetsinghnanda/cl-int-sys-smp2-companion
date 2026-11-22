# IDE answers: run_01M81VYRN04SNXNMA559N6SS01

Written by the tsl post-tool hook from AskUserQuestion. Do not edit: artifacts cite these
entries as the user's own answers.

## A-01
- step: req
- attempt: 1
- asked_at: 2026-11-22T15:10:36Z
- answered_by: eng-a037
- tool_use_id: toolu_01PRvVxDjTJtht8b7MmXN2eQ
- question: REQ-7: I assumed each status count links to the same appointment list with only the Status filter changed to that status: the doctor/patient and date-range filters are kept and paging goes back to page 1 (the ticket's AC5 says 'the same list with the Status filter set to that status'). Is that right?
- options: Yes, confirm | Keep current page too
- answer: Yes, confirm

## A-02
- step: req
- attempt: 1
- asked_at: 2026-11-22T15:10:36Z
- answered_by: eng-a037
- tool_use_id: toolu_01PRvVxDjTJtht8b7MmXN2eQ
- question: Q-01: 'Appointment list' (ticket line 17, PRD APT-9) is not in the glossary. Should the requirements treat it as the ticket defines it: the list for one doctor or one patient, filtered by date range and status, 20 per page, ordered by start time?
- options: Use ticket definition | Ask tech lead
- answer: Use ticket definition

## A-03
- step: design
- attempt: 1
- asked_at: 2026-11-22T15:25:58Z
- answered_by: eng-a037
- tool_use_id: toolu_01SzeWG5QuS8w3L8u51e9r7B
- question: Q-01: The stories need per-status counts for the doctor/patient/date-range filter, ignoring Status. How should the API return them? (Statuses are SCHEDULED, COMPLETED, CANCELLED, NO_SHOW per the data model.)
- options: status_counts on list | New counts endpoint
- answer: status_counts on list

## A-04
- step: design
- attempt: 1
- asked_at: 2026-11-22T15:25:58Z
- answered_by: eng-a037
- tool_use_id: toolu_01SzeWG5QuS8w3L8u51e9r7B
- question: C3: I assumed `AppointmentFilter` (api/src/repositories/appointmentRepo.ts:39) holds the doctor, patient, date-range and status filters, so the counts query can reuse it with status removed. Is that right?
- options: Yes, confirm | Not sure, implement checks
- answer: Yes, confirm

## A-05
- step: design
- attempt: 1
- asked_at: 2026-11-22T15:25:58Z
- answered_by: eng-a037
- tool_use_id: toolu_01SzeWG5QuS8w3L8u51e9r7B
- question: C4: I assumed AppointmentsPage (web/src/pages/AppointmentsPage.tsx) keeps its filters in the URL query string, so each count can be a link to the same page with `status` set and `page` reset to 1. Is that right?
- options: Yes, confirm | Filters are local state | Not sure, implement checks
- answer: Yes, confirm
