---
schema: change.v1
run_id: run_01M5VBPY40KETZZR1EY7H5KK0V
step: impl
attempt: 1
skill: implement@1.0.1
inputs:
  - path: artifacts/HMS-1/run_01M5VBPY40KETZZR1EY7H5KK0V/design.md
    sha256: b35d0758b7a70010b1e5bf16befa8a356a6c9c2b09beb353e093f34384cad393
  - path: artifacts/HMS-1/run_01M5VBPY40KETZZR1EY7H5KK0V/split.md
    sha256: 8193194ed9f5168ece5dab22f4c694ae9fe98e787bfd5dd0a55ae234091fed5e
  - path: context/conventions.md
    sha256: dc53d94ec34eb193eeb06fdd0db19eb86a24c29f7d3c434e7c212d61038ca852
  - path: context/generated/module-map.json
    sha256: 31d9ca15805fc1b6488e8da89fdb0ee7714adbcdfda176d69581ab8f3d6482da
pack_sha:
  sample-system: 415369398bdad3122ac084a00766a01a3034f900
code:
  - repo: sample-system
    base_sha: 415369398bdad3122ac084a00766a01a3034f900
    head_sha: 69f98a95926be7cfe5530d2cea11799486225c7e
    diff_sha256: 2bdea1042d97fc708d0c534a27675e5e201e462e3310a5affbf392d6a4960412
    files:
      - api/src/repositories/patientRepo.ts
      - api/src/services/patientService.ts
      - api/src/types.ts
      - web/src/api/client.ts
      - web/src/api/types.ts
      - web/src/pages/PatientsPage.tsx
claims:
  - id: C1
    text: "SQL lives in `api/src/repositories` and rules in `api/src/services`."
    cites: ["context/conventions.md#code-layout"]
  - id: C2
    text: "Lists answer items, page, `page_size` and total, 20 per page."
    cites: ["context/conventions.md#lists"]
  - id: C3
    text: "Logs never carry `medical history`."
    cites: ["context/conventions.md#logging-and-data"]
  - id: C4
    text: "Patient list SQL lives in `PatientRepo`."
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/repositories/patientRepo.ts:PatientRepo"]
  - id: C5
    text: "Patient list rules live in `PatientService`."
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/services/patientService.ts:PatientService"]
  - id: C6
    text: "The web patient list page is `PatientsPage`."
    cites: ["context/generated/module-map.json#sym:sample-system:web/src/pages/PatientsPage.tsx:PatientsPage"]
  - id: C7
    text: "The web API client is the `api` symbol in client.ts."
    cites: ["context/generated/module-map.json#sym:sample-system:web/src/api/client.ts:api"]
  - id: C8
    text: "The web app has a date-time formatter `dateTime`."
    cites: ["context/generated/module-map.json#sym:sample-system:web/src/lib/format.ts:dateTime"]
  - id: C9
    text: "The web app reads settings through `useSettings`."
    cites: ["context/generated/module-map.json#sym:sample-system:web/src/hooks/useSettings.tsx:useSettings"]
  - id: C10
    text: "Patient routes are defined in `patientsRouter`."
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/routes/patients.ts:patientsRouter"]
  - id: C11
    text: "The design's list item addition is `next_appointment`, an object or null."
    cites: ["artifacts/HMS-1/run_01M5VBPY40KETZZR1EY7H5KK0V/design.md#L174"]
trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10, REQ-11, REQ-12, REQ-13]
known: [S-2, S-3, S-5]
assumed: [S-1, S-4]
unknown:
  - {id: U-1, blocking: false}
  - {id: U-2, blocking: false}
questions:
  - id: Q-01
    blocking: false
    ask: tech lead
    text: "The new list field next_appointment {id, starts_at, doctor_name} is snake_case as the design specifies, while existing patient item fields are camelCase (fullName, registeredAt). Keep the design's names or switch to camelCase?"
    why: "The design's naming was assumed (its Q-04); the product code uses camelCase for item fields, so the API now mixes both styles."
    evidence: ["artifacts/HMS-1/run_01M5VBPY40KETZZR1EY7H5KK0V/design.md#L174", "context/conventions.md#lists"]
    options: ["Keep snake_case as designed", "Rename to nextAppointment {id, startsAt, doctorName}"]
    default: "Keep the design's names for this ticket."
  - id: Q-02
    blocking: false
    ask: product owner
    text: "Is the cell text \"<date time> · <doctor name>\" acceptable for the Next appointment column?"
    why: "The design fixes the parts (start in the hospital zone and doctor name) but not how they are joined in one cell."
    evidence: ["artifacts/HMS-1/run_01M5VBPY40KETZZR1EY7H5KK0V/split.md#L157"]
    options: ["Keep as is", "Show the doctor on a second line"]
    default: "Keep as is."
summary: "GET /api/patients list items gain next_appointment (id, starts_at, doctor_name) or null, computed in one lateral join per page in PatientRepo; PatientsPage shows it in a new Next appointment column with a link, or None."
---
# Change: HMS-1

## Summary
The patient list now carries each patient's next appointment, as the design specifies. In the API, `PatientRepo.list` wraps the existing page query and adds one `LEFT JOIN LATERAL` per page that picks the earliest appointment with status `SCHEDULED` starting after the current instant, with its doctor's name; all values, including the current instant, are bound parameters. Search, status filter, sort order and page size are unchanged, and the patient columns selected are still the existing list columns, so no medical history is read. `PatientService.list` passes the service clock to the repository and returns `PatientListItem`s. In the web app the client's patient list type becomes `PatientListItem`, and `PatientsPage` adds a "Next appointment" column that shows the start with the existing `dateTime` formatter in the hospital zone from `useSettings` (falling back to the default zone when the setting is missing or invalid), plus the doctor's name as plain text, linked to `/appointments/:id`; with no next appointment it shows "None". `patientsRouter` and `AppointmentRepo` are untouched.

## Changes
- `api/src/types.ts`: new `NextAppointment` and `PatientListItem` types (PatientService, PatientRepo).
- `api/src/repositories/patientRepo.ts`: `list` takes `now` and adds the lateral next-appointment lookup; maps it to `next_appointment` or `null` (PatientRepo).
- `api/src/services/patientService.ts`: `list` returns `Page<PatientListItem>` and passes `this.clock()` (PatientService).
- `web/src/api/types.ts`: the same `NextAppointment` and `PatientListItem` types for the web (api client (client.ts)).
- `web/src/api/client.ts`: `patients` returns `Page<PatientListItem>` (api client (client.ts)).
- `web/src/pages/PatientsPage.tsx`: new column, link and "None" text, using `useSettings` and `dateTime` with a zone fallback (PatientsPage, dateTime formatter, useSettings).

## Known
- S-2: the lateral lookup filters to status `SCHEDULED` and start after now, ordered by start, so Cancelled, Completed and No-show never appear and a Cancelled appointment before a later Scheduled one is skipped; `null` renders "None" (C4).
- S-3: the value links to `/appointments/:id` (C6).
- S-5: the page query, its search, filter, order and page size are unchanged; the list still selects only the existing list columns, so no medical history is read or logged (C1, C2, C3, C4, C10).

## Assumed
- S-1: the column, start time and doctor name are built in PatientRepo, PatientService and PatientsPage (C4, C5, C6, C7). The hospital-zone display assumes `useSettings` carries the zone (C8, C9), as the design assumes; a missing or invalid zone falls back to the default zone.
- S-4: the doctor's name comes from the appointment's doctor row with no status filter, so an Inactive doctor's name is still shown. Basis: the design's S-4 assumption that doctors with appointments are made Inactive, never deleted.

## Unknown
- U-1 (non-blocking): the API now mixes snake_case `next_appointment` (C11) with camelCase item fields (Q-01).
- U-2 (non-blocking): the exact cell layout of time and doctor name (Q-02).

## Open questions
- Q-01 (non-blocking, tech lead): keep the design's snake_case field names or switch to camelCase? Default: keep the design's names.
- Q-02 (non-blocking, product owner): is "<date time> · <doctor name>" in one cell acceptable? Default: keep as is.
