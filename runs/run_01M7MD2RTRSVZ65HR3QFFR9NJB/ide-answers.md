# IDE answers: run_01M7MD2RTRSVZ65HR3QFFR9NJB

Written by the tsl post-tool hook from AskUserQuestion. Do not edit: artifacts cite these
entries as the user's own answers.

## A-01
- step: impact
- attempt: 1
- asked_at: 2026-11-17T10:05:44Z
- answered_by: eng-dbe4
- tool_use_id: toolu_01Km2QjbPxACUEc1aYYLfU4V
- question: Q-01: The Awaiting outcome tile must link to an appointment list showing exactly the Scheduled appointments whose start time is before now (REQ-6). Today GET /api/appointments filters only by status and whole hospital-local days (from/to), so status=SCHEDULED&to=today would also show this afternoon's not-yet-started appointments. How should the list express "started before now"?
- options: New filter on GET /api/appointments (proposed default) | Reuse status + to=yesterday | Reuse status + to=today
- answer: New filter on GET /api/appointments (proposed default)

## A-02
- step: design
- attempt: 1
- asked_at: 2026-11-17T10:15:01Z
- answered_by: eng-dbe4
- tool_use_id: toolu_01PLsqUdd9ZMruexCkBDR54v
- question: Q-01: GET /api/dashboard totals will gain the Awaiting outcome count (impact). The other totals are camelCase numbers (appointmentsToday, upcoming, noShows). What should the new totals field be called?
- options: awaitingOutcome (proposed default) | pastScheduled | overdue
- answer: awaitingOutcome (proposed default)

## A-03
- step: design
- attempt: 1
- asked_at: 2026-11-17T10:15:01Z
- answered_by: eng-dbe4
- tool_use_id: toolu_01PLsqUdd9ZMruexCkBDR54v
- question: Q-02: GET /api/appointments gains a filter for Scheduled appointments that started before now (A-01). How should the tile's link express it?
- options: started=true, server now (proposed default) | before=<ISO instant>
- answer: started=true, server now (proposed default)
