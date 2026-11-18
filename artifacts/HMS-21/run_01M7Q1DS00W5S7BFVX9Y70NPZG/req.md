---
schema: intent.v1
run_id: run_01M7Q1DS00W5S7BFVX9Y70NPZG
step: req
attempt: 1
skill: requirement-review@1.4.0
inputs:
  - path: artifacts/HMS-21/run_01M7Q1DS00W5S7BFVX9Y70NPZG/ticket.md
    sha256: e739b80b0d1d1ab60f3783d9a2e0badcf519d8540ca0d1727dd6630004f3d15f
  - path: context/intent-baseline.md
    sha256: 9bfd821aeb2fb9524863e346707e049e4fcfb949a6e55b90223a79194946a215
  - path: context/glossary.md
    sha256: 430a90e14c50f969f7f757089c61cf254eb21596ed6e60abb8bf59286822b9ff
pack_sha:
  sample-system: 7681f87749883d71abb4011d118366ca1242e1cc
claims:
  - id: C1
    text: "Completed and `No-show` are allowed only after the start time."
    cites: ["context/intent-baseline.md#closed-appointments"]
  - id: C2
    text: "Only `Scheduled` appointments can change; a Scheduled appointment is booked and still open."
    cites: ["context/glossary.md#scheduled"]
  - id: C3
    text: "Each dashboard tile links to the list it counts (`each linking to the list it`)."
    cites: ["context/glossary.md#dashboard"]
  - id: C4
    text: "A period is Today, Last 7 days or `Last 30 days`; the dashboard tiles use the same rule."
    cites: ["context/glossary.md#period"]
  - id: C5
    text: "`Medical history` never appears in lists, the dashboard or logs."
    cites: ["context/intent-baseline.md#data-protection"]
  - id: C6
    text: "The dashboard data is served from `api/src/routes/dashboard.ts`."
    cites: ["context/generated/api-inventory.json#http:GET:/api/dashboard"]
  - id: C7
    text: "The appointment list is served from `api/src/routes/appointments.ts`."
    cites: ["context/generated/api-inventory.json#http:GET:/api/appointments"]
  - id: C8
    text: "The hospital time zone is `Asia/Kolkata`."
    cites: ["context/intent-baseline.md#constraints"]
trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10, REQ-11, REQ-12, REQ-13]
known: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10, REQ-11, REQ-12, REQ-13]
assumed: []
unknown: []
requirements:
  - id: REQ-1
    title: Awaiting outcome tile
    type: functional
    status: known
    statement: "The dashboard has a tile labelled `Awaiting outcome`."
    cites: ["artifacts/HMS-21/run_01M7Q1DS00W5S7BFVX9Y70NPZG/ticket.md#L16"]
  - id: REQ-2
    title: Count started Scheduled appointments
    type: functional
    status: known
    statement: "The tile shows the number of Scheduled appointments whose `start time is before now`."
    cites: ["artifacts/HMS-21/run_01M7Q1DS00W5S7BFVX9Y70NPZG/ticket.md#L17", "context/glossary.md#scheduled"]
  - id: REQ-3
    title: Completed never counted
    type: functional
    status: known
    statement: "`Completed` appointments are never counted."
    cites: ["artifacts/HMS-21/run_01M7Q1DS00W5S7BFVX9Y70NPZG/ticket.md#L18"]
  - id: REQ-4
    title: Cancelled never counted
    type: functional
    status: known
    statement: "`Cancelled` appointments are never counted."
    cites: ["artifacts/HMS-21/run_01M7Q1DS00W5S7BFVX9Y70NPZG/ticket.md#L18"]
  - id: REQ-5
    title: No-show never counted
    type: functional
    status: known
    statement: "`No-show` appointments are never counted."
    cites: ["artifacts/HMS-21/run_01M7Q1DS00W5S7BFVX9Y70NPZG/ticket.md#L18"]
  - id: REQ-6
    title: Not-yet-started Scheduled never counted
    type: functional
    status: known
    statement: "Scheduled appointments that `have not` started yet are never counted."
    cites: ["artifacts/HMS-21/run_01M7Q1DS00W5S7BFVX9Y70NPZG/ticket.md#L18-L19"]
  - id: REQ-7
    title: Count ignores the period selector
    type: functional
    status: known
    statement: "The count does not change with the `period selector`."
    cites: ["artifacts/HMS-21/run_01M7Q1DS00W5S7BFVX9Y70NPZG/ticket.md#L20"]
  - id: REQ-8
    title: Tile links to the matching appointment list
    type: functional
    status: known
    statement: "The tile links to the appointment list showing exactly the appointments it counts (`DSH-8`)."
    cites: ["artifacts/HMS-21/run_01M7Q1DS00W5S7BFVX9Y70NPZG/ticket.md#L21", "context/glossary.md#dashboard"]
  - id: REQ-9
    title: Zero awaiting
    type: functional
    status: known
    statement: "When there are no such appointments, the tile `shows 0`."
    cites: ["artifacts/HMS-21/run_01M7Q1DS00W5S7BFVX9Y70NPZG/ticket.md#L22"]
  - id: REQ-10
    title: Other tiles unchanged
    type: constraint
    status: known
    statement: "The `other tiles` are unchanged."
    cites: ["artifacts/HMS-21/run_01M7Q1DS00W5S7BFVX9Y70NPZG/ticket.md#L23"]
  - id: REQ-11
    title: Next upcoming list unchanged
    type: constraint
    status: known
    statement: "The list of the `next 10 upcoming` appointments is unchanged."
    cites: ["artifacts/HMS-21/run_01M7Q1DS00W5S7BFVX9Y70NPZG/ticket.md#L23"]
  - id: REQ-12
    title: No medical history
    type: constraint
    status: known
    statement: "No `medical history` appears on the tile, the dashboard or the linked list."
    cites: ["artifacts/HMS-21/run_01M7Q1DS00W5S7BFVX9Y70NPZG/ticket.md#L24", "context/intent-baseline.md#data-protection"]
  - id: REQ-13
    title: No contact details
    type: constraint
    status: known
    statement: "No `contact details` appear on the tile or the dashboard."
    cites: ["artifacts/HMS-21/run_01M7Q1DS00W5S7BFVX9Y70NPZG/ticket.md#L24"]
unknowns: []
questions: []
answers: []
glossary:
  - {term: Front-desk staff, found: true}
  - {term: Dashboard, found: true}
  - {term: Tile, found: true}
  - {term: Period, found: true}
  - {term: Scheduled, found: true}
  - {term: Completed, found: true}
  - {term: Cancelled, found: true}
  - {term: No-show, found: true}
  - {term: Medical history, found: true}
  - {term: Awaiting outcome, found: false}
  - {term: Contact details, found: false}
---
# Requirements review: HMS-21

## Summary
HMS-21 adds an `Awaiting outcome` tile that counts Scheduled appointments whose start time has already passed, so staff can close them (ticket L12–L17). The count ignores the period selector and must link to a list holding exactly the counted appointments (L20–L21). The main risk is the boundary at "now": an appointment that has started but not ended is counted, while one starting later today is not. A list filtered by whole local days would therefore not match the count. The local product checkout already appears to contain an HMS-21 implementation that the pinned context pack (`7681f87`) does not show, so later steps must reconcile with that code.

## Requirements
- REQ-1 Awaiting outcome tile — known — The dashboard has a tile labelled `Awaiting outcome`.
- REQ-2 Count started Scheduled appointments — known — The tile counts Scheduled appointments whose `start time is before now`.
- REQ-3 Completed never counted — known — `Completed` appointments are never counted.
- REQ-4 Cancelled never counted — known — `Cancelled` appointments are never counted.
- REQ-5 No-show never counted — known — `No-show` appointments are never counted.
- REQ-6 Not-yet-started Scheduled never counted — known — Scheduled appointments that `have not` started yet are never counted.
- REQ-7 Count ignores the period selector — known — The count does not change with the `period selector`.
- REQ-8 Tile links to the matching appointment list — known — The tile links to the appointment list showing exactly the counted appointments (`DSH-8`).
- REQ-9 Zero awaiting — known — With no such appointments, the tile `shows 0`.
- REQ-10 Other tiles unchanged — known — The `other tiles` are unchanged.
- REQ-11 Next upcoming list unchanged — known — The `next 10 upcoming` appointments list is unchanged.
- REQ-12 No medical history — known — No `medical history` appears on the tile, the dashboard or the linked list.
- REQ-13 No contact details — known — No `contact details` appear on the tile or the dashboard.

## Known
- REQ-1, REQ-2: ticket AC1–AC2 (L16–L17). Scheduled means booked and still open (C2), and Completed and No-show are set only after the start time (C1), so started Scheduled appointments are the ones awaiting an outcome.
- REQ-3, REQ-4, REQ-5, REQ-6: ticket AC3 (L18–L19), one requirement per excluded group.
- REQ-7: ticket AC4 (L20). The other period tiles follow the period rule (C4); this one does not.
- REQ-8: ticket AC5 (L21). Each tile links to the list it counts (C3). The appointment list is served from `api/src/routes/appointments.ts` (C7).
- REQ-9: ticket AC6 (L22).
- REQ-10, REQ-11: ticket AC7 (L23).
- REQ-12: ticket AC8 (L24) and baseline data protection (C5).
- REQ-13: ticket AC8 (L24).

## Assumed
None.

## Unknown
None.

## Open questions
None open. No questions were asked in the IDE in this step: every ticket item is stated directly, and how the list expresses "started before now" is a contract choice for the impact and design steps.

## Out of scope
- How the appointment list filters to "started before now" (a new filter or otherwise) is left to impact and design. Today the list is served from `api/src/routes/appointments.ts` (C7); its filters are not part of this step's inputs.
- Glossary: "Awaiting outcome" is not defined. Ticket AC2 (L17) defines the count, which is enough for REQ-2.
- Glossary: "Contact details" is not defined. AC8 needs only that none appear.
- The PRD references (APT-7, DSH-7, DSH-8, DSH-9) are cited by the ticket. The PRD itself is not an input to this step and was not read.
- The hospital time zone is `Asia/Kolkata` (C8). "Before now" compares instants, so the zone matters only if the list is filtered by local dates.
