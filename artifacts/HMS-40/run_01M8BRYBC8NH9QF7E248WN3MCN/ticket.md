---
schema: ticket-snapshot.v1
run_id: run_01M8BRYBC8NH9QF7E248WN3MCN
ticket: HMS-40
source: file
fetched_at: '2026-11-26T11:13:09Z'
title: Settings API returns the hospital's current day
issue_type: Story
source_sha256: aacc8b4bff172fc91dfa3c96239ddcfd902cac20a615817d383e1fcbd28dcf86
---
# HMS-40 Settings API returns the hospital's current day (invented, A2 code-loop sample run only)

As a web developer, I want the settings endpoint to tell me today's date in the hospital time zone,
so that pages that default to "today" use the hospital's day, not the browser's.

`GET /settings` (api/src/routes/health.ts) returns `timeZone`, `currency` and `pageSize` (PRD NFR-2).

Acceptance criteria:
1. `GET /settings` also returns `today`: the current day in the hospital time zone, as `YYYY-MM-DD`,
   computed with the existing day helper in `api/src/lib/time.ts`.
2. Just after midnight in the hospital time zone, `today` is the new day, even when it is still the
   previous day in UTC (and the other way round).
3. The existing fields and `GET /health` are unchanged.
