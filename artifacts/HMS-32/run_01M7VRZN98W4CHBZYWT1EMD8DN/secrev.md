---
schema: secure-review.v1
run_id: run_01M7VRZN98W4CHBZYWT1EMD8DN
step: secrev
attempt: 1
skill: secure-code-review@1.3.0
inputs:
  - path: artifacts/HMS-32/run_01M7VRZN98W4CHBZYWT1EMD8DN/impl.md
    sha256: bb9f8d189ea1429142ada5e724d9a917142133a58be8cab15a3a7df646ca9be3
  - path: context/conventions.md
    sha256: dc53d94ec34eb193eeb06fdd0db19eb86a24c29f7d3c434e7c212d61038ca852
  - path: context/architecture.md
    sha256: 9faab093d10933a46f6473b25c80728f8745e597100e650cd2537bbafb2b595a
pack_sha:
  sample-system: 7681f87749883d71abb4011d118366ca1242e1cc
claims:
  - id: C1
    text: "The web app reaches the API only through `web/src/api/client.ts`; the change adds no new route or client call."
    cites: ["context/architecture.md#components"]
  - id: C2
    text: "In the MVP every request is the actor `staff` and every permission is granted, so the change adds no new trust boundary."
    cites: ["context/architecture.md#identity"]
  - id: C3
    text: "Logs never carry request bodies, `contact details` or medical history; the change adds no logging."
    cites: ["context/conventions.md#logging-and-data"]
  - id: C4
    text: "The fee is rendered as React text through `rupees(feePaise)`, which React escapes; no raw HTML is written."
    cites: ["sample-system:web/src/pages/BookAppointmentPage.tsx#L95"]
  - id: C5
    text: "On a failed load the fee is hidden (`!chosenDoctor.error`) and the error message is not shown, so no server error text leaks into the page."
    cites: ["sample-system:web/src/pages/BookAppointmentPage.tsx#L45"]
  - id: C6
    text: "The booking request still goes through `api.book` with the same fields, so the fee cannot be tampered into a booking."
    cites: ["sample-system:web/src/pages/BookAppointmentPage.tsx#L63"]
  - id: C7
    text: "A doctor id from the URL is coerced with `Number(v)` before it is used in a request path, so it cannot add path segments."
    cites: ["sample-system:web/src/pages/BookAppointmentPage.tsx#L12"]
  - id: C8
    text: "The fee is read with `api.doctor`, which returns the full doctor record."
    cites: ["sample-system:web/src/pages/BookAppointmentPage.tsx#L43"]
trace: [REQ-6, REQ-7, REQ-9, REQ-13, REQ-14]
known: [SF-1]
assumed: []
unknown: []
questions: []
scans:
  - kind: sast
    report_sha256: 0cfdb2ad9ceed9c8b46272bd1556f28428536fdb2b5009bf77bf352e4d6b232e
  - kind: secrets
    report_sha256: 5a1cea4400bf5f18180b6d2eff35db1be83f6e03aa19603fd628c0484b8d90a3
findings:
  - id: SF-1
    severity: minor
    text: "The page fetches the full doctor record (phone, email, upcoming appointments with patient names) only to read feePaise; prefer a narrower source such as the picker's list row, so less personal data sits in the browser."
    cites: ["sample-system:web/src/pages/BookAppointmentPage.tsx#L43"]
---
# Secure code review: HMS-32

## Summary
Overall risk is low. The change is display-only on one web page. It adds no route, no client call beyond the existing `api.doctor` (C1, C8), no new trust boundary (C2), no logging (C3) and no dependency. The fee is escaped React text (C4). A failed load hides the fee without showing server error text (C5). The booking request is unchanged (C6), and a URL doctor id is coerced to a number before it reaches a request path (C7).

The most important finding is SF-1 (minor): to show one number, the page loads the doctor's whole record, including contact details and the names of patients on upcoming appointments. None of it is rendered or logged, and the same staff user can already open that record on the doctor page, but it is more personal data in the browser than the feature needs.

Both verifier scans from the implement step are attached unchanged (SAST and secrets, by report hash).

## Findings
- SF-1 (minor): the full doctor record is fetched only for `feePaise`. Fix direction: take the fee from the picker's search result row, or from a narrower field, instead of the detail call. Cite: sample-system:web/src/pages/BookAppointmentPage.tsx#L43.

## Known
- SF-1: seen directly in the changed line, which calls `api.doctor` for the fee (C8).

## Assumed
None.

## Unknown
None.

## Open questions
None.
