---
schema: ut-result.v1
run_id: run_01M7MD2RTRSVZ65HR3QFFR9NJB
step: ut
attempt: 1
skill: unit-test@1.2.0
inputs:
  - {path: artifacts/HMS-21/run_01M7MD2RTRSVZ65HR3QFFR9NJB/impl.md, sha256: 603d12bce385e4b5a6b74b10ce66a08c29fab4a69f57d6b55fba5d9ab524da1a}
  - {path: artifacts/HMS-21/run_01M7MD2RTRSVZ65HR3QFFR9NJB/split.md, sha256: dcc06d1291e79f057d47f662fdc9b9f93994a0b3c695173acb9df34f900df06e}
  - {path: context/test-strategy.md, sha256: f4bbc9d5d63dfc433c9fe466a7aa71a4fad0147baafe1f47db0c06e117ce6cba}
  - {path: context/generated/test-layout.json, sha256: c0752ab8543e23547679dbe34f6381f656d36b0eb5506efb2a56f94dcb729cd6}
pack_sha: {sample-system: 7681f87749883d71abb4011d118366ca1242e1cc}
code:
  - repo: sample-system
    base_sha: 2a3757c7624e675192e9278c0e8331cf32620430
    head_sha: ceb4be7e3a52d89cc7989ee97e33e1e24dd25039
    diff_sha256: a36ea24ea3aeb382d9045af5b5bec5e4b67e44c5c92aa87722fb0d73ebb6f467
    files: [api/test/dashboard-awaiting-outcome.test.ts, web/src/__tests__/AppointmentsPage.test.tsx, web/src/__tests__/DashboardPage.test.tsx]
claims:
  - {id: C1, text: "API tests use Vitest and Supertest against a throwaway PostgreSQL 16 with a `fixed clock`.", cites: ["context/test-strategy.md#layers"]}
  - {id: C2, text: "Every requirement has a test whose name `starts with its ID`.", cites: ["context/test-strategy.md#requirement-ids"]}
  - {id: C3, text: "The verifier `skips coverage`, so no run reports coverage.", cites: ["context/test-strategy.md#gaps"]}
  - {id: C4, text: "The fixture gives doctor 1 `15-minute slots, Mon–Fri 09:00–12:00`.", cites: ["sample-system:api/test/helpers.ts#L23"]}
  - {id: C5, text: "The test clock starts at `2026-10-19T02:30:00Z`, Monday 08:00 in Kolkata.", cites: ["sample-system:api/test/helpers.ts#L10"]}
  - {id: C6, text: "Fixture patients are invented, for example `Test Patient One`.", cites: ["sample-system:api/test/helpers.ts#L45"]}
  - {id: C7, text: "The existing HMS-3 tile test notes `Its own database`, so other tests do not move its counts.", cites: ["sample-system:api/test/dashboard-no-shows.test.ts#L7"]}
  - {id: C8, text: "Web tile tests stub `fetch` and render the page in a MemoryRouter.", cites: ["sample-system:web/src/__tests__/DashboardPage.test.tsx#L16"]}
  - {id: C9, text: "The change adds `totals.awaitingOutcome` and the started filter.", cites: ["artifacts/HMS-21/run_01M7MD2RTRSVZ65HR3QFFR9NJB/impl.md#summary"]}
trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10, REQ-11]
known: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10, REQ-11, S-1, S-2, S-3, S-4]
assumed: []
unknown: []
questions: []
---
# Unit tests: HMS-21

## Summary
The tests cover the change's two new behaviours (C9), each with a normal path and an error or edge path:
- **The count.** `GET /api/dashboard` gains `totals.awaitingOutcome`.
- **The list.** `GET /api/appointments` gains the `started=true` filter. The web tile and the list page pass that filter through.

The API tests run in their own database with a fixed clock (C1, C7), using invented fixture data (C6). They use doctor 1's 15-minute Monday slots (C4), starting from a test clock of Monday 08:00 (C5). The appointments set up on Monday are:
- 09:00 Scheduled;
- 09:15 Completed;
- 09:30 No-show;
- 09:45 Cancelled;
- 10:00 Scheduled, exactly at now;
- 10:30 Scheduled.

There is also a Tuesday 10:00 Scheduled appointment. The clock is set to Monday 10:00.

Each test name starts with HMS-21 and its REQ id (C2). The test counts and pass/fail results are not reported here. tsl records them in checks.json when it runs the verifier (H3). Coverage is not reported, because the verifier skips it (C3).

## Tests
- `api/test/dashboard-awaiting-outcome.test.ts` (new, 11 tests):
  - REQ-2, REQ-3, REQ-4: the count is 1. Only 09:00 counts. Completed, No-show and Cancelled are excluded, and 10:00 (exactly now) and 10:30 are not counted (S-1).
  - REQ-5: the count is the same for today, 7d and 30d. On Tuesday 09:00 it is 3 for every period (S-2).
  - REQ-7 edge: the count is 0 before any appointment has started.
  - REQ-6: `?status=SCHEDULED&started=true` returns exactly the counted appointment. On the next day it returns 09:00, 10:00 and 10:30, and its total equals the count (S-3).
  - REQ-6 edges:
    - with no `started`, the list is unchanged (4 Scheduled);
    - an empty `started=` is treated as absent.
  - REQ-6 error: `started` set to `false`, `yes` or `1` is a 400 field error on `started`.
  - REQ-8, REQ-9: the other totals stay `upcoming 2, completed 1, cancelled 1, noShows 1`, and `next` is 10:30 Monday and 10:00 Tuesday (S-4).
  - REQ-10, REQ-11: neither the dashboard nor the tile's list contains contact details or medical history (S-4).
- `web/src/__tests__/DashboardPage.test.tsx` (4 tests added, C8):
  - REQ-1, REQ-6: the tile shows 3 and links to `/appointments?status=SCHEDULED&started=true`.
  - REQ-5: after the period changes, the tile's link still carries no period.
  - REQ-7: the tile shows 0.
  - REQ-8: the Upcoming tile keeps its count and link, and there are 8 tiles.
- `web/src/__tests__/AppointmentsPage.test.tsx` (new, 2 tests):
  - REQ-6: `started` from the URL reaches the API call.
  - REQ-6 edge: without it, no `started` is sent.

## Known
- REQ-1 to REQ-11 and S-1 to S-4: each is covered by at least one named test above.

## Assumed
None.

## Unknown
None.

## Open questions
None.
