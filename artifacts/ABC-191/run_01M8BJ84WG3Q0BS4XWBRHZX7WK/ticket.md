---
schema: ticket-snapshot.v1
run_id: run_01M8BJ84WG3Q0BS4XWBRHZX7WK
ticket: ABC-191
source: file
fetched_at: '2026-11-26T09:16:10Z'
title: 'ABC-191: lastDays accepts a day count below 1 and returns an inverted window
  (invented, A3 validation only)'
source_sha256: 43668afdcc1f13921815f3153ab2e9b947bc2a5972a6a6313d5fcf31fc927d6e
---
# ABC-191: lastDays accepts a day count below 1 and returns an inverted window (invented, A3 validation only)

`lastDays(now, tz, days)` in `api/src/lib/periods.ts` is the shared period rule for the dashboard and the doctor page.

**Observed:** called with `days = 0` (or a negative number) it returns a window whose `from` day is after its `to` day, and `start` is after `end`. Queries over that window silently count nothing.

**Expected:** a day count that is not a whole number of at least 1 is rejected with a `RangeError` that names the value, so a wrong caller fails loudly instead of showing 0.

**Scope:** `api/src/lib/periods.ts` only. Today's callers (dashboard periods, the doctor page's 30 days) pass valid counts and must behave exactly as before.
