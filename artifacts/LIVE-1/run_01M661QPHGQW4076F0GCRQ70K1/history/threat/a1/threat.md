---
schema: threat-model.v1
run_id: run_01M661QPHGQW4076F0GCRQ70K1
step: threat
attempt: 1
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
claims:
  - id: C1
    text: "Every API request passes through `identify` and routes call requirePermission; in the MVP every request is the actor staff."
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
known: [T-1, T-2, T-3]
assumed: []
unknown: []
questions: []
threats:
  - id: T-1
    component: PatientsPage
    stride: information_disclosure
    threat: "The count tells the viewer how many patients match a search, which could confirm whether a given person is a patient."
    mitigation: "Existing control: the list is behind identify and requirePermission (C1), and the same rows are already shown to that viewer, so the count discloses nothing new."
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
- T-1 PatientsPage, information disclosure: the count could confirm a person is a patient; mitigated by the existing identity and permission check, and the rows are already visible.
- T-2 GET /api/patients list handler, information disclosure: more fields could leak into the list; mitigated by the medical-history rule and the unchanged response shape.
- T-3 GET /api/patients list handler, denial of service: extra counting load; mitigated because no new request is added.

## Known
- T-1, T-2, T-3: each mitigation is an existing control or an unchanged behaviour cited above.

## Assumed
- None.

## Unknown
- None.

## Open questions
- None.
