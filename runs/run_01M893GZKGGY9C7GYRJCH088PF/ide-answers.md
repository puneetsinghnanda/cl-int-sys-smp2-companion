# IDE answers: run_01M893GZKGGY9C7GYRJCH088PF

Written by the tsl post-tool hook from AskUserQuestion. Do not edit: artifacts cite these
entries as the user's own answers.

## A-01
- step: req
- attempt: 1
- asked_at: 2026-11-25T10:22:00Z
- answered_by: eng-dbe4
- tool_use_id: toolu_01XMAi6TMpJaL7E4KbeX3yZZ
- question: Q-01: The ticket says a past Scheduled appointment "is marked Stale" but not who or what marks it. The system has no background jobs yet (architecture#open-points). How should it become Stale?
- options: Automatic job (proposed default) | Derived when read | Staff mark it manually
- answer: Automatic job (proposed default)

## A-02
- step: req
- attempt: 1
- asked_at: 2026-11-25T10:22:00Z
- answered_by: eng-dbe4
- tool_use_id: toolu_01XMAi6TMpJaL7E4KbeX3yZZ
- question: Q-02: The ticket says "within one day" but not when that day starts. When is the one day counted from?
- options: 24h after start time (proposed default) | 24h after end time | End of next local day
- answer: 24h after start time (proposed default)

## A-03
- step: req
- attempt: 1
- asked_at: 2026-11-25T10:22:00Z
- answered_by: eng-dbe4
- tool_use_id: toolu_01XMAi6TMpJaL7E4KbeX3yZZ
- question: Q-03: Can a Stale appointment change afterwards? Today Completed, Cancelled and No-show never change (intent-baseline#closed-appointments) and only Scheduled appointments can change (glossary#scheduled).
- options: Staff can still close it (proposed default) | Final, never changes | Only mark Completed/No-show
- answer: Staff can still close it (proposed default)

## A-04
- step: req
- attempt: 1
- asked_at: 2026-11-25T10:22:00Z
- answered_by: eng-dbe4
- tool_use_id: toolu_01XMAi6TMpJaL7E4KbeX3yZZ
- question: Q-04: Where should the Stale state show in the web app?
- options: Wherever status shows (proposed default) | Also a dashboard tile | Status label only
- answer: Wherever status shows (proposed default)

## A-05
- step: req
- attempt: 1
- asked_at: 2026-11-25T10:22:24Z
- answered_by: eng-dbe4
- tool_use_id: toolu_01Gxebjyeq3SWRnd3gipwSAS
- question: REQ-6: I assumed an appointment that is cancelled or marked No-show within the one day is not marked Stale, because the ticket's rule applies only to Scheduled appointments (ticket#L17) and a cancelled or No-show appointment is no longer Scheduled (glossary#scheduled). Is that right?
- options: Yes, confirm | No-show still becomes Stale
- answer: Yes, confirm

## A-06
- step: req
- attempt: 1
- asked_at: 2026-11-25T10:23:06Z
- answered_by: eng-dbe4
- tool_use_id: toolu_01KHKNH5DdYR3gN6BTDGtjYy
- question: Q-05: "Stale" is not in the glossary (context/glossary.md). Should the glossary get an entry for it? (Non-blocking; the req step only records your choice, it does not edit the glossary.)
- options: Add entry from REQ-2 (proposed default) | No glossary entry
- answer: Add entry from REQ-2 (proposed default)

## A-07
- step: req
- attempt: 2
- asked_at: 2026-11-25T10:29:39Z
- answered_by: eng-dbe4
- tool_use_id: toolu_01GdTwX77C8zi2TCiVbSsN86
- question: REQ-10: I assumed a Stale appointment that staff reschedule becomes Scheduled again at its new time, because you said Stale is only a state to flag that the appointment needs attention (open-questions Q-01). Is that right?
- options: Yes, confirm | Stays Stale until closed
- answer: Yes, confirm

## A-08
- step: design
- attempt: 1
- asked_at: 2026-11-25T10:46:16Z
- answered_by: eng-dbe4
- tool_use_id: toolu_01EDsFtZ9m8Hxrf9XF17ythW
- question: Q-01: The Stale job runs inside the API process on a timer (no new infrastructure; architecture says no background jobs exist yet). How often should it run? This sets how soon after the 24h an appointment shows as Stale.
- options: Every 15 minutes (proposed default) | Every hour | Every 5 minutes
- answer: Every hour

## A-09
- step: design
- attempt: 1
- asked_at: 2026-11-25T10:46:16Z
- answered_by: eng-dbe4
- tool_use_id: toolu_01EDsFtZ9m8Hxrf9XF17ythW
- question: Q-02: Every appointment change records an actor (updated_by and the history event's actor); today it is always `staff`. What actor should the automatic Stale job record?
- options: system (proposed default) | staff
- answer: system (proposed default)

## A-10
- step: design
- attempt: 1
- asked_at: 2026-11-25T10:46:16Z
- answered_by: eng-dbe4
- tool_use_id: toolu_01EDsFtZ9m8Hxrf9XF17ythW
- question: C9: The status and event CHECK constraints in the appointments migration have no names, so the new migration must drop them by Postgres's default names. I assumed those are `appointment_status_check` and `appointment_event_event_check` (implement must confirm). Is that right?
- options: Yes, confirm | Look them up at migrate time
- answer: Yes, confirm
