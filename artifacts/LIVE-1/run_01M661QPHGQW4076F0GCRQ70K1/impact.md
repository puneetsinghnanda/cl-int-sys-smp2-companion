---
schema: impact.v1
run_id: run_01M661QPHGQW4076F0GCRQ70K1
step: impact
attempt: 1
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
trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5]
known: [REQ-1, REQ-2, REQ-3, REQ-4]
assumed: [REQ-5]
unknown:
  - id: U-1
    blocking: false
questions:
  - id: Q-01
    blocking: false
    ask: tech lead
    text: "Does GET /api/patients already return the total number of patients that match the search, or only the rows of one page?"
    why: "If it returns a total, only the web module changes; otherwise the API changes too (REQ-5)."
    evidence: ["context/generated/api-inventory.json#http:GET:/api/patients"]
    options: ["It returns a total; only the web module changes", "It returns rows only; the API also changes"]
    default: "The design step checks the route and the list conventions and marks the API changed only if no total is returned."
modules:
  - id: module:sample-system:web
    change: changed
    cites: ["context/generated/module-map.json#module:sample-system:web", "context/architecture.md#components"]
  - id: module:sample-system:api
    change: existing
    cites: ["context/generated/module-map.json#module:sample-system:api", "context/generated/api-inventory.json#http:GET:/api/patients"]
  - id: module:sample-system:db
    change: existing
    cites: ["context/generated/module-map.json#module:sample-system:db"]
apis:
  - id: http:GET:/api/patients
    change: existing
    cites: ["context/generated/api-inventory.json#http:GET:/api/patients"]
tables:
  - id: table:patient
    change: existing
    cites: ["context/generated/db-schema.json#table:patient"]
---
# Impact analysis: LIVE-1

## Summary
LIVE-1 adds a count of matching patients to the patient list in the web app. The list reads patients
through GET /api/patients, which reads the patient table; neither the route nor the table needs a new
shape if the route already reports how many patients match. The main risk is REQ-5: if the route returns
only one page of rows, the API must also report the total (U-1, Q-01). No background jobs read patients.

## Impacted modules
- module:sample-system:web (changed): the patient list shows the count and updates it with the search.
- module:sample-system:api (existing): serves GET /api/patients; changes only if Q-01 finds no total.
- module:sample-system:db (existing): holds the patient table; no change.
- http:GET:/api/patients (existing): read by the patient list; shape unchanged unless Q-01 says otherwise.
- table:patient (existing): read only.

## Known
- REQ-1, REQ-2, REQ-3, REQ-4: shown in the web patient list, which reads GET /api/patients (C1, C2).

## Assumed
- REQ-5: the count covers every match; this holds without an API change only if the route reports a total.

## Unknown
- U-1 (non-blocking): whether GET /api/patients reports the total number of matches (Q-01).

## Open questions
- Q-01 (non-blocking, tech lead): does GET /api/patients return the total? Default: the design step checks and decides.
