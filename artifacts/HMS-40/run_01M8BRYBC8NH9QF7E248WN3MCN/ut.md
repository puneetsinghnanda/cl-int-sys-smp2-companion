---
schema: ut-result.v1
run_id: run_01M8BRYBC8NH9QF7E248WN3MCN
step: ut
attempt: 1
skill: unit-test@1.3.0
inputs:
  - {path: artifacts/HMS-40/run_01M8BRYBC8NH9QF7E248WN3MCN/impl.md, sha256: f32f7a3f9020c0ef937f0f4dc6bbb6cf3af5b9d3f0707c61d3590b2c59b1a133}
  - {path: artifacts/HMS-40/run_01M8BRYBC8NH9QF7E248WN3MCN/split.md, sha256: 7ce601066fce0ccfb2969ed8f41153375268416bfc548e75869166c890cafee0}
  - {path: context/test-strategy.md, sha256: f4bbc9d5d63dfc433c9fe466a7aa71a4fad0147baafe1f47db0c06e117ce6cba}
  - {path: context/generated/test-layout.json, sha256: c0752ab8543e23547679dbe34f6381f656d36b0eb5506efb2a56f94dcb729cd6}
pack_sha: {sample-system: 7681f87749883d71abb4011d118366ca1242e1cc}
code:
  - {repo: sample-system, base_sha: 3f3e53018956ee81a3dbee04d3c367d720a6bfa6, head_sha: b92fae365fa0fc99fa47b10f193a008a4a8aba2c, diff_sha256: 28d9c993958f7c7117690fa017ecbb84dafb92d9ddc06b95e9c2308743d3a6ff, files: [api/test/settings-today.test.ts]}
claims:
  - {id: C1, text: "The change touches `api/src/routes/health.ts`.", cites: ["artifacts/HMS-40/run_01M8BRYBC8NH9QF7E248WN3MCN/impl.md#claim:C1"]}
  - {id: C2, text: "API tests use `Vitest` and Supertest.", cites: ["context/test-strategy.md#layers"]}
trace: [REQ-1, REQ-2, REQ-3, REQ-4]
known: [S-1]
assumed: []
unknown: []
questions: []
---
# Unit tests: HMS-40

## Summary
New tests cover the `today` field of GET /api/settings: both sides of hospital midnight, a zone behind UTC, and the full response shape. They need no database because the route runs no query. tsl records the test results in checks.json when it runs the verifier (H3).

## Tests
- `api/test/settings-today.test.ts`: S-1 AC1 (hospital midnight in Asia/Kolkata), AC2 (zone behind UTC), AC3 (existing fields returned beside `today`). The route has no error response, so the boundary instants are the edge path.
- `api/test/auth.test.ts` (updated in the implement step): S-1 AC4, the health response, and the settings response. It needs PostgreSQL.

## Known
- S-1: each criterion has a test.

## Assumed
None

## Unknown
None

## Open questions
None
