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

## A-06
- step: req
- attempt: 2
- asked_at: 2026-11-27T08:33:03Z
- answered_by: eng-dffd
- tool_use_id: toolu_01Y7mnK4EFo7kLYwAKMz64DW
- question: Q-05: When staff reschedule a Stale appointment to a new future time, what state does it end in?
- options: Scheduled again (proposed default) | Stays Stale
- answer: Scheduled again (proposed default)

## A-07
- step: design
- attempt: 1
- asked_at: 2026-11-27T08:42:20Z
- answered_by: eng-dffd
- tool_use_id: toolu_01G9gdmG5W11iBdjX5nTXc8R
- question: Q-01: The API's appointment status values are upper case (SCHEDULED, COMPLETED, CANCELLED, NO_SHOW). What value should the new state and its history event use?
- options: STALE for both (proposed default) | STALE status, MARKED_STALE event
- answer: STALE for both (proposed default)

## A-08
- step: design
- attempt: 1
- asked_at: 2026-11-27T08:42:20Z
- answered_by: eng-dffd
- tool_use_id: toolu_01G9gdmG5W11iBdjX5nTXc8R
- question: Q-02: How often should the background job that marks appointments Stale run?
- options: Every 15 minutes (proposed default) | Every 5 minutes | Every hour
- answer: Every hour

## A-09
- step: design
- attempt: 1
- asked_at: 2026-11-27T08:42:20Z
- answered_by: eng-dffd
- tool_use_id: toolu_01G9gdmG5W11iBdjX5nTXc8R
- question: Q-03: Where should the job run? The system has one Render web service and no worker or cron service today.
- options: Timer inside the API process (proposed default) | Separate Render cron job
- answer: Timer inside the API process (proposed default)

## A-10
- step: design
- attempt: 1
- asked_at: 2026-11-27T08:42:20Z
- answered_by: eng-dffd
- tool_use_id: toolu_01G9gdmG5W11iBdjX5nTXc8R
- question: C9: I assumed the web status labels (STATUS_LABEL) are in web/src/lib/format.ts, because StatusBadge imports them from there in the local checkout; the generated module map does not list that symbol. Is that right?
- options: Yes, confirm | Not sure; implement must confirm
- answer: Yes, confirm
