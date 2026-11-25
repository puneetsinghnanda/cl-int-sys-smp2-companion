---
schema: secure-review.v1
run_id: run_01M89BPMN0XE2ZPMFG17PNG5CP
step: secrev
attempt: 1
skill: secure-code-review@1.3.0
inputs:
  - path: artifacts/HMS-27/run_01M89BPMN0XE2ZPMFG17PNG5CP/impl.md
    sha256: 2dabf579260515e7dff6115264f838e92ca1bcda9b1771e2d43e7a9436256b19
  - path: context/conventions.md
    sha256: dc53d94ec34eb193eeb06fdd0db19eb86a24c29f7d3c434e7c212d61038ca852
  - path: context/architecture.md
    sha256: 9faab093d10933a46f6473b25c80728f8745e597100e650cd2537bbafb2b595a
pack_sha:
  sample-system: 7681f87749883d71abb4011d118366ca1242e1cc
claims:
  - id: C1
    text: "GET /api/patients keeps its permission check `requirePermission('patients:read')`; no route was added or changed."
    cites: ["sample-system:api/src/routes/patients.ts#L12"]
  - id: C2
    text: "The new not_seen query value is validated with `oneOf`, so anything but true or false is a 400 ValidationError."
    cites: ["sample-system:api/src/services/patientService.ts#L35"]
  - id: C3
    text: "The cutoff instant is bound as a query parameter via `p(f.lastVisitBefore)`, never interpolated."
    cites: ["sample-system:api/src/repositories/patientRepo.ts#L132"]
  - id: C4
    text: "The `LAST_VISIT_AT` subquery is a constant string with no user input."
    cites: ["sample-system:api/src/repositories/patientRepo.ts#L30"]
  - id: C5
    text: "The last-visit join selects only the start and `d.full_name AS doctor_name`."
    cites: ["sample-system:api/src/repositories/patientRepo.ts#L150"]
  - id: C6
    text: "The list columns stay free of medical history (`PAT-8`)."
    cites: ["sample-system:api/src/repositories/patientRepo.ts#L44"]
  - id: C7
    text: "Logs carry method, route and error message only, `never request bodies`; the change adds no logging."
    cites: ["context/conventions.md#logging-and-data"]
  - id: C8
    text: "The web cell renders the doctor name as React text (`Never` when absent), so it is escaped, not raw HTML."
    cites: ["sample-system:web/src/pages/PatientsPage.tsx#L96"]
  - id: C9
    text: "In the MVP every request is the actor `staff` and every permission is granted."
    cites: ["context/architecture.md#identity"]
trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10, REQ-11, REQ-12, REQ-13, REQ-14]
known: [SF-1]
assumed: []
unknown: []
questions: []
scans:
  - kind: sast
    report_sha256: 15f1785901442339b2e9295dbed7ce1c7d146ffde2341501ea9f6eada41e022f
  - kind: secrets
    report_sha256: 5a1cea4400bf5f18180b6d2eff35db1be83f6e03aa19603fd628c0484b8d90a3
findings:
  - id: SF-1
    severity: minor
    text: "not_seen=true runs a per-patient max() subquery on the open list; index COMPLETED visits if it slows."
    cites: ["sample-system:api/src/repositories/patientRepo.ts#L132"]
---
# Secure code review: HMS-27

## Summary
The overall risk is low. The change adds one optional query flag and two read-only fields to an existing, permission-guarded list endpoint (C1). It adds no route, no write path, no dependency, no secret and no log line (C7). The new input is allow-listed (C2), and the only new SQL value is bound as a parameter (C3); the subquery text is constant (C4). The response adds only a date and the doctor name already shown for the next appointment (C5), and medical history stays out of the list (C6). The web page renders the values as escaped text (C8).

The most important item is SF-1 (minor), a query-cost note rather than a vulnerability. Sign-in does not exist yet and every permission is granted (C9), so the list is as open as before. That is an existing state, not one this change introduces.

Both verifier scans are attached by hash, unchanged: sast `15f17859…` and secrets `5a1cea44…`.

## Findings
- SF-1 (minor): with `not_seen=true`, the count and page queries evaluate a correlated `max()` over each patient's appointments for every patient row, so repeated calls on the open list cost a scan. Fix direction: rely on the existing appointment (patient_id, starts_at) index, or add a partial index on COMPLETED rows if the list slows. Cite: sample-system:api/src/repositories/patientRepo.ts#L132.

## Known
- SF-1: read from the changed query (C3, C4). Its impact is bounded by the existing patient/start index and by MVP data sizes.

## Assumed
None.

## Unknown
None.

## Open questions
None.
