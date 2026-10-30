---
schema: ticket-snapshot.v1
run_id: run_01M661QPHGQW4076F0GCRQ70K1
ticket: LIVE-1
source: file
fetched_at: '2026-10-30T09:19:58Z'
title: Show how many patients match on the patient list
issue_type: Story
source_sha256: dbd6dd03cdf963fa04ffb334c85e852653b7f8d8287f80845fa2ebbd62ef13f5
---
# LIVE-1 Show how many patients match on the patient list

As front-desk staff, I want to see how many patients the patient list is showing, so that I know
whether my search found one patient or many before I scroll.

The patient list already supports search and sort (PRD PAT-2).

## Acceptance criteria
- The patient list shows the number of patients that match the current search, for example
  "12 patients".
- The number updates when the search text changes.
- With no match, the list says "0 patients" and keeps the existing empty message.
