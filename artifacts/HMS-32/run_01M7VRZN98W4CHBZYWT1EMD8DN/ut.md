---
schema: ut-result.v1
run_id: run_01M7VRZN98W4CHBZYWT1EMD8DN
step: ut
attempt: 1
skill: unit-test@1.3.0
inputs:
  - path: artifacts/HMS-32/run_01M7VRZN98W4CHBZYWT1EMD8DN/impl.md
    sha256: bb9f8d189ea1429142ada5e724d9a917142133a58be8cab15a3a7df646ca9be3
  - path: artifacts/HMS-32/run_01M7VRZN98W4CHBZYWT1EMD8DN/split.md
    sha256: 95071688965ea9fa4eec8630258d808558b92c00cf8f0e8944e5c930e60f1774
  - path: context/test-strategy.md
    sha256: f4bbc9d5d63dfc433c9fe466a7aa71a4fad0147baafe1f47db0c06e117ce6cba
  - path: context/generated/test-layout.json
    sha256: c0752ab8543e23547679dbe34f6381f656d36b0eb5506efb2a56f94dcb729cd6
pack_sha:
  sample-system: 7681f87749883d71abb4011d118366ca1242e1cc
code:
  - repo: sample-system
    base_sha: ad2694b9ec6b5d7f580197ed8b812309b3875e0c
    head_sha: 201ab97758f7b830822e06962cc0aa3b035dfcea
    diff_sha256: e44ee64ceaa5b8d0c8c064d2635876993f92e0e0d98331120c4018816cb9153b
    files: ["web/src/__tests__/BookAppointmentPage.test.tsx"]
claims:
  - id: C1
    text: "Web tests use Vitest with jsdom and `Testing Library`."
    cites: ["context/test-strategy.md#layers"]
  - id: C2
    text: "Every requirement has a test whose name `starts with its ID`."
    cites: ["context/test-strategy.md#requirement-ids"]
  - id: C3
    text: "Before this change the book appointment page had no web test: `other pages have no web tests`."
    cites: ["context/test-strategy.md#gaps"]
  - id: C4
    text: "The verifier `skips coverage`, so no coverage is reported."
    cites: ["context/test-strategy.md#gaps"]
  - id: C5
    text: "The change under test is in `BookAppointmentPage`."
    cites: ["artifacts/HMS-32/run_01M7VRZN98W4CHBZYWT1EMD8DN/impl.md#claim:C1"]
  - id: C6
    text: "The new file follows the page-test pattern of `DoctorDetailPage.test.tsx`: a stubbed fetch and a MemoryRouter."
    cites: ["context/test-strategy.md#gaps"]
trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10, REQ-11, REQ-12, REQ-13, REQ-14]
known: [S-1, S-2, S-3, S-4]
assumed: []
unknown: []
questions: []
---
# Unit tests: HMS-32

## Summary
One new web test file covers the consultation fee on `BookAppointmentPage` (C5) with Vitest and Testing Library (C1), following the neighbouring page test `DoctorDetailPage.test.tsx` (C6). The page had no web tests before (C3). Every test name starts with `HMS-32 REQ-n` and the story criterion it checks (C2). It covers the normal path (fee shown, from the picker and from the URL), the error path (fee load fails, booking still works), the edges (0 paise, Indian grouping of one lakh rupees, no stale fee while the next doctor loads), and the "unchanged" partitions (booking request, free, booked and past slots, a day without availability, the other fields, no private data). All data is invented (Test Doctor 001, doctor1@example.com, +91 00000 range); there are no network calls, only a stubbed fetch.

Test results and coverage: tsl records them in checks.json when it runs the verifier (H3). This step reports none itself, and the verifier skips coverage (C4).

## Tests
`web/src/__tests__/BookAppointmentPage.test.tsx` (new):
- HMS-32 REQ-1/REQ-2 (S-1 AC1): choosing a doctor shows "Consultation fee" ₹1,500.00 as a term in the `details` list.
- HMS-32 REQ-4 (S-1 AC2): no fee and no label before a doctor is chosen.
- HMS-32 REQ-5 (S-1 AC3) edge: 0 paise shows ₹0.00.
- HMS-32 REQ-10 (S-1 AC4): a doctor from `?doctorId=` shows its fee without picking.
- HMS-32 REQ-2 (S-1 AC5) edge: 10000000 paise shows ₹1,00,000.00, not ₹100,000.00.
- HMS-32 REQ-3 (S-2 AC1): switching from doctor A to doctor B shows ₹500.00 and drops ₹1,500.00, on the same page.
- HMS-32 REQ-9 (S-2 AC2) error: a 500 on the fee load shows no fee, and booking still sends one request.
- HMS-32 REQ-3/REQ-9 (S-2 AC3) edge: while doctor B's fee is still loading, and after it fails, doctor A's ₹1,500.00 is not shown.
- HMS-32 REQ-6 (S-3 AC1): the fee list holds no input, select or textarea.
- HMS-32 REQ-7 (S-3 AC2): the booking request body is exactly doctorId, patientId, startsAt and reason, with no fee.
- HMS-32 REQ-8/REQ-11 (S-3 AC3/AC4, S-4 AC1): with the fee shown, the free slot is enabled and the booked and past slots stay disabled.
- HMS-32 REQ-8/REQ-11 (S-3 AC5, S-4 AC2) edge: a day without availability still shows "The doctor has no availability on this day."
- HMS-32 REQ-12 (S-4 AC3): Doctor, Patient, Date, Time, Reason for visit, Book and Cancel are all still there.
- HMS-32 REQ-13/REQ-14 (S-4 AC4/AC5): with the fee shown, no allergy, blood group, phone or email of the doctor or patient appears.

## Known
- S-1: every criterion has a named test (AC1 to AC5).
- S-2: every criterion has a named test (AC1 to AC3), including the error path.
- S-3: every criterion has a named test (AC1 to AC5).
- S-4: every criterion has a named test (AC1 to AC5).

## Assumed
None.

## Unknown
None.

## Open questions
None.
