---
schema: triage.v1
run_id: run_01M5XRKT5GS1J5S7SNA59XFVA2
step: triage
attempt: 1
skill: defect-triage@1.0.1
inputs:
  - path: artifacts/HMS-2/run_01M5XRKT5GS1J5S7SNA59XFVA2/ticket.md
    sha256: 9193bb2b2435df7faa3c804d9a84ca49adebc1e96d0e520805463037a9b1b9b7
  - path: context/generated/module-map.json
    sha256: 31d9ca15805fc1b6488e8da89fdb0ee7714adbcdfda176d69581ab8f3d6482da
  - path: context/architecture.md
    sha256: 9faab093d10933a46f6473b25c80728f8745e597100e650cd2537bbafb2b595a
  - path: context/data-model.md
    sha256: 2386ae151d54a73cdbe4fd9e0b69f8e8539c2870f5e52c12e5ed0191cfa95e55
pack_sha:
  sample-system: 415369398bdad3122ac084a00766a01a3034f900
claims:
  - id: C1
    text: "For a patient with no upcoming Scheduled appointment, the Next appointment column shows `None`."
    cites: ["artifacts/HMS-2/run_01M5XRKT5GS1J5S7SNA59XFVA2/ticket.md#L20"]
  - id: C2
    text: "The expected display for that patient is a `hyphen`."
    cites: ["artifacts/HMS-2/run_01M5XRKT5GS1J5S7SNA59XFVA2/ticket.md#L14"]
  - id: C3
    text: "Patients with an upcoming Scheduled appointment must stay `unchanged`."
    cites: ["artifacts/HMS-2/run_01M5XRKT5GS1J5S7SNA59XFVA2/ticket.md#L25"]
  - id: C4
    text: "The module map has a web page symbol named `PatientsPage`."
    cites: ["context/generated/module-map.json#sym:sample-system:web/src/pages/PatientsPage.tsx:PatientsPage"]
  - id: C5
    text: "The web app talks to the API only through `web/src/api/client.ts`."
    cites: ["context/architecture.md#components"]
  - id: C6
    text: "The API has a router symbol named `patientsRouter`."
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/routes/patients.ts:patientsRouter"]
trace: [HMS-2]
known: [SY-1, SY-2]
assumed: [A-1]
unknown: [{id: U-1, blocking: false}]
questions: []
candidates:
  - component: sym:sample-system:web/src/pages/PatientsPage.tsx:PatientsPage
    likelihood: high
    cites:
      - "artifacts/HMS-2/run_01M5XRKT5GS1J5S7SNA59XFVA2/ticket.md#L13"
      - "context/generated/module-map.json#sym:sample-system:web/src/pages/PatientsPage.tsx:PatientsPage"
  - component: sym:sample-system:api/src/routes/patients.ts:patientsRouter
    likelihood: low
    cites:
      - "context/generated/module-map.json#sym:sample-system:api/src/routes/patients.ts:patientsRouter"
      - "context/architecture.md#components"
---
# Defect triage: HMS-2

## Symptoms
- SY-1: for a patient with no upcoming Scheduled appointment, the "Next appointment" column on the
  patient list shows the word "None" (ticket L13, L20; C1).
- SY-2: the expected text is a hyphen "-" (ticket L14, L21; C2). Patients that do have an upcoming
  Scheduled appointment must keep showing its start and doctor, linked to the appointment (L25-L26; C3).

## Candidates
1. `sym:sample-system:web/src/pages/PatientsPage.tsx:PatientsPage` (high). The symptom is the text
   shown on the patient list, a display choice, and `PatientsPage` is the web page whose name and
   path match that list (C4, A-1). Next check: in `web/src/pages/PatientsPage.tsx`, find the branch
   of the Next appointment cell taken when the item has no next appointment and confirm it renders
   the literal "None".
2. `sym:sample-system:api/src/routes/patients.ts:patientsRouter` (low). The page gets its data from
   the API through the client (C5), and the patients router serves the list (C6). It would be the
   cause only if the API sent the string "None" for an empty value. This is not verified here (U-1).
   Next check: confirm the GET /api/patients list item carries an empty value, not the text "None",
   for such a patient. If the page check above confirms the literal, this candidate is ruled out.

Only one candidate is high: the symptom is the text shown for an empty value, which the page decides.

## Known
- SY-1: stated in the ticket's actual result (L20).
- SY-2: stated in the ticket's expected result and acceptance criteria (L21, L24-L26).

## Assumed
- A-1: `PatientsPage` renders the patient list and its "Next appointment" column. Basis: the ticket
  names the patient list (L13), and the module map's only patient list page by name and path is
  `web/src/pages/PatientsPage.tsx` (C4). This is an inference from the name and path, not a stated fact.

## Unknown
- U-1 (non-blocking): whether the API sends an empty value or the text "None" for a patient with no
  next appointment; the implement step settles it by reading the page code.

## Open questions
- None.
