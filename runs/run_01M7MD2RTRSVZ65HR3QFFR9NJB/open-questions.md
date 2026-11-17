---
schema: open-questions.v1
run_id: run_01M7MD2RTRSVZ65HR3QFFR9NJB
ticket: HMS-21
workflow: brownfield-change@1.4.0
step: secrev@1.2.0
created_at: '2026-11-17T09:55:21Z'
status: open
---

# Open questions — HMS-21 (req step)

Answer each question below the line `answer:`. Keep the ids. Then run:
`/truminds:resume run_01M7MD2RTRSVZ65HR3QFFR9NJB`

## Q-01 · non-blocking · ask: product owner
**Question:** Does the 'no contact details' rule (ticket AC8, PRD DSH-9) also apply to the appointment list the Awaiting outcome tile opens, or only to the tile and dashboard?
**Proposed default:** none
answer:
answered_by:
decision:

## Q-02 · non-blocking · ask: product owner · round 2
**Question:** May contact details (phone, email) appear in the appointment list that the Awaiting outcome tile opens, or should the no-contact-details rule (REQ-11) also cover that list? The artifact records this as its own non-blocking Q-01.
**Proposed default:** none
answer:
answered_by:
decision:

## Q-03 · non-blocking · ask: product owner · round 2
**Question:** May contact details (phone, email) appear in the appointment list that the Awaiting outcome tile opens, or should the no-contact-details rule also cover that list?
**Why it matters:** REQ-11 forbids contact details on the tile and the dashboard only; the list the tile opens is not named, so S-4 AC5 checks the tile and dashboard only.
**Evidence:** artifacts/HMS-21/run_01M7MD2RTRSVZ65HR3QFFR9NJB/req.md#item:REQ-11, context/glossary.md#tile
**Options:**
  A. Tile and dashboard only, as REQ-11 states
  B. Also the appointment list the tile opens
**Proposed default:** Tile and dashboard only, as REQ-11 states
answer:
answered_by:
decision:

## Q-04 · non-blocking · ask: product owner · round 3
**Question:** Should the appointment list show a visible 'Started before now' indicator with a clear action when it is opened from the Awaiting outcome tile, or is a URL-only filter acceptable?
**Proposed default:** none
answer:
answered_by:
decision:

## Q-05 · non-blocking · ask: tech lead · round 4
**Question:** T-2 and T-8 accept unbounded count scans (awaitingOutcome has no lower bound, and the list total is unpaged) with no new control. Do you accept these performance risks for HMS-21, or should implement add a status/starts_at index?
**Proposed default:** none
answer:
answered_by:
decision:

## Q-06 · non-blocking · ask: tech lead · round 5
**Question:** Do you accept, for HMS-21, that the Awaiting outcome count and the started=true list total have no lower time bound (threat T-2 and T-8), or should a lower bound or a (status, starts_at) index be added?
**Why it matters:** The threat model accepts both as risks, but no artifact of this run asks anyone to confirm them. The cost grows with Scheduled appointments left without an outcome.
**Evidence:** artifacts/HMS-21/run_01M7MD2RTRSVZ65HR3QFFR9NJB/threat.md#item:T-2, artifacts/HMS-21/run_01M7MD2RTRSVZ65HR3QFFR9NJB/threat.md#item:T-8, sample-system:api/src/services/dashboardService.ts#L58, sample-system:api/src/repositories/appointmentRepo.ts#L78
**Options:**
  A. Accept the risk for now (no change)
  B. Add a (status, starts_at) index in a follow-up
  C. Add a lower time bound (changes REQ-5 behaviour)
**Proposed default:** Accept the risk for now (no change)
answer:
answered_by:
decision:
