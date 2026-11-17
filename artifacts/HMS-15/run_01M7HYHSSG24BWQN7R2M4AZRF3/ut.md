---
schema: ut-result.v1
run_id: run_01M7HYHSSG24BWQN7R2M4AZRF3
step: ut
attempt: 2
skill: unit-test@1.2.0
inputs:
  - path: artifacts/HMS-15/run_01M7HYHSSG24BWQN7R2M4AZRF3/impl.md
    sha256: 8d0cdda0228533bf20acfae2614ed1e7051e24f67db59538f0a060c00e87d78b
  - path: artifacts/HMS-15/run_01M7HYHSSG24BWQN7R2M4AZRF3/split.md
    sha256: 37e1c1b4a8f6927e7237a6d3526934d298e5ce3a870e6da718f067bf13f3507d
  - path: context/test-strategy.md
    sha256: f4bbc9d5d63dfc433c9fe466a7aa71a4fad0147baafe1f47db0c06e117ce6cba
  - path: context/generated/test-layout.json
    sha256: c0752ab8543e23547679dbe34f6381f656d36b0eb5506efb2a56f94dcb729cd6
pack_sha:
  sample-system: 7681f87749883d71abb4011d118366ca1242e1cc
code:
  - repo: sample-system
    base_sha: 18acc740b6c5d482d388549aa27e244bea382131
    head_sha: f653856e28ef77d6a2ef2bfbc791c84dda189a45
    diff_sha256: a4a9461929060c729626eac4da01f07f82048dbd06d4cb6accdc176e7ff9d051
    files:
      - api/test/patient-detail-next-appointment.test.ts
      - web/src/__tests__/PatientDetailPage.test.tsx
resolves:
  - id: F-1
    how: "A new HMS-15 REQ-10 API test uses a clock that flips across the appointment start on every read and asserts next_appointment equals the first listed appointment."
claims:
  - id: C1
    text: "API tests use `Vitest` and Supertest against a throwaway PostgreSQL with a fixed clock; web tests use jsdom and Testing Library."
    cites: ["context/test-strategy.md#layers"]
  - id: C2
    text: "Every test name starts with its requirement ID, as the `Requirement IDs` rule asks."
    cites: ["context/test-strategy.md#requirement-ids"]
  - id: C3
    text: "The shared test helpers fix now at Monday 08:00 Kolkata via `NOW`."
    cites: ["sample-system:api/test/helpers.ts#L10"]
  - id: C4
    text: "The change adds `nextAppointment(id, now)` to the repository."
    cites: ["artifacts/HMS-15/run_01M7HYHSSG24BWQN7R2M4AZRF3/impl.md#claim:C1"]
  - id: C5
    text: "The service reads `this.clock()` once per detail request."
    cites: ["artifacts/HMS-15/run_01M7HYHSSG24BWQN7R2M4AZRF3/impl.md#claim:C3"]
  - id: C6
    text: "Only the date and time are the link (`Date and time (proposed default)`)."
    cites: ["runs/run_01M7HYHSSG24BWQN7R2M4AZRF3/ide-answers.md#a-04"]
  - id: C7
    text: "The verifier skips coverage because the project has no `coverage script`."
    cites: ["context/test-strategy.md#gaps"]
trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10, REQ-11]
known: [S-1, S-2, S-3, S-4]
assumed: []
unknown: []
questions: []
---
# Unit tests: HMS-15

## Summary
The tests cover the HMS-15 change at both layers named by the test strategy (C1). Every test name starts with its requirement ID (C2).
- API: `GET /api/patients/:id` and the POST/PATCH responses now carry `next_appointment`. The tests check the normal path, the empty path, the strict `> now` boundary, closed statuses, agreement with the patients list, a single clock read per request, and the existing 404.
- Web: the patient page line, its link, the empty state, the local-date display, and the unchanged rest of the page.

No production code was changed. This step had no verifier result, so tests and coverage are left out here. tsl records them in checks.json when it runs the verifier (H3); coverage is skipped by the verifier in this project (C7).

## Tests
- `api/test/patient-detail-next-appointment.test.ts`, which uses the fixed clock from the helpers (C3) and an extra clock per check where needed:
  - REQ-5: null when there is no upcoming Scheduled appointment.
  - REQ-4: picks the earliest start, not the first booked, and returns `{id, starts_at, doctor_name}`; the detail page agrees with the list; an unknown patient is still 404.
  - REQ-7: an appointment that starts exactly at now is excluded; one that has already started is excluded; the field is null once all have started.
  - REQ-6: an earlier Cancelled, Completed or No-show appointment is skipped.
  - REQ-8: the appointments array still lists every appointment.
  - EP-3: PATCH returns the same `next_appointment` as GET.
  - EP-2: a new patient gets null.
  - REQ-10: a clock that alternates 09:29 and 09:31 local on every read straddles the 09:30 start. `next_appointment` must equal the first entry of `appointments`, which proves one read of `this.clock()` per request (C5).
- `web/src/__tests__/PatientDetailPage.test.tsx`:
  - REQ-1, REQ-2, REQ-3: "Next appointment:", then the date and time with text identical to the table's When cell, then the doctor's name.
  - REQ-9: 14 Mar 18:40 UTC shows as 15 Mar 2027, the local date.
  - REQ-11: there is exactly one link, `/appointments/7`, on the date and time only (C6).
  - REQ-5: "No upcoming appointment" without a link, both with and without other appointments.
  - REQ-8: the Book appointment button and the table rows stay, both with and without an upcoming appointment.

Resolved review findings:
- F-1: A new HMS-15 REQ-10 API test uses a clock that flips across the appointment start on every read and asserts next_appointment equals the first listed appointment.

## Known
- S-1: covered by the API selection, boundary and single-clock tests and the web line, format and local-date tests. The no-live-refresh behaviour in AC4 follows from fetching once per page load and has no separate unit test.
- S-2: covered by the API Cancelled, Completed, No-show, started and exactly-now tests and the web empty-state tests.
- S-3: covered by the web link tests.
- S-4: covered by the web tests that check the Book appointment button and the table rows, with and without an upcoming appointment.

## Assumed
None.

## Unknown
None.

## Open questions
None.
