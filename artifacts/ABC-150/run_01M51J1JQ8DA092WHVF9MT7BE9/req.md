---
schema: intent.v1
run_id: run_01M51J1JQ8DA092WHVF9MT7BE9
step: req
attempt: 1
skill: requirement-review@1.0.0
inputs:
  - path: artifacts/ABC-150/run_01M51J1JQ8DA092WHVF9MT7BE9/ticket.md
    sha256: 5ca8043486590796c3045e4f31a7acdcd11d7e27b19dfe6b1e06d28783f4cf19
  - path: context/intent-baseline.md
    sha256: 9bfd821aeb2fb9524863e346707e049e4fcfb949a6e55b90223a79194946a215
  - path: context/glossary.md
    sha256: 7623345ad8bad33b8a14a713b452edb2a36834e8b1c2228b76f3aa154034f16e
pack_sha:
  sample-system: bd98ff08b83f8762c5e5c073ba5fadef88169b33
claims:
  - id: C1
    text: "Staff manage `doctors, patients and appointments` end to end; the baseline names no orders."
    cites: ["context/intent-baseline.md#goals"]
  - id: C2
    text: "The hospital time zone is `Asia/Kolkata`."
    cites: ["context/intent-baseline.md#constraints"]
  - id: C3
    text: "The ticket's environment is staging with API server time zone `UTC`."
    cites: ["artifacts/ABC-150/run_01M51J1JQ8DA092WHVF9MT7BE9/ticket.md#L20"]
  - id: C4
    text: "The failing orders are always placed in the `last minute before midnight`."
    cites: ["artifacts/ABC-150/run_01M51J1JQ8DA092WHVF9MT7BE9/ticket.md#L22"]
trace: [REQ-1, REQ-2, REQ-3]
known: [REQ-1]
assumed: [REQ-2]
unknown: [U-1, U-2, U-3]
requirements:
  - id: REQ-1
    title: Late-evening order listed on its own day
    type: functional
    status: known
    statement: "An order placed at `23:59:30` UTC is listed in order history when from and to are set to that same day."
    cites: ["artifacts/ABC-150/run_01M51J1JQ8DA092WHVF9MT7BE9/ticket.md#L14", "artifacts/ABC-150/run_01M51J1JQ8DA092WHVF9MT7BE9/ticket.md#L15", "artifacts/ABC-150/run_01M51J1JQ8DA092WHVF9MT7BE9/ticket.md#L17"]
  - id: REQ-2
    title: No order is lost between consecutive days
    type: functional
    status: assumed
    statement: "Every order appears in history on exactly one day, so an order placed in the `last minute before midnight` is never missing from both that day and the next."
    basis: "The ticket reports the order appears on neither day; a reasonable reading is that day ranges must not leave a gap at the day boundary."
    cites: ["artifacts/ABC-150/run_01M51J1JQ8DA092WHVF9MT7BE9/ticket.md#L18", "artifacts/ABC-150/run_01M51J1JQ8DA092WHVF9MT7BE9/ticket.md#L22"]
  - id: REQ-3
    title: Time zone that defines an order's day
    type: constraint
    status: unknown
    statement: "The day used by the history from/to filters is defined in a time zone not yet decided: the ticket reports `UTC`, the baseline hospital zone is `Asia/Kolkata`."
    cites: ["artifacts/ABC-150/run_01M51J1JQ8DA092WHVF9MT7BE9/ticket.md#L20", "context/intent-baseline.md#constraints"]
unknowns:
  - id: U-1
    text: "The ticket is about `order history`, but the system baseline covers only doctors, patients and appointments; no orders exist in it."
    question: Q-01
  - id: U-2
    text: "The ticket's day is in `UTC` (API server zone), while the baseline says days use the hospital zone `Asia/Kolkata`."
    question: Q-02
  - id: U-3
    text: "The terms `order` and `order history` are not in the glossary."
    question: Q-01
questions:
  - id: Q-01
    blocking: true
    ask: product owner
    text: "Does ABC-150 belong to this system (SYS-SMP2, hospital management)? The ticket describes `order history`, which the baseline and glossary do not contain."
    why: Design cannot start without knowing which feature, if any, the ticket refers to in this system.
    evidence: ["artifacts/ABC-150/run_01M51J1JQ8DA092WHVF9MT7BE9/ticket.md#L11", "context/intent-baseline.md#goals", "context/glossary.md#glossary"]
    options: ["Wrong system: stop this run and file the ticket against the right system", "It means appointment history on this system: rewrite the ticket in those terms", "It is a new orders feature for this system"]
    default: null
  - id: Q-02
    blocking: true
    ask: tech lead
    text: "Which time zone defines 'that same day' in the history filter: `UTC` as the ticket reports, or the hospital zone `Asia/Kolkata` from the baseline?"
    why: The fix for a day-boundary bug depends entirely on which zone draws the boundary.
    evidence: ["artifacts/ABC-150/run_01M51J1JQ8DA092WHVF9MT7BE9/ticket.md#L20", "context/intent-baseline.md#constraints"]
    options: ["UTC", "Asia/Kolkata (hospital zone)"]
    default: null
glossary:
  - {term: order, found: false}
  - {term: order history, found: false}
---
# Requirements review: ABC-150

## Summary
ABC-150 reports a bug: an order placed at 23:59:30 UTC does not show in order history for that day, or for
the next day. It happens to a few orders every day, always in the last minute before midnight. The main
risk is that this ticket does not seem to belong to this system. The SYS-SMP2 baseline and glossary
cover doctors, patients and appointments, and neither mentions orders (U-1, U-3). A second blocking point
is the time zone: the ticket's day is in UTC, but the baseline puts days in the hospital zone
Asia/Kolkata (U-2). Design cannot start until both are answered.

## Requirements
- REQ-1 Late-evening order listed on its own day — known — an order placed at `23:59:30` UTC is listed when from and to are that same day.
- REQ-2 No order is lost between consecutive days — assumed — every order appears in history on exactly one day.
- REQ-3 Time zone that defines an order's day — unknown — UTC (ticket) or Asia/Kolkata (baseline).

## Known
- REQ-1: the ticket's steps and expected result state it directly (L14, L15, L17).

## Assumed
- REQ-2: the ticket says the order appears on neither day (L18). A reasonable reading is that the day ranges must have no gap at midnight, so each order shows on exactly one day.

## Unknown
- U-1: the ticket concerns order history, which this system's baseline does not contain → Q-01 (blocking).
- U-2: the day is UTC in the ticket but Asia/Kolkata in the baseline → Q-02 (blocking).
- U-3: "order" and "order history" are missing from the glossary → Q-01.

## Open questions
- Q-01 (blocking, product owner): does ABC-150 belong to this system at all? It needs an answer before design.
- Q-02 (blocking, tech lead): which time zone defines a day for the history filter, UTC or Asia/Kolkata? It needs an answer before design.

## Out of scope
- The reporter's support-agent handle is not repeated here. It is not needed for any requirement.
- How often the bug happens (a few orders per day) is evidence, not a requirement.
- Environment detail (staging) is context for reproduction, not a requirement.
