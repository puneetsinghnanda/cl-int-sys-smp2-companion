# IDE answers: run_01M7Q1DS00W5S7BFVX9Y70NPZG

Written by the tsl post-tool hook from AskUserQuestion. Do not edit: artifacts cite these
entries as the user's own answers.

## A-01
- step: impact
- attempt: 1
- asked_at: 2026-11-18T10:52:31Z
- answered_by: eng-dbe4
- tool_use_id: toolu_01VjjuWDwX4c5X6btkC9acCr
- question: Q-01: The Awaiting outcome tile must link to an appointment list holding exactly the Scheduled appointments whose start time is before now (REQ-8). The list's existing filters (AppointmentFilter) work on status and dates, so a date filter ending today would also show appointments later today that haven't started. How should the list express "started before now"?
- options: New filter, server now (proposed default) | Client sends an instant | Reuse the date filters
- answer: New filter, server now (proposed default)

## A-02
- step: design
- attempt: 1
- asked_at: 2026-11-18T11:02:04Z
- answered_by: eng-dbe4
- tool_use_id: toolu_01Ay2NK42uzqUQ2ArQ8T1rUd
- question: Q-01: GET /api/dashboard totals will gain the Awaiting outcome count. The other totals are camelCase numbers (appointmentsToday, upcoming, noShows). What should the new totals field be called?
- options: awaitingOutcome (proposed default) | startedScheduled | overdue
- answer: awaitingOutcome (proposed default)

## A-03
- step: design
- attempt: 1
- asked_at: 2026-11-18T11:02:04Z
- answered_by: eng-dbe4
- tool_use_id: toolu_01Ay2NK42uzqUQ2ArQ8T1rUd
- question: Q-02: GET /api/appointments gains an optional filter for appointments that started before the server's now (A-01). How should the query parameter be named and valued, which the tile link will also carry?
- options: started=true (proposed default) | startedBefore=now | awaiting=true
- answer: started=true (proposed default)
