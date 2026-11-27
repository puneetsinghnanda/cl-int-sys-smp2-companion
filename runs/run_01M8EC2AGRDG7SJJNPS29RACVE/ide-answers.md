# IDE answers: run_01M8EC2AGRDG7SJJNPS29RACVE

Written by the tsl post-tool hook from AskUserQuestion. Do not edit: artifacts cite these
entries as the user's own answers.

## A-01
- step: req
- attempt: 1
- asked_at: 2026-11-27T11:27:17Z
- answered_at: 2026-11-27T11:28:20Z
- answered_by: eng-dffd
- tool_use_id: toolu_016e4Vbr92AdQT5Qg2tUZSYx
- question: Q-01: How should appointments become Stale? The pack says no background jobs exist yet and that clean-ups would be the first one.
- options: Periodic background job (proposed default) | Derived on read | Marked when lists are read
- answer: Periodic background job (proposed default)

## A-02
- step: req
- attempt: 1
- asked_at: 2026-11-27T11:27:17Z
- answered_at: 2026-11-27T11:28:20Z
- answered_by: eng-dffd
- tool_use_id: toolu_016e4Vbr92AdQT5Qg2tUZSYx
- question: Q-02: When does the 'within one day' window end for a past Scheduled appointment?
- options: 24 hours after starts_at (proposed default) | 24 hours after ends_at | End of next hospital day
- answer: 24 hours after starts_at (proposed default)

## A-03
- step: req
- attempt: 1
- asked_at: 2026-11-27T11:27:17Z
- answered_at: 2026-11-27T11:28:20Z
- answered_by: eng-dffd
- tool_use_id: toolu_016e4Vbr92AdQT5Qg2tUZSYx
- question: Q-03: Once an appointment is Stale, can staff still change it (complete, no-show, cancel, reschedule)?
- options: No, Stale never changes (proposed default) | Yes, can still be closed | Yes, can be rescheduled
- answer: Yes, can be rescheduled

## A-04
- step: req
- attempt: 1
- asked_at: 2026-11-27T11:27:17Z
- answered_at: 2026-11-27T11:28:20Z
- answered_by: eng-dffd
- tool_use_id: toolu_016e4Vbr92AdQT5Qg2tUZSYx
- question: Q-04: Which actor should the appointment history record when an appointment is marked Stale?
- options: staff (proposed default) | system
- answer: system

## A-05
- step: req
- attempt: 1
- asked_at: 2026-11-27T11:28:32Z
- answered_at: 2026-11-27T11:29:18Z
- answered_by: eng-dffd
- tool_use_id: toolu_01HM86fx7soQdmgP8ktSpyeT
- question: REQ-7: From your Q-03 answer, I assumed a Stale appointment can only be rescheduled (back to Scheduled), and cannot be marked Completed, No-show or Cancelled. Is that right?
- options: Yes, confirm | Also allow Cancel | Allow all closing actions
- answer: Allow all closing actions

## A-06
- step: req
- attempt: 1
- asked_at: 2026-11-27T11:28:32Z
- answered_at: 2026-11-27T11:29:18Z
- answered_by: eng-dffd
- tool_use_id: toolu_01HM86fx7soQdmgP8ktSpyeT
- question: REQ-9: I assumed the web app shows the status label 'Stale' wherever an appointment's status is shown today (lists, appointment page), because the ticket wants it not to look Scheduled. Is that right?
- options: Yes, confirm | Also add a list filter
- answer: Also add a list filter

## A-07
- step: req
- attempt: 1
- asked_at: 2026-11-27T11:28:32Z
- answered_at: 2026-11-27T11:29:18Z
- answered_by: eng-dffd
- tool_use_id: toolu_01HM86fx7soQdmgP8ktSpyeT
- question: REQ-10: I assumed Stale appointments never appear among the dashboard's next upcoming appointments. Is that right?
- options: Yes, confirm | Show them flagged
- answer: Yes, confirm
