---
schema: ut-result.v1
run_id: run_01M820V4J0VSJHKNZZ5VK0BT2Y
step: ut
attempt: 2
skill: unit-test@1.3.0
inputs:
  - path: artifacts/HMS-31/run_01M820V4J0VSJHKNZZ5VK0BT2Y/impl.md
    sha256: 51c1d75b880628f2f9a209c6df2fbc6b037b82aa389b6a1880dc9b76cd6b1bbe
  - path: artifacts/HMS-31/run_01M820V4J0VSJHKNZZ5VK0BT2Y/split.md
    sha256: bd26e52b86c31239358a3a893ff920c053cf6830e2406107bd5bda4c61c58536
  - path: context/test-strategy.md
    sha256: f4bbc9d5d63dfc433c9fe466a7aa71a4fad0147baafe1f47db0c06e117ce6cba
  - path: context/generated/test-layout.json
    sha256: c0752ab8543e23547679dbe34f6381f656d36b0eb5506efb2a56f94dcb729cd6
pack_sha:
  sample-system: 7681f87749883d71abb4011d118366ca1242e1cc
code:
  - repo: sample-system
    base_sha: 0647279f5203c198dc770534c97b8d3ffdb9e99a
    head_sha: 100822eae8f6f341dbe5dbcae92491337cb183a1
    diff_sha256: 58cbbd7cfa5057904e51139468a7051f3cabf05b5d7d71ce28376ba1fc698653
    files:
      - api/test/appointments-doctor-details.test.ts
      - web/src/__tests__/AppointmentDetailPage.test.tsx
claims:
  - id: C1
    text: "API tests use `Vitest` and Supertest against a throwaway PostgreSQL with a fixed clock; web tests use Testing Library."
    cites: ["context/test-strategy.md#layers"]
  - id: C2
    text: "Every requirement has at least one test whose name starts with its ID, so the new tests start with HMS-31 and the REQ id (`New requirements get new IDs and tests`)."
    cites: ["context/test-strategy.md#requirement-ids"]
  - id: C3
    text: "Existing API appointment tests live in `api/test/appointments.test.ts`."
    cites: ["context/generated/test-layout.json#test:sample-system:api/test/appointments.test.ts"]
  - id: C4
    text: "Web page tests stub fetch and render in a MemoryRouter, as in `web/src/__tests__/DoctorDetailPage.test.tsx`."
    cites: ["context/generated/test-layout.json#test:sample-system:web/src/__tests__/DoctorDetailPage.test.tsx"]
  - id: C5
    text: "The changed detail read is `AppointmentRepo`'s detail."
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/repositories/appointmentRepo.ts:AppointmentRepo"]
  - id: C6
    text: "The changed page is `AppointmentDetailPage`."
    cites: ["context/generated/module-map.json#sym:sample-system:web/src/pages/AppointmentDetailPage.tsx:AppointmentDetailPage"]
  - id: C7
    text: "The verifier skips coverage because the project has no `coverage` script."
    cites: ["context/test-strategy.md#gaps"]
  - id: C8
    text: "An Inactive specialization or department is shown with its name followed by `(Inactive)`."
    cites: ["runs/run_01M820V4J0VSJHKNZZ5VK0BT2Y/ide-answers.md#a-01"]
resolves:
  - id: F-1
    how: "The API REQ-5 test now also changes the doctor's specialization and checks the Completed, No-show and Cancelled appointments show the current one."
  - id: F-2
    how: "The web tests now also check that Completed and No-show appointment pages keep their rows, closed notice and history."
trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10, REQ-11]
known: [S-1, S-2, S-3, S-4]
assumed: []
unknown:
  - {id: U-1, blocking: false}
questions: []
---
# Unit tests: HMS-31

## Summary
Two new test files cover the change. The API test checks the appointment detail's new doctor fields
from start to finish against the test database: values, current-ness after the doctor's fee,
department and specialization change, Inactive entries, privacy, the unchanged list shape and the 404
error path. The web test checks the three new rows on the appointment page, the fee format, the
"(Inactive)" marker, the doctor link, and the unchanged page for Scheduled, Completed, Cancelled and
No-show appointments. tsl records the test counts in checks.json when it runs the verifier (H3).
Coverage is not reported, because the project has no coverage script (C7).

Resolved review findings:
- F-1: The API REQ-5 test now also changes the doctor's specialization and checks the Completed, No-show and Cancelled appointments show the current one.
- F-2: The web tests now also check that Completed and No-show appointment pages keep their rows, closed notice and history.

## Tests
- api/test/appointments-doctor-details.test.ts. It is its own database, because the later tests change doctor 1 and specialization 2.
  - HMS-31 REQ-1/REQ-2/REQ-3: the detail doctor is exactly {id, code, fullName, specialization, department, feePaise} with the fixture values (S-1 AC1–AC3).
  - HMS-31 (EP-2, EP-4): the booking and cancel responses carry the same doctor details.
  - HMS-31 REQ-9/REQ-10: the detail has no doctor phone or email and no patient allergies or notes, even for the patient with medical history (S-3 AC2, AC3).
  - HMS-31 REQ-11: the list items keep doctor {id, code, fullName}.
  - HMS-31 REQ-11 error: an unknown appointment is still 404.
  - HMS-31 REQ-5: after the fee, department and specialization change, the Completed, No-show and Cancelled appointments show the current values (S-2 AC1, AC2).
  - HMS-31 REQ-7 edge: an Inactive specialization comes back with status INACTIVE while the department stays ACTIVE (S-2 AC4).
  - HMS-31 REQ-6: an Inactive doctor still has the current details (S-2 AC3).
- web/src/__tests__/AppointmentDetailPage.test.tsx
  - HMS-31 REQ-1/REQ-2/REQ-3: the rows "Specialization", "Department" and "Consultation fee" come right after "Doctor", and 50000 paise is shown as ₹500.00 (S-1 AC1–AC3).
  - HMS-31 REQ-4 edge: 12345 paise is shown as ₹123.45 (S-1 AC4).
  - HMS-31 REQ-7: an Inactive specialization or an Inactive department shows "(Inactive)" and the other does not (S-2 AC4, AC5).
  - HMS-31 REQ-8: the doctor name still links to /doctors/1 (S-3 AC1).
  - HMS-31 REQ-11: a Scheduled appointment keeps its four actions and its history (S-4 AC1).
  - HMS-31 REQ-11: a Completed appointment keeps its rows, the closed notice, no actions and its history, including "Marked completed" (S-4 AC2).
  - HMS-31 REQ-11: a Cancelled appointment keeps its reason and the closed notice (S-4 AC3).
  - HMS-31 REQ-11: a No-show appointment keeps its rows, the closed notice, no actions and its history, including "Marked no-show" (S-4 AC4).

## Known
- S-1: covered by the API values test and the web rows and fee tests.
- S-2: covered by the API REQ-5 (fee, department and specialization), REQ-6 and REQ-7 tests and the web "(Inactive)" tests (A-01, C8).
- S-3: covered by the web link test and the API privacy test.
- S-4: covered by the web Scheduled, Completed, Cancelled and No-show regression tests and the API list-shape test.

## Assumed
None.

## Unknown
- U-1 (non-blocking): the glossary has no entry for Specialization, Department or Consultation fee. This is already open as runs/run_01M820V4J0VSJHKNZZ5VK0BT2Y/open-questions.md#q-02 and does not affect the tests.

## Open questions
None new. runs/run_01M820V4J0VSJHKNZZ5VK0BT2Y/open-questions.md#q-02 stays open (non-blocking, tech lead).
