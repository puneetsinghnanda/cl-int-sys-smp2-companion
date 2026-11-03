---
schema: open-questions.v1
run_id: run_01M6GRSJH8H42FGV284MDTDAZY
ticket: HMS-4
workflow: brownfield-change@1.4.0
step: design@1.7.0
created_at: '2026-11-03T13:46:15Z'
status: open
---

# Open questions — HMS-4 (split step)

Answer each question below the line `answer:`. Keep the ids. Then run:
`/truminds:resume run_01M6GRSJH8H42FGV284MDTDAZY`

## Q-01 · non-blocking · ask: tech lead
**Question:** Does REQ-13 ('No medical history appears in the patient list') also cover the data the list receives from the server, or only what is shown on screen?
**Proposed default:** none
answer:
answered_by:
decision:

## Q-04 · non-blocking · ask: tech lead · round 2
**Question:** Does PatientService only pass the new last-visit field from PatientRepo through to the patients route, or does it build the list item shape and need the field added to that mapping?
**Why it matters:** Design must say which API files change; the facts list PatientService but show no call edges or bodies, and the local checkout is on PR #7's branch, not the pinned SHA.
**Evidence:** context/generated/module-map.json#sym:sample-system:api/src/services/patientService.ts:PatientService, context/architecture.md#components, runs/run_01M6GRSJH8H42FGV284MDTDAZY/ide-answers.md#a-09
**Options:**
  A. Pass-through only
  B. Service maps list items
**Proposed default:** Pass-through only; implement confirms against current main
answer:
answered_by:
decision:

## Q-05 · non-blocking · ask: tech lead · round 2
**Question:** Where is the web's patient list item type declared: next to `api` in web/src/api/client.ts, or in PatientsPage.tsx?
**Why it matters:** Design must name the file whose type gains the field; no sym fact declares the list item type.
**Evidence:** context/generated/module-map.json#sym:sample-system:web/src/api/client.ts:api, context/generated/module-map.json#sym:sample-system:web/src/pages/PatientsPage.tsx:PatientsPage, runs/run_01M6GRSJH8H42FGV284MDTDAZY/ide-answers.md#a-10
**Options:**
  A. In web/src/api/client.ts
  B. In PatientsPage.tsx
**Proposed default:** In web/src/api/client.ts; implement confirms against current main
answer:
answered_by:
decision:

## Q-02 · non-blocking · ask: tech lead · round 3
**Question:** Impact's Q-04 and Q-05 were addressed to the tech lead, and design now carries them as assumptions (PatientService passes the field through; the list item type is in client.ts). Do you accept implement confirming both against current main?
**Proposed default:** none
answer:
answered_by:
decision:
