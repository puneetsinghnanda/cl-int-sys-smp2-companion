---
schema: open-questions.v1
run_id: run_01M6DN23JRRYT113FF078VYBSF
ticket: HMS-4
workflow: brownfield-change@1.3.0
step: impact@1.1.1
created_at: '2026-11-02T08:37:10Z'
status: open
---

# Open questions — HMS-4 (impact step)

Answer each question below the line `answer:`. Keep the ids. Then run:
`/truminds:resume run_01M6DN23JRRYT113FF078VYBSF`

## Q-01 · non-blocking · ask: tech lead
**Question:** Does the web patient list page load its rows from GET /api/patients (through web/src/api/client.ts), or from another route?
**Why it matters:** The facts list the route and the client module, but no generated fact links that page to the route. Design needs to know which response to extend.
**Evidence:** context/generated/api-inventory.json#http:GET:/api/patients, context/architecture.md#components
**Options:**
  A. GET /api/patients (proposed default)
  B. Another route; name it
**Proposed default:** GET /api/patients
answer:
answered_by:
decision:
