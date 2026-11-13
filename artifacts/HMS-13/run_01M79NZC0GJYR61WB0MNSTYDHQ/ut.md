---
schema: ut-result.v1
run_id: run_01M79NZC0GJYR61WB0MNSTYDHQ
step: ut
attempt: 1
skill: unit-test@1.1.0
inputs:
  - path: artifacts/HMS-13/run_01M79NZC0GJYR61WB0MNSTYDHQ/impl.md
    sha256: 0f2c4f20475bbde55247932470829b031ff1e2cdc92e598958cdae21bc853691
  - path: artifacts/HMS-13/run_01M79NZC0GJYR61WB0MNSTYDHQ/split.md
    sha256: fe24d61d00ae55f25e7c5ca0830c60ca1a538922ee61b9e6460f81033870bd6c
  - path: context/test-strategy.md
    sha256: f4bbc9d5d63dfc433c9fe466a7aa71a4fad0147baafe1f47db0c06e117ce6cba
  - path: context/generated/test-layout.json
    sha256: c0752ab8543e23547679dbe34f6381f656d36b0eb5506efb2a56f94dcb729cd6
pack_sha:
  sample-system: 7681f87749883d71abb4011d118366ca1242e1cc
claims:
  - id: C1
    text: "API tests use Vitest and `Supertest` against a throwaway PostgreSQL with a fixed clock."
    cites: ["context/test-strategy.md#layers"]
  - id: C2
    text: "Web tests use Vitest with jsdom and `Testing Library`."
    cites: ["context/test-strategy.md#layers"]
  - id: C3
    text: "Every requirement has a test whose name `starts with its ID`."
    cites: ["context/test-strategy.md#requirement-ids"]
  - id: C4
    text: "Without a PostgreSQL cluster or URL, `database tests are skipped`."
    cites: ["context/test-strategy.md#database-for-tests"]
  - id: C5
    text: "`The verifier skips coverage`, so no run reports coverage."
    cites: ["context/test-strategy.md#gaps"]
  - id: C6
    text: "The change adds the two fields in `AppointmentService.get`."
    cites: ["artifacts/HMS-13/run_01M79NZC0GJYR61WB0MNSTYDHQ/impl.md#claim:C1"]
  - id: C7
    text: "The doctor page's `50-appointment limit` is not copied, so the 51st appointment must still be flagged."
    cites: ["runs/run_01M79NZC0GJYR61WB0MNSTYDHQ/ide-answers.md#a-02"]
  - id: C8
    text: "The API tests' fixed now is `Monday 19 October 2026`, 08:00 in Asia/Kolkata."
    cites: ["sample-system:api/test/helpers.ts#L9"]
  - id: C9
    text: "Fixture doctor 1 has `15-minute slots`, Mon–Fri 09:00–12:00."
    cites: ["sample-system:api/test/helpers.ts#L23"]
  - id: C10
    text: "Existing page tests stub the API with `vi.stubGlobal('fetch'`."
    cites: ["sample-system:web/src/__tests__/DoctorDetailPage.test.tsx#L19"]
  - id: C11
    text: "The hospital time zone used for the midnight case is `Asia/Kolkata`."
    cites: ["artifacts/HMS-13/run_01M79NZC0GJYR61WB0MNSTYDHQ/split.md#claim:C4"]
trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9]
known: [S-1, S-2, S-3, S-4, S-5]
assumed: []
unknown: []
questions: []
code:
  - repo: sample-system
    base_sha: b5d082e73b9ecc1b29c7beb4092ff57c7faa6392
    head_sha: 2568c550257064044fe6c0482848cf42442fcd2a
    diff_sha256: 5e8b9e91c69a77ac4e329ff9209fe323a6b698329a87e9e27290eccedf1d4824
    files:
      - api/test/appointments-fit-edges.test.ts
      - web/src/__tests__/AppointmentDetailPage.test.tsx
---
# Unit tests: HMS-13

## Summary
These tests cover the change to `AppointmentService.get` (C6) and to the appointment page. They add the edge and boundary cases that the implement step's tests (api/test/appointments-fit.test.ts) did not cover. The tests follow the strategy:
- API tests use Supertest with the fixed clock (C1, C8, C9).
- Web tests use Testing Library with a stubbed fetch (C2, C10).
- Every test name starts with its requirement id (C3).

No production code was changed. tsl records the test counts in checks.json when it runs the verifier (H3). Coverage is not reported because the verifier skips it for this project (C5). The API tests need PostgreSQL and are skipped without it (C4).

## Tests
- api/test/appointments-fit-edges.test.ts:
  - S-1 AC4: the 51st upcoming appointment is flagged while the doctor page, capped at 50, does not list it (C7). The 50th stays unflagged.
  - S-1 AC5: Tuesday 01:00 in Asia/Kolkata, which is Monday 19:30 UTC, is flagged against a Monday-only 19:00–20:00 block (C11). An implementation that used the UTC weekday would fail this.
  - REQ-3 boundary: the flag holds at the exact start instant and clears one minute later.
  - S-4 AC2/AC3: Completed and No-show appointments of an Inactive doctor carry no flag, both in the close responses and afterwards on GET.
  - Error path: an unknown appointment still returns 404 without the new fields.
- web/src/__tests__/AppointmentDetailPage.test.tsx:
  - S-1 AC1: the outside-availability notice, placed above the actions.
  - S-2 AC1: the Inactive notice, placed above the actions.
  - S-3 AC1: both notices, outside-availability first.
  - S-1 AC2: no notice when neither condition applies.
  - S-4: no notice on Completed, Cancelled and No-show appointments, even with an Inactive doctor and a flag set to true by mistake.
  - S-5 AC1: the details, the four actions and the history are still shown alongside the notices.

## Known
- S-1: covered by the AC4 (no cap), AC5 (local weekday) and REQ-3 boundary API tests, plus the page tests for AC1 and AC2.
- S-2: covered by the page test for the Inactive notice and its placement.
- S-3: covered by the page test for the order of both notices.
- S-4: covered by the Completed and No-show API test and the page tests for all three closed statuses.
- S-5: covered by the page test checking that the details, actions and history are unchanged.

## Assumed
None.

## Unknown
None.

## Open questions
None.
