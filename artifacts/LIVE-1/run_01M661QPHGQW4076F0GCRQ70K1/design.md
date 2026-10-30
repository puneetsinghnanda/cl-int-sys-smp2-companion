---
schema: design.v1
run_id: run_01M661QPHGQW4076F0GCRQ70K1
step: design
attempt: 1
skill: design@1.4.1
inputs:
  - path: artifacts/LIVE-1/run_01M661QPHGQW4076F0GCRQ70K1/impact.md
    sha256: d9ee34843ec1cc08df1e0e55f8130b04894f77e3787c566ca3a46986d7b23dec
  - path: artifacts/LIVE-1/run_01M661QPHGQW4076F0GCRQ70K1/split.md
    sha256: b4e6622becffa36ae90e954275eb94df9ea45e61936a5f3218496527b6f429e9
  - path: artifacts/LIVE-1/run_01M661QPHGQW4076F0GCRQ70K1/req.md
    sha256: 9514b56924da6973313b7c5448cdd9f9f0335306e31e8a0294dab29fad5a138b
  - path: context/intent-baseline.md
    sha256: 9bfd821aeb2fb9524863e346707e049e4fcfb949a6e55b90223a79194946a215
  - path: context/architecture.md
    sha256: 9faab093d10933a46f6473b25c80728f8745e597100e650cd2537bbafb2b595a
  - path: context/generated/module-map.json
    sha256: 04f84ff09b4ad6ffaba59ec846716d25f03c8aae22292958a47ebb9247632345
  - path: context/generated/api-inventory.json
    sha256: 9ba29278d8d66b7341defc3c67d6e7d68508ce25ee2991ba256be2b37194ec61
pack_sha:
  sample-system: bd98ff08b83f8762c5e5c073ba5fadef88169b33
claims:
  - id: C1
    text: "The patient list is the `PatientsPage` component of the web module."
    cites: ["context/generated/module-map.json#sym:sample-system:web/src/pages/PatientsPage.tsx:PatientsPage"]
  - id: C2
    text: "List routes answer with `total` next to the items of one page."
    cites: ["context/conventions.md#lists"]
  - id: C3
    text: "The patient list reads `GET /api/patients`."
    cites: ["context/generated/api-inventory.json#http:GET:/api/patients"]
  - id: C4
    text: "The web app talks to the API only through `web/src/api/client.ts`."
    cites: ["context/architecture.md#components"]
trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5]
known: [S-2]
assumed: [S-1]
unknown: []
questions: []
components:
  - name: PatientsPage
    module: module:sample-system:web
    change: changed
  - name: GET /api/patients list handler
    module: module:sample-system:api
    change: existing
endpoints:
  - method: GET
    path: /api/patients
    change: existing
---
# Design: LIVE-1

## Overview
PatientsPage shows a count line above the patient table, "<total> patients", built from the `total`
that GET /api/patients already returns with each page of results (C2, C3). The count is part of the
same response as the rows, so it follows the search text and the status filter with no extra request,
and it counts every match rather than the rows on the current page (S-1, assumed REQ-5). With no match
it shows "0 patients" and the existing empty message stays (S-2). Exactly one match reads "1 patient",
the intent's Q-02 default. Trade-off: the count reuses the list response instead of a separate count
call, so it can never disagree with the rows shown.

## Components
- PatientsPage (module:sample-system:web, changed): adds the count line from the list response's total.
- GET /api/patients list handler (module:sample-system:api, existing): already returns total; unchanged.
- GET /api/patients (existing): request and response shapes unchanged.

## Known
- S-2: zero count and the kept empty message need only the existing total and empty message.

## Assumed
- S-1: counts every match (REQ-5, intent Q-01 default), which the list total gives (C2).

## Unknown
- None.

## Open questions
- None new; the intent's Q-01 and Q-02 defaults apply.
