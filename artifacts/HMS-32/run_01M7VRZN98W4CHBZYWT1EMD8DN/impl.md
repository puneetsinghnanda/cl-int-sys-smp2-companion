---
schema: change.v1
run_id: run_01M7VRZN98W4CHBZYWT1EMD8DN
step: impl
attempt: 1
skill: implement@1.3.0
inputs:
  - path: artifacts/HMS-32/run_01M7VRZN98W4CHBZYWT1EMD8DN/design.md
    sha256: 506181598019221de0b9e00d9e735b3e04e02bf02c2d84a761f52c306955b848
  - path: artifacts/HMS-32/run_01M7VRZN98W4CHBZYWT1EMD8DN/split.md
    sha256: 95071688965ea9fa4eec8630258d808558b92c00cf8f0e8944e5c930e60f1774
  - path: context/conventions.md
    sha256: dc53d94ec34eb193eeb06fdd0db19eb86a24c29f7d3c434e7c212d61038ca852
  - path: context/generated/module-map.json
    sha256: 4158c8fcd239c22cb8f1029dd11f61d8839867ff8088bd9ab76197e2db96db4b
pack_sha:
  sample-system: 7681f87749883d71abb4011d118366ca1242e1cc
code:
  - repo: sample-system
    base_sha: 7681f87749883d71abb4011d118366ca1242e1cc
    head_sha: cd9f691cea185bcbb6b3cb5f3417548ace42e4d9
    diff_sha256: e99f8b394dc9c0ea8f734e7c19f14dde692fdc16ced457d8dc0bd87b85ef7208
    files: ["web/src/pages/BookAppointmentPage.tsx"]
claims:
  - id: C1
    text: "The changed component is `BookAppointmentPage`."
    cites: ["context/generated/module-map.json#sym:sample-system:web/src/pages/BookAppointmentPage.tsx:BookAppointmentPage"]
  - id: C2
    text: "The fee is formatted with the existing helper `rupees`."
    cites: ["context/generated/module-map.json#sym:sample-system:web/src/lib/format.ts:rupees"]
  - id: C3
    text: "The fee is loaded with the existing hook `useAsync`."
    cites: ["context/generated/module-map.json#sym:sample-system:web/src/hooks/useAsync.ts:useAsync"]
  - id: C4
    text: "The design notes that `useAsync` keeps the previous data while a new key loads (`loading: true`), so the fee must be checked against the chosen doctor."
    cites: ["artifacts/HMS-32/run_01M7VRZN98W4CHBZYWT1EMD8DN/design.md#claim:C11"]
  - id: C5
    text: "The doctor is read through the existing client call on the `api` object."
    cites: ["context/generated/module-map.json#sym:sample-system:web/src/api/client.ts:api"]
  - id: C6
    text: "The doctor page already shows the fee as a `Consultation fee` term in a details list, which this change reuses."
    cites: ["sample-system:web/src/pages/DoctorDetailPage.tsx#L79"]
  - id: C7
    text: "Logs never carry `contact details` or medical history; this change adds no logging."
    cites: ["context/conventions.md#logging-and-data"]
trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10, REQ-11, REQ-12, REQ-13, REQ-14]
known: [CO-1]
assumed: []
unknown: []
questions: []
summary: "BookAppointmentPage now loads the chosen doctor with the existing api.doctor call and shows 'Consultation fee' with rupees(feePaise) below the Doctor field, only when the loaded doctor is the one chosen now; nothing else on the page changes."
---
# Change: HMS-32

## Summary
CO-1 (changed): `BookAppointmentPage` (C1) gets one more `useAsync` load (C3), keyed on the chosen `doctorId`, that calls the existing `api.doctor` (C5). The doctor comes from the Doctor picker or from the URL, so both get a fee (runs/run_01M7VRZN98W4CHBZYWT1EMD8DN/ide-answers.md#a-01).

The fee is shown only when that load has finished without an error and the loaded doctor's id equals the chosen `doctorId`. `useAsync` keeps the previous doctor's data while the next one loads (C4), so this check stops doctor A's fee from staying on screen after staff choose doctor B. On a load error no fee is shown and the form still works (runs/run_01M7VRZN98W4CHBZYWT1EMD8DN/ide-answers.md#a-02).

The fee appears directly below the Doctor field as a `details` list with the term "Consultation fee" and `rupees(feePaise)` (C2), the same pattern the doctor page uses (C6). It is plain text, not an input. The slot list, patient picker, date, reason, `submit` and the `api.book` request are untouched, and no logging is added (C7).

CO-2 to CO-5 and EP-1 are existing and unchanged, as the design says.

## Changes
- `web/src/pages/BookAppointmentPage.tsx` (CO-1): import `rupees`; add the `chosenDoctor` load and the `feePaise` value; render the "Consultation fee" block after the Doctor picker.
- Tests: none added. The design asks for no regression test; tests for the stories are the unit-test step's job.

## Known
- CO-1: built as the design describes, on the module-map component `BookAppointmentPage` (C1), with existing `rupees`, `useAsync` and `api` (C2, C3, C5).

## Assumed
None.

## Unknown
None.

## Open questions
None.
