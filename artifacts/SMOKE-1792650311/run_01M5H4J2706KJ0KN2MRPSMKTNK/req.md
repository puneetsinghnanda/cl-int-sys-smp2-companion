---
schema: intent.v1
run_id: run_01M5H4J2706KJ0KN2MRPSMKTNK
step: req
attempt: 1
skill: requirement-review@1.0.0
inputs:
- path: artifacts/SMOKE-1792650311/run_01M5H4J2706KJ0KN2MRPSMKTNK/ticket.md
  sha256: a95cdb24ab2e11506c71a20da0b6b25bf126d5d6694cbec3cdc09795b9235e76
- path: context/intent-baseline.md
  sha256: 9bfd821aeb2fb9524863e346707e049e4fcfb949a6e55b90223a79194946a215
- path: context/glossary.md
  sha256: 9bfd556283382d3d3835bf3b12514acc4f6941dc24f21cba0c8eaa4cd658a36c
pack_sha:
  sample-system: 415369398bdad3122ac084a00766a01a3034f900
claims:
- id: C1
  text: The ticket asks for a heading
  cites:
  - artifacts/SMOKE-1792650311/run_01M5H4J2706KJ0KN2MRPSMKTNK/ticket.md#L12
trace:
- REQ-1
known:
- REQ-1
assumed: []
unknown: []
requirements:
- id: REQ-1
  title: Front page heading
  type: functional
  status: known
  statement: The front page shows a heading.
  cites:
  - artifacts/SMOKE-1792650311/run_01M5H4J2706KJ0KN2MRPSMKTNK/ticket.md#L12
unknowns: []
questions: []
glossary: []
---

# Requirements review

## Summary
The smoke ticket asks for a heading on the front page.

## Requirements
REQ-1 front page heading.

## Known
REQ-1 from the ticket.

## Assumed
None.

## Unknown
None.

## Open questions
None.

## Out of scope
Everything else.
