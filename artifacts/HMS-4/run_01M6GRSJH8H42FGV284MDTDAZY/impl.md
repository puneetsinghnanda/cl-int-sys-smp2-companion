---
schema: change.v1
run_id: run_01M6GRSJH8H42FGV284MDTDAZY
step: impl
attempt: 1
skill: implement@1.1.0
inputs:
- path: artifacts/HMS-4/run_01M6GRSJH8H42FGV284MDTDAZY/design.md
  sha256: f9caf706ba92848aaa8be4ce155d2505c138a5e39292e0c5527eedec728526bd
- path: artifacts/HMS-4/run_01M6GRSJH8H42FGV284MDTDAZY/split.md
  sha256: 1097c5a30f050a771deca3a06a8eb429ba26a1b31dd0aada1164d17b95a31518
- path: context/conventions.md
  sha256: dc53d94ec34eb193eeb06fdd0db19eb86a24c29f7d3c434e7c212d61038ca852
- path: context/generated/module-map.json
  sha256: 04f84ff09b4ad6ffaba59ec846716d25f03c8aae22292958a47ebb9247632345
pack_sha:
  sample-system: bd98ff08b83f8762c5e5c073ba5fadef88169b33
code:
- repo: sample-system
  base_sha: c2709e164850ab7e177e4bccfc42a5dd9b961fdc
  head_sha: 93dafae15ebba86de316657c932afb5ce0039a1c
  diff_sha256: bac1bbb41ebf62465b8bd782879d03bcbfa575a00adde477549e15542d8c89b7
  files:
  - api/src/repositories/patientRepo.ts
  - api/src/types.ts
  - api/test/patients-last-visit.test.ts
  - api/test/patients.test.ts
  - web/src/__tests__/PatientsPage.test.tsx
  - web/src/api/types.ts
  - web/src/pages/PatientsPage.tsx
claims:
- id: C1
  text: "SQL is kept in `api/src/repositories`."
  cites: ["context/conventions.md#code-layout"]
- id: C2
  text: "Lists answer items, page, page_size and total, `20 per page`."
  cites: ["context/conventions.md#lists"]
- id: C3
  text: "The patient list query lives in `PatientRepo`."
  cites: ["context/generated/module-map.json#sym:sample-system:api/src/repositories/patientRepo.ts:PatientRepo"]
- id: C4
  text: "The list service is `PatientService`."
  cites: ["context/generated/module-map.json#sym:sample-system:api/src/services/patientService.ts:PatientService"]
- id: C5
  text: "The list page is `PatientsPage`."
  cites: ["context/generated/module-map.json#sym:sample-system:web/src/pages/PatientsPage.tsx:PatientsPage"]
- id: C6
  text: "Next appointment is formatted by the `startsAt` helper in PatientsPage.tsx; Last visit reuses it."
  cites: ["context/generated/module-map.json#sym:sample-system:web/src/pages/PatientsPage.tsx:startsAt"]
- id: C7
  text: "The design gives each list item `lastVisitAt`, null when the patient has no Completed appointment."
  cites: ["artifacts/HMS-4/run_01M6GRSJH8H42FGV284MDTDAZY/design.md#claim:C13"]
- id: C8
  text: "Logs never carry `request bodies`; this change adds no logging."
  cites: ["context/conventions.md#logging-and-data"]
trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10, REQ-11, REQ-12, REQ-13, REQ-14, REQ-15]
known: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10, REQ-11, REQ-12, REQ-13, REQ-14, REQ-15]
assumed: []
unknown:
- {id: U-1, blocking: false}
questions:
- id: Q-01
  blocking: false
  ask: tech lead
  text: Draft PR 7 from the earlier HMS-4 run makes the same change; should it be closed once this run's PR is open?
  why: The user chose to build fresh from main (A-07), so two open PRs for HMS-4 would compete.
  evidence: ["runs/run_01M6GRSJH8H42FGV284MDTDAZY/ide-answers.md#a-07"]
  options: [close PR 7, keep both open]
  default: close PR 7 after this run's PR opens
summary: "PatientRepo's list query now adds lastVisitAt per patient: a second LEFT JOIN LATERAL picks the starts_at of the patient's COMPLETED appointment with the latest start (LIMIT 1, so one row per patient), returned as ISO 8601 UTC or null. PatientService passes it through unchanged. The API and web PatientListItem types gain lastVisitAt: string | null, and PatientsPage shows a Last visit column after Next appointment, formatted with the same startsAt/dateTime call and zone, or '-' when null. Paging, search, sort, the Next appointment column and the no-medical-history list columns are unchanged; HMS-4 regression tests were added and two existing tests updated for the new field and column."
---
# Change: HMS-4

## Summary
The patient list now carries and shows each patient's last visit.
- CO-1: `PatientRepo.list` adds a second `LEFT JOIN LATERAL` on `appointment`, filtered to `status = 'COMPLETED'`, ordered by `starts_at DESC`, `LIMIT 1`. It returns exactly one row per patient, so paging and search cannot repeat rows. It reads `(patient_id, starts_at)` through appointment_patient_start_idx and takes no request input; every list parameter is still bound. `LIST_COLUMNS` is unchanged, so no medical-history column is selected (C1, C3).
- CO-2: confirmed against the code as a pure pass-through. `PatientService.list` returns the repository items as they are, so it needs no edit (C4). This settles the design's CO-2 assumption.
- CO-3: `GET /api/patients` returns `lastVisitAt` in each item, as an ISO 8601 UTC string or null (C7). The route's parameters, paging and sort are unchanged (C2).
- CO-4: `PatientsPage` has a new "Last visit" header and cell after Next appointment. The cell reuses `startsAt(lastVisitAt, timeZone)`, the same call and zone as Next appointment, renders as plain text, and shows "-" when the value is null. The column has no sort control (C5, C6).
- CO-5: `dateTime` is unchanged.
- CO-6: the list item type is declared in `web/src/api/types.ts`, which `web/src/api/client.ts` imports, not in client.ts itself. The new field was added there and to the API-side `api/src/types.ts`. This settles the design's CO-6 assumption.
- CO-7: read only, with no migration.
- No logging was added (C8).

## Changes
- api/src/repositories/patientRepo.ts: CO-1, the last-visit lateral join and the `lastVisitAt` mapping.
- api/src/types.ts: CO-3, `PatientListItem.lastVisitAt: string | null`.
- web/src/api/types.ts: CO-6, `PatientListItem.lastVisitAt: string | null`.
- web/src/pages/PatientsPage.tsx: CO-4, the Last visit header and cell.
- api/test/patients-last-visit.test.ts (new), HMS-4 regression tests:
  - REQ-8: null when there is no Completed appointment.
  - REQ-2, REQ-3, REQ-4: the latest Completed start, whatever order they were closed in.
  - REQ-7: a later Cancelled, No-show or Scheduled appointment is ignored.
  - REQ-9, REQ-10, REQ-11: one row per patient, 20 per page, order and search unchanged.
- api/test/patients.test.ts: the PAT-8 key list now includes `lastVisitAt`. It pins the list item shape, which the design marks changed (REQ-13 is still checked: no medical-history key).
- web/src/__tests__/PatientsPage.test.tsx:
  - New HMS-4 tests: REQ-1 (the column header), REQ-5 and REQ-6 (same format and zone as Next appointment, 01:00 Kolkata on 10 March for 19:30 UTC on 9 March), and REQ-8 ("-" when null).
  - Updates for the new column: fixtures gain `lastVisitAt`, and `nextCell` reads the second-to-last cell, because Last visit is now the last column (REQ-12).

## Known
- REQ-1 to REQ-8: built in CO-1, CO-3 and CO-4 as described above and covered by the new HMS-4 tests.
- REQ-9, REQ-10, REQ-11: the page query, search filter and ORDER BY are untouched; the lateral join keeps one row per patient.
- REQ-12: the Next appointment cell and data are unchanged; the existing HMS-2 tests still target that cell.
- REQ-13: `LIST_COLUMNS` is unchanged, and the PAT-8 test still checks the exact key set.
- REQ-14: no highlight, threshold or filter is added.
- REQ-15: the column has no sort control, and the sort keys are unchanged.

## Assumed
None. The design's two assumptions (CO-2 and CO-6) were checked against the code and are settled in the Summary.

## Unknown
- U-1 (non-blocking): the tests were not run in this step; the verifier (H3) runs them. Draft PR 7 from the earlier HMS-4 run makes an overlapping change (Q-01).

## Open questions
- Q-01 (non-blocking, tech lead): Close draft PR 7 once this run's PR is open? Default: close it, because the user chose to build fresh from main (A-07).
