---
schema: ticket-snapshot.v1
run_id: run_01M51PD7F8SXTJQX3M2VGQ085G
ticket: ABC-175
source: file
fetched_at: '2026-10-16T06:29:21Z'
title: Store order notes
issue_type: Story
source_sha256: d8e98784b48d03d4be31ede4def98f98a66204483e4a3d2f16b07f61b0cdd3ad
---
# ABC-175 Store order notes

As a support agent I want to add a short note to an order, so that the next agent sees
what was agreed with the customer.

Acceptance:
- A note is plain text of at most 500 characters.
- An order can have many notes; the order page lists them newest first.
- Notes are kept as long as the order is kept.
- Notes are stored in a new table, so this change needs a database migration.
