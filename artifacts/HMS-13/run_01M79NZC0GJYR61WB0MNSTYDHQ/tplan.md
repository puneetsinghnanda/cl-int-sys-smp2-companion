---
schema: test-plan.v1
run_id: run_01M79NZC0GJYR61WB0MNSTYDHQ
step: tplan
attempt: 1
skill: test-plan@1.1.0
inputs:
  - path: artifacts/HMS-13/run_01M79NZC0GJYR61WB0MNSTYDHQ/split.md
    sha256: fe24d61d00ae55f25e7c5ca0830c60ca1a538922ee61b9e6460f81033870bd6c
  - path: artifacts/HMS-13/run_01M79NZC0GJYR61WB0MNSTYDHQ/design.md
    sha256: 385aa8939befa99e8d415e886be5a4faac02d8447880d56dc1a4477012f582fb
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
    text: "Each requirement has a test whose name `starts with its ID`."
    cites: ["context/test-strategy.md#requirement-ids"]
  - id: C4
    text: "The project has no browser tests: `no page is tested end to end in a browser`."
    cites: ["context/test-strategy.md#gaps"]
  - id: C5
    text: "Without PostgreSQL, `database tests are skipped`."
    cites: ["context/test-strategy.md#database-for-tests"]
  - id: C6
    text: "The DOC-7 rule on the doctor page is already tested: `DOC-7: changing availability keeps appointments and flags those outside it`."
    cites: ["context/generated/test-layout.json#test:sample-system:api/test/doctors.test.ts"]
  - id: C7
    text: "The rule function is already unit-tested: `DOC-7: tells whether an appointment still fits the availability`."
    cites: ["context/generated/test-layout.json#test:sample-system:api/test/lib.test.ts"]
  - id: C8
    text: "Page tests live beside `web/src/__tests__/DoctorDetailPage.test.tsx`."
    cites: ["context/generated/test-layout.json#test:sample-system:web/src/__tests__/DoctorDetailPage.test.tsx"]
  - id: C9
    text: "The changed detail endpoint adds the field `doctorStatus`, plus outsideAvailability."
    cites: ["artifacts/HMS-13/run_01M79NZC0GJYR61WB0MNSTYDHQ/design.md#item:EP-1"]
  - id: C10
    text: "The shared `SUMMARY_SELECT` and AppointmentSummary stay unchanged."
    cites: ["artifacts/HMS-13/run_01M79NZC0GJYR61WB0MNSTYDHQ/design.md#item:CO-5"]
  - id: C11
    text: "The doctor page's `50-appointment limit` is not copied."
    cites: ["runs/run_01M79NZC0GJYR61WB0MNSTYDHQ/ide-answers.md#a-02"]
  - id: C12
    text: "The hospital time zone is `Asia/Kolkata`."
    cites: ["artifacts/HMS-13/run_01M79NZC0GJYR61WB0MNSTYDHQ/split.md#claim:C4"]
  - id: C13
    text: "Closed or already-started appointments get `false (proposed default)` for the flag."
    cites: ["runs/run_01M79NZC0GJYR61WB0MNSTYDHQ/ide-answers.md#a-08"]
trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9]
known: [TP-1, TP-2, TP-3, TP-4, TP-5, TP-6, TP-7, TP-8, TP-9, TP-10, TP-11, TP-12, TP-13, TP-14, TP-15, TP-16, TP-17, TP-18, TP-19, TP-20]
assumed: []
unknown: []
questions: []
tests:
  - id: TP-1
    title: "Not-started Scheduled appointment outside every block is flagged, as on the doctor page"
    covers: [S-1/AC1, S-1/AC2]
    level: integration
  - id: TP-2
    title: "Not-started Scheduled appointment inside a block is not flagged"
    covers: [S-1/AC2]
    level: integration
  - id: TP-3
    title: "Page shows the outside-availability notice above the actions"
    covers: [S-1/AC1]
    level: unit
  - id: TP-4
    title: "Scheduled appointment outside every block is not flagged after its start"
    covers: [S-1/AC3]
    level: integration
  - id: TP-5
    title: "Boundary: flagged at the exact start instant, not one minute later"
    covers: [S-1/AC3]
    level: integration
  - id: TP-6
    title: "51st upcoming appointment outside availability is flagged; the doctor page list omits it"
    covers: [S-1/AC4]
    level: integration
  - id: TP-7
    title: "Tuesday 01:00 Asia/Kolkata is flagged against a Monday-only 19:00 to 20:00 block"
    covers: [S-1/AC5]
    level: integration
  - id: TP-8
    title: "Inactive doctor, not started, inside availability: doctorStatus INACTIVE, flag false"
    covers: [S-2/AC1]
    level: integration
  - id: TP-9
    title: "Inactive doctor, Scheduled appointment after its start: doctorStatus INACTIVE"
    covers: [S-2/AC2]
    level: integration
  - id: TP-10
    title: "Page shows only the Inactive notice above the actions, also for a past start"
    covers: [S-2/AC1, S-2/AC2]
    level: unit
  - id: TP-11
    title: "Page shows no notice for an Active doctor and flag false"
    covers: [S-2/AC3, S-1/AC2]
    level: unit
  - id: TP-12
    title: "Inactive doctor and outside availability: doctorStatus INACTIVE and flag true"
    covers: [S-3/AC1]
    level: integration
  - id: TP-13
    title: "Page shows both notices, outside-availability first, above the actions"
    covers: [S-3/AC1]
    level: unit
  - id: TP-14
    title: "Cancel, complete and no-show return flag false, with availability changed beforehand"
    covers: [S-4/AC1, S-4/AC2, S-4/AC3]
    level: integration
  - id: TP-15
    title: "Page shows neither notice on closed statuses, even with a stray flag"
    covers: [S-4/AC1, S-4/AC2, S-4/AC3]
    level: unit
  - id: TP-16
    title: "With notices, details, actions and a non-empty history still show"
    covers: [S-5/AC1]
    level: unit
  - id: TP-17
    title: "With no notice, the page shows its details, actions and history unchanged"
    covers: [S-5/AC2]
    level: unit
  - id: TP-18
    title: "Closed appointment shows the closed-appointment text and history unchanged"
    covers: [S-5/AC3]
    level: unit
  - id: TP-19
    title: "Negative: unknown appointment id returns 404 without the new fields"
    covers: [S-1/AC1, S-2/AC1]
    level: integration
  - id: TP-20
    title: "Negative: no extra doctor field in the detail, no new field in summaries"
    covers: [S-5/AC1, S-5/AC2]
    level: integration
---
# Test plan: HMS-13

## Scope
In scope: every acceptance criterion of S-1 to S-5. The tests cover the two new detail fields (C9) and the two notices on the appointment page.
- API behaviour is proven at integration level with Supertest and the fixed clock (C1). These tests run only when PostgreSQL is available (C5).
- The page is proven at unit level with Testing Library (C2), next to the existing page tests (C8).
- Test names start with "HMS-13 REQ-n" (C3).

Reused, not duplicated: the existing DOC-7 tests for the doctor page (C6) and for the rule function (C7). TP-1 only re-checks parity with the doctor page.

Out of scope:
- Browser end-to-end tests, which the project does not have (C4). The page tests and API tests together cover the path.
- The shared summary query's own behaviour, which is unchanged (C10). TP-20 checks only that no new field leaks into summaries.

## Tests
- TP-1 (integration, S-1/AC1, S-1/AC2): a not-started Scheduled appointment outside every block is flagged, and the doctor page flags it too.
- TP-2 (integration, S-1/AC2): inside a block, the flag is false.
- TP-3 (unit, S-1/AC1): the outside-availability notice text appears above the Reschedule action.
- TP-4 (integration, S-1/AC3): no flag after the start (C13).
- TP-5 (integration, S-1/AC3): boundary at the start instant and one minute later.
- TP-6 (integration, S-1/AC4): the 51st appointment is flagged although the doctor page list stops at 50 (C11).
- TP-7 (integration, S-1/AC5): the local weekday decides across UTC midnight in Asia/Kolkata (C12).
- TP-8 (integration, S-2/AC1): doctorStatus INACTIVE, with the flag false inside availability.
- TP-9 (integration, S-2/AC2): doctorStatus INACTIVE after the start time.
- TP-10 (unit, S-2/AC1, S-2/AC2): the Inactive notice alone, placed above the actions, including for a past start.
- TP-11 (unit, S-2/AC3, S-1/AC2): no notice for an Active doctor inside availability.
- TP-12 (integration, S-3/AC1): both fields set for an Inactive doctor outside availability.
- TP-13 (unit, S-3/AC1): both notices, in the confirmed order.
- TP-14 (integration, S-4/AC1, S-4/AC2, S-4/AC3): closed statuses return false, with the availability changed before closing so the assertion can fail.
- TP-15 (unit, S-4/AC1, S-4/AC2, S-4/AC3): closed statuses show no notice, even with a stray flag.
- TP-16 (unit, S-5/AC1): the details, actions and a non-empty history stay with the notices.
- TP-17 (unit, S-5/AC2): with no notice, the page is unchanged.
- TP-18 (unit, S-5/AC3): the closed-appointment text and history stay.
- TP-19 (integration, negative, S-1/AC1, S-2/AC1): an unknown id returns 404 without the new fields.
- TP-20 (integration, negative, S-5/AC1, S-5/AC2): no extra doctor field in the detail, and no new field in summaries (C10).

## Known
- TP-1 to TP-20: each traces to an acceptance criterion of the stories, at the level the strategy uses for that layer (C1, C2).

## Assumed
None.

## Unknown
None.

## Open questions
None.
