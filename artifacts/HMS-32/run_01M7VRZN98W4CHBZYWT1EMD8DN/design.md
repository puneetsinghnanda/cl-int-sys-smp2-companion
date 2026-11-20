---
schema: design.v1
run_id: run_01M7VRZN98W4CHBZYWT1EMD8DN
step: design
attempt: 1
skill: design@1.9.0
inputs:
  - path: artifacts/HMS-32/run_01M7VRZN98W4CHBZYWT1EMD8DN/split.md
    sha256: 95071688965ea9fa4eec8630258d808558b92c00cf8f0e8944e5c930e60f1774
  - path: artifacts/HMS-32/run_01M7VRZN98W4CHBZYWT1EMD8DN/req.md
    sha256: 090d7caec2892d7d704168997202146560d5e2a0ecf4bd50423664f4dcd23d2b
  - path: context/intent-baseline.md
    sha256: 9bfd821aeb2fb9524863e346707e049e4fcfb949a6e55b90223a79194946a215
  - path: context/architecture.md
    sha256: 9faab093d10933a46f6473b25c80728f8745e597100e650cd2537bbafb2b595a
  - path: context/generated/module-map.json
    sha256: 4158c8fcd239c22cb8f1029dd11f61d8839867ff8088bd9ab76197e2db96db4b
  - path: context/generated/api-inventory.json
    sha256: ba024ab1b4b4151d26ce1afa77bd2dab1cec645ea737ce37f98e0e8674f795e9
pack_sha:
  sample-system: 7681f87749883d71abb4011d118366ca1242e1cc
claims:
  - id: C1
    text: "The book appointment page is the component `BookAppointmentPage`."
    cites: ["context/generated/module-map.json#sym:sample-system:web/src/pages/BookAppointmentPage.tsx:BookAppointmentPage"]
  - id: C2
    text: "The page is served at the web route `/appointments/new`."
    cites: ["context/generated/api-inventory.json#ui:/appointments/new"]
  - id: C3
    text: "The web app already has the paise-to-rupees helper `rupees`."
    cites: ["context/generated/module-map.json#sym:sample-system:web/src/lib/format.ts:rupees"]
  - id: C4
    text: "The web app talks to the API only through `web/src/api/client.ts`."
    cites: ["context/architecture.md#components"]
  - id: C5
    text: "The client object `api` is declared in the web API client."
    cites: ["context/generated/module-map.json#sym:sample-system:web/src/api/client.ts:api"]
  - id: C6
    text: "One doctor is read with the existing route `/api/doctors/:id`."
    cites: ["context/generated/api-inventory.json#http:GET:/api/doctors/:id"]
  - id: C7
    text: "The web type for one doctor is `DoctorDetail`."
    cites: ["context/generated/module-map.json#sym:sample-system:web/src/api/types.ts:DoctorDetail"]
  - id: C8
    text: "The web doctor type carries the fee as `feePaise`."
    cites: ["sample-system:web/src/api/types.ts#L30-L42"]
  - id: C9
    text: "A doctor has a fee in `integer paise`."
    cites: ["context/data-model.md#doctors"]
  - id: C10
    text: "The page loads data with the hook `useAsync`."
    cites: ["context/generated/module-map.json#sym:sample-system:web/src/hooks/useAsync.ts:useAsync"]
  - id: C11
    text: "On a new key `useAsync` sets `loading: true` but keeps the previous data until the new result arrives, and clears data on an error."
    cites: ["sample-system:web/src/hooks/useAsync.ts#L10-L26"]
  - id: C12
    text: "The page already calls `api.doctor` to label a doctor that arrives in the URL."
    cites: ["sample-system:web/src/pages/BookAppointmentPage.tsx#L28-L40"]
  - id: C13
    text: "Booking is sent with `api.book`, which this change leaves as it is."
    cites: ["sample-system:web/src/pages/BookAppointmentPage.tsx#L50-L60"]
  - id: C14
    text: "The web app is the module `web`."
    cites: ["context/generated/module-map.json#module:sample-system:web"]
trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10, REQ-11, REQ-12, REQ-13, REQ-14]
known: [CO-1, CO-2, CO-3, CO-4, CO-5, EP-1]
assumed: []
unknown: []
questions: []
components:
  - id: CO-1
    name: BookAppointmentPage (consultation fee)
    module: module:sample-system:web
    change: changed
    cites: ["context/generated/module-map.json#sym:sample-system:web/src/pages/BookAppointmentPage.tsx:BookAppointmentPage", "context/generated/module-map.json#module:sample-system:web", "runs/run_01M7VRZN98W4CHBZYWT1EMD8DN/ide-answers.md#a-01", "runs/run_01M7VRZN98W4CHBZYWT1EMD8DN/ide-answers.md#a-02"]
  - id: CO-2
    name: rupees helper
    module: module:sample-system:web
    change: existing
    cites: ["context/generated/module-map.json#sym:sample-system:web/src/lib/format.ts:rupees"]
  - id: CO-3
    name: api.doctor client call
    module: module:sample-system:web
    change: existing
    cites: ["context/generated/module-map.json#sym:sample-system:web/src/api/client.ts:api", "context/architecture.md#components"]
  - id: CO-4
    name: DoctorDetail type
    module: module:sample-system:web
    change: existing
    cites: ["context/generated/module-map.json#sym:sample-system:web/src/api/types.ts:DoctorDetail"]
  - id: CO-5
    name: useAsync hook
    module: module:sample-system:web
    change: existing
    cites: ["context/generated/module-map.json#sym:sample-system:web/src/hooks/useAsync.ts:useAsync"]
endpoints:
  - id: EP-1
    method: GET
    path: /api/doctors/:id
    change: existing
    fields: []
    cites: ["context/generated/api-inventory.json#http:GET:/api/doctors/:id"]
answers: []
---
# Design: HMS-32

## Overview
The change lives entirely in the web app (C14) on the book appointment page, `BookAppointmentPage` at `/appointments/new` (C1, C2). No API, database or migration changes: the fee already exists as integer paise (C9) and reaches the web app as `feePaise` on the `DoctorDetail` type (C7, C8) from the existing `GET /api/doctors/:id` (C6), called through `api.doctor` in the API client, the only way the web app talks to the API (C4, C5).

How it works (CO-1):
- The page adds one `useAsync` load (C10) keyed on the chosen `doctorId` that calls `api.doctor(doctorId)` when a doctor is chosen, and loads nothing when none is.
- `doctorId` is set both by the Doctor picker and from the URL, so a doctor filled in from the URL gets its fee the same way (REQ-10, runs/run_01M7VRZN98W4CHBZYWT1EMD8DN/ide-answers.md#a-01).
- Directly below the Doctor field the page shows the label "Consultation fee" and `rupees(feePaise)` (C3) as plain text, not an input (REQ-6). `rupees` already gives the ₹ sign, Indian grouping and two decimals, so 150000 shows as ₹1,500.00, 10000000 as ₹1,00,000.00 and 0 as ₹0.00 (REQ-2, REQ-5).
- The fee is shown only when the load has finished, has no error, and the loaded doctor's `id` equals the current `doctorId`. Because `useAsync` keeps the previous doctor's data while the next one loads (C11), this check stops doctor A's fee from showing after staff choose doctor B (S-2 AC1, AC3). On an error `useAsync` clears the data, so no fee shows and the form stays usable (REQ-9, runs/run_01M7VRZN98W4CHBZYWT1EMD8DN/ide-answers.md#a-02).
- Nothing else on the page changes: the slot list, the patient picker, the date, the reason and the `api.book` call stay as they are (C13), so the booking request carries no fee (REQ-7, REQ-8, REQ-11, REQ-12). The new block shows only the fee, never the doctor's phone or email or any patient data (REQ-13, REQ-14).

Trade-offs:
- Reading the fee with `api.doctor` instead of from the picker's search results: one extra request per chosen doctor, but one path for picker and URL doctors, and the picker's `onChange` stays unchanged.
- A URL doctor is read twice (once for its label (C12), once for the fee): a small cost, kept so the existing label logic is untouched.
- Fee placed directly below the Doctor field: next to the choice it depends on, not next to the Book button.

## Components
- CO-1 BookAppointmentPage (changed): adds the fee load and the "Consultation fee" block described above. Serves S-1, S-2, S-3, S-4.
- CO-2 rupees helper (existing): formats the fee; unchanged.
- CO-3 api.doctor client call (existing): reads one doctor; unchanged.
- CO-4 DoctorDetail type (existing): already carries `feePaise`; unchanged.
- CO-5 useAsync hook (existing): runs the fee load; unchanged.
- EP-1 GET /api/doctors/:id (existing): no new or changed fields; the page reads its existing `feePaise` (integer paise).

Story coverage: S-1 (CO-1, CO-2, CO-3, EP-1), S-2 (CO-1, CO-5), S-3 (CO-1; booking and slots untouched), S-4 (CO-1; rest of page untouched).

## Known
- CO-1: the page component and its module are module-map facts (C1, C14).
- CO-2, CO-3, CO-4, CO-5: existing symbols from the module map (C3, C5, C7, C10).
- EP-1: an existing route in the api inventory (C6).

## Assumed
None.

## Unknown
None.

## Open questions
None. No contract or file location was left open: no endpoint changes, and every file and type used is a module-map fact.

Answered in the IDE: no new answers in this step. The design applies the req-step answers runs/run_01M7VRZN98W4CHBZYWT1EMD8DN/ide-answers.md#a-01 (fee for a URL doctor) and runs/run_01M7VRZN98W4CHBZYWT1EMD8DN/ide-answers.md#a-02 (no fee on a load error, booking still works).
