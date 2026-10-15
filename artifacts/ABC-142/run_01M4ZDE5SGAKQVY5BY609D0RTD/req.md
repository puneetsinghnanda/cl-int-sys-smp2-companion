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
    text: "Order history is the list of a customer's orders in a date range, newest first, 20 per page."
    cites: ["context/glossary.md#order-history"]
  - id: C2
    text: "Recent activity is the status changes of one order shown on the order page, read from order_events."
    cites: ["context/glossary.md#recent-activity"]
  - id: C3
    text: "System lists show 20 items per page, newest first."
    cites: ["context/intent-baseline.md#constraints"]
  - id: C4
    text: "Customers and support staff can see where orders are and what has changed."
    cites: ["context/intent-baseline.md#goals"]
  - id: C5
    text: "Status history is the full audit trail of an order's status changes, kept for the life of the order; it is a different term from recent activity."
    cites: ["context/glossary.md#status-history", "context/glossary.md#recent-activity"]
  - id: C6
    text: "Archived orders (older than 24 months) are kept but hidden from order history."
    cites: ["context/glossary.md#archived-order"]
trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7]
known: [REQ-1, REQ-2, REQ-4, REQ-5, REQ-6]
assumed: [REQ-7]
unknown: [U-1, U-2, U-3, U-4, U-5, U-6]
requirements:
  - id: REQ-1
    title: Default 30-day order history
    type: functional
    status: known
    statement: "When a customer opens order history without choosing dates, it shows that customer's orders from the last 30 days."
    cites: ["artifacts/ABC-142/run_01M4ZDE5SGAKQVY5BY609D0RTD/ticket.md#L12", "artifacts/ABC-142/run_01M4ZDE5SGAKQVY5BY609D0RTD/ticket.md#L16", "context/glossary.md#order-history"]
  - id: REQ-2
    title: Recent activity on the order page
    type: functional
    status: known
    statement: "The order page shows the recent activity (status changes) of that order."
    cites: ["artifacts/ABC-142/run_01M4ZDE5SGAKQVY5BY609D0RTD/ticket.md#L17", "context/glossary.md#recent-activity"]
  - id: REQ-3
    title: 30-day window for recent activity
    type: functional
    status: unknown
    statement: "Recent activity on the order page is limited to the same 30 days as order history; what the 30 days are counted from is not stated (see U-1)."
    cites: ["artifacts/ABC-142/run_01M4ZDE5SGAKQVY5BY609D0RTD/ticket.md#L17"]
  - id: REQ-4
    title: Newest first
    type: constraint
    status: known
    statement: "Orders in order history are listed newest first."
    cites: ["artifacts/ABC-142/run_01M4ZDE5SGAKQVY5BY609D0RTD/ticket.md#L18", "context/intent-baseline.md#constraints"]
  - id: REQ-5
    title: 20 orders per page
    type: constraint
    status: known
    statement: "Order history shows 20 orders per page."
    cites: ["artifacts/ABC-142/run_01M4ZDE5SGAKQVY5BY609D0RTD/ticket.md#L18", "context/intent-baseline.md#constraints"]
  - id: REQ-6
    title: Existing from/to date filters keep working
    type: constraint
    status: known
    statement: "The existing from and to date filters on order history keep their current behaviour."
    cites: ["artifacts/ABC-142/run_01M4ZDE5SGAKQVY5BY609D0RTD/ticket.md#L19"]
  - id: REQ-7
    title: Explicit dates replace the default range
    type: functional
    status: assumed
    statement: "When the customer sets a from or to date, the chosen range is used instead of the 30-day default."
    basis: "The 30-day range applies 'by default' and 'without choosing dates', and order history is defined as a list in a date range, so a chosen range should take precedence."
    cites: ["artifacts/ABC-142/run_01M4ZDE5SGAKQVY5BY609D0RTD/ticket.md#L13", "artifacts/ABC-142/run_01M4ZDE5SGAKQVY5BY609D0RTD/ticket.md#L16", "artifacts/ABC-142/run_01M4ZDE5SGAKQVY5BY609D0RTD/ticket.md#L19", "context/glossary.md#order-history"]
unknowns:
  - id: U-1
    text: "The ticket says recent activity covers 'the same 30 days' but does not say whether the window is counted back from today or from the order's date."
    question: Q-01
  - id: U-2
    text: "How '30 days' is measured is not stated: rolling 30 x 24 hours or 30 calendar days including today, and in which time zone."
    question: Q-02
  - id: U-3
    text: "The user story says 'this month' while the acceptance criterion says 'last 30 days'; these differ near month boundaries."
    question: Q-03
  - id: U-4
    text: "'Order page' is not in the glossary."
    question: Q-04
  - id: U-5
    text: "'From and to date filters' is not in the glossary, and their current behaviour (which date they filter on, whether bounds are inclusive) is not described in the inputs."
    question: Q-05
  - id: U-6
    text: "The inputs do not say whether order_events keeps at least 30 days of status changes for every order, which REQ-3 depends on."
    question: Q-06
questions:
  - id: Q-01
    blocking: false
    ask: product owner
    text: "For recent activity on the order page, are the 30 days counted back from today, or from the order's creation date?"
    why: "It decides which status changes the order page query returns; an order older than 30 days would show no activity under the 'from today' reading."
    evidence: ["artifacts/ABC-142/run_01M4ZDE5SGAKQVY5BY609D0RTD/ticket.md#L17", "context/glossary.md#recent-activity"]
    options: ["Last 30 days counted back from today", "First 30 days after the order was placed", "No time limit (show all recent activity)"]
    default: "Last 30 days counted back from today, matching the order history default."
  - id: Q-02
    blocking: false
    ask: product owner
    text: "Is 'last 30 days' a rolling 30 x 24 hours from now, or 30 calendar days including today, and in which time zone?"
    why: "Orders placed near the boundary appear or disappear depending on the answer, and tests need an exact rule."
    evidence: ["artifacts/ABC-142/run_01M4ZDE5SGAKQVY5BY609D0RTD/ticket.md#L16"]
    options: ["Rolling 30 x 24 hours from now", "30 calendar days including today in the customer's time zone", "30 calendar days including today in UTC"]
    default: null
  - id: Q-03
    blocking: false
    ask: product owner
    text: "The story says 'everything I bought this month' but the acceptance says 'last 30 days'. Is the rolling 30 days the intended rule?"
    why: "Calendar month and rolling 30 days give different results at month boundaries."
    evidence: ["artifacts/ABC-142/run_01M4ZDE5SGAKQVY5BY609D0RTD/ticket.md#L13", "artifacts/ABC-142/run_01M4ZDE5SGAKQVY5BY609D0RTD/ticket.md#L16"]
    options: ["Rolling last 30 days", "Current calendar month"]
    default: "Rolling last 30 days, as written in the acceptance criteria."
  - id: Q-04
    blocking: false
    ask: tech lead
    text: "Which screen is 'the order page', and should it be added to the glossary?"
    why: "REQ-2 and REQ-3 change this page; design needs to know which component it is."
    evidence: ["artifacts/ABC-142/run_01M4ZDE5SGAKQVY5BY609D0RTD/ticket.md#L17", "context/glossary.md#recent-activity"]
    options: ["The single-order detail page that shows recent activity", "Another page"]
    default: "The single-order detail page where recent activity is shown."
  - id: Q-05
    blocking: false
    ask: tech lead
    text: "How do the existing from and to date filters behave today (date field used, inclusive bounds, behaviour when only one is set)?"
    why: "REQ-6 requires them to keep working; a regression test needs the current behaviour written down."
    evidence: ["artifacts/ABC-142/run_01M4ZDE5SGAKQVY5BY609D0RTD/ticket.md#L19"]
    options: ["Document current behaviour from code during design", "Product owner states the expected behaviour"]
    default: "Capture the current behaviour from code during design and keep it unchanged."
  - id: Q-06
    blocking: false
    ask: tech lead
    text: "Does order_events keep at least 30 days of status changes for every order?"
    why: "If events are purged or summarised sooner, REQ-3 cannot be met from order_events alone."
    evidence: ["context/glossary.md#recent-activity", "context/glossary.md#status-history"]
    options: ["Yes, at least 30 days are kept", "No, retention is shorter"]
    default: null
glossary:
  - {term: order, found: true}
  - {term: order history, found: true}
  - {term: recent activity, found: true}
  - {term: order page, found: false}
  - {term: from and to date filters, found: false}
---
# Requirements review: ABC-142

## Summary
ABC-142 asks that order history show a customer's orders from the last 30 days by default,
newest first and 20 per page, while the existing from/to date filters keep working. It also
asks that the order page limit an order's recent activity to "the same 30 days". The paging
and ordering rules match the system constraints (C3). The main risks are the undefined anchor
for the recent-activity window (U-1), the undefined meaning of "30 days" (U-2) and the
"this month" vs "30 days" wording (U-3). No question blocks design; each has a proposed
default or a named owner.

## Requirements
- REQ-1 Default 30-day order history (known): with no dates chosen, order history shows the customer's orders from the last 30 days.
- REQ-2 Recent activity on the order page (known): the order page shows the order's recent activity (status changes).
- REQ-3 30-day window for recent activity (unknown): recent activity is limited to the same 30 days; the anchor is undefined (U-1).
- REQ-4 Newest first (known): order history is listed newest first.
- REQ-5 20 orders per page (known): order history shows 20 orders per page.
- REQ-6 Existing from/to date filters keep working (known): their current behaviour is unchanged.
- REQ-7 Explicit dates replace the default range (assumed): a chosen from/to range is used instead of the 30-day default.

## Known
- REQ-1: stated in ticket L12 and L16; consistent with the order-history glossary entry.
- REQ-2: stated in ticket L17; recent activity is defined in the glossary (C2).
- REQ-4: stated in ticket L18 and in the baseline constraints (C3).
- REQ-5: stated in ticket L18 and in the baseline constraints (C3).
- REQ-6: stated in ticket L19.

## Assumed
- REQ-7: a chosen from/to range overrides the 30-day default. Basis: the 30 days apply only "by default" and "without choosing dates" (ticket L13, L16), and order history is a list in a date range (C1).

## Unknown
- U-1: is the recent-activity window counted from today or from the order date? (Q-01)
- U-2: is "30 days" rolling or calendar-based, and in which time zone? (Q-02)
- U-3: "this month" in the story vs "last 30 days" in the acceptance. (Q-03)
- U-4: "order page" is not in the glossary. (Q-04)
- U-5: "from and to date filters" is not in the glossary and their current behaviour is not documented. (Q-05)
- U-6: does order_events keep at least 30 days for every order? (Q-06)

## Open questions
- Q-01 (non-blocking, product owner): what the recent-activity 30-day window is counted from. Default: back from today.
- Q-02 (non-blocking, product owner): rolling vs calendar 30 days, and the time zone. No default.
- Q-03 (non-blocking, product owner): "this month" vs "last 30 days". Default: rolling 30 days.
- Q-04 (non-blocking, tech lead): which screen is "the order page". Default: the single-order detail page.
- Q-05 (non-blocking, tech lead): current from/to filter behaviour. Default: capture it from code during design and keep it.
- Q-06 (non-blocking, tech lead): order_events retention for 30 days. No default.

## Out of scope
- Ticket note L22 (support staff are often asked about orders two or three weeks old) is the motivation, not a separate requirement.
- Archived orders (older than 24 months, C6) and the full status history (C5) are unchanged by this ticket and were not turned into requirements.
- No credentials, personal data or instruction-like text were found in the ticket, the baseline sections read or the glossary.
