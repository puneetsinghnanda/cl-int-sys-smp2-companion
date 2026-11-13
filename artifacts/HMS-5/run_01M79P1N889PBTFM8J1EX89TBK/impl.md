---
schema: change.v1
run_id: run_01M79P1N889PBTFM8J1EX89TBK
step: impl
attempt: 1
skill: implement@1.2.0
inputs:
  - {path: artifacts/HMS-5/run_01M79P1N889PBTFM8J1EX89TBK/design.md, sha256: 799df78b60dcfbc79dc1465cb608d0c935c43fb6ef1c5d88f90d28404b564347}
  - {path: artifacts/HMS-5/run_01M79P1N889PBTFM8J1EX89TBK/split.md, sha256: db8ef532a356ccb24887a9fadcb6dc8a8fbc5338ae76d37cb71b224324490de8}
  - {path: context/conventions.md, sha256: dc53d94ec34eb193eeb06fdd0db19eb86a24c29f7d3c434e7c212d61038ca852}
  - {path: context/generated/module-map.json, sha256: 4158c8fcd239c22cb8f1029dd11f61d8839867ff8088bd9ab76197e2db96db4b}
pack_sha: {sample-system: 7681f87749883d71abb4011d118366ca1242e1cc}
code:
  - repo: sample-system
    base_sha: 7681f87749883d71abb4011d118366ca1242e1cc
    head_sha: 38a07d57032d92bfaa63deba9850290fae93aef1
    diff_sha256: 5c2904ce1f7d18603aff130952eed5b2d31720de5fccbdfb5e7af9e90b32717e
    files: [api/src/repositories/patientRepo.ts, api/src/services/patientService.ts, api/src/types.ts, web/src/api/types.ts, web/src/pages/PatientDetailPage.tsx]
claims:
  - {id: C1, text: "Patient SQL lives in the `PatientRepo` class.", cites: ["context/generated/module-map.json#sym:sample-system:api/src/repositories/patientRepo.ts:PatientRepo"]}
  - {id: C2, text: "Patient rules live in the `PatientService` class.", cites: ["context/generated/module-map.json#sym:sample-system:api/src/services/patientService.ts:PatientService"]}
  - {id: C3, text: "The API's patient detail type is `PatientDetail` in api/src/types.ts.", cites: ["context/generated/module-map.json#sym:sample-system:api/src/types.ts:PatientDetail"]}
  - {id: C4, text: "The web's patient detail type is `PatientDetail` in web/src/api/types.ts.", cites: ["context/generated/module-map.json#sym:sample-system:web/src/api/types.ts:PatientDetail"]}
  - {id: C5, text: "The patient page component is `PatientDetailPage`.", cites: ["context/generated/module-map.json#sym:sample-system:web/src/pages/PatientDetailPage.tsx:PatientDetailPage"]}
  - {id: C6, text: "The design adds `PatientRepo.visitSummary(id)` to count COMPLETED and NO_SHOW appointments.", cites: ["artifacts/HMS-5/run_01M79P1N889PBTFM8J1EX89TBK/design.md#item:CO-1"]}
  - {id: C7, text: "The user chose the field shape `visitSummary { completed, noShow }`.", cites: ["runs/run_01M79P1N889PBTFM8J1EX89TBK/ide-answers.md#a-03"]}
  - {id: C8, text: "SQL belongs in `api/src/repositories` and rules in api/src/services.", cites: ["context/conventions.md#code-layout"]}
  - {id: C9, text: "Logs never carry request bodies, `contact details` or medical history.", cites: ["context/conventions.md#logging-and-data"]}
  - {id: C10, text: "The design places the block above the `Appointments` heading on PatientDetailPage.", cites: ["artifacts/HMS-5/run_01M79P1N889PBTFM8J1EX89TBK/design.md#item:CO-5"]}
trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9]
known: [S-1, S-2, S-3]
assumed: []
unknown: []
questions: []
summary: "Adds visitSummary { completed, noShow } to the patient detail. PatientRepo.visitSummary runs one count per patient with status filters for COMPLETED and NO_SHOW and converts both to numbers. PatientService.get attaches it, both PatientDetail types declare it, and PatientDetailPage shows it above the Appointments heading."
---
# Change: HMS-5

## Summary
This change implements the design's CO-1 to CO-5 and EP-1 to EP-3:
- **CO-1:** `PatientRepo` gains `visitSummary(id)`. It runs one query that counts the patient's appointments with `count(*) FILTER (WHERE status = 'COMPLETED')` and `FILTER (WHERE status = 'NO_SHOW')`, filtered by a bound `patient_id = $1` parameter.
  - Postgres returns `count(*)` as text, so both values go through `Number()`, the same pattern as `appointmentCount` (threat T-7).
  - A patient with no rows gets 0 and 0.
- **CO-2:** `PatientService.get` adds `visitSummary` to the detail. POST and PATCH build their response with the same `get`, so EP-2 and EP-3 carry the field with no extra code.
- **CO-3 and CO-4:** both `PatientDetail` types declare `visitSummary: { completed: number; noShow: number }` (A-03). `PatientRepo.get` now omits `visitSummary` as well as `appointments` from its return type, because the service adds both.
- **CO-5:** `PatientDetailPage` shows a "Visit summary" heading and a details list ("Completed", "No-show") directly above the `Appointments` heading. Nothing else on the page changed.
- No logging, storage or URL use was added (C9).

## Changes
- api/src/repositories/patientRepo.ts: CO-1. New `visitSummary(id)` method; the return type of `get` now omits `visitSummary`.
- api/src/services/patientService.ts: CO-2. `get` adds `visitSummary`, which also covers EP-1, EP-2 and EP-3.
- api/src/types.ts: CO-3. `PatientDetail.visitSummary`.
- web/src/api/types.ts: CO-4. `PatientDetail.visitSummary`.
- web/src/pages/PatientDetailPage.tsx: CO-5. Visit summary block above Appointments.
- Tests: none added. The design asks for no regression test, and the unit-test step owns the new tests. The existing API patient tests use `toMatchObject`, so the extra field doesn't break them.

## Known
- S-1: the counts are scoped by patient_id, cover all time with no period filter, and use the shape you chose (C6, C7).
- S-2: only COMPLETED and NO_SHOW are counted, by status not by date, so a past appointment that is still Scheduled counts as neither. A patient with no rows gets 0 and 0.
- S-3: the only page edit is the new block above the existing Appointments heading (C10). The medical history and `AppointmentTable` are untouched.

## Assumed
None.

## Unknown
None.

## Open questions
None.
