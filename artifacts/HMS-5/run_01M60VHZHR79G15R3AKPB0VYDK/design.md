---
schema: design.v1
run_id: run_01M60VHZHR79G15R3AKPB0VYDK
step: design
attempt: 1
skill: design@1.4.1
inputs:
  - path: artifacts/HMS-5/run_01M60VHZHR79G15R3AKPB0VYDK/impact.md
    sha256: 81af31e5c9b85dcfbe566237f3187a3e616ccaff7c1b523b1981babdc587393d
  - path: artifacts/HMS-5/run_01M60VHZHR79G15R3AKPB0VYDK/split.md
    sha256: 70a37e2163c9ffeee2153255649beda54907325e5980a65864572ae50d997b4a
  - path: artifacts/HMS-5/run_01M60VHZHR79G15R3AKPB0VYDK/req.md
    sha256: 7163b98904b9c026b5fe2fca4f8b92bd8e20975a6e468d30003d1f0f96c67a46
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
    text: "API lists are paged, `20 per page`."
    cites: ["context/conventions.md#lists"]
  - id: C2
    text: "List responses use snake_case keys such as `page_size`."
    cites: ["context/conventions.md#lists"]
  - id: C3
    text: "SQL lives in `api/src/repositories` and rules in `api/src/services`."
    cites: ["context/conventions.md#code-layout"]
  - id: C4
    text: "The web app talks to the API only through `web/src/api/client.ts`."
    cites: ["context/architecture.md#components"]
  - id: C5
    text: "One patient is served by `GET /api/patients/:id` in `api/src/routes/patients.ts`."
    cites: ["context/generated/api-inventory.json#http:GET:/api/patients/:id"]
  - id: C6
    text: "The patients router is `patientsRouter`."
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/routes/patients.ts:patientsRouter"]
  - id: C7
    text: "Patient rules live in `PatientService`."
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/services/patientService.ts:PatientService"]
  - id: C8
    text: "Appointment SQL lives in `AppointmentRepo`."
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/repositories/appointmentRepo.ts:AppointmentRepo"]
  - id: C9
    text: "The patient page component is `PatientDetailPage`."
    cites: ["context/generated/module-map.json#sym:sample-system:web/src/pages/PatientDetailPage.tsx:PatientDetailPage"]
  - id: C10
    text: "The patient edit form maps the patient response in `fromPatient`."
    cites: ["context/generated/module-map.json#sym:sample-system:web/src/pages/PatientFormPage.tsx:fromPatient"]
  - id: C11
    text: "The patient edit page is the web route `/patients/:id/edit`."
    cites: ["context/generated/api-inventory.json#ui:/patients/:id/edit"]
  - id: C12
    text: "Logs never carry request bodies, contact details or `medical history`."
    cites: ["context/conventions.md#logging-and-data"]
  - id: C13
    text: "Appointment status values include `COMPLETED` and `NO_SHOW`."
    cites: ["context/data-model.md#appointments"]
trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9]
known: [REQ-1, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8]
assumed: [REQ-2, REQ-3, REQ-9]
unknown: []
questions:
  - id: Q-01
    blocking: false
    ask: tech lead
    text: "Is it acceptable to add a `visit_summary` object with `completed` and `no_show` counts to the `GET /api/patients/:id` response?"
    why: The design adopts the impact default (impact Q-01). The field names are a design choice that the API contract will carry.
    evidence: ["artifacts/HMS-5/run_01M60VHZHR79G15R3AKPB0VYDK/impact.md#open-questions", "context/conventions.md#lists"]
    options: ["Add visit_summary to GET /api/patients/:id", "Add a new read-only endpoint for the counts", "Compute counts in the web page"]
    default: "Add visit_summary {completed, no_show} to GET /api/patients/:id."
  - id: Q-02
    blocking: false
    ask: product owner
    text: "Should the counts cover the patient's whole history, or only a recent period (REQ-9)?"
    why: The count query has no date filter under the default.
    evidence: ["artifacts/HMS-5/run_01M60VHZHR79G15R3AKPB0VYDK/req.md#assumed"]
    options: ["All time", "A fixed recent period"]
    default: "All time."
components:
  - {name: PatientDetailPage, module: "module:sample-system:web", change: changed}
  - {name: PatientFormPage, module: "module:sample-system:web", change: existing}
  - {name: api client (web/src/api/client.ts), module: "module:sample-system:web", change: existing}
  - {name: patientsRouter, module: "module:sample-system:api", change: existing}
  - {name: PatientService, module: "module:sample-system:api", change: changed}
  - {name: AppointmentRepo, module: "module:sample-system:api", change: changed}
  - {name: PatientRepo, module: "module:sample-system:api", change: existing}
endpoints:
  - {method: GET, path: /api/patients/:id, change: changed}
  - {method: GET, path: /api/appointments, change: existing}
  - {method: POST, path: /api/appointments/:id/complete, change: existing}
  - {method: POST, path: /api/appointments/:id/no-show, change: existing}
---
# Design: HMS-5

## Overview
The counts are computed on the server. A new count query in `AppointmentRepo` counts the patient's
appointments with status `COMPLETED` and `NO_SHOW`, with no date filter (REQ-9). Scheduled and Cancelled
rows are therefore never counted (REQ-4, REQ-5). Statuses with no rows return 0, not a missing value (REQ-6).
On the patient read, `PatientService` calls this query and adds `visit_summary: { completed, no_show }`
to the existing `GET /api/patients/:id` response. The route in `patientsRouter` stays as it is.
`PatientDetailPage` renders a "Visit summary" block with both numbers above its existing appointments
list (REQ-1 to REQ-3) and leaves everything else as it is (REQ-7, REQ-8). No new endpoint, table,
migration or infrastructure is needed.

Trade-offs:
- Server-side counting over client-side counting: API lists are paged at 20 per page (C1), so counting
  the page's appointments in the browser would undercount for patients with more appointments. The cost
  is one extra aggregate query per patient read.
- A field on the existing response over a new endpoint: one request per page load and no new route to
  secure. The cost is that the patient response grows for every caller, including `PatientFormPage` (C10, C11),
  which ignores unknown fields.
- `visit_summary` uses snake_case keys to match the existing response style (C2).
- The counts carry no medical history and no contact data, so the logging rule (C12) is unaffected.

## Components
- PatientDetailPage (module:sample-system:web) — changed: renders the Visit summary above the appointments list. S-1, S-2.
- PatientFormPage (module:sample-system:web) — existing: reads the same response through `fromPatient` and ignores the new field. S-3.
- api client (web/src/api/client.ts, module:sample-system:web) — existing: the patient response type gains `visit_summary`. No new call.
- patientsRouter (module:sample-system:api) — existing: `GET /api/patients/:id` passes the service result through as today.
- PatientService (module:sample-system:api) — changed: the get-by-id path adds `visit_summary` from the repository count. S-1, S-2.
- AppointmentRepo (module:sample-system:api) — changed: new count query by `patient_id` and status (COMPLETED, NO_SHOW). S-1, S-2.
- PatientRepo (module:sample-system:api) — existing: the patient row read and the medical history handling stay as they are. S-3.
- GET /api/patients/:id — changed: the response gains `visit_summary { completed, no_show }`. Existing fields stay as they are.
- GET /api/appointments — existing: may feed the page's appointments list. Not touched (S-3).
- POST /api/appointments/:id/complete — existing: raises the Completed count indirectly.
- POST /api/appointments/:id/no-show — existing: raises the No-show count indirectly.

Story coverage: S-1 → PatientDetailPage, PatientService, AppointmentRepo, GET /api/patients/:id.
S-2 → AppointmentRepo (status filter, zero default), PatientDetailPage. S-3 → PatientDetailPage (no
other edits), PatientFormPage, PatientRepo and GET /api/appointments all existing.

## Known
- REQ-1: the block is placed above the appointments list in `PatientDetailPage` (C9).
- REQ-4, REQ-5: the count query filters on COMPLETED and NO_SHOW only (C13), so Scheduled and Cancelled rows are excluded.
- REQ-6: a status with no rows is returned as 0.
- REQ-7: the appointments list code and `GET /api/appointments` are not touched.
- REQ-8: `PatientRepo` and the medical history rendering are not touched. Existing response fields are kept (C5).

## Assumed
- REQ-2, REQ-3: the counts reach the page as `visit_summary.completed` and `visit_summary.no_show` on `GET /api/patients/:id`. Basis: impact default (impact Q-01) and the snake_case response style (C2). Confirm via Q-01.
- REQ-9: the count query has no date filter. Basis: the ticket names no period (intent REQ-9). Confirm via Q-02.

## Unknown
No item is unknown. No story is left unserved, and the design adds no new infrastructure, store or external service.

## Open questions
- Q-01 (non-blocking, tech lead): add `visit_summary { completed, no_show }` to `GET /api/patients/:id`? Default: yes.
- Q-02 (non-blocking, product owner): all-time counts or a recent period? Default: all time.
