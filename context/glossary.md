---
schema: context-section.v1
section: glossary
owner: eng-db6b
origin: human
status: reviewed
verified_at: {sample-system: 7cf61b938808101b7195cc80e8da2238ac370a23}
---

# Glossary {#glossary}

## Order {#order}
A purchase placed by one customer. It has one current status and one or more lines.

## Order history {#order-history}
The list of a customer's orders in a date range, newest first, 20 per page.

## Recent activity {#recent-activity}
The status changes of one order shown on the order page, read from `order_events`.

## Status history {#status-history}
The full audit trail of an order's status changes, kept for the life of the order.

## Archived order {#archived-order}
An order older than 24 months. It is kept but hidden from order history.

## SHIPPD {#shippd}
Legacy spelling of SHIPPED written by an old export. Still accepted.
