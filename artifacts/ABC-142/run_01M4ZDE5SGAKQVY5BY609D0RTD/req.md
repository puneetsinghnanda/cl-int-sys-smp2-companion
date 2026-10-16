---
schema: intent.v1
run_id: run_01M4ZDE5SGAKQVY5BY609D0RTD
step: req
attempt: 1
skill: requirement-review@1.0.0
inputs:
  - path: artifacts/ABC-142/run_01M4ZDE5SGAKQVY5BY609D0RTD/ticket.md
    sha256: bc4d97780197208b7f5207006ed8257bafcf0bea3e4bc83ff3f59f8f296c2d91
  - path: context/intent-baseline.md
    sha256: fe5abd6e231a94224afb75fbe43590ec9c100401d6f093cbe518c87def05142b
  - path: context/glossary.md
    sha256: 5f73bd0d339c2ef136e417d68fe041a54f4d8ce29619225fcb7de77674516044
pack_sha:
  sample-system: 7cf61b938808101b7195cc80e8da2238ac370a23
claims:
  - id: C1
    text: "The intent baseline sets the order history default to the last 7 days."
    cites: ["context/intent-baseline.md#order-history"]
  - id: C2
    text: "The intent baseline says recent activity on the order page covers the last 30 days."
    cites: ["context/intent-baseline.md#order-history"]
  - id: C3
    text: "Lists show 20 items per page, newest first."
    cites: ["context/intent-baseline.md#constraints"]
  - id: C4
    text: "Order history is the list of a customer's orders in a date range, newest first, 20 per page."
    cites: ["context/glossary.md#order-history"]
  - id: C5
    text: "Recent activity is the status changes of one order shown on the order page, read from order_events."
    cites: ["context/glossary.md#recent-activity"]
  - id: C6
    text: "A system goal is that customers and support staff can see where orders are and what has changed."
    cites: ["context/intent-baseline.md#goals"]
trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6]
known: [REQ-2, REQ-3, REQ-4, REQ-5]
assumed: [REQ-6]
unknown: [U-1, U-2, U-3, U-4, U-5]
requirements:
  - id: REQ-1
    title: Order history default range is the last 30 days
    type: functional
    status: unknown
    statement: "When a customer opens order history without choosing dates, it shows orders from the last 30 days."
    cites: ["artifacts/ABC-142/run_01M4ZDE5SGAKQVY5BY609D0RTD/ticket.md#L16", "context/intent-baseline.md#order-history"]
  - id: REQ-2
    title: Order page recent activity covers 30 days
    type: functional
    status: known
    statement: "The order page shows the recent activity of the order for the last 30 days."
    cites: ["artifacts/ABC-142/run_01M4ZDE5SGAKQVY5BY609D0RTD/ticket.md#L17", "context/intent-baseline.md#order-history"]
  - id: REQ-3
    title: Order history is sorted newest first
    type: functional
    status: known
    statement: "Orders in order history are listed newest first."
    cites: ["artifacts/ABC-142/run_01M4ZDE5SGAKQVY5BY609D0RTD/ticket.md#L18", "context/intent-baseline.md#constraints"]
  - id: REQ-4
    title: Order history shows 20 orders per page
    type: constraint
    status: known
    statement: "Order history shows 20 orders per page."
    cites: ["artifacts/ABC-142/run_01M4ZDE5SGAKQVY5BY609D0RTD/ticket.md#L18", "context/intent-baseline.md#constraints"]
  - id: REQ-5
    title: Existing from and to date filters keep working
    type: functional
    status: known
    statement: "The existing from and to date filters on order history keep their current behaviour."
    cites: ["artifacts/ABC-142/run_01M4ZDE5SGAKQVY5BY609D0RTD/ticket.md#L19", "context/glossary.md#order-history"]
  - id: REQ-6
    title: Chosen dates replace the default range
    type: functional
    status: assumed
    statement: "When the customer sets a from or to date filter, that range is used instead of the default range."
    basis: "The ticket frames the default as a way to see orders 'without choosing dates', which implies chosen dates take over; the filters must keep working."
    cites: ["artifacts/ABC-142/run_01M4ZDE5SGAKQVY5BY609D0RTD/ticket.md#L13", "artifacts/ABC-142/run_01M4ZDE5SGAKQVY5BY609D0RTD/ticket.md#L19"]
unknowns:
  - id: U-1
    text: "The ticket sets the order history default to 30 days; the intent baseline sets it to 7 days."
    question: Q-01
  - id: U-2
    text: "It is unclear whether 'last 30 days' is a rolling 30-day window or the current calendar month ('this month')."
    question: Q-02
  - id: U-3
    text: "Term 'customer' is not defined in the glossary."
    question: Q-03
  - id: U-4
    text: "Term 'order page' is not defined in the glossary (it is only used inside the recent activity entry)."
    question: Q-03
  - id: U-5
    text: "Term 'date filter' (from and to) is not defined in the glossary."
    question: Q-03
questions:
  - id: Q-01
    blocking: true
    ask: product owner
    text: "Should the order history default change from the baseline's 7 days to 30 days, and should the intent baseline be updated to match?"
    why: "The default range sets what the query returns and what the tests assert; design cannot start while the ticket and baseline disagree."
    evidence: ["artifacts/ABC-142/run_01M4ZDE5SGAKQVY5BY609D0RTD/ticket.md#L16", "context/intent-baseline.md#order-history"]
    options: ["Change the default to 30 days and update the baseline", "Keep the 7-day default and change the ticket"]
    default: null
  - id: Q-02
    blocking: false
    ask: product owner
    text: "Is 'last 30 days' a rolling window ending now, or the current calendar month?"
    why: "The ticket's acceptance says 'last 30 days' but the story says 'this month'; the two give different results near month boundaries."
    evidence: ["artifacts/ABC-142/run_01M4ZDE5SGAKQVY5BY609D0RTD/ticket.md#L13", "artifacts/ABC-142/run_01M4ZDE5SGAKQVY5BY609D0RTD/ticket.md#L16"]
    options: ["Rolling 30 days ending now", "Current calendar month"]
    default: "Rolling 30 days ending now, as written in the acceptance criteria"
  - id: Q-03
    blocking: false
    ask: tech lead
    text: "Should 'customer', 'order page' and 'date filter' be added to the glossary?"
    why: "The ticket uses these terms but the glossary does not define them, so later steps may read them differently."
    evidence: ["artifacts/ABC-142/run_01M4ZDE5SGAKQVY5BY609D0RTD/ticket.md#L12", "artifacts/ABC-142/run_01M4ZDE5SGAKQVY5BY609D0RTD/ticket.md#L17", "artifacts/ABC-142/run_01M4ZDE5SGAKQVY5BY609D0RTD/ticket.md#L19", "context/glossary.md#recent-activity"]
    options: ["Add the three terms", "Leave the glossary as is"]
    default: "Add the three terms using their ordinary meaning in this ticket"
glossary:
  - {term: order, found: true}
  - {term: order history, found: true}
  - {term: recent activity, found: true}
  - {term: customer, found: false}
  - {term: order page, found: false}
  - {term: date filter, found: false}
---
# Requirements review: ABC-142

## Summary
ABC-142 asks for order history to show the last 30 days by default, so customers can see recent orders without choosing dates. It also confirms the 30-day recent activity window on the order page, newest-first sorting, 20 orders per page, and that the existing from and to filters keep working. The main risk is that the intent baseline sets the order history default to 7 days, which conflicts with the ticket (blocking Q-01). It is also unclear whether "last 30 days" means a rolling window or the calendar month, and three ticket terms are missing from the glossary.

## Requirements
- REQ-1 Order history default range is the last 30 days. **unknown**: when no dates are chosen, order history shows orders from the last 30 days.
- REQ-2 Order page recent activity covers 30 days. **known**: the order page shows the order's recent activity for the last 30 days.
- REQ-3 Order history is sorted newest first. **known**: orders are listed newest first.
- REQ-4 Order history shows 20 orders per page. **known**: order history shows 20 orders per page.
- REQ-5 Existing from and to date filters keep working. **known**: the existing from and to filters keep their current behaviour.
- REQ-6 Chosen dates replace the default range. **assumed**: a from or to filter replaces the default range.

## Known
- REQ-2: stated in the ticket (L17) and matches the baseline's 30-day recent activity window (C2).
- REQ-3: stated in the ticket (L18) and the baseline constraints (C3).
- REQ-4: stated in the ticket (L18) and the baseline constraints (C3).
- REQ-5: stated in the ticket (L19); the glossary defines order history as a list in a date range (C4).

## Assumed
- REQ-6: chosen from and to dates replace the default 30-day range. Basis: the ticket presents the default as a way to see orders "without choosing dates" (L13), and the filters must keep working (L19).

## Unknown
- U-1: the ticket's 30-day default conflicts with the baseline's 7-day default (C1). Raises Q-01.
- U-2: it is unclear whether "last 30 days" is a rolling window or "this month". Raises Q-02.
- U-3: "customer" is not in the glossary. Raises Q-03.
- U-4: "order page" is not in the glossary. Raises Q-03.
- U-5: "date filter" is not in the glossary. Raises Q-03.

## Open questions
- Q-01 (**blocking**, product owner): should the order history default move from 7 to 30 days, and should the baseline be updated?
- Q-02 (non-blocking, product owner): is "last 30 days" a rolling window or the calendar month? Proposed default: a rolling 30-day window.
- Q-03 (non-blocking, tech lead): should "customer", "order page" and "date filter" be added to the glossary? Proposed default: add them.

## Out of scope
- The ticket note that support is often asked about orders two to three weeks old (L22) is background for the change, not a requirement.
- Views for support staff (C6) are not asked for in this ticket.
- Archived orders (older than 24 months) are not affected by a 30-day window and are not addressed.
