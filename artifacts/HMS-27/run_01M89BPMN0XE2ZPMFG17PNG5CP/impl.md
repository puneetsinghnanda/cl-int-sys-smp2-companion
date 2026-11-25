---
schema: change.v1
run_id: run_01M89BPMN0XE2ZPMFG17PNG5CP
step: impl
attempt: 2
skill: implement@1.3.0
inputs:
  - path: artifacts/HMS-27/run_01M89BPMN0XE2ZPMFG17PNG5CP/design.md
    sha256: 9ec492060a643e697a01e4ddc48ebc0977bfdbd2103b3852f0d3eeeef01f6a1d
  - path: artifacts/HMS-27/run_01M89BPMN0XE2ZPMFG17PNG5CP/split.md
    sha256: 3f92b6f319ce0bc8d22bd48258fa93ea3a73480096486aaa8646237ad43331b4
  - path: context/conventions.md
    sha256: dc53d94ec34eb193eeb06fdd0db19eb86a24c29f7d3c434e7c212d61038ca852
  - path: context/generated/module-map.json
    sha256: 4158c8fcd239c22cb8f1029dd11f61d8839867ff8088bd9ab76197e2db96db4b
pack_sha:
  sample-system: 7681f87749883d71abb4011d118366ca1242e1cc
code:
  - repo: sample-system
    base_sha: 7681f87749883d71abb4011d118366ca1242e1cc
    head_sha: 8b2fdea35ef633ec96055e55583407cf1a9e2a75
    diff_sha256: 96268bb1cbb21c30e8aa9730e09686357fc44e46c505ce4900a8df7cc0aecae4
    files:
      - api/src/repositories/patientRepo.ts
      - api/src/services/patientService.ts
      - api/src/types.ts
      - api/test/patients.test.ts
      - web/src/__tests__/PatientsPage.test.tsx
      - web/src/api/types.ts
      - web/src/lib/format.ts
      - web/src/pages/PatientsPage.tsx
claims:
  - id: C1
    text: "The patient list SQL is in `PatientRepo` (api/src/repositories/patientRepo.ts)."
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/repositories/patientRepo.ts:PatientRepo"]
  - id: C2
    text: "The list row type `ListRow` gains the last-visit columns."
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/repositories/patientRepo.ts:ListRow"]
  - id: C3
    text: "`PatientService` parses not_seen, computes the cutoff and maps the last visit to a local date."
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/services/patientService.ts:PatientService", "artifacts/HMS-27/run_01M89BPMN0XE2ZPMFG17PNG5CP/design.md#item:CO-2"]
  - id: C4
    text: "The boolean flag is parsed with `oneOf`, as DoctorService parses with_today."
    cites: ["sample-system:api/src/services/doctorService.ts#L36"]
  - id: C5
    text: "The cutoff instant comes from `zonedToUtc` on the hospital-local day 12 calendar months before today."
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/lib/time.ts:zonedToUtc"]
  - id: C6
    text: "The last-visit date is the hospital-local day from `localDay`."
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/lib/time.ts:localDay"]
  - id: C7
    text: "The API type `PatientListItem` gains a last_visit field."
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/types.ts:PatientListItem"]
  - id: C8
    text: "The web type `PatientListItem` mirrors the API type."
    cites: ["context/generated/module-map.json#sym:sample-system:web/src/api/types.ts:PatientListItem"]
  - id: C9
    text: "`PatientsPage` shows the Last visit column and the Not seen for a year checkbox."
    cites: ["context/generated/module-map.json#sym:sample-system:web/src/pages/PatientsPage.tsx:PatientsPage"]
  - id: C10
    text: "The existing `dateTime` helper prints a time, so a date-only helper is added beside it."
    cites: ["context/generated/module-map.json#sym:sample-system:web/src/lib/format.ts:dateTime", "runs/run_01M89BPMN0XE2ZPMFG17PNG5CP/ide-answers.md#a-07"]
  - id: C11
    text: "The web list request goes through `api` in web/src/api/client.ts and its `query` builder."
    cites: ["context/generated/module-map.json#sym:sample-system:web/src/api/client.ts:api", "runs/run_01M89BPMN0XE2ZPMFG17PNG5CP/ide-answers.md#a-08"]
  - id: C12
    text: "The existing HMS-2 test helper `nextCell` took the row's last cell, which is now Last visit."
    cites: ["context/generated/module-map.json#sym:sample-system:web/src/__tests__/PatientsPage.test.tsx:nextCell"]
  - id: C13
    text: "Lists answer `{ items, page, page_size, total }`; total now counts the filtered patients."
    cites: ["context/conventions.md#lists"]
  - id: C14
    text: "The PAT-8 test pinned the exact list item keys and failed on the new `last_visit` key."
    cites: ["runs/run_01M89BPMN0XE2ZPMFG17PNG5CP/verifier/impl/a1/test.log#L86"]
trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10, REQ-11, REQ-12, REQ-13, REQ-14]
known: [CO-1, CO-2, CO-3, CO-4, CO-5, CO-7, CO-8, EP-1]
assumed: [CO-6]
unknown: []
questions: []
resolves:
  - id: H3
    how: "The PAT-8 test now expects the designed last_visit key and checks it carries only date and doctor_name."
summary: "GET /api/patients now returns last_visit (latest COMPLETED appointment: hospital-local date and doctor) and accepts not_seen=true, which keeps only patients whose last visit is before the same local date 12 months ago. The patient list shows a Last visit column and a Not seen for a year checkbox."
---
# Change: HMS-27

## Summary
The change follows the approved design:
- CO-1 (PatientRepo): adds a second `LEFT JOIN LATERAL` that picks each page row's latest COMPLETED appointment and its doctor's name. It also adds an optional `lastVisitBefore` condition on the latest COMPLETED `starts_at`. The condition goes in the shared WHERE, so the count query and the page query (before LIMIT/OFFSET) both filter. Never-visited patients have a NULL last visit and never match. The list still selects no medical-history columns.
- CO-2 (PatientService): parses `not_seen` with Fields `oneOf` over "true" and "false" (C4), so any other value is a 400 ValidationError. It takes today in the hospital zone and steps back 12 calendar months with `yearBefore`, clamping 29 February to the month's last day. It turns local midnight of that day into the UTC cutoff with `zonedToUtc` (C5), and maps each row's last-visit instant to `{ date: YYYY-MM-DD local, doctor_name }` (C6).
- CO-3 (route): unchanged. It already passes `req.query` to the service.
- CO-4 and CO-5: add `LastVisit` and `last_visit: LastVisit | null` to the API and web `PatientListItem` types (C7, C8).
- CO-7 (PatientsPage): adds a "Last visit" header after "Next appointment". Cells show "10 Mar 2026 · Dr A", or "Never" when there is no visit. A "Not seen for a year" checkbox sits next to the status select and goes through the existing `set` helper, so toggling it resets to page 1 (C9).
- CO-8: a new `dateOnly` helper in web/src/lib/format.ts formats the API's local day with no time-zone shift (C10).

Retry (H3): H3: The PAT-8 test now expects the designed last_visit key and checks it carries only date and doctor_name. The verifier showed that PAT-8 in api/test/patients.test.ts pins the exact key set of a list item (C14). The design adds `last_visit` to that response, so rule 10 allows updating the test. No product code changed in this retry.

Trade-off: `yearBefore` lives next to its only caller in patientService.ts rather than in lib/time.ts. That keeps the edit inside the design's components; the cost is that it is not shared with other time helpers.

## Changes
- api/src/repositories/patientRepo.ts — CO-1: the last-visit lateral join, the `lastVisitBefore` filter, `ListRow` columns and a `ListItemRow` return type.
- api/src/services/patientService.ts — CO-2: `not_seen` parsing, `yearBefore`, the UTC cutoff and the local-date mapping.
- api/src/types.ts — CO-4: `LastVisit` and `PatientListItem.last_visit`.
- web/src/api/types.ts — CO-5: the same types for the web app.
- web/src/lib/format.ts — CO-8: the new `dateOnly` helper.
- web/src/pages/PatientsPage.tsx — CO-7: the Last visit column and the Not seen for a year checkbox, with `not_seen` passed in the list filters.
- api/test/patients.test.ts — test updated under rule 10 (REQ-12): the PAT-8 key list now includes `last_visit`, and it checks that `last_visit` holds only `date` and `doctor_name`.
- web/src/__tests__/PatientsPage.test.tsx — test updated under rule 10 (REQ-1): the HMS-2 `nextCell` helper took the row's last cell, which is now Last visit (C12). It now finds the "Next appointment" column by its header, and the fixtures carry `last_visit: null`. No new tests: the unit-test step writes them.

## Known
- CO-1, CO-2, CO-4, CO-5, CO-7, CO-8: built as the design describes, in the files the module map proves (C1–C3, C7–C10).
- CO-3: no edit needed; the route already forwards the query.
- EP-1: the GET /api/patients contract is as designed. `query.not_seen`, `items[].last_visit` (`date` YYYY-MM-DD hospital-local, `doctor_name`) and `total` now counts filtered rows (C13).

## Assumed
- CO-6: the design marked the api client changed. `api.patients(params)` already sends every parameter through `query` (C11), so the page passes `not_seen` in its filters and web/src/api/client.ts needs no edit. Basis: A-08 confirmed the request goes through `api` and `query`. This satisfies that without a redundant method.

## Unknown
None.

## Open questions
None.
