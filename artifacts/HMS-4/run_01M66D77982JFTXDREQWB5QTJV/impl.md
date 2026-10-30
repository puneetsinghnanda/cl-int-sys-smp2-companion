---
schema: change.v1
run_id: run_01M66D77982JFTXDREQWB5QTJV
step: impl
attempt: 1
skill: implement@1.0.1
inputs:
  - path: artifacts/HMS-4/run_01M66D77982JFTXDREQWB5QTJV/design.md
    sha256: b31d420e0453c1145cb4585599d95a4f6cec5281f16b6f5300467e2fc94ed5d3
  - path: artifacts/HMS-4/run_01M66D77982JFTXDREQWB5QTJV/split.md
    sha256: f49a45c822dcc797bdba004c411c08fa32db8a41468d2957a34eb264d72f9764
  - path: context/conventions.md
    sha256: dc53d94ec34eb193eeb06fdd0db19eb86a24c29f7d3c434e7c212d61038ca852
  - path: context/generated/module-map.json
    sha256: 04f84ff09b4ad6ffaba59ec846716d25f03c8aae22292958a47ebb9247632345
pack_sha:
  sample-system: bd98ff08b83f8762c5e5c073ba5fadef88169b33
code:
  - repo: sample-system
    base_sha: 6358971fc7b27e2198f87a516dad8c5a58cc38a6
    head_sha: baf4bed63d2a30920fa8b4e97721e3f533f4ce0e
    diff_sha256: 6288636e03c2eab0534a762fca32bf503cc19ff7063e8c6f398544f532d5598f
    files:
      - api/src/repositories/patientRepo.ts
      - api/src/types.ts
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
    text: "The list page is `PatientsPage`."
    cites: ["context/generated/module-map.json#sym:sample-system:web/src/pages/PatientsPage.tsx:PatientsPage"]
  - id: C5
    text: "`PatientsPage.tsx` has the `startsAt` helper reused for the new column."
    cites: ["context/generated/module-map.json#sym:sample-system:web/src/pages/PatientsPage.tsx:startsAt"]
  - id: C6
    text: "Instants are stored in `UTC`."
    cites: ["context/architecture.md#time-and-slots"]
  - id: C7
    text: "Logs never carry `request bodies`; this change adds no logging."
    cites: ["context/conventions.md#logging-and-data"]
trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10, REQ-11, REQ-12, REQ-13, REQ-14, REQ-15]
known: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10, REQ-11, REQ-12]
assumed: [REQ-13, REQ-14, REQ-15]
unknown:
  - {id: U-1, blocking: false}
  - {id: U-2, blocking: false}
questions:
  - id: Q-01
    blocking: false
    ask: product owner
    text: Where should the Last visit column sit in the patient list?
    why: The ticket names the column but not its position; it was placed before Next appointment so the Next appointment column stays the last cell, as existing tests and users expect.
    evidence: ["artifacts/HMS-4/run_01M66D77982JFTXDREQWB5QTJV/split.md#L77"]
    options: [before Next appointment, after Next appointment]
    default: before Next appointment
  - id: Q-02
    blocking: false
    ask: product owner
    text: Should the Last visit cell show only the date, or the same date and time as Next appointment?
    why: The startsAt helper shows date and time; REQ-2 says date while REQ-14 says the same format as Next appointment. The code follows REQ-14.
    evidence: ["artifacts/HMS-4/run_01M66D77982JFTXDREQWB5QTJV/split.md#L81"]
    options: [same date and time as Next appointment, date only]
    default: same date and time as Next appointment
summary: "The patient list query now returns last_visit_at (the start of each patient's latest COMPLETED appointment, or null) and PatientsPage shows it in a new Last visit column, formatted in the hospital zone with the existing startsAt helper and '-' when null. Paging, search, sort, the Next appointment column and the no-medical-history rule are unchanged."
---
# Change: HMS-4

## Summary
- **PatientRepo list query** (api, changed). A scalar subquery in the existing per-page SELECT returns `last_visit_at`: `max(starts_at)` over the patient's appointments with `status = 'COMPLETED'`. Cancelled, No-show and Scheduled appointments never count. It takes no request input, and the page query's WHERE, ORDER BY, LIMIT/OFFSET and count are untouched. `LIST_COLUMNS` is unchanged, so no medical history is selected. The field name is the design's default for its Q-01.
- **PatientListItem type** (api and web, changed). Adds `last_visit_at: string | null`, always present, as an ISO UTC instant.
- **PatientsPage Last visit column** (web, changed). A new "Last visit" column shows `startsAt(last_visit_at, timeZone)`, the same hospital-zone formatter as Next appointment, rendered as text, or "-" when null. The Next appointment column is unchanged.
- **PatientService, patientsRouter, API client, appointment table**: no edits; they stay as they are.
- **No logging is added** in the API, the client or the page (threat T-9).

## Changes
- `api/src/repositories/patientRepo.ts`: PatientRepo list query. Adds the last-visit subquery and maps it to an ISO string or null (REQ-2, REQ-4, REQ-5, REQ-6, REQ-13).
- `api/src/types.ts`: the API's PatientListItem gains `last_visit_at` (REQ-2).
- `web/src/api/types.ts`: the web's PatientListItem gains `last_visit_at` (REQ-2).
- `web/src/pages/PatientsPage.tsx`: the PatientsPage Last visit column (REQ-1, REQ-3, REQ-7, REQ-14, REQ-15).
- `api/test/patients.test.ts`: the PAT-8 test pins the exact list item keys, and the design changes that shape, so `last_visit_at` is added to the expected keys. The no-medical-history check (REQ-12) still runs.
- `web/src/__tests__/PatientsPage.test.tsx`: the shared fixture gains `last_visit_at: null` so it matches the changed type (REQ-2). No assertion changed.

## Known
- REQ-1 to REQ-3: the column, its value from the latest Completed appointment, and the hospital zone via `startsAt`.
- REQ-4 to REQ-6: the filter `status = 'COMPLETED'` excludes Cancelled, No-show and Scheduled.
- REQ-7: a null value renders "-".
- REQ-8 to REQ-10: the paged query, its search WHERE and its ORDER BY are unchanged, so it still returns 20 per page.
- REQ-11: the Next appointment column markup is unchanged.
- REQ-12: `LIST_COLUMNS` still has no medical history columns, and the PAT-8 test still checks the exact keys.

## Assumed
- REQ-13: "most recent" is `max(starts_at)`, per the design.
- REQ-14: the format is the existing `startsAt` (date and time), the same as Next appointment.
- REQ-15: the displayed value is the start instant of that appointment, in the hospital zone.

## Unknown
- U-1 (non-blocking): the column position. It was placed before Next appointment (Q-01).
- U-2 (non-blocking): date only, or date and time (Q-02).

## Open questions
- Q-01 (non-blocking, product owner): Where should the column go? Default: before Next appointment.
- Q-02 (non-blocking, product owner): Date only, or date and time? Default: the same date and time as Next appointment.
