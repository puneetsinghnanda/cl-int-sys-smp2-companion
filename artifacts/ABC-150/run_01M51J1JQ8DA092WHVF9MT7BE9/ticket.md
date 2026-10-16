---
schema: ticket-snapshot.v1
run_id: run_01M51J1JQ8DA092WHVF9MT7BE9
ticket: ABC-150
source: file
fetched_at: '2026-10-16T05:13:05Z'
title: Orders placed just before midnight are missing from history
issue_type: Bug
source_sha256: 610bce0f8948d61b88ace55df4cdfc42edaf6326b37b42a7f62eb695599a5369
---
# ABC-150 Orders placed just before midnight are missing from history

Steps to reproduce:
1. Place an order at 23:59:30 UTC.
2. Open order history with from and to set to that same day.

Expected: the order is listed for that day.
Actual: the order is not listed. It does not appear on the next day either.

Environment: staging, API server time zone UTC.
Reported by: support agent sup-2c91.
Frequency: a few orders every day, always placed in the last minute before midnight.
