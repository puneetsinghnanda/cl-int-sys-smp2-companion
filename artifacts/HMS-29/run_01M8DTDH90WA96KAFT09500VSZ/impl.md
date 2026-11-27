---
schema: change.v1
run_id: run_01M8DTDH90WA96KAFT09500VSZ
step: impl
attempt: 1
skill: implement@1.4.0
inputs:
  - path: artifacts/HMS-29/run_01M8DTDH90WA96KAFT09500VSZ/design.md
    sha256: f2985e53c1f7b0d92979e0a8d545d25538e874fd7904f73750fd2e353158879e
  - path: artifacts/HMS-29/run_01M8DTDH90WA96KAFT09500VSZ/split.md
    sha256: e3d898f637b4c06aca5f52dd5b2aa2e8805aa36cce0d6c292f70756d736be215
  - path: context/conventions.md
    sha256: dc53d94ec34eb193eeb06fdd0db19eb86a24c29f7d3c434e7c212d61038ca852
  - path: context/generated/module-map.json
    sha256: 4158c8fcd239c22cb8f1029dd11f61d8839867ff8088bd9ab76197e2db96db4b
pack_sha:
  sample-system: 7681f87749883d71abb4011d118366ca1242e1cc
code:
  - repo: sample-system
    base_sha: 7681f87749883d71abb4011d118366ca1242e1cc
    head_sha: c603a43c178a3a420286c9f08d01c5ed21001863
    diff_sha256: b6adf076b3f658dc28b5e8adf1cc050ed7e3c6cb539e9849912e044112d814cf
    files: ["web/src/pages/SettingsPage.tsx"]
claims:
  - id: C1
    text: "The design marks `RefList`, which renders the Settings tables, as the changed component."
    cites: ["artifacts/HMS-29/run_01M8DTDH90WA96KAFT09500VSZ/design.md#item:CO-1"]
  - id: C2
    text: "The design adds a new local helper `doctorsLinkLabel` in SettingsPage.tsx for the link's accessible name."
    cites: ["artifacts/HMS-29/run_01M8DTDH90WA96KAFT09500VSZ/design.md#item:CO-2"]
  - id: C3
    text: "The link's query string is built with the existing `query` helper."
    cites: ["artifacts/HMS-29/run_01M8DTDH90WA96KAFT09500VSZ/design.md#item:CO-4"]
  - id: C4
    text: "The doctor list reads its department filter from the URL parameter `departmentId`."
    cites: ["artifacts/HMS-29/run_01M8DTDH90WA96KAFT09500VSZ/design.md#claim:C6"]
  - id: C5
    text: "The doctor list reads its specialization filter from the URL parameter `specializationId`."
    cites: ["artifacts/HMS-29/run_01M8DTDH90WA96KAFT09500VSZ/design.md#claim:C7"]
  - id: C6
    text: "The accessible name follows the pattern `3 doctors in Cardiology` for both kinds, with \"1 doctor in <name>\" for a count of 1."
    cites:
      - "artifacts/HMS-29/run_01M8DTDH90WA96KAFT09500VSZ/design.md#claim:C17"
      - "runs/run_01M8DTDH90WA96KAFT09500VSZ/ide-answers.md#a-01"
  - id: C7
    text: "In-app links use `Link` from `react-router-dom`, as the doctor list already does."
    cites: ["artifacts/HMS-29/run_01M8DTDH90WA96KAFT09500VSZ/design.md#claim:C16"]
  - id: C8
    text: "The endpoint `/api/doctors` stays existing; the design changes no API."
    cites: ["artifacts/HMS-29/run_01M8DTDH90WA96KAFT09500VSZ/design.md#item:EP-1"]
trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8]
known: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, CO-1, CO-2]
assumed: []
unknown: []
questions: []
summary: "On the Settings page, a Doctors count above 0 is now a link to the doctor list filtered to that department (departmentId) or specialization (specializationId), with no status filter, and an aria-label such as \"3 doctors in Cardiology\"; a count of 0 stays plain text. One file changed: web/src/pages/SettingsPage.tsx."
---
# Change: HMS-29

## Summary
This change implements CO-1 and CO-2 of the design in `web/src/pages/SettingsPage.tsx`, and
nothing else.

- CO-1 (`RefList`, C1): when a row's `doctorCount` is above 0, the Doctors cell is now a `Link`
  (C7) to `/doctors` plus a query string built with the existing `query` helper (C3). A department
  row sets `departmentId` and a specialization row sets `specializationId` (C4, C5). A small
  `DOCTOR_FILTER` map picks the parameter from the row's kind. The link sets no status, so the
  doctor list opens at "Any status" and lists as many doctors as the count (REQ-3). A count of 0
  is still the plain number (REQ-4). Active and Inactive rows take the same branch (REQ-5). The
  visible text is still the number, and the other cells, actions and add form are untouched
  (REQ-7).
- CO-2 (`doctorsLinkLabel`, C2): builds the link's `aria-label`: "<n> doctors in <name>", or
  "1 doctor in <name>" when the count is 1 (C6, REQ-6).

No API, type or doctor list code changed (C8, REQ-8). The design names no regression tests, and no
existing test renders the Settings page, so this step adds or changes no tests. Broad tests are
the unit-test step's job.

## Changes
- web/src/pages/SettingsPage.tsx (CO-1, CO-2): imports `Link` and `query`. Adds `DOCTOR_FILTER`
  and `doctorsLinkLabel`. The Doctors cell renders the link when the count is above 0, and the
  plain number otherwise.

## Known
- REQ-1: a department count links to `/doctors?departmentId=<id>` (C4).
- REQ-2: a specialization count links to `/doctors?specializationId=<id>` (C5).
- REQ-3: the link sets no status parameter, so the list shows "Any status".
- REQ-4: a count of 0 is not wrapped in a link.
- REQ-5: the link does not depend on the row's status.
- REQ-6: `doctorsLinkLabel` gives the accessible name from A-01 (C6).
- REQ-7: only the Doctors cell changed on the Settings page.
- REQ-8: the doctor list code is untouched (C8).
- CO-1: RefList changed as the design says (C1).
- CO-2: doctorsLinkLabel added as the design says (C2).

## Assumed
None.

## Unknown
None.

## Open questions
None.
