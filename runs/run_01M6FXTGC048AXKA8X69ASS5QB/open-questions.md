---
schema: open-questions.v1
run_id: run_01M6FXTGC048AXKA8X69ASS5QB
ticket: HMS-7
workflow: brownfield-change@1.3.0
step: impact@1.1.1
created_at: '2026-11-03T07:06:05Z'
status: open
---

# Open questions — HMS-7 (impact step)

Answer each question below the line `answer:`. Keep the ids. Then run:
`/truminds:resume run_01M6FXTGC048AXKA8X69ASS5QB`

## Q-01 · non-blocking · ask: tech lead
**Question:** Does GET /api/appointments (and the /appointments page) already accept filters for doctor, status and a start-date window, so the count can link to it unchanged?
**Why it matters:** REQ-6 and REQ-7 need a list filtered to doctor, No-show and the 30-day window; the generated facts list the route but not its query parameters.
**Evidence:** context/generated/api-inventory.json#http:GET:/api/appointments, context/generated/api-inventory.json#ui:/appointments
**Options:**
  A. Filters exist; link only
  B. Some filters missing; extend the route and page
**Proposed default:** Design reads api/src/routes/appointments.ts and AppointmentsPage and extends only the missing filters.
answer:
answered_by:
decision:

## Q-02 · non-blocking · ask: tech lead
**Question:** Does the dashboard's PERIODS include a Last 30 days period with a No-shows count, and can its window rule be reused per doctor?
**Why it matters:** REQ-4 (answer A-01) reuses the tile's window and rule; the facts show a PERIODS symbol but not its values.
**Evidence:** context/generated/module-map.json#sym:sample-system:api/src/services/dashboardService.ts:PERIODS
**Options:**
  A. Yes; extract and share the window rule
  B. No such period; ask the product owner
**Proposed default:** Design reads dashboardService.ts and shares its window computation with the doctor count.
answer:
answered_by:
decision:

## Q-03 · non-blocking · ask: tech lead
**Question:** Should the 30-day No-show count be added to the GET /api/doctors/:id response (also read by the /doctors/:id/edit form) or served by a separate read used only by the doctor page?
**Why it matters:** GET /api/doctors/:id is also read by DoctorFormPage (fromDoctor), whose data goes back through PATCH /api/doctors/:id; a new read-only field there must not leak into the edit form or the PATCH body.
**Evidence:** context/generated/api-inventory.json#ui:/doctors/:id/edit, context/generated/module-map.json#sym:sample-system:web/src/pages/DoctorFormPage.tsx:fromDoctor, context/generated/api-inventory.json#http:PATCH:/api/doctors/:id
**Options:**
  A. Add to GET /api/doctors/:id; edit form and PATCH ignore it
  B. Separate read for the doctor page; doctor detail response unchanged
**Proposed default:** Add to GET /api/doctors/:id as a read-only field; fromDoctor does not map it and PATCH does not accept it; design confirms against current source.
answer:
answered_by:
decision:
