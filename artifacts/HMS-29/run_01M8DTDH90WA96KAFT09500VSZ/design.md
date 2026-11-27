---
schema: design.v1
run_id: run_01M8DTDH90WA96KAFT09500VSZ
step: design
attempt: 2
skill: design@1.10.0
inputs:
  - path: artifacts/HMS-29/run_01M8DTDH90WA96KAFT09500VSZ/split.md
    sha256: e3d898f637b4c06aca5f52dd5b2aa2e8805aa36cce0d6c292f70756d736be215
  - path: artifacts/HMS-29/run_01M8DTDH90WA96KAFT09500VSZ/req.md
    sha256: ff03e26e49a1e532c308fc010cae551de526a9e156225092dc2de150e408fe5b
  - path: context/intent-baseline.md
    sha256: 9bfd821aeb2fb9524863e346707e049e4fcfb949a6e55b90223a79194946a215
  - path: context/architecture.md
    sha256: 9faab093d10933a46f6473b25c80728f8745e597100e650cd2537bbafb2b595a
  - path: context/generated/module-map.json
    sha256: 4158c8fcd239c22cb8f1029dd11f61d8839867ff8088bd9ab76197e2db96db4b
  - path: context/generated/api-inventory.json
    sha256: ba024ab1b4b4151d26ce1afa77bd2dab1cec645ea737ce37f98e0e8674f795e9
pack_sha:
  sample-system: 7681f87749883d71abb4011d118366ca1242e1cc
resolves:
  - id: F-1
    how: "C1 and C18 now cite the SettingsPage lines where RefList renders both tables and the Doctors cell."
  - id: F-2
    how: "C3 and C19 now cite the RefEntry `doctorCount` field and the API mapping from `doctor_count`."
  - id: F-3
    how: "C20 to C23 now cite that GET /api/doctors filters by status only when one is given."
claims:
  - id: C1
    text: "The Settings page renders the department and the specialization table with `RefList`, once with kind departments and once with kind specializations."
    cites:
      - "context/generated/module-map.json#sym:sample-system:web/src/pages/SettingsPage.tsx:RefList"
      - "sample-system:web/src/pages/SettingsPage.tsx#L16-L17"
  - id: C2
    text: "The Settings page component `SettingsPage` is declared in web/src/pages/SettingsPage.tsx, so that file exists."
    cites: ["context/generated/module-map.json#sym:sample-system:web/src/pages/SettingsPage.tsx:SettingsPage"]
  - id: C3
    text: "Each Settings row is a RefEntry whose doctor count field is `doctorCount`, next to id, name and status."
    cites:
      - "context/generated/module-map.json#sym:sample-system:web/src/api/types.ts:RefEntry"
      - "sample-system:web/src/api/types.ts#L18-L23"
  - id: C4
    text: "The doctor list is the web route `/doctors`."
    cites: ["context/generated/api-inventory.json#ui:/doctors"]
  - id: C5
    text: "The doctor list page is the component `DoctorsPage`."
    cites: ["context/generated/module-map.json#sym:sample-system:web/src/pages/DoctorsPage.tsx:DoctorsPage"]
  - id: C6
    text: "The doctor list reads its department filter from the URL parameter `departmentId`."
    cites: ["sample-system:web/src/pages/DoctorsPage.tsx#L21"]
  - id: C7
    text: "The doctor list reads its specialization filter from the URL parameter `specializationId`."
    cites: ["sample-system:web/src/pages/DoctorsPage.tsx#L22"]
  - id: C8
    text: "The doctor list reads its status filter from the URL with `params.get('status')`, empty when the parameter is absent."
    cites: ["sample-system:web/src/pages/DoctorsPage.tsx#L23"]
  - id: C9
    text: "The web client has a `query` helper that builds a URL query string from filter values."
    cites: ["context/generated/module-map.json#sym:sample-system:web/src/api/client.ts:query"]
  - id: C10
    text: "The doctor list endpoint GET /api/doctors already exists in `api/src/routes/doctors.ts`."
    cites: ["context/generated/api-inventory.json#http:GET:/api/doctors"]
  - id: C11
    text: "The department list endpoint GET /api/departments already exists in `api/src/routes/departments.ts`."
    cites: ["context/generated/api-inventory.json#http:GET:/api/departments"]
  - id: C12
    text: "The specialization list endpoint GET /api/specializations already exists in `api/src/routes/specializations.ts`."
    cites: ["context/generated/api-inventory.json#http:GET:/api/specializations"]
  - id: C13
    text: "The web app talks to the API only through `web/src/api/client.ts`."
    cites: ["context/architecture.md#components"]
  - id: C14
    text: "The Settings count is a `doctor_count` of every doctor using the entry, with no condition on doctor status."
    cites: ["sample-system:api/src/repositories/referenceRepo.ts#L24"]
  - id: C15
    text: "The reference list behind the Settings tables and the doctor list filter options returns every entry, Active or Inactive, ordered by `lower(r.name)`."
    cites: ["sample-system:api/src/repositories/referenceRepo.ts#L29"]
  - id: C16
    text: "The doctor list already uses `Link` from `react-router-dom` for in-app links."
    cites: ["sample-system:web/src/pages/DoctorsPage.tsx#L1"]
  - id: C17
    text: "The accessible name follows the pattern `3 doctors in Cardiology` for both kinds, with \"1 doctor in <name>\" for a count of 1."
    cites:
      - "artifacts/HMS-29/run_01M8DTDH90WA96KAFT09500VSZ/req.md#item:REQ-6"
      - "runs/run_01M8DTDH90WA96KAFT09500VSZ/ide-answers.md#a-01"
  - id: C18
    text: "In RefList, the Doctors cell of each row prints `e.doctorCount` as plain text today."
    cites: ["sample-system:web/src/pages/SettingsPage.tsx#L61"]
  - id: C19
    text: "The API maps the SQL count to the response field with `doctorCount: Number(r.doctor_count)`, so the web field is the same count as C14."
    cites: ["sample-system:api/src/repositories/referenceRepo.ts#L13"]
  - id: C20
    text: "The doctor list SQL adds a status condition only `if (f.status !== undefined)`."
    cites: ["sample-system:api/src/repositories/doctorRepo.ts#L92"]
  - id: C21
    text: "The doctor service reads the status filter with `f.oneOf('status', RECORD_STATUSES)`, so a missing status parameter gives no status filter."
    cites: ["sample-system:api/src/services/doctorService.ts#L45"]
  - id: C22
    text: "The doctor service reads `query.departmentId` and `query.specializationId` from the request query."
    cites: ["sample-system:api/src/services/doctorService.ts#L40-L42"]
  - id: C23
    text: "The GET /api/doctors route passes `req.query` to the doctor service list."
    cites: ["sample-system:api/src/routes/doctors.ts#L16"]
trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8]
known: [S-1, S-2, S-3, S-4, CO-1, CO-2, CO-3, CO-4, CO-5, EP-1, EP-2, EP-3]
assumed: []
unknown: []
questions: []
components:
  - id: CO-1
    name: RefList (Settings page department and specialization tables)
    module: module:sample-system:web
    change: changed
    cites:
      - context/generated/module-map.json#sym:sample-system:web/src/pages/SettingsPage.tsx:RefList
      - sample-system:web/src/pages/SettingsPage.tsx#L16-L17
      - sample-system:web/src/pages/SettingsPage.tsx#L61
  - id: CO-2
    name: doctorsLinkLabel (accessible-name helper, local to web/src/pages/SettingsPage.tsx)
    module: module:sample-system:web
    change: new
    cites:
      - context/generated/module-map.json#sym:sample-system:web/src/pages/SettingsPage.tsx:SettingsPage
      - artifacts/HMS-29/run_01M8DTDH90WA96KAFT09500VSZ/req.md#item:REQ-6
      - runs/run_01M8DTDH90WA96KAFT09500VSZ/ide-answers.md#a-01
  - id: CO-3
    name: DoctorsPage (doctor list, reads filters from the URL)
    module: module:sample-system:web
    change: existing
    cites:
      - context/generated/module-map.json#sym:sample-system:web/src/pages/DoctorsPage.tsx:DoctorsPage
      - context/generated/api-inventory.json#ui:/doctors
  - id: CO-4
    name: query (URL query-string helper in web/src/api/client.ts)
    module: module:sample-system:web
    change: existing
    cites:
      - context/generated/module-map.json#sym:sample-system:web/src/api/client.ts:query
  - id: CO-5
    name: RefEntry (web type of a Settings row)
    module: module:sample-system:web
    change: existing
    cites:
      - context/generated/module-map.json#sym:sample-system:web/src/api/types.ts:RefEntry
      - sample-system:web/src/api/types.ts#L18-L23
endpoints:
  - id: EP-1
    method: GET
    path: /api/doctors
    change: existing
    fields: []
    cites:
      - context/generated/api-inventory.json#http:GET:/api/doctors
      - sample-system:api/src/routes/doctors.ts#L16
      - sample-system:api/src/services/doctorService.ts#L40-L45
      - sample-system:api/src/repositories/doctorRepo.ts#L90-L92
  - id: EP-2
    method: GET
    path: /api/departments
    change: existing
    fields: []
    cites:
      - context/generated/api-inventory.json#http:GET:/api/departments
      - sample-system:api/src/repositories/referenceRepo.ts#L13
  - id: EP-3
    method: GET
    path: /api/specializations
    change: existing
    fields: []
    cites:
      - context/generated/api-inventory.json#http:GET:/api/specializations
      - sample-system:api/src/repositories/referenceRepo.ts#L13
answers: []
---
# Design: HMS-29

## Overview
This is a web-only change to the Settings page. The page renders both tables with `RefList`, once
for departments and once for specializations (CO-1, C1). Today each row's Doctors cell prints
`e.doctorCount` as plain text (C18). That field is declared on `RefEntry` (C3). The API fills it
from the SQL `doctor_count` (C19), which counts doctors of every status (C14).

When the count is above 0, the cell becomes a react-router `Link` (C16) to the doctor list route
`/doctors` (C4). A department row links to `/doctors?departmentId=<id>` and a specialization row
to `/doctors?specializationId=<id>`. These are the URL parameters the doctor list already reads
(C6, C7), and the link is built with the existing `query` helper (CO-4, C9).

The link sets no `status` parameter, so the doctor list's status filter stays empty, which is
"Any status" (C8). The doctor list passes its filters to GET /api/doctors through the API client
(C13). The route hands `req.query` to the service (C23), which reads the department and
specialization ids (C22). A missing status gives no status filter (C21), and the SQL adds a status
condition only when one is given (C20). So the linked list returns doctors of every status and
matches the Settings count (REQ-3).

A row with count 0 keeps the plain number (REQ-4). Active and Inactive rows take the same branch,
and the doctor list's filter options include Inactive entries (C15), so the filter shows the
entry's name for Inactive rows too (REQ-5). A small new helper, `doctorsLinkLabel` (CO-2), builds
the link's `aria-label`: "<n> doctors in <name>", or "1 doctor in <name>" when the count is 1
(C17). No API, type, database or doctor list change is needed (EP-1 to EP-3, CO-3 and CO-5 stay
as they are). That keeps REQ-7 and REQ-8.

Trade-offs:
- The `aria-label` replaces the visible number as the accessible name. Screen readers hear the full sentence while the visible column stays a bare number, as before (REQ-7). The cost is that the label must stay in step with the visible count; both come from the same `doctorCount` field (C3).
- `doctorsLinkLabel` lives in SettingsPage.tsx, not a shared lib. This keeps the change in one file. The cost is that a later screen would have to move it to share it.
- The URL parameter name is chosen from the row's kind ("departments" gives `departmentId`, "specializations" gives `specializationId`). This needs no new prop. The cost is one small mapping in RefList.
- The link leaves the status parameter out rather than sending an empty value. The doctor list treats a missing parameter as "Any status" (C8), and `query` drops empty values (C9).

Resolved review findings:
- F-1: C1 and C18 now cite the SettingsPage lines where RefList renders both tables and the Doctors cell.
- F-2: C3 and C19 now cite the RefEntry `doctorCount` field and the API mapping from `doctor_count`.
- F-3: C20 to C23 now cite that GET /api/doctors filters by status only when one is given.

## Components
- CO-1 RefList, changed (module:sample-system:web): renders both Settings tables (C1). When `doctorCount` is above 0, the Doctors cell (C18) renders `Link` to `/doctors` with `departmentId` or `specializationId` set to the row id; otherwise it shows the plain number. The other cells, the actions and the add form are untouched.
- CO-2 doctorsLinkLabel, new (module:sample-system:web, a local function in web/src/pages/SettingsPage.tsx, which exists per C2): (count, name) gives "<n> doctors in <name>", or "1 doctor in <name>" for a count of 1. Used as the link's `aria-label`.
- CO-3 DoctorsPage, existing: already reads `departmentId`, `specializationId` and `status` from the URL (C5 to C8). Not changed.
- CO-4 query, existing: builds the link's query string (C9). Not changed.
- CO-5 RefEntry, existing: already has id, name, status and `doctorCount` (C3). Not changed.
- EP-1 GET /api/doctors, existing: no new or changed fields. It filters by department and specialization when given, and by status only when given (C10, C20 to C23).
- EP-2 GET /api/departments, existing: no new or changed fields. It returns `doctorCount` per entry (C11, C19).
- EP-3 GET /api/specializations, existing: no new or changed fields. It returns `doctorCount` per entry (C12, C19).

Story mapping:
- S-1 (department link): CO-1, CO-3, CO-4, EP-1, EP-2.
- S-2 (specialization link): CO-1, CO-3, CO-4, EP-1, EP-3.
- S-3 (accessible name): CO-2, CO-1.
- S-4 (rest unchanged): CO-1 changes only the Doctors cell; CO-3, CO-5 and EP-1 to EP-3 stay as they are.

## Known
- S-1, S-2: served by CO-1 linking to the existing filters of CO-3 (C6, C7). GET /api/doctors with no status returns every status (C20, C21), and the Settings count covers every status (C14, C19).
- S-3: served by CO-2, with the wording from A-01 (C17).
- S-4: served by changing only the Doctors cell in CO-1 (C18); no API or doctor list change.
- CO-1: RefList renders both tables (C1), and its Doctors cell is at C18.
- CO-2: a new local function in SettingsPage.tsx, a file the module map shows exists (C2).
- CO-3: DoctorsPage is a module-map symbol (C5), at the route `/doctors` (C4).
- CO-4: query is a module-map symbol (C9).
- CO-5: RefEntry is declared with `doctorCount` (C3).
- EP-1, EP-2, EP-3: listed in the API inventory (C10, C11, C12) and not changed.

## Assumed
None.

## Unknown
None.

## Open questions
None open.

Answered in the IDE: none in this step. The earlier answer A-01 decides CO-2's wording (C17).
