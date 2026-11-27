# IDE answers: run_01M8E7Q99RKS5X0BN20RNDJM8C

Written by the tsl post-tool hook from AskUserQuestion. Do not edit: artifacts cite these
entries as the user's own answers.

## A-01
- step: req
- attempt: 1
- asked_at: 2026-11-27T10:13:09Z
- answered_by: eng-dffd
- tool_use_id: toolu_01VA9w2op38iyZUdyxets9a7
- question: Q-01: HMS-34 marks a past Scheduled appointment Stale if it is not Completed or rescheduled "within one day". One day from when, and how long?
- options: 24h after start (proposed default) | 24h after end | End of next local day
- answer: 24h after start (proposed default)

## A-02
- step: req
- attempt: 1
- asked_at: 2026-11-27T10:13:09Z
- answered_by: eng-dffd
- tool_use_id: toolu_01VA9w2op38iyZUdyxets9a7
- question: Q-02: Once an appointment is Stale, can staff still change it? The baseline says Completed, Cancelled and No-show never change, and only Scheduled appointments can change.
- options: Stale never changes (proposed default) | Can still be closed | Can be closed or rescheduled
- answer: Can be closed or rescheduled

## A-03
- step: req
- attempt: 1
- asked_at: 2026-11-27T10:13:09Z
- answered_by: eng-dffd
- tool_use_id: toolu_01VA9w2op38iyZUdyxets9a7
- question: Q-03: When an appointment becomes Stale, should the change be written to appointment_event, and which actor goes on it? Today the actor is always `staff`.
- options: Event, actor staff (proposed default) | Event, actor system | No event
- answer: Event, actor system

## A-04
- step: req
- attempt: 1
- asked_at: 2026-11-27T10:13:09Z
- answered_by: eng-dffd
- tool_use_id: toolu_01VA9w2op38iyZUdyxets9a7
- question: Q-04: Where should staff see the Stale state?
- options: Lists and page, with filter (proposed default) | Lists and page only | Also a dashboard tile
- answer: Lists and page, with filter (proposed default)

## A-05
- step: req
- attempt: 1
- asked_at: 2026-11-27T10:13:46Z
- answered_by: eng-dffd
- tool_use_id: toolu_01SLGoopNzsFJRfvQK2o2CoZ
- question: Q-05: "Stale" is not in the glossary. Should the glossary gain an entry for it?
- options: Add entry (proposed default) | No glossary entry
- answer: Add entry (proposed default)
