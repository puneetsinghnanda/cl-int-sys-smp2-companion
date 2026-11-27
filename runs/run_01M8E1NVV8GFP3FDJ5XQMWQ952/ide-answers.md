# IDE answers: run_01M8E1NVV8GFP3FDJ5XQMWQ952

Written by the tsl post-tool hook from AskUserQuestion. Do not edit: artifacts cite these
entries as the user's own answers.

## A-01
- step: req
- attempt: 1
- asked_at: 2026-11-27T08:29:19Z
- answered_by: eng-dffd
- tool_use_id: toolu_01MjoPtybsKeHHKtZR7Y5qnE
- question: Q-01: The ticket says "within one day" (HMS-34 L13, L17-L20) but not one day from what. When does a past Scheduled appointment become Stale?
- options: 24h after start time (proposed default) | 24h after end time | End of next hospital day
- answer: 24h after start time (proposed default)

## A-02
- step: req
- attempt: 1
- asked_at: 2026-11-27T08:29:19Z
- answered_by: eng-dffd
- tool_use_id: toolu_01MjoPtybsKeHHKtZR7Y5qnE
- question: Q-02: The system has no background jobs yet (architecture open points). How should appointments get marked Stale?
- options: Scheduled background job (proposed default) | Lazily on read | Derived, not stored
- answer: Scheduled background job (proposed default)

## A-03
- step: req
- attempt: 1
- asked_at: 2026-11-27T08:29:19Z
- answered_by: eng-dffd
- tool_use_id: toolu_01MjoPtybsKeHHKtZR7Y5qnE
- question: Q-03: The baseline says only Scheduled appointments can change and closed ones never change. Can a Stale appointment still be changed by staff?
- options: Yes: complete, no-show, cancel, reschedule (proposed default) | No, Stale is closed | Only Completed or No-show
- answer: Yes: complete, no-show, cancel, reschedule (proposed default)

## A-04
- step: req
- attempt: 1
- asked_at: 2026-11-27T08:29:19Z
- answered_by: eng-dffd
- tool_use_id: toolu_01MjoPtybsKeHHKtZR7Y5qnE
- question: REQ-8: I assumed marking an appointment Stale is recorded in appointment_event like every other change, with actor `staff` (the only MVP actor). Is that right?
- options: Yes, confirm | Yes, but actor `system` | No event
- answer: Yes, but actor `system`

## A-05
- step: req
- attempt: 1
- asked_at: 2026-11-27T08:29:52Z
- answered_by: eng-dffd
- tool_use_id: toolu_013cHsSYxWG2LpN78DbUjonf
- question: Q-04: "Stale" is not in the glossary. Should it be added with this meaning: "A Scheduled appointment whose start time passed 24 hours ago without being marked Completed or rescheduled; it stays open"?
- options: Yes, add that entry (proposed default) | No glossary entry
- answer: Yes, add that entry (proposed default)
