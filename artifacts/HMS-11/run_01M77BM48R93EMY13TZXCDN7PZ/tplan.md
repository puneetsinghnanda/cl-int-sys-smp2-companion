---
schema: test-plan.v1
run_id: run_01M77BM48R93EMY13TZXCDN7PZ
step: tplan
attempt: 1
skill: test-plan@1.1.0
inputs:
  - path: artifacts/HMS-11/run_01M77BM48R93EMY13TZXCDN7PZ/split.md
    sha256: d4dc5ed961e49e2c861c4e3256457f48726f4adc5af1ce2363cc97491f9a4d26
  - path: artifacts/HMS-11/run_01M77BM48R93EMY13TZXCDN7PZ/design.md
    sha256: 5bbb2ba02b9021065e0ec9ff8410030043417595f497f433b47f9e00817a8237
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
    text: "Every requirement's test name starts with its ID, for example `DOC-1`."
    cites: ["context/test-strategy.md#requirement-ids"]
  - id: C3
    text: "Without a test database, from `HMS_TEST_DATABASE_URL` or local binaries, database tests are skipped."
    cites: ["context/test-strategy.md#database-for-tests"]
  - id: C4
    text: "No page is tested `end to end` in a browser."
    cites: ["context/test-strategy.md#gaps"]
  - id: C5
    text: "Neighbouring API tests live in `api/test/appointments.test.ts` and its siblings under api/test."
    cites: ["context/generated/test-layout.json#test:sample-system:api/test/appointments.test.ts"]
  - id: C6
    text: "The new endpoint under test is `earliest-slots` on the specializations router."
    cites: ["artifacts/HMS-11/run_01M77BM48R93EMY13TZXCDN7PZ/design.md#claim:C17"]
  - id: C7
    text: "The web app is `existing, no change` in this design."
    cites: ["artifacts/HMS-11/run_01M77BM48R93EMY13TZXCDN7PZ/design.md#L218"]
  - id: C8
    text: "The window is `today to today+6`."
    cites: ["artifacts/HMS-11/run_01M77BM48R93EMY13TZXCDN7PZ/design.md#claim:C21"]
  - id: C9
    text: "Name ties compare ignoring case, with `doctor id as the final tie-break`."
    cites: ["artifacts/HMS-11/run_01M77BM48R93EMY13TZXCDN7PZ/design.md#claim:C23"]
  - id: C10
    text: "The new route requires `appointments:read`."
    cites: ["artifacts/HMS-11/run_01M77BM48R93EMY13TZXCDN7PZ/design.md#claim:C19"]
  - id: C11
    text: "slot.start is an ISO-8601 UTC instant such as `2026-10-20T04:00:00.000Z`."
    cites: ["artifacts/HMS-11/run_01M77BM48R93EMY13TZXCDN7PZ/design.md#L156"]
  - id: C12
    text: "The hospital time zone is `Asia/Kolkata`, so local and UTC dates differ just after local midnight."
    cites: ["artifacts/HMS-11/run_01M77BM48R93EMY13TZXCDN7PZ/split.md#claim:C1"]
trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10, REQ-11, REQ-12, REQ-13, REQ-14, REQ-15, REQ-16, REQ-17]
known: [S-1, S-2, S-3, S-4, S-5, S-6]
assumed: []
unknown: []
questions: []
tests:
  - {id: TP-1, title: "Entry carries id, code, full name, department name and the first FREE slot as date, UTC start and HH:MM label", covers: [S-1/AC1, S-1/AC2], level: integration}
  - {id: TP-2, title: "A BOOKED first slot is skipped for the next FREE slot that date", covers: [S-1/AC3], level: integration}
  - {id: TP-3, title: "A slot freed by a Cancelled appointment is returned again", covers: [S-1/AC4], level: integration}
  - {id: TP-4, title: "An Inactive doctor with an earlier slot and an Active doctor of another specialization are both left out", covers: [S-1/AC5], level: integration}
  - {id: TP-5, title: "Boundary: at 08:00 local, a FREE slot at 11:30 on today+6 is inside the window", covers: [S-2/AC1], level: integration}
  - {id: TP-6, title: "Boundary: at 08:00 local, a FREE slot at 07:30 on the day after today+6 gives slot null", covers: [S-2/AC2], level: integration}
  - {id: TP-7, title: "At 09:05 local the started 09:00 slot is never returned", covers: [S-2/AC3], level: integration}
  - {id: TP-8, title: "Just after local midnight, while the UTC date is still the day before, the window counts from the local date", covers: [S-2/AC4], level: integration}
  - {id: TP-9, title: "An earlier slot start comes first", covers: [S-3/AC1], level: integration}
  - {id: TP-10, title: "Same slot start: names compare ignoring case (dr. alpha before Dr. Beta)", covers: [S-3/AC2], level: integration}
  - {id: TP-11, title: "Same name and same slot start: the lower doctor id comes first", covers: [S-3/AC3], level: integration}
  - {id: TP-12, title: "Entries with slot null come last, ordered ignoring case (dr. amy before Dr. Bob)", covers: [S-3/AC4], level: integration}
  - {id: TP-13, title: "Null-slot entries with the same name: the lower doctor id comes first", covers: [S-3/AC5], level: integration}
  - {id: TP-14, title: "A 21st Active doctor is returned, with no page, page_size or total", covers: [S-4/AC1], level: integration}
  - {id: TP-15, title: "An Active specialization whose only doctor is Inactive gives { items: [] }", covers: [S-4/AC2], level: integration}
  - {id: TP-16, title: "Negative: an unknown specialization id gives 404 NOT_FOUND", covers: [S-4/AC3], level: integration}
  - {id: TP-17, title: "Negative: a malformed path id gives 400 before any lookup", covers: [S-4/AC3], level: integration}
  - {id: TP-18, title: "Negative: an Inactive specialization with Active doctors gives 400 \"specialization is Inactive\"", covers: [S-4/AC4], level: integration}
  - {id: TP-19, title: "Each entry's key set is exactly id, code, fullName, departmentName and slot; no doctor phone or email", covers: [S-5/AC1], level: integration}
  - {id: TP-20, title: "With the first slot booked by a patient with allergies and notes, no patient name or medical history appears", covers: [S-5/AC2], level: integration}
  - {id: TP-21, title: "Day schedule for a date with a BOOKED appointment marks only that slot BOOKED", covers: [S-6/AC1], level: integration}
  - {id: TP-22, title: "Day schedule for a date with no appointment is all FREE", covers: [S-6/AC2], level: integration}
  - {id: TP-23, title: "Day schedule for a date with only a Cancelled appointment is all FREE with no appointments listed", covers: [S-6/AC3], level: integration}
  - {id: TP-24, title: "A FREE slot returned by the endpoint books through POST /api/appointments", covers: [S-6/AC4], level: integration}
---
# Test plan: HMS-11

## Scope
In scope: the new endpoint `earliest-slots` (C6), which requires `appointments:read` (C10), and the S-6 promise that the day schedule and booking do not change.

- **Level:** every test is an API integration test, with Vitest and `Supertest` against a throwaway PostgreSQL and a fixed clock (C1).
  - The rules under test are the slot-state rule, the window and the sort. They live in a service over the database, and the comparator is not exported.
  - A request against a seeded database is therefore the lowest level that proves each criterion. A unit test of a private helper would not prove the route's answer.
- **Clock:** the clock is fixed or moved per test. That pins:
  - the window, `today to today+6` (C8);
  - the boundaries either side of 08:00 local;
  - the `Asia/Kolkata` local-midnight case (C12);
  - the UTC slot start, such as `2026-10-20T04:00:00.000Z` (C11).
- **Ordering:** ordering tests use invented doctors whose names differ only in case, and duplicate names, to prove `doctor id as the final tie-break` (C9).
- **Before and after (S-6 AC1–AC3):** the day-schedule tests assert the exact slot states the existing rule gives. The same assertions hold on the base branch, so a change in the day schedule would fail them.
- **Where tests go:** in a new file under api/test, next to `api/test/appointments.test.ts` (C5). Each test name starts with its requirement id (C2).
- **Without a database:** where no test database is available, the suites are skipped (C3).

Out of scope:
- **Web tests:** the web app is `existing, no change` (C7).
- **End-to-end tests:** no page is tested `end to end` in this system (C4).
- **Manual tests:** none are needed, because every criterion can be automated.

## Tests
- TP-1 (integration): the entry has id, code, full name, department name and the first FREE slot as date, UTC start and HH:MM label. Covers S-1/AC1, S-1/AC2.
- TP-2 (integration): a BOOKED first slot is skipped. Covers S-1/AC3.
- TP-3 (integration): a slot freed by a Cancelled appointment is returned. Covers S-1/AC4.
- TP-4 (integration): an Inactive doctor with an earlier slot, and a doctor of another specialization, are left out. Covers S-1/AC5.
- TP-5 (integration, boundary): 11:30 on today+6 is inside the window. Covers S-2/AC1.
- TP-6 (integration, boundary): 07:30 on the day after today+6 gives null. Covers S-2/AC2.
- TP-7 (integration): a started slot is never returned. Covers S-2/AC3.
- TP-8 (integration): the window follows the local date across UTC midnight. Covers S-2/AC4.
- TP-9 (integration): an earlier slot comes first. Covers S-3/AC1.
- TP-10 (integration): names compare ignoring case. Covers S-3/AC2.
- TP-11 (integration): same name and slot, lower id first. Covers S-3/AC3.
- TP-12 (integration): null slots come last, ignoring case. Covers S-3/AC4.
- TP-13 (integration): null slots with the same name, lower id first. Covers S-3/AC5.
- TP-14 (integration): 21 Active doctors are returned unpaged. Covers S-4/AC1.
- TP-15 (integration): only an Inactive doctor gives `{ items: [] }`. Covers S-4/AC2.
- TP-16 (integration, negative): an unknown id gives 404. Covers S-4/AC3.
- TP-17 (integration, negative): a malformed path id gives 400. This is the negative test for the endpoint's one input field. Covers S-4/AC3.
- TP-18 (integration, negative): an Inactive specialization gives 400 "specialization is Inactive". Covers S-4/AC4.
- TP-19 (integration): the exact key set, with no phone or email. Covers S-5/AC1.
- TP-20 (integration): no patient name or medical history. Covers S-5/AC2.
- TP-21 (integration): the day schedule with a BOOKED appointment. Covers S-6/AC1.
- TP-22 (integration): the day schedule with no appointment. Covers S-6/AC2.
- TP-23 (integration): the day schedule with only a Cancelled appointment. Covers S-6/AC3.
- TP-24 (integration): a returned slot books through the existing endpoint. Covers S-6/AC4.

## Known
- S-1: TP-1 to TP-4 cover all five criteria.
- S-2: TP-5 to TP-8 cover the window boundaries, started slots and the hospital zone.
- S-3: TP-9 to TP-13 cover slot order, case-insensitive names and id tie-breaks, with and without a slot.
- S-4: TP-14 to TP-18 cover the unpaged list, the empty list and the 404 and 400 answers.
- S-5: TP-19 and TP-20 cover doctor contact data and patient data.
- S-6: TP-21 to TP-24 cover the day schedule for each partition, and booking.

## Assumed
None.

## Unknown
None.

## Open questions
None.
