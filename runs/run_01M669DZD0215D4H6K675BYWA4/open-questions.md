---
schema: open-questions.v1
run_id: run_01M669DZD0215D4H6K675BYWA4
ticket: HMS-6
workflow: brownfield-change@1.3.0
step: impact@1.1.1
created_at: '2026-10-30T11:57:08Z'
status: open
---

# Open questions — HMS-6 (req step)

Answer each question below the line `answer:`. Keep the ids. Then run:
`/truminds:resume run_01M669DZD0215D4H6K675BYWA4`

## Q-01 · non-blocking · ask: product owner
**Question:** Should Inactive doctors who have appointments today appear on the doctors list with a Today count? If not, C8 can be dropped.
**Proposed default:** none
answer:
answered_by:
decision:

## Q-02 · non-blocking · ask: product owner
**Question:** Does 'walk-in' imply any new booking behaviour, or is it only the motivation for the Today column?
**Why it matters:** If walk-ins need their own flow, it is a separate requirement.
**Evidence:** artifacts/HMS-6/run_01M669DZD0215D4H6K675BYWA4/ticket.md#L14
**Options:**
  A. Motivation only; no new behaviour
  B. New walk-in booking flow (separate ticket)
**Proposed default:** Motivation only; no new behaviour
answer:
answered_by:
decision:

## Q-03 · non-blocking · ask: tech lead
**Question:** Is the 'doctors list' the existing doctors list page described by PRD DOC-2?
**Why it matters:** Impact analysis needs the exact screen and endpoint to change.
**Evidence:** artifacts/HMS-6/run_01M669DZD0215D4H6K675BYWA4/ticket.md#L16
**Options:**
  A. Existing doctors list (PRD DOC-2)
  B. Another view
**Proposed default:** Existing doctors list (PRD DOC-2)
answer:
answered_by:
decision:

## Q-04 · non-blocking · ask: tech lead
**Question:** Does the existing appointment list support filtering by doctor and by a single day? If not, may this change add those filters?
**Why it matters:** REQ-6 and REQ-7 need a link target that applies both filters.
**Evidence:** artifacts/HMS-6/run_01M669DZD0215D4H6K675BYWA4/ticket.md#L24, context/glossary.md#dashboard
**Options:**
  A. Filters exist; link to them
  B. Add the missing filter(s) in this change
**Proposed default:** Use existing filters; add any missing doctor/day filter in this change
answer:
answered_by:
decision:

## Q-05 · non-blocking · ask: tech lead
**Question:** Confirm 'today' is the calendar day in the hospital time zone Asia/Kolkata, regardless of the server's or browser's zone.
**Why it matters:** Day boundaries decide which appointments are counted.
**Evidence:** context/intent-baseline.md#constraints, artifacts/HMS-6/run_01M669DZD0215D4H6K675BYWA4/ticket.md#L20
**Options:**
  A. Asia/Kolkata calendar day
  B. Server or browser local day
**Proposed default:** Asia/Kolkata calendar day
answer:
answered_by:
decision:

## Q-06 · non-blocking · ask: tech lead
**Question:** Does the dashboard's 'Appointments today' tile count every status except Cancelled? If the tile differs, which rule wins?
**Why it matters:** REQ-3 and REQ-4 must agree; a mismatch would make the column and tile disagree.
**Evidence:** artifacts/HMS-6/run_01M669DZD0215D4H6K675BYWA4/ticket.md#L21, context/glossary.md#tile
**Options:**
  A. Tile already matches; reuse its rule
  B. Tile differs; follow the ticket and raise a separate fix
**Proposed default:** Tile differs; follow the ticket and raise a separate fix
answer:
answered_by:
decision:

## Q-07 · non-blocking · ask: product owner
**Question:** Should a count of 0 also be a link to the (empty) filtered appointment list?
**Why it matters:** Affects rendering and tests for REQ-5 and REQ-6.
**Evidence:** artifacts/HMS-6/run_01M669DZD0215D4H6K675BYWA4/ticket.md#L23, context/glossary.md#tile
**Options:**
  A. Every count links, including 0
  B. 0 is plain text
**Proposed default:** Every count links, including 0, like dashboard tiles
answer:
answered_by:
decision:

## Q-08 · non-blocking · ask: product owner · round 2
**Question:** If the dashboard tile turns out not to count every status except Cancelled, should S-1 AC4 (Today count equals the tile value) be dropped or deferred until the tile fix lands?
**Proposed default:** none
answer:
answered_by:
decision:

## Q-09 · non-blocking · ask: product owner · round 2
**Question:** Does 'walk-in' imply any new booking behaviour, or is it only the motivation for the Today column?
**Why it matters:** A walk-in flow would need its own story.
**Evidence:** artifacts/HMS-6/run_01M669DZD0215D4H6K675BYWA4/req.md#L135
**Options:**
  A. Motivation only; no new behaviour
  B. New walk-in booking flow (separate ticket)
**Proposed default:** Motivation only; no new behaviour
answer:
answered_by:
decision:

## Q-10 · non-blocking · ask: tech lead · round 2
**Question:** Is the 'doctors list' the existing doctors list page described by PRD DOC-2?
**Why it matters:** S-1 and S-3 change that screen.
**Evidence:** artifacts/HMS-6/run_01M669DZD0215D4H6K675BYWA4/req.md#L143
**Options:**
  A. Existing doctors list (PRD DOC-2)
  B. Another view
**Proposed default:** Existing doctors list (PRD DOC-2)
answer:
answered_by:
decision:

## Q-11 · non-blocking · ask: tech lead · round 2
**Question:** Does the existing appointment list support filtering by doctor and by a single day? If not, may this change add those filters?
**Why it matters:** S-2 needs a link target that applies both filters.
**Evidence:** artifacts/HMS-6/run_01M669DZD0215D4H6K675BYWA4/req.md#L151
**Options:**
  A. Filters exist; link to them
  B. Add the missing filter(s) in this change
**Proposed default:** Add the missing filter(s) in this change
answer:
answered_by:
decision:

## Q-12 · non-blocking · ask: tech lead · round 2
**Question:** Confirm 'today' is the calendar day in the hospital time zone Asia/Kolkata.
**Why it matters:** Day boundaries decide which appointments S-1 counts.
**Evidence:** artifacts/HMS-6/run_01M669DZD0215D4H6K675BYWA4/req.md#L159
**Options:**
  A. Asia/Kolkata calendar day
  B. Server or browser local day
**Proposed default:** Asia/Kolkata calendar day
answer:
answered_by:
decision:

## Q-13 · non-blocking · ask: tech lead · round 2
**Question:** Does the dashboard's 'Appointments today' tile count every status except Cancelled? If the tile differs, which rule wins?
**Why it matters:** S-1 AC3 and AC4 must agree.
**Evidence:** artifacts/HMS-6/run_01M669DZD0215D4H6K675BYWA4/req.md#L167
**Options:**
  A. Tile already matches; reuse its rule
  B. Tile differs; follow the ticket and raise a separate fix
**Proposed default:** Tile differs; follow the ticket and raise a separate fix
answer:
answered_by:
decision:

## Q-14 · non-blocking · ask: product owner · round 2
**Question:** Should a count of 0 also be a link to the (empty) filtered appointment list?
**Why it matters:** Decides whether S-2 applies to rows showing 0.
**Evidence:** artifacts/HMS-6/run_01M669DZD0215D4H6K675BYWA4/req.md#L175
**Options:**
  A. Every count links, including 0
  B. 0 is plain text
**Proposed default:** Every count links, including 0
answer:
answered_by:
decision:

## Q-15 · non-blocking · ask: tech lead · round 3
**Question:** Should the Today count go on the shared DoctorSummary type, which would also change GET /api/doctors/:id via DoctorDetail? Or should it go on a list-only row type returned only when the doctors list asks for it?
**Proposed default:** none
answer:
answered_by:
decision:

## Q-16 · non-blocking · ask: tech lead · round 3
**Question:** Do GET /api/appointments and AppointmentsPage already accept a doctor filter and a single-day filter? If not, may this change add them?
**Why it matters:** REQ-6 and REQ-7 need a link target filtered to one doctor and today; if the filters are missing, GET /api/appointments and AppointmentsPage become changed instead of existing.
**Evidence:** context/generated/api-inventory.json#http:GET:/api/appointments, context/generated/module-map.json#sym:sample-system:web/src/pages/AppointmentsPage.tsx:AppointmentsPage
**Options:**
  A. Filters exist; link to them
  B. Add the missing filter(s) in this change
**Proposed default:** Add the missing filter(s) in this change
answer:
answered_by:
decision:

## Q-17 · non-blocking · ask: tech lead · round 3
**Question:** Where does DashboardService compute the 'Appointments today' count, and does it exclude only CANCELLED? May the doctors count reuse that same predicate?
**Why it matters:** REQ-4 needs the doctors count and the tile to agree; the facts show DashboardService exists but not its counting rule.
**Evidence:** context/generated/module-map.json#sym:sample-system:api/src/services/dashboardService.ts:DashboardService, context/generated/api-inventory.json#http:GET:/api/dashboard
**Options:**
  A. Reuse the dashboard's predicate, shared between both
  B. Write a separate query for the doctors count
**Proposed default:** Reuse the dashboard's predicate, shared between both
answer:
answered_by:
decision:

## Q-18 · non-blocking · ask: tech lead · round 3
**Question:** GET /api/doctors is also called by the BookAppointmentPage doctor picker. Should the Today count be computed on every call, or only when the doctors list asks for it?
**Why it matters:** Computing it on every call adds a count query to each picker search and changes the picker's response for no benefit.
**Evidence:** context/generated/module-map.json#sym:sample-system:web/src/pages/BookAppointmentPage.tsx:BookAppointmentPage, context/generated/api-inventory.json#http:GET:/api/doctors
**Options:**
  A. Only when the doctors list asks for it
  B. On every call
**Proposed default:** Only when the doctors list asks for it
answer:
answered_by:
decision:
