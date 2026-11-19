---
schema: ut-result.v1
run_id: run_01M7T1CAT0DDAGAWYAP7TPJ3YB
step: ut
attempt: 1
skill: unit-test@1.3.0
inputs:
  - path: artifacts/HMS-24/run_01M7T1CAT0DDAGAWYAP7TPJ3YB/impl.md
    sha256: c9ae830fc32ea02998373070619455888e04b1ce4e45956966e15701d01cc409
  - path: artifacts/HMS-24/run_01M7T1CAT0DDAGAWYAP7TPJ3YB/split.md
    sha256: 7d2c8670cb2f4c61269fc2b9aaf0af2b59ba9a8d61e0be58f13a334769407a85
  - path: context/test-strategy.md
    sha256: f4bbc9d5d63dfc433c9fe466a7aa71a4fad0147baafe1f47db0c06e117ce6cba
  - path: context/generated/test-layout.json
    sha256: c0752ab8543e23547679dbe34f6381f656d36b0eb5506efb2a56f94dcb729cd6
pack_sha:
  sample-system: 7681f87749883d71abb4011d118366ca1242e1cc
code:
  - repo: sample-system
    base_sha: 4101fc5e4230d56edbb1a97d6073daa139df922a
    head_sha: 35a8ebf10ddd0f32bf05d5c2a70d10975403296a
    diff_sha256: 9423f9ef6b50a2f8f5ceb98e1f448302e1abcfce9e521a2e1970677005602991
    files:
      - api/test/patients-visit-summary.test.ts
      - web/src/__tests__/VisitSummary.test.tsx
      - web/src/__tests__/format.test.ts
claims:
  - id: C1
    text: "API tests use Vitest and `Supertest` against a throwaway PostgreSQL 16 with a fixed clock."
    cites: ["context/test-strategy.md#layers"]
  - id: C2
    text: "Web tests use Vitest with jsdom and `Testing Library`."
    cites: ["context/test-strategy.md#layers"]
  - id: C3
    text: "Every requirement has a test whose name starts with its ID, and `New requirements get new IDs and tests`."
    cites: ["context/test-strategy.md#requirement-ids"]
  - id: C4
    text: "Patient API tests live beside `api/test/patients-next-appointment.test.ts`, the HMS-1 tests on the same patient routes."
    cites: ["context/generated/test-layout.json#test:sample-system:api/test/patients-next-appointment.test.ts"]
  - id: C5
    text: "Formatting helpers are tested in `web/src/__tests__/format.test.ts`."
    cites: ["context/generated/test-layout.json#test:sample-system:web/src/__tests__/format.test.ts"]
  - id: C6
    text: "The verifier skips coverage because the project has `no coverage script`."
    cites: ["context/test-strategy.md#gaps"]
  - id: C7
    text: "Without PostgreSQL 16 binaries or `HMS_TEST_DATABASE_URL`, database tests are skipped."
    cites: ["context/test-strategy.md#database-for-tests"]
trace: [REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10, REQ-11, REQ-12, REQ-13, REQ-14]
known: [REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10, REQ-11, REQ-12, REQ-13, REQ-14]
assumed: []
unknown: []
questions: []
---
# Unit tests: HMS-24

## Summary
The tests cover the HMS-24 change at both layers the strategy names (C1, C2). Every test name
starts with its requirement ID (C3).
- API, against a real throwaway database with the fixed clock: the four counts, including a past
  unclosed Scheduled appointment and a future Cancelled one; upcoming following `now`; the most
  recent Completed appointment chosen over a later-booked one and a later No-show. They also cover
  zero counts with `lastVisit: null`, a patient whose only closed appointment is a No-show, the
  local-day edge (Tuesday 00:30 in Kolkata is 19:00 UTC on Monday, so the date is the Tuesday),
  the summary's exact keys with no reason or medical history, and the unknown patient 404 that
  must stay unchanged.
- Web: the `VisitSummary` component's counts line with zeros, the date-only link to
  `/appointments/:id` with the doctor's name as plain text, "Last visit: none yet",
  "No visits yet", and the case of zero counts with appointments; plus `localDate`'s output and
  its day-boundary edges.

Results: data holds no verifier result yet, so this artifact reports no test counts or coverage.
tsl records them in checks.json when it runs the verifier (H3). Coverage is skipped by the
verifier (C6). Database tests run only where PostgreSQL 16 is available (C7).

Not covered at unit level: REQ-1 (placement above the table on the full page) and REQ-14 for the
other pages. The patient page has no web test harness today. Both are left to manual or end-to-end
checks. REQ-6's "more rows than the table loads" case is covered by design, since the count query
has no row limit, rather than by inserting that many appointments.

## Tests
- api/test/patients-visit-summary.test.ts (new, next to the HMS-1 patient tests, C4): REQ-2 to REQ-6, REQ-8 to REQ-14 through `GET /api/patients/:id`.
- web/src/__tests__/VisitSummary.test.tsx (new): REQ-2 to REQ-5, REQ-7 to REQ-12 on the component.
- web/src/__tests__/format.test.ts (extended, C5): REQ-8, `localDate` output and day-boundary edges.

## Known
- REQ-2, REQ-3, REQ-4, REQ-5: API count tests and the web counts line.
- REQ-6: counts across both fixture doctors and several days match the appointment list.
- REQ-7: fixed format with zero counts (web).
- REQ-8: date-only local day on the API (UTC-midnight edge) and in the web helper.
- REQ-9, REQ-10: doctor name as plain text; the single link goes to the appointment.
- REQ-11: "none yet" in the web test, and `lastVisit: null` with only a No-show on the API.
- REQ-12: "No visits yet" in the web test, and zero counts on the API.
- REQ-13: exact summary keys, and no reason or medical text in the response.
- REQ-14: the unknown-patient 404 is unchanged.

## Assumed
None.

## Unknown
None.

## Open questions
None.
