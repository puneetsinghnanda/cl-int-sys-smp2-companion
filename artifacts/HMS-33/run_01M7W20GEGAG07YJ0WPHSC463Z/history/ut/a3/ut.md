---
schema: ut-result.v1
run_id: run_01M7W20GEGAG07YJ0WPHSC463Z
step: ut
attempt: 3
skill: unit-test@1.3.0
inputs:
  - path: artifacts/HMS-33/run_01M7W20GEGAG07YJ0WPHSC463Z/impl.md
    sha256: 9a068691d7737d9adcc8c4515e667f65c6f20e24684d5b883e0c09c28c213df2
  - path: artifacts/HMS-33/run_01M7W20GEGAG07YJ0WPHSC463Z/split.md
    sha256: c05e9c99ae954bc1723d7c72ed43e179acd5ddc73fc04085968a1a344df9ef19
  - path: context/test-strategy.md
    sha256: f4bbc9d5d63dfc433c9fe466a7aa71a4fad0147baafe1f47db0c06e117ce6cba
  - path: context/generated/test-layout.json
    sha256: c0752ab8543e23547679dbe34f6381f656d36b0eb5506efb2a56f94dcb729cd6
pack_sha:
  sample-system: 7681f87749883d71abb4011d118366ca1242e1cc
claims:
  - id: C1
    text: "Web tests use Vitest with jsdom and `Testing Library`."
    cites: ["context/test-strategy.md#layers"]
  - id: C2
    text: "API tests use Vitest and `Supertest` against a throwaway PostgreSQL."
    cites: ["context/test-strategy.md#layers"]
  - id: C3
    text: "Without a test database, `database tests are skipped`."
    cites: ["context/test-strategy.md#database-for-tests"]
  - id: C4
    text: "The list endpoint's existing tests live in `api/test/appointments.test.ts`."
    cites: ["context/generated/test-layout.json#test:sample-system:api/test/appointments.test.ts"]
  - id: C5
    text: "The verifier skips coverage because the project has `no coverage script`."
    cites: ["context/test-strategy.md#gaps"]
  - id: C6
    text: "The change adds `AppointmentListItem` and the table's opt-in column."
    cites: ["artifacts/HMS-33/run_01M7W20GEGAG07YJ0WPHSC463Z/impl.md#claim:C1"]
trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-8, REQ-9, REQ-10, REQ-11, REQ-12, REQ-13, REQ-14, REQ-15, REQ-16, REQ-17]
known: [S-1, S-2, S-3, S-4, S-5, S-6]
assumed: []
unknown: []
questions: []
code:
  - repo: sample-system
    base_sha: 09580b7da0e53573ff52d014ffcf6f99e8047e42
    head_sha: 934d577e2e9b20a1cedd2b74e5b791f17eb075c3
    diff_sha256: 9c2031b3cec3fd9d6f4a7d9f8e5debc0a5acda22b1ac02a1ca91242a9a9b106f
    files:
      - api/test/appointments.test.ts
      - web/src/__tests__/AppointmentsPage.test.tsx
---
# Unit tests: HMS-33

## Summary
Attempt 3 fixes the H3 lint failure of attempt 2: the web test's fetch mock declared two parameters it never used, which the unused-variables rule refuses. The mock now takes no parameters and the request URL is read from its recorded calls through a small helper. Attempt 1's build failure (an untyped `mock.calls` index) stays fixed by the same helper. No test logic changed; attempt 2's verifier run passed every test, failing only lint.

Tests cover the new `cancellationReason` on list items (API) and the opt-in Cancellation reason column (web) (C6). Web tests use Vitest, jsdom and Testing Library with a stubbed fetch and invented data (C1); API tests extend the existing appointment suite with Supertest (C2, C4), which runs only when a test database is available (C3). Normal paths: a Cancelled row shows its reason; edge and error paths: exactly 60 characters, one past 60, 300 characters, characters outside the basic plane, a Cancelled row with no stored reason, other statuses, and tables without the prop. tsl records the test counts in checks.json when it runs the verifier (H3); coverage is not reported because the project has no coverage script (C5).

## Tests
- web/src/__tests__/AppointmentsPage.test.tsx (new, 14 tests):
  - S-1: column after Status (AC1), reason shown (AC2), exactly 60 characters whole (AC3), shown as entered (AC4).
  - S-2: one past 60 cut to 60 plus "…" (AC1), 300 characters cut (AC2), code points not split.
  - S-3: Scheduled, Completed, No-show show "-" (AC1 to AC3); Cancelled with no stored reason shows "-".
  - S-4: status filter sent and every Cancelled row shows its reason (AC1); second page rows show their own reason (AC3).
  - S-5: the table without the prop keeps its five columns and hides the reason (AC1 to AC3); the list adds only the reason column (AC4).
  - S-6: the appointment code still links to its own page (AC1).
- api/test/appointments.test.ts (3 tests added):
  - list items carry the reason for Cancelled and null otherwise (S-1, S-3);
  - the Cancelled filter gives a reason on every item (S-4 AC1);
  - the patient detail response does not gain the field (S-5 AC3, A-02).

## Known
- S-1 to S-6: each story's acceptance criteria are covered as listed under Tests. S-2 AC3 (full reason on the appointment's page) and S-6 AC2, AC3 (cancel and book) rely on unchanged code covered by the existing tests APT-6 and APT-11 and APT-1 in the same API file. S-4 AC2 (date range) is unchanged list filtering covered by APT-9.

## Assumed
None.

## Unknown
None.

## Open questions
None.
