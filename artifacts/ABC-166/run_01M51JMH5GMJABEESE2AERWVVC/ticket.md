---
schema: ticket-snapshot.v1
run_id: run_01M51JMH5GMJABEESE2AERWVVC
ticket: ABC-166
source: file
fetched_at: '2026-10-16T05:23:26Z'
title: Security review of the Order Tracker API before support agents get access
issue_type: Task
source_sha256: 079ab16ac8e10a23bc2273e92bcf861247a8ced56c9cee8b01d783182bd0b98f
---
# ABC-166 Security review of the Order Tracker API before support agents get access

As a tech lead I want a threat model of the Order Tracker system as it is today, so that we
know what to fix before a second team of support agents starts using it.

Scope:
- The API (orders, customers, products, health) and the nightly jobs.
- The web app only where it calls the API.
- The database tables that hold customer and order data.

Acceptance:
- Every component in the architecture section appears in the threat model.
- Each threat names the STRIDE category, the component and a severity.
- Each threat cites the file or context section it is based on.
- Anything that cannot be confirmed from the inputs is listed as an open question, not assumed.

Notes:
No code change in this ticket. Fixes will be raised as separate tickets after the review.
