---
schema: test-plan.v1
run_id: run_01M7HH0DM88V7JHMVZPSYX5BWE
step: tplan
attempt: 1
skill: test-plan@1.1.0
inputs:
  - path: artifacts/HMS-14/run_01M7HH0DM88V7JHMVZPSYX5BWE/split.md
    sha256: 88e8cd0212df1bcdf5574b26e6415ef50243df439a57102ca81559c2f68fc854
  - path: artifacts/HMS-14/run_01M7HH0DM88V7JHMVZPSYX5BWE/design.md
    sha256: 4623a1e818ebe4e3423442e8697a9912d8940c0b19ebe9de19fb8b428bc6a035
  - path: context/test-strategy.md
    sha256: f4bbc9d5d63dfc433c9fe466a7aa71a4fad0147baafe1f47db0c06e117ce6cba
  - path: context/generated/test-layout.json
    sha256: c0752ab8543e23547679dbe34f6381f656d36b0eb5506efb2a56f94dcb729cd6
pack_sha:
  sample-system: 7681f87749883d71abb4011d118366ca1242e1cc
claims:
  - id: C1
    text: "API tests run with `Vitest and Supertest` against a throwaway PostgreSQL 16 with a fixed clock, so route checks are integration level."
    cites: ["context/test-strategy.md#layers"]
  - id: C2
    text: "Web tests run with Vitest, jsdom and `Testing Library`, so page checks are unit level."
    cites: ["context/test-strategy.md#layers"]
  - id: C3
    text: "No page is tested end to end in a browser: `no page is tested end to end in a browser`."
    cites: ["context/test-strategy.md#gaps"]
  - id: C4
    text: "The existing API tests already pin the unchanged booking rules, e.g. `APT-2: refuses times off the slot grid, outside availability, in the past or already taken`."
    cites: ["context/generated/test-layout.json#test:sample-system:api/test/appointments.test.ts"]
  - id: C5
    text: "The existing API tests pin `APT-3: a patient cannot be in two Scheduled appointments at once, with any doctors`."
    cites: ["context/generated/test-layout.json#test:sample-system:api/test/appointments.test.ts"]
  - id: C6
    text: "The existing API tests pin `APT-4: two simultaneous bookings of one slot leave exactly one appointment`."
    cites: ["context/generated/test-layout.json#test:sample-system:api/test/appointments.test.ts"]
  - id: C7
    text: "The design changes one endpoint, the appointment detail at `/api/appointments/:id`, adding doctor_status and patient_status."
    cites: ["artifacts/HMS-14/run_01M7HH0DM88V7JHMVZPSYX5BWE/design.md#item:EP-1"]
  - id: C8
    text: "The reason reaches the book page in router `Navigation state (proposed default)`, a new client-side input."
    cites: ["runs/run_01M7HH0DM88V7JHMVZPSYX5BWE/ide-answers.md#a-09"]
  - id: C9
    text: "The action routes are regression-checked readers of the detail shape: `Yes, include actions (proposed default)`."
    cites: ["runs/run_01M7HH0DM88V7JHMVZPSYX5BWE/ide-answers.md#a-05"]
trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10, REQ-11, REQ-12, REQ-13, REQ-14, REQ-15, REQ-16, REQ-17, REQ-18, REQ-19]
known: [TP-1, TP-2, TP-3, TP-4, TP-5, TP-6, TP-7, TP-8, TP-9, TP-10, TP-11, TP-12, TP-13, TP-14, TP-15, TP-16, TP-17, TP-18, TP-19, TP-20, TP-21, TP-22, TP-23]
assumed: []
unknown: []
questions: []
tests:
  - {id: TP-1, title: "Cancelled appointment page shows an enabled Book again", covers: [S-1/AC1], level: unit}
  - {id: TP-2, title: "No-show appointment page shows Book again", covers: [S-1/AC2], level: unit}
  - {id: TP-3, title: "Scheduled and Completed appointment pages show no Book again", covers: [S-1/AC3, S-1/AC4], level: unit}
  - {id: TP-4, title: "Book again navigates to the book page with doctorId and patientId, no date and no slot", covers: [S-2/AC1, S-2/AC3, S-2/AC4], level: unit}
  - {id: TP-5, title: "Book page with no date defaults to hospital-local today at 00:30 Asia/Kolkata on 17 November (19:00 UTC on 16 November)", covers: [S-2/AC2, S-2/AC3], level: unit}
  - {id: TP-6, title: "Book again puts the visit reason in navigation state and never the cancellation reason", covers: [S-3/AC1, S-3/AC2], level: unit}
  - {id: TP-7, title: "Original without a reason passes no state and the book page reason starts empty", covers: [S-3/AC3], level: unit}
  - {id: TP-8, title: "Prefilled reason can be edited before booking", covers: [S-3/AC4], level: unit}
  - {id: TP-9, title: "Reason with a 301st character in state is cut to the 300-character limit and the field keeps maxLength 300", covers: [S-3/AC5], level: unit}
  - {id: TP-10, title: "Negative: a non-text reason in navigation state is ignored", covers: [S-3/AC3, S-6/AC3], level: unit}
  - {id: TP-11, title: "Inactive doctor disables Book again with 'Doctor is inactive'", covers: [S-4/AC1], level: unit}
  - {id: TP-12, title: "Inactive patient disables Book again with 'Patient is inactive'", covers: [S-4/AC2], level: unit}
  - {id: TP-13, title: "Both Inactive show both reasons", covers: [S-4/AC3], level: unit}
  - {id: TP-14, title: "Detail GET returns doctor_status and patient_status as ACTIVE, then INACTIVE after the doctor or patient is made Inactive", covers: [S-4/AC1, S-4/AC2], level: integration}
  - {id: TP-15, title: "Negative: unknown appointment id on the detail GET still answers 404 without the new fields", covers: [S-6/AC1], level: integration}
  - {id: TP-16, title: "Booking again refuses an Inactive doctor and an Inactive patient server-side", covers: [S-4/AC1, S-4/AC2], level: integration}
  - {id: TP-17, title: "Slot taken after the page loaded is refused with 'slot no longer available' (existing APT-4 test reused)", covers: [S-5/AC1], level: integration}
  - {id: TP-18, title: "Overlapping patient appointment is refused (existing APT-3 test reused)", covers: [S-5/AC2], level: integration}
  - {id: TP-19, title: "Only free slots are offered and bookable (existing APT-2 test reused)", covers: [S-5/AC5], level: integration}
  - {id: TP-20, title: "Booking again creates a new id and leaves the original appointment and its history unchanged", covers: [S-5/AC3, S-5/AC4], level: integration}
  - {id: TP-21, title: "Scheduled appointment still offers reschedule, cancel, completed and no-show; the four action routes return the detail with both statuses", covers: [S-6/AC1], level: integration}
  - {id: TP-22, title: "Book page opened from a patient page link with address ids and no state starts with an empty reason", covers: [S-6/AC2], level: unit}
  - {id: TP-23, title: "Book page opened with nothing in its address works as today, reason empty", covers: [S-6/AC3], level: unit}
---
# Test plan: HMS-14

## Scope
In scope: the Book again button on the appointment page (where it shows, its disabled reasons, where it navigates, its state), the book page's reason handover and date default, the extended appointment detail response (C7), and regression of the four action routes (C9) and the unchanged booking rules.

Levels follow the strategy:
- Page behaviour is unit level, with Vitest, jsdom and Testing Library (C2).
- Route and database behaviour is integration level, with Vitest, Supertest and a throwaway PostgreSQL 16 (C1).
- No e2e tests: the system has no browser e2e layer (C3), and every criterion can be proved at the two lower levels.

Out of scope:
- The booking rules themselves (APT-2/3/4). They are reused through the existing tests (C4, C5, C6), not re-planned.
- Sign-in, which does not exist in the MVP.

## Tests
- TP-1 (unit): a Cancelled appointment page shows an enabled Book again. Covers S-1/AC1.
- TP-2 (unit): a No-show appointment page shows Book again. Covers S-1/AC2.
- TP-3 (unit): Scheduled and Completed pages have no Book again. Covers S-1/AC3, S-1/AC4.
- TP-4 (unit): Book again navigates with doctorId and patientId only, with no date and no slot, so the original's date is never reused. Covers S-2/AC1, S-2/AC3, S-2/AC4.
- TP-5 (unit): the book page with no date picks hospital-local today across UTC midnight. Covers S-2/AC2, S-2/AC3.
- TP-6 (unit): the visit reason goes in navigation state (C8); the cancellation reason never does. Covers S-3/AC1, S-3/AC2.
- TP-7 (unit): no reason means no state and an empty field. Covers S-3/AC3.
- TP-8 (unit): the prefilled reason is editable. Covers S-3/AC4.
- TP-9 (unit, boundary): the 301st character is cut and maxLength stays 300. Covers S-3/AC5.
- TP-10 (unit, negative for the new navigation-state input): a non-text reason is ignored. Covers S-3/AC3, S-6/AC3.
- TP-11 (unit): an Inactive doctor gives "Doctor is inactive". Covers S-4/AC1.
- TP-12 (unit): an Inactive patient gives "Patient is inactive". Covers S-4/AC2.
- TP-13 (unit): both Inactive show both reasons. Covers S-4/AC3.
- TP-14 (integration): the detail GET returns the two statuses as ACTIVE, then INACTIVE. Covers S-4/AC1, S-4/AC2.
- TP-15 (integration, negative for EP-1): an unknown id still answers 404 without the new fields. Covers S-6/AC1.
- TP-16 (integration): the server refuses an Inactive doctor and an Inactive patient on booking. Covers S-4/AC1, S-4/AC2.
- TP-17 (integration, reuse APT-4): slot no longer available. Covers S-5/AC1.
- TP-18 (integration, reuse APT-3): patient overlap is refused. Covers S-5/AC2.
- TP-19 (integration, reuse APT-2): only free slots. Covers S-5/AC5.
- TP-20 (integration): a new id, and the original and its history are unchanged. Covers S-5/AC3, S-5/AC4.
- TP-21 (integration plus unit): Scheduled actions are unchanged, and the four action routes return both statuses. Covers S-6/AC1.
- TP-22 (unit): a patient-page link with ids and no state starts with an empty reason. Covers S-6/AC2.
- TP-23 (unit): an empty address works as today. Covers S-6/AC3.

## Known
- TP-1 to TP-13, TP-22, TP-23: page behaviour, provable with Testing Library at unit level (C2).
- TP-14 to TP-16, TP-20, TP-21: route behaviour, provable with Supertest at integration level (C1).
- TP-17 to TP-19: unchanged booking rules, already pinned by the existing APT tests (C4, C5, C6).

## Assumed
None

## Unknown
None

## Open questions
None
