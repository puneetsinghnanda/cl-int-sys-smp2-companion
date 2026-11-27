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

## A-06
- step: req
- attempt: 2
- asked_at: 2026-11-27T10:17:17Z
- answered_by: eng-dffd
- tool_use_id: toolu_01Hgu8F6PtSXiWrdSB1oFWUT
- question: Q-06: When staff reschedule a Stale appointment to a new time, which state does it end in? Booking overlap rules only count Scheduled appointments.
- options: Scheduled again (proposed default) | Stays Stale
- answer: Scheduled again (proposed default)

## A-07
- step: req
- attempt: 2
- asked_at: 2026-11-27T10:17:17Z
- answered_by: eng-dffd
- tool_use_id: toolu_01Hgu8F6PtSXiWrdSB1oFWUT
- question: Q-07: The glossary defines Cancelled as called off before it took place. Can staff mark a Stale (past) appointment Cancelled?
- options: Yes, Cancelled allowed (proposed default) | No, only Completed or No-show
- answer: Yes, Cancelled allowed (proposed default)

## A-08
- step: design
- attempt: 1
- asked_at: 2026-11-27T10:25:53Z
- answered_by: eng-dffd
- tool_use_id: toolu_01Hzhd4jRpy3GbEqBwMBXBMw
- question: Q-01: No background jobs exist yet. How should the API mark Scheduled appointments Stale once 24h after start has passed?
- options: Sweep on read (proposed default) | In-process timer | Both
- answer: In-process timer

## A-09
- step: design
- attempt: 1
- asked_at: 2026-11-27T10:27:38Z
- answered_by: eng-dffd
- tool_use_id: toolu_01YNGXQTxFtcijgTZt32JKtJ
- question: Q-02: How often should the in-process timer run the Stale sweep? An appointment can stay Scheduled for up to this long after the 24h mark.
- options: Every 5 minutes (proposed default) | Every 1 minute | Every 15 minutes
- answer: Every hour

## A-10
- step: design
- attempt: 2
- asked_at: 2026-11-27T10:34:41Z
- answered_by: eng-dffd
- tool_use_id: toolu_01FoPZ9m3WnGA4KocP4K9EYd
- question: C21: I assumed the Stale glossary entry goes in docs/PRD.md, but the PRD has no glossary section; the glossary is the companion's context/glossary.md, whose Scheduled entry ("Only Scheduled appointments can change") also goes stale. Where should the change go?
- options: Glossary + PRD status | Glossary only | PRD status only
- answer: Glossary only
