---
schema: change.v1
run_id: run_01M60VHZHR79G15R3AKPB0VYDK
step: impl
attempt: 1
skill: implement@1.0.1
inputs:
  - path: artifacts/HMS-5/run_01M60VHZHR79G15R3AKPB0VYDK/design.md
    sha256: 8deafa866532fab9ef893732597031376c69447af56a8d73760a0458cf6e701a
  - path: artifacts/HMS-5/run_01M60VHZHR79G15R3AKPB0VYDK/split.md
    sha256: 70a37e2163c9ffeee2153255649beda54907325e5980a65864572ae50d997b4a
  - path: context/conventions.md
    sha256: dc53d94ec34eb193eeb06fdd0db19eb86a24c29f7d3c434e7c212d61038ca852
  - path: context/generated/module-map.json
    sha256: 04f84ff09b4ad6ffaba59ec846716d25f03c8aae22292958a47ebb9247632345
pack_sha:
  sample-system: bd98ff08b83f8762c5e5c073ba5fadef88169b33
code:
  - repo: sample-system
    base_sha: bd98ff08b83f8762c5e5c073ba5fadef88169b33
    head_sha: 7bef5b1fadd8369907db208be40ceef33a4612fb
    diff_sha256: 0a064bb4a3ae7d5e214e5664acf18cb37d853805f73f85621c09b8e61d2a0eb2
    files:
      - api/src/repositories/appointmentRepo.ts
      - api/src/repositories/patientRepo.ts
      - api/src/services/patientService.ts
      - api/src/types.ts
      - web/src/api/types.ts
      - web/src/pages/PatientDetailPage.tsx
claims:
  - id: C1
    text: "SQL lives in `api/src/repositories` and rules in `api/src/services`."
    cites: ["context/conventions.md#code-layout"]
  - id: C2
    text: "Logs never carry request bodies, contact details or `medical history`."
    cites: ["context/conventions.md#logging-and-data"]
  - id: C3
    text: "Appointment SQL lives in `AppointmentRepo`."
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/repositories/appointmentRepo.ts:AppointmentRepo"]
  - id: C4
    text: "Patient rules live in `PatientService`."
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/services/patientService.ts:PatientService"]
  - id: C5
    text: "Patient rows are read by `PatientRepo`."
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/repositories/patientRepo.ts:PatientRepo"]
  - id: C6
    text: "The patient page component is `PatientDetailPage`."
    cites: ["context/generated/module-map.json#sym:sample-system:web/src/pages/PatientDetailPage.tsx:PatientDetailPage"]
  - id: C7
    text: "The edit form maps the patient response in `fromPatient`."
    cites: ["context/generated/module-map.json#sym:sample-system:web/src/pages/PatientFormPage.tsx:fromPatient"]
  - id: C8
    text: "The design adds `visit_summary` with `completed` and `no_show` counts to the patient response."
    cites: ["artifacts/HMS-5/run_01M60VHZHR79G15R3AKPB0VYDK/design.md#overview"]
trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9]
known: [REQ-1, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8]
assumed: [REQ-2, REQ-3, REQ-9]
unknown: []
questions:
  - id: Q-01
    blocking: false
    ask: tech lead
    text: "The existing patient response uses camelCase keys (for example `medicalHistory`), while the design names the new field `visit_summary` with `no_show`. Keep the design's snake_case, or rename to camelCase?"
    why: The code follows the design as written. A rename later changes the API contract and the web type.
    evidence: ["artifacts/HMS-5/run_01M60VHZHR79G15R3AKPB0VYDK/design.md#assumed", "artifacts/HMS-5/run_01M60VHZHR79G15R3AKPB0VYDK/design.md#open-questions"]
    options: ["Keep visit_summary { completed, no_show } as designed", "Rename to visitSummary { completed, noShow }"]
    default: "Keep the design's visit_summary { completed, no_show }."
summary: "Adds a Visit summary to the patient page. AppointmentRepo counts the patient's COMPLETED and NO_SHOW appointments in one parameterised query with no date filter. PatientService adds the counts to GET /api/patients/:id as visit_summary. PatientDetailPage shows them above the appointments list. No migration, no new endpoint, and no change to the appointments list or medical history."
---
# Change: HMS-5

## Summary
The change follows the design: the server counts and the page renders.
- `AppointmentRepo` (C3) gets `visitSummary(patientId)`. It is one SQL statement using `count(*) FILTER` for `COMPLETED` and `NO_SHOW`, with the patient id as bound parameter `$1` and the statuses as constants in code (threat T-10, T-11). It has no date filter (REQ-9). Scheduled and Cancelled rows match neither filter (REQ-4, REQ-5). The aggregate always returns one row, so a patient with no matching appointments gets 0 for each count (REQ-6).
- `PatientService.get` (C4) adds `visit_summary` to the existing patient response, using the same `id` as the patient read (T-7). Existing fields are unchanged.
- `PatientDetailPage` (C6) renders a "Visit summary" heading and a details list with Completed and No-show, between the medical history and the "Appointments" heading. The counts render as plain React text nodes, which React escapes (T-2). Nothing else on the page changed (REQ-7, REQ-8).
- `PatientFormPage` was not changed. `fromPatient` (C7) already copies named fields only, so the counts are never sent back in a PATCH (T-3).
- Nothing new is logged (C2).

## Changes
- api/src/repositories/appointmentRepo.ts — AppointmentRepo (changed): new `visitSummary` count query, and imports the `VisitSummary` type.
- api/src/services/patientService.ts — PatientService (changed): `get` returns `visit_summary` from `AppointmentRepo.visitSummary`.
- api/src/types.ts — the API type for GET /api/patients/:id (changed): `PatientDetail.visit_summary` and a new `VisitSummary` interface.
- api/src/repositories/patientRepo.ts — PatientRepo (existing in the design): a type-only edit. `get` now returns `Omit<PatientDetail, 'appointments' | 'visit_summary'>`, so it type-checks after the new field. There is no behaviour change.
- web/src/api/types.ts — api client (web/src/api/client.ts) types (existing, type gains a field): `PatientDetail.visit_summary`.
- web/src/pages/PatientDetailPage.tsx — PatientDetailPage (changed): the Visit summary block above the appointments list.
- Tests: none added. The design asks for no regression tests, and no existing test pins the exact patient response shape. Broad tests are the unit-test step's job.

## Known
- REQ-1: the block is rendered before the "Appointments" heading in `PatientDetailPage` (C6).
- REQ-4, REQ-5: the query filters only on `COMPLETED` and `NO_SHOW`.
- REQ-6: `count(*) FILTER` returns 0 when nothing matches, and the code also falls back to 0.
- REQ-7: the appointments list code (`AppointmentTable`, `PatientRepo.appointments`) is unchanged.
- REQ-8: the medical history markup and `PatientRepo` data are unchanged. The `PatientRepo` edit is type-only (C5).

## Assumed
- REQ-2, REQ-3: the counts travel as `visit_summary.completed` and `visit_summary.no_show` (C8), as the design assumed. The key style differs from the camelCase patient fields, so it is raised in Q-01.
- REQ-9: the counts cover all time, with no date filter in the query, as the design assumed.

## Unknown
No item is unknown. The change stays within the limits: 6 files, well under the line limit, and no sensitive paths (auth, migrations, CI).

## Open questions
- Q-01 (non-blocking, tech lead): keep `visit_summary { completed, no_show }` as designed, or rename to camelCase to match `medicalHistory`? Default: keep the design.
