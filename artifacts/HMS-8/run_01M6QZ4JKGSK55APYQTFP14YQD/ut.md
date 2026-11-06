---
schema: ut-result.v1
run_id: run_01M6QZ4JKGSK55APYQTFP14YQD
step: ut
attempt: 1
skill: unit-test@1.1.0
inputs:
  - path: artifacts/HMS-8/run_01M6QZ4JKGSK55APYQTFP14YQD/impl.md
    sha256: 64eabc5d66a4d1a05ab0755635b9faf8985e6940de2426991f73b10a90101aad
  - path: artifacts/HMS-8/run_01M6QZ4JKGSK55APYQTFP14YQD/split.md
    sha256: 27a56fb6c5c77620388a68804cade76e555a471888785e875f98064fab7c3d95
  - path: context/test-strategy.md
    sha256: f4bbc9d5d63dfc433c9fe466a7aa71a4fad0147baafe1f47db0c06e117ce6cba
  - path: context/generated/test-layout.json
    sha256: 57a01551ebf3a7429a78830822e19e5384fc83b3f91c6b101bbcfb9cec22038f
pack_sha:
  sample-system: c2709e164850ab7e177e4bccfc42a5dd9b961fdc
code:
  - repo: sample-system
    base_sha: 2f3154df19709798a39553fc45513a3e929f65bc
    head_sha: 356c98b8d91e6a05aa694ce916a58d1ffabd7bf2
    diff_sha256: e8f270fbfd229ecac485df8eba855b843c5d984f583d57cab7317a97731ba415
    files:
      - api/test/appointments.test.ts
      - web/src/__tests__/AppointmentsPage.test.tsx
claims:
  - id: C1
    text: "API tests use Vitest and `Supertest` against a throwaway PostgreSQL 16; web tests use Vitest with jsdom and Testing Library."
    cites: ["context/test-strategy.md#layers"]
  - id: C2
    text: "Every requirement has a test whose name `starts with its ID`, so the new tests are named HMS-8 REQ-n."
    cites: ["context/test-strategy.md#requirement-ids"]
  - id: C3
    text: "Without PostgreSQL binaries or `HMS_TEST_DATABASE_URL`, database tests are skipped."
    cites: ["context/test-strategy.md#database-for-tests"]
  - id: C4
    text: "The verifier skips coverage because the project has `no coverage script`."
    cites: ["context/test-strategy.md#gaps"]
  - id: C5
    text: "The existing API suite already cancels appointment 1 for patient 1 with reason `Patient requested`, so patient 1 has a Cancelled appointment for the new test."
    cites: ["sample-system:api/test/appointments.test.ts#L64"]
  - id: C6
    text: "Neighbouring web page tests stub `fetch` with vi.stubGlobal and render inside a MemoryRouter."
    cites: ["sample-system:web/src/__tests__/DoctorsPage.test.tsx#L27"]
trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-14]
known: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-14]
assumed: []
unknown: []
questions: []
---
# Unit tests: HMS-8

## Summary
This step adds unit tests for every function the change touched. It covers both the normal path and an error or edge path, and no production code changed.
- **API:** `AppointmentService.list` and `AppointmentRepo.list` (CO-5, CO-6), added to the existing Supertest suite (C1, C5).
- **Web:** AppointmentsPage (CO-1), in a new Testing Library file that follows the neighbouring page tests (C6).

The DoctorsPage Today link test (CO-3, CO-4) was already updated in the implement step.

Test names start with `HMS-8 REQ-n` (C2). Test data is invented: Test Doctor 001/002 and Test Specialization 001.

No verifier result was available yet, so tests and coverage are left out. tsl records them in checks.json when it runs the verifier (H3). Coverage is not measured in this project (C4), and the database tests skip without PostgreSQL (C3).

## Tests
**`api/test/appointments.test.ts`**
- "HMS-8 REQ-7, REQ-8: status=NOT_CANCELLED lists every status except Cancelled". This is the normal path:
  - For patient 1, the NOT_CANCELLED total equals the any-status total minus the Cancelled total.
  - No Cancelled item is returned.
- "HMS-8 REQ-8: any other status value is a 400 on status". This is the error path: an unknown status value gets a 400, and the field error names the allowed values, including NOT_CANCELLED.

**`web/src/__tests__/AppointmentsPage.test.tsx`**
- REQ-1: a doctorId from the URL shows the doctor's label (name, code and specialization), not the id.
- REQ-2: the doctor search box shows with no doctor filter, and Clear All stays hidden when no filter is set.
- REQ-3: the chooser offers an Active and an Inactive doctor, and its doctor searches send no status.
- REQ-4, REQ-5, REQ-14:
  - "Change" clears only doctorId, and the patient, status, from and to stay.
  - Choosing another doctor sets doctorId=2 and keeps the same filters, and the new label shows.
- REQ-6: Clear All with only status and dates set clears everything (the request has no query), and the button then hides.
- REQ-7, REQ-8: choosing "Any except Cancelled" sends `status=NOT_CANCELLED` and keeps doctorId.
- REQ-8: a URL with `status=NOT_CANCELLED` opens with that option selected.

## Known
- REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-14: AppointmentsPage tests against the Picker and label behaviour of the change.
- REQ-6: the Clear All test.
- REQ-7, REQ-8: the API tests (normal and 400) and the web status option tests.

## Assumed
None.

## Unknown
None.

## Open questions
None.
