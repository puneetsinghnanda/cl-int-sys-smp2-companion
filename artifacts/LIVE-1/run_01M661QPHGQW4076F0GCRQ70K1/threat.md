---
schema: threat-model.v1
run_id: run_01M661QPHGQW4076F0GCRQ70K1
step: threat
attempt: 2
skill: threat-model@1.0.1
inputs:
  - path: artifacts/LIVE-1/run_01M661QPHGQW4076F0GCRQ70K1/design.md
    sha256: 352b347ebaf9c4496e3c68531a5185b15d70f3e30799ca9cca5f6d5ce2f71d58
  - path: context/architecture.md
    sha256: 9faab093d10933a46f6473b25c80728f8745e597100e650cd2537bbafb2b595a
  - path: context/data-model.md
    sha256: 2386ae151d54a73cdbe4fd9e0b69f8e8539c2870f5e52c12e5ed0191cfa95e55
  - path: context/generated/api-inventory.json
    sha256: 9ba29278d8d66b7341defc3c67d6e7d68508ce25ee2991ba256be2b37194ec61
pack_sha:
  sample-system: bd98ff08b83f8762c5e5c073ba5fadef88169b33
resolves:
  - id: F-2
    how: "T-1 now rests on the count revealing nothing beyond rows the same viewer already sees, and its reliance on sign-in replacing the MVP identity functions is recorded as assumed, with Q-01 to the tech lead."
claims:
  - id: C1
    text: "In the MVP every request is the actor `staff` and every permission is granted; sign-in replaces identify and requirePermission."
    cites: ["context/architecture.md#identity"]
  - id: C2
    text: "`Medical history` leaves the database only for the patient's own page."
    cites: ["context/data-model.md#patients"]
  - id: C3
    text: "The patient list reads `GET /api/patients`."
    cites: ["context/generated/api-inventory.json#http:GET:/api/patients"]
  - id: C4
    text: "The design reuses the `total` of the existing list response, with no new request."
    cites: ["artifacts/LIVE-1/run_01M661QPHGQW4076F0GCRQ70K1/design.md#overview"]
trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5]
known: [T-2, T-3]
assumed: [T-1]
unknown: []
questions:
  - id: Q-01
    blocking: false
    ask: tech lead
    text: "Until sign-in replaces the MVP identity functions, every caller is treated as staff. Is it acceptable that the patient count, like the list itself, is open to any caller until then?"
    why: "T-1's protection depends on access control that the MVP does not enforce yet."
    evidence: ["context/architecture.md#identity"]
    options: ["Acceptable for the MVP", "Wait for sign-in before shipping"]
    default: "Acceptable for the MVP: the count shows nothing the list rows do not already show."
threats:
  - id: T-1
    component: PatientsPage
    stride: information_disclosure
    threat: "The count tells the viewer how many patients match a search, which could confirm whether a given person is a patient."
    mitigation: "The count reveals nothing beyond the rows the same viewer can already page through. Real access control depends on sign-in replacing identify and requirePermission, which in the MVP grant every permission (C1); recorded as assumed with Q-01."
  - id: T-2
    component: GET /api/patients list handler
    stride: information_disclosure
    threat: "Extending the list response to support the count could leak more patient fields, such as medical history, into a list."
    mitigation: "Existing control: medical history leaves the database only for the patient's own page (C2); the design keeps the response shape unchanged (C4)."
  - id: T-3
    component: GET /api/patients list handler
    stride: denial_of_service
    threat: "A count that is recomputed on every search keystroke could add load to the list route."
    mitigation: "Existing behaviour: the design adds no request and reuses the total already returned with each page (C4)."
---
# Threat model: LIVE-1

## Components
- PatientsPage: shows patient rows and, after this change, the number of matching patients.
- GET /api/patients list handler: returns one page of patients and the total; unchanged by this design.

## Threats
- T-1 PatientsPage, information disclosure (highest risk): the count could confirm a person is a patient; it shows nothing beyond the visible rows, and real access control waits for sign-in (assumed, Q-01).
- T-2 GET /api/patients list handler, information disclosure: more fields could leak into the list; mitigated by the medical-history rule and the unchanged response shape.
- T-3 GET /api/patients list handler, denial of service: extra counting load; mitigated because no new request is added.

## Known
- T-2: the medical-history rule and the unchanged response shape are cited.
- T-3: the design adds no request.

## Assumed
- T-1: access control arrives with sign-in, which replaces the MVP identity functions that grant every permission (C1).

## Unknown
- None.

## Open questions
- Q-01 (non-blocking, tech lead): is an open count acceptable until sign-in? Default: acceptable for the MVP.
