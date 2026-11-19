# IDE answers: run_01M7SQ3HAGJ230D5FQ7P2H5AA4

Written by the tsl post-tool hook from AskUserQuestion. Do not edit: artifacts cite these
entries as the user's own answers.

## A-01
- step: req
- attempt: 1
- asked_at: 2026-11-19T11:06:42Z
- answered_by: eng-3c51
- tool_use_id: toolu_01Vrptxd7CLcKbQaDeeoufdH
- question: REQ-2: I assumed a doctor 'has availability today' when they have at least one availability block on today's weekday in hospital time (`Asia/Kolkata`), since the glossary defines availability as weekly blocks. Is that right?
- options: Yes, confirm | Only if slots remain
- answer: Yes, confirm

## A-02
- step: req
- attempt: 1
- asked_at: 2026-11-19T11:06:42Z
- answered_by: eng-3c51
- tool_use_id: toolu_01Vrptxd7CLcKbQaDeeoufdH
- question: Q-01: Booked slots must be counted 'the same way as the day schedule'. Which of today's appointments count as booked?
- options: Same as day schedule code (proposed default) | Scheduled only | Scheduled, Completed, No-show
- answer: Same as day schedule code (proposed default)

## A-03
- step: req
- attempt: 1
- asked_at: 2026-11-19T11:06:42Z
- answered_by: eng-3c51
- tool_use_id: toolu_01Vrptxd7CLcKbQaDeeoufdH
- question: REQ-7: I assumed a doctor whose remaining slots today have all passed or are booked is still listed, with 0 free and 'Fully booked', since passed slots are not free (AC2) and AC4 covers zero free slots. Is that right?
- options: Yes, confirm | Hide them
- answer: Yes, confirm

## A-04
- step: req
- attempt: 1
- asked_at: 2026-11-19T11:06:42Z
- answered_by: eng-3c51
- tool_use_id: toolu_01Vrptxd7CLcKbQaDeeoufdH
- question: REQ-8: I assumed the doctor's name links to the doctor page (/doctors/:id) with the day schedule set to today. Is that right?
- options: Yes, confirm | Link with explicit date
- answer: Yes, confirm

## A-05
- step: design
- attempt: 1
- asked_at: 2026-11-19T11:30:45Z
- answered_by: eng-3c51
- tool_use_id: toolu_01Q2z9cHEy6jeX9WTrT4rAiS
- question: Q-01: Where should the API return the 'On duty today' rows? The dashboard page already loads GET /api/dashboard (camelCase fields such as `appointmentsToday`).
- options: Add `onDuty` to dashboard (proposed default) | New GET /api/dashboard/on-duty
- answer: Add `onDuty` to dashboard (proposed default)

## A-06
- step: design
- attempt: 1
- asked_at: 2026-11-19T11:30:45Z
- answered_by: eng-3c51
- tool_use_id: toolu_01Q2z9cHEy6jeX9WTrT4rAiS
- question: Q-02: How should each on-duty row carry the doctor, department and next free slot? Day schedule slots already return both `start` (ISO UTC) and `label` (local HH:MM).
- options: Mirror slot shape (proposed default) | Label only | Instant only
- answer: Mirror slot shape (proposed default)
