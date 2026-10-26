---
schema: triage.v1
run_id: run_01M5W1KE4G93A1QNB55F4BCG74
step: triage
attempt: 1
skill: defect-triage@1.0.1
inputs:
  - path: artifacts/HMS-2/run_01M5W1KE4G93A1QNB55F4BCG74/ticket.md
    sha256: 92fcbbe36d4ecabbefe1e2fdca76b73da82b1bb7eb154b277ef5ffcb0de517ae
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
    cites: ["artifacts/HMS-2/run_01M5W1KE4G93A1QNB55F4BCG74/ticket.md#L20"]
  - id: C2
    text: "The expected display for that patient is a hyphen: the column shows `\"-\"`."
    cites: ["artifacts/HMS-2/run_01M5W1KE4G93A1QNB55F4BCG74/ticket.md#L21"]
  - id: C3
    text: "Patients with an upcoming Scheduled appointment must stay `unchanged`."
    cites: ["artifacts/HMS-2/run_01M5W1KE4G93A1QNB55F4BCG74/ticket.md#L25"]
  - id: C4
    text: "The patient list page is the `PatientsPage` component in the web app."
    cites: ["context/generated/module-map.json#sym:sample-system:web/src/pages/PatientsPage.tsx:PatientsPage"]
  - id: C5
    text: "The web app talks to the API only through `web/src/api/client.ts`."
    cites: ["context/architecture.md#components"]
trace: [HMS-2]
known: [SY-1, SY-2]
assumed: [A-1]
unknown: []
questions: []
candidates:
  - component: sym:sample-system:web/src/pages/PatientsPage.tsx:PatientsPage
    likelihood: high
    cites:
      - "artifacts/HMS-2/run_01M5W1KE4G93A1QNB55F4BCG74/ticket.md#L13"
      - "context/generated/module-map.json#sym:sample-system:web/src/pages/PatientsPage.tsx:PatientsPage"
  - component: module:sample-system:api
    likelihood: low
    cites:
      - "artifacts/HMS-2/run_01M5W1KE4G93A1QNB55F4BCG74/ticket.md#L18"
      - "context/architecture.md#components"
---
# Defect triage: HMS-2

## Symptoms
- SY-1: for a patient with no upcoming Scheduled appointment, the "Next appointment" column on the
  patient list shows the word "None" (ticket L13, L20; C1).
- SY-2: the expected text is a hyphen "-" (ticket L14, L21; C2). Patients that do have an upcoming
  Scheduled appointment must keep showing its start and doctor, linked to the appointment (L25-L26; C3).

## Candidates
1. `sym:sample-system:web/src/pages/PatientsPage.tsx:PatientsPage` (high). The text is a display
   choice on the patient list page (C4), so the word "None" is most likely a literal in the page's
   column rendering for an empty next appointment. Next check: in PatientsPage, find the branch that
   renders the column when the list item has no next appointment and confirm it outputs "None".
2. `module:sample-system:api` (low). The web app gets list data only through its API client (C5).
   The API could be sending the text "None", but the ticket describes a missing appointment, not a
   wrong value, so this is unlikely. Next check: confirm the GET /api/patients item carries an empty
   value (not the string "None") for such a patient.

Only one candidate is high: the symptom is purely what text the page shows for an empty value.

## Known
- SY-1: stated in the ticket's actual result (L20).
- SY-2: stated in the ticket's expected result and acceptance criteria (L21, L24-L26).

## Assumed
- A-1: the "Next appointment" column is rendered by `PatientsPage`. Basis: the ticket names the
  patient list (L13) and the module map lists `PatientsPage` as the patient list page (C4). The
  context pack predates the column, so the module map does not show the column itself.

## Unknown
- None.

## Open questions
- None.
