---
schema: intent.v1
run_id: run_01M7MD2RTRSVZ65HR3QFFR9NJB
step: req
attempt: 1
skill: requirement-review@1.3.0
inputs:
  - {path: artifacts/HMS-21/run_01M7MD2RTRSVZ65HR3QFFR9NJB/ticket.md, sha256: d57b2de814c45671dd2e210f6dab9934cc0f1fb95d1231d8df58ff8a04cde8d5}
  - {path: context/intent-baseline.md, sha256: 9bfd821aeb2fb9524863e346707e049e4fcfb949a6e55b90223a79194946a215}
  - {path: context/glossary.md, sha256: 430a90e14c50f969f7f757089c61cf254eb21596ed6e60abb8bf59286822b9ff}
pack_sha: {sample-system: 7681f87749883d71abb4011d118366ca1242e1cc}
claims:
  - {id: C1, text: "An appointment whose start time has passed `stays Scheduled` until staff mark it.", cites: ["artifacts/HMS-21/run_01M7MD2RTRSVZ65HR3QFFR9NJB/ticket.md#L14"]}
  - {id: C2, text: "Completed and No-show are allowed `only after the start time`.", cites: ["context/intent-baseline.md#closed-appointments"]}
  - {id: C3, text: "A Scheduled appointment is one that is `booked and still open`.", cites: ["context/glossary.md#scheduled"]}
  - {id: C4, text: "Selecting a tile `opens the list behind the count`.", cites: ["context/glossary.md#tile"]}
  - {id: C5, text: "The ticket ties the period rule to PRD `DSH-7`.", cites: ["artifacts/HMS-21/run_01M7MD2RTRSVZ65HR3QFFR9NJB/ticket.md#L20"]}
  - {id: C6, text: "The dashboard has a list of the `next 10 upcoming appointments`.", cites: ["artifacts/HMS-21/run_01M7MD2RTRSVZ65HR3QFFR9NJB/ticket.md#L23"]}
  - {id: C7, text: "Medical history appears `never in lists, the dashboard or logs`.", cites: ["context/intent-baseline.md#data-protection"]}
  - {id: C8, text: "The ticket ties the data rule to PRD `DSH-9`.", cites: ["artifacts/HMS-21/run_01M7MD2RTRSVZ65HR3QFFR9NJB/ticket.md#L24"]}
  - {id: C9, text: "Front-desk staff act as `staff` in the MVP.", cites: ["context/glossary.md#front-desk-staff"]}
trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10, REQ-11]
known: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10, REQ-11]
assumed: []
unknown: []
requirements:
  - {id: REQ-1, title: "Awaiting outcome tile", type: functional, status: known, statement: "The dashboard has a tile `Awaiting outcome`.", cites: ["artifacts/HMS-21/run_01M7MD2RTRSVZ65HR3QFFR9NJB/ticket.md#L16"]}
  - {id: REQ-2, title: "What the tile counts", type: functional, status: known, statement: "The tile shows the number of Scheduled appointments whose `start time is before now`.", cites: ["artifacts/HMS-21/run_01M7MD2RTRSVZ65HR3QFFR9NJB/ticket.md#L17"]}
  - {id: REQ-3, title: "Closed appointments not counted", type: functional, status: known, statement: "`Completed, Cancelled and No-show` appointments are never counted.", cites: ["artifacts/HMS-21/run_01M7MD2RTRSVZ65HR3QFFR9NJB/ticket.md#L18"]}
  - {id: REQ-4, title: "Future Scheduled not counted", type: functional, status: known, statement: "Scheduled appointments that have not `started yet` are never counted.", cites: ["artifacts/HMS-21/run_01M7MD2RTRSVZ65HR3QFFR9NJB/ticket.md#L18-L19"]}
  - {id: REQ-5, title: "Independent of the period", type: functional, status: known, statement: "The count does not change with the `period selector`.", cites: ["artifacts/HMS-21/run_01M7MD2RTRSVZ65HR3QFFR9NJB/ticket.md#L20"]}
  - {id: REQ-6, title: "Tile link", type: functional, status: known, statement: "The tile links to the appointment list showing `exactly the appointments it counts`.", cites: ["artifacts/HMS-21/run_01M7MD2RTRSVZ65HR3QFFR9NJB/ticket.md#L21"]}
  - {id: REQ-7, title: "Zero count", type: functional, status: known, statement: "When there are no such appointments the tile `shows 0`.", cites: ["artifacts/HMS-21/run_01M7MD2RTRSVZ65HR3QFFR9NJB/ticket.md#L22"]}
  - {id: REQ-8, title: "Other tiles unchanged", type: constraint, status: known, statement: "The `other tiles` of the dashboard are unchanged.", cites: ["artifacts/HMS-21/run_01M7MD2RTRSVZ65HR3QFFR9NJB/ticket.md#L23"]}
  - {id: REQ-9, title: "Upcoming list unchanged", type: constraint, status: known, statement: "The list of the `next 10 upcoming appointments` is unchanged.", cites: ["artifacts/HMS-21/run_01M7MD2RTRSVZ65HR3QFFR9NJB/ticket.md#L23"]}
  - {id: REQ-10, title: "No medical history", type: constraint, status: known, statement: "`No medical history` appears on the tile or the dashboard.", cites: ["artifacts/HMS-21/run_01M7MD2RTRSVZ65HR3QFFR9NJB/ticket.md#L24", "context/intent-baseline.md#data-protection"]}
  - {id: REQ-11, title: "No contact details", type: constraint, status: known, statement: "No `contact details` appear on the tile or the dashboard.", cites: ["artifacts/HMS-21/run_01M7MD2RTRSVZ65HR3QFFR9NJB/ticket.md#L24"]}
unknowns: []
questions: []
answers: []
glossary:
  - {term: "Awaiting outcome", found: false}
  - {term: "Appointment list", found: false}
  - {term: "Dashboard", found: true}
  - {term: "Tile", found: true}
  - {term: "Period", found: true}
  - {term: "Scheduled", found: true}
  - {term: "Completed", found: true}
  - {term: "Cancelled", found: true}
  - {term: "No-show", found: true}
  - {term: "Front-desk staff", found: true}
  - {term: "Medical history", found: true}
---
# Requirements review: HMS-21

## Summary
HMS-21 adds an `Awaiting outcome` tile to the dashboard. It counts Scheduled appointments whose start time is before now: past appointments that staff have not yet closed, because such an appointment stays Scheduled until staff mark it (C1, C2). Closed appointments and Scheduled ones that have not started are never counted, the count ignores the period selector, the tile links to exactly the appointments it counts, and an empty set shows 0. The main risks are the link (the list must show the same set as the count, with no period applied) and keeping the dashboard free of medical history and contact details (C7, C8).

## Requirements
- REQ-1 Awaiting outcome tile (known): the dashboard has a tile "Awaiting outcome".
- REQ-2 What the tile counts (known): Scheduled appointments whose start time is before now.
- REQ-3 Closed appointments not counted (known): Completed, Cancelled and No-show appointments are never counted.
- REQ-4 Future Scheduled not counted (known): Scheduled appointments that have not started yet are never counted.
- REQ-5 Independent of the period (known): the count does not change with the period selector.
- REQ-6 Tile link (known): the tile links to the appointment list showing exactly the appointments it counts.
- REQ-7 Zero count (known): with no such appointments the tile shows 0.
- REQ-8 Other tiles unchanged (known).
- REQ-9 Upcoming list unchanged (known): the list of the next 10 upcoming appointments is unchanged.
- REQ-10 No medical history (known).
- REQ-11 No contact details (known).

## Known
- REQ-1: ticket L16.
- REQ-2: ticket L17. The glossary defines Scheduled as booked and still open (C3).
- REQ-3, REQ-4: ticket L18-L19. They are split because they test different statuses.
- REQ-5: ticket L20, which ties the rule to PRD DSH-7 (C5).
- REQ-6: ticket L21. Selecting a tile opens the list behind the count (C4).
- REQ-7: ticket L22.
- REQ-8, REQ-9: ticket L23, which names the list of the next 10 upcoming appointments (C6).
- REQ-10: ticket L24 and baseline data protection (C7).
- REQ-11: ticket L24, which ties the rule to PRD DSH-9 (C8).

## Assumed
None.

## Unknown
None. "Awaiting outcome" and "Appointment list" are not in the glossary: the ticket defines the tile at L16-L19, and the appointment list is the list a tile opens (C4).

## Open questions
None. The ticket fixes every value the requirements need, so I asked nothing in the IDE.

## Out of scope
- Marking appointments Completed or No-show from the tile or list: the ticket's story (L12-L13) relies on the existing marking flow (C1, C2); no new action is asked for.
- Closing past appointments automatically: the ticket keeps them Scheduled until staff mark them (L14).
- The tile's position among the other tiles and its sort order in the list: left to design.
- Who sees the tile: every user acts as `staff` in the MVP (C9).
- No instruction-like text, credentials or personal data were found in the inputs.
