---
schema: impact.v1
run_id: run_01M661QPHGQW4076F0GCRQ70K1
step: impact
attempt: 2
skill: impact-analysis@1.1.1
inputs:
  - path: artifacts/LIVE-1/run_01M661QPHGQW4076F0GCRQ70K1/req.md
    sha256: 9514b56924da6973313b7c5448cdd9f9f0335306e31e8a0294dab29fad5a138b
  - path: context/generated/module-map.json
    sha256: 04f84ff09b4ad6ffaba59ec846716d25f03c8aae22292958a47ebb9247632345
  - path: context/generated/api-inventory.json
    sha256: 9ba29278d8d66b7341defc3c67d6e7d68508ce25ee2991ba256be2b37194ec61
  - path: context/generated/db-schema.json
    sha256: f047583e5b452649edd65cf48083feaad4c664531202fba78652d06b415749f5
  - path: context/architecture.md
    sha256: 9faab093d10933a46f6473b25c80728f8745e597100e650cd2537bbafb2b595a
  - path: context/data-model.md
    sha256: 2386ae151d54a73cdbe4fd9e0b69f8e8539c2870f5e52c12e5ed0191cfa95e55
pack_sha:
  sample-system: bd98ff08b83f8762c5e5c073ba5fadef88169b33
resolves:
  - id: F-1
    how: "The list convention says list routes answer with a `total`, so GET /api/patients stays existing with an unchanged shape and the open question about the total is dropped (C5)."
claims:
  - id: C1
    text: "The patient list is served by `GET /api/patients`, declared in api/src/routes/patients.ts."
    cites: ["context/generated/api-inventory.json#http:GET:/api/patients"]
  - id: C2
    text: "The web app talks to the API only through `web/src/api/client.ts`."
    cites: ["context/architecture.md#components"]
  - id: C3
    text: "Patients are stored in the `patient` table."
    cites: ["context/generated/db-schema.json#table:patient"]
  - id: C4
    text: "No background `jobs` exist yet, so no job reads the patient list."
    cites: ["context/architecture.md#open-points"]
  - id: C5
    text: "List routes answer with `total` next to the items of one page, so the count of every match is already available."
    cites: ["context/conventions.md#lists"]
  - id: C6
    text: "The patient list is the `PatientsPage` component of the web module."
    cites: ["context/generated/module-map.json#sym:sample-system:web/src/pages/PatientsPage.tsx:PatientsPage"]
  - id: C7
    text: "The existing web test `PatientsPage.test.tsx` covers the patient list and is affected."
    cites: ["context/generated/test-layout.json#test:sample-system:web/src/__tests__/PatientsPage.test.tsx"]
trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5]
known: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5]
assumed: []
unknown: []
questions: []
modules:
  - id: module:sample-system:web
    change: changed
    cites: ["context/generated/module-map.json#module:sample-system:web", "context/generated/module-map.json#sym:sample-system:web/src/pages/PatientsPage.tsx:PatientsPage", "context/architecture.md#components"]
  - id: module:sample-system:api
    change: existing
    cites: ["context/generated/module-map.json#module:sample-system:api", "context/conventions.md#lists"]
  - id: module:sample-system:db
    change: existing
    cites: ["context/generated/module-map.json#module:sample-system:db"]
apis:
  - id: http:GET:/api/patients
    change: existing
    cites: ["context/generated/api-inventory.json#http:GET:/api/patients", "context/conventions.md#lists"]
tables:
  - id: table:patient
    change: existing
    cites: ["context/generated/db-schema.json#table:patient"]
---
# Impact analysis: LIVE-1

## Summary
LIVE-1 adds a count of matching patients to the patient list (PatientsPage) in the web app. The list
reads GET /api/patients, which by the list convention already answers with a total next to one page of
items, so the route, the API module and the patient table keep their shape. Only the web module changes,
and its existing PatientsPage test is affected. No background jobs read patients.

## Impacted modules
- module:sample-system:web (changed): PatientsPage shows the total as the count and updates it with the search.
- module:sample-system:api (existing): serves GET /api/patients, which already returns a total.
- module:sample-system:db (existing): holds the patient table; no change.
- http:GET:/api/patients (existing): read by the patient list; shape unchanged.
- table:patient (existing): read only.

## Known
- REQ-1, REQ-2, REQ-3, REQ-4: shown by PatientsPage, which reads GET /api/patients (C1, C2, C6).
- REQ-5: the route's total counts every match, not one page (C5).

## Assumed
- None.

## Unknown
- None.

## Open questions
- None.
