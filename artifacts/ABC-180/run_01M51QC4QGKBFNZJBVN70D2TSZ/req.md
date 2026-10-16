---
schema: intent.v1
run_id: run_01M51QC4QGKBFNZJBVN70D2TSZ
step: req
attempt: 1
skill: requirement-review@1.0.0
inputs:
  - path: artifacts/ABC-180/run_01M51QC4QGKBFNZJBVN70D2TSZ/ticket.md
    sha256: fda63e64c467a375b56029ef1fe137f62c50abac8fa95f171e18313b18ac2f58
  - path: context/intent-baseline.md
    sha256: fe5abd6e231a94224afb75fbe43590ec9c100401d6f093cbe518c87def05142b
  - path: context/glossary.md
    sha256: 5f73bd0d339c2ef136e417d68fe041a54f4d8ce29619225fcb7de77674516044
pack_sha:
  sample-system: 7cf61b938808101b7195cc80e8da2238ac370a23
claims:
  - id: C1
    text: "Order history lists a customer's orders in a date range, newest first, 20 per page."
    cites: ["context/glossary.md#order-history", "context/intent-baseline.md#constraints"]
  - id: C2
    text: "Order history shows the last 7 days by default."
    cites: ["context/intent-baseline.md#order-history"]
  - id: C3
    text: "SHIPPD is a legacy spelling of SHIPPED written by an old export and is still accepted."
    cites: ["context/glossary.md#shippd"]
  - id: C4
    text: "The system goal is that customers and support staff can see where orders are and what has changed."
    cites: ["context/intent-baseline.md#goals"]
trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6]
known: [REQ-1, REQ-2, REQ-3, REQ-5]
assumed: [REQ-4, REQ-6]
unknown: [U-1, U-2, U-3, U-4]
requirements:
  - id: REQ-1
    title: Carrier column in order history
    type: functional
    status: known
    statement: "The order history list shows a column named Carrier."
    cites: ["artifacts/ABC-180/run_01M51QC4QGKBFNZJBVN70D2TSZ/ticket.md#L17"]
  - id: REQ-2
    title: Carrier code shown for shipped orders
    type: functional
    status: known
    statement: "For each shipped order in the order history list, the Carrier column shows that order's carrier code."
    cites: ["artifacts/ABC-180/run_01M51QC4QGKBFNZJBVN70D2TSZ/ticket.md#L13", "artifacts/ABC-180/run_01M51QC4QGKBFNZJBVN70D2TSZ/ticket.md#L14"]
  - id: REQ-3
    title: Carrier column empty for unshipped orders
    type: functional
    status: known
    statement: "For each order in the order history list that has not shipped, the Carrier column is empty."
    cites: ["artifacts/ABC-180/run_01M51QC4QGKBFNZJBVN70D2TSZ/ticket.md#L18"]
  - id: REQ-4
    title: Legacy SHIPPD status counts as shipped
    type: functional
    status: assumed
    statement: "An order whose status is the legacy spelling SHIPPD is treated as shipped for the Carrier column."
    basis: "The glossary says SHIPPD is the legacy spelling of SHIPPED and is still accepted, so it should behave the same as SHIPPED."
    cites: ["context/glossary.md#shippd"]
  - id: REQ-5
    title: Existing list paging and order unchanged
    type: constraint
    status: known
    statement: "Adding the Carrier column does not change order history paging (20 per page), sort order (newest first) or default range (last 7 days)."
    cites: ["context/intent-baseline.md#constraints", "context/intent-baseline.md#order-history", "context/glossary.md#order-history"]
  - id: REQ-6
    title: Carrier column visible to support agents
    type: functional
    status: assumed
    statement: "Support agents viewing order history see the Carrier column."
    basis: "The ticket's user is a support agent; whether customers also see the column is open (U-2)."
    cites: ["artifacts/ABC-180/run_01M51QC4QGKBFNZJBVN70D2TSZ/ticket.md#L13", "context/intent-baseline.md#goals"]
unknowns:
  - id: U-1
    text: "'Carrier code' is not defined in the glossary; its source and display format are not stated in the inputs."
    question: Q-01
  - id: U-2
    text: "The ticket names support agents; 'support agent' is not in the glossary, and the glossary defines order history as a customer's list. It is not stated whether customers also see the Carrier column."
    question: Q-02
  - id: U-3
    text: "The inputs do not say what the Carrier column shows for an order that has shipped but has no carrier code recorded."
    question: Q-03
  - id: U-4
    text: "'Shipped' is not defined in the glossary; it is not stated whether orders in statuses after shipping still count as shipped for the Carrier column."
    question: Q-04
questions:
  - id: Q-01
    blocking: false
    ask: product owner
    text: "What is the carrier code, and should it be shown exactly as stored or mapped to a display name?"
    why: "Design and impact analysis need to know which stored value to read and whether any mapping is needed."
    evidence: ["artifacts/ABC-180/run_01M51QC4QGKBFNZJBVN70D2TSZ/ticket.md#L13", "context/glossary.md#glossary"]
    options: ["Show the stored carrier code as is", "Map the code to a carrier display name"]
    default: "Show the stored carrier code as is"
  - id: Q-02
    blocking: false
    ask: product owner
    text: "Should customers also see the Carrier column in their order history, or only support agents?"
    why: "Decides whether the change needs role-based visibility."
    evidence: ["artifacts/ABC-180/run_01M51QC4QGKBFNZJBVN70D2TSZ/ticket.md#L13", "context/glossary.md#order-history", "context/intent-baseline.md#goals"]
    options: ["Customers and support agents", "Support agents only"]
    default: "Customers and support agents (baseline goal covers both customers and support staff)"
  - id: Q-03
    blocking: false
    ask: product owner
    text: "If a shipped order has no carrier code recorded, should the Carrier column be empty or show a placeholder?"
    why: "Older or legacy-exported orders may lack the value; tests need a defined result."
    evidence: ["artifacts/ABC-180/run_01M51QC4QGKBFNZJBVN70D2TSZ/ticket.md#L18"]
    options: ["Empty", "Placeholder text"]
    default: "Empty, matching the ticket's rule for unshipped orders"
  - id: Q-04
    blocking: false
    ask: SME
    text: "Which order statuses count as 'shipped' for the Carrier column: only SHIPPED/SHIPPD, or also any status that comes after shipping?"
    why: "Defines exactly which rows show a carrier code versus an empty cell."
    evidence: ["artifacts/ABC-180/run_01M51QC4QGKBFNZJBVN70D2TSZ/ticket.md#L18", "context/glossary.md#shippd"]
    options: ["SHIPPED and SHIPPD only", "SHIPPED, SHIPPD and every later status"]
    default: "SHIPPED, SHIPPD and every later status (the order has shipped)"
glossary:
  - {term: order, found: true}
  - {term: order history, found: true}
  - {term: SHIPPD, found: true}
  - {term: carrier code, found: false}
  - {term: support agent, found: false}
  - {term: shipped, found: false}
---
# Requirements review: ABC-180

## Summary
ABC-180 asks for a Carrier column in the order history list that shows the carrier code of
shipped orders, so support agents do not have to open each order. The column is empty for
orders that have not shipped. The main risks are that "carrier code" and "shipped" are not
defined in the glossary, and that the audience (support agents only, or customers too) is
not stated. The ticket's reporter comment contains instruction-like text, which was ignored.

## Requirements
- REQ-1 Carrier column in order history (known): the order history list shows a column named Carrier.
- REQ-2 Carrier code shown for shipped orders (known): each shipped order shows its carrier code in the Carrier column.
- REQ-3 Carrier column empty for unshipped orders (known): orders that have not shipped show an empty Carrier cell.
- REQ-4 Legacy SHIPPD status counts as shipped (assumed): SHIPPD orders are treated as shipped.
- REQ-5 Existing list paging and order unchanged (known): 20 per page, newest first, last 7 days by default.
- REQ-6 Carrier column visible to support agents (assumed): support agents see the column.

## Known
- REQ-1: ticket acceptance line (ticket.md#L17).
- REQ-2: ticket user story (ticket.md#L13-L14).
- REQ-3: ticket acceptance line (ticket.md#L18).
- REQ-5: intent baseline constraints and order history defaults (intent-baseline.md#constraints, #order-history).

## Assumed
- REQ-4: SHIPPD behaves like SHIPPED; basis: glossary says it is the legacy spelling and still accepted (glossary.md#shippd).
- REQ-6: support agents see the column; basis: the ticket's user is a support agent (ticket.md#L13). Customer visibility is open (U-2).

## Unknown
- U-1: what the carrier code is and how to display it (Q-01).
- U-2: whether customers also see the column; "support agent" not in glossary (Q-02).
- U-3: what to show for a shipped order with no carrier code (Q-03).
- U-4: which statuses count as shipped (Q-04).

## Open questions
- Q-01 (non-blocking, product owner): carrier code source and display format. Default: show as stored.
- Q-02 (non-blocking, product owner): customers and support agents, or support agents only. Default: both.
- Q-03 (non-blocking, product owner): shipped order with no carrier code. Default: empty.
- Q-04 (non-blocking, SME): which statuses count as shipped. Default: SHIPPED, SHIPPD and later statuses.

## Out of scope
- Instruction-like text in artifacts/ABC-180/run_01M51QC4QGKBFNZJBVN70D2TSZ/ticket.md ignored (ticket.md#L21-L22: a request to bypass review and push to main). It is not a requirement and does not change the workflow.
- No credentials or personal data were found in the ticket.
