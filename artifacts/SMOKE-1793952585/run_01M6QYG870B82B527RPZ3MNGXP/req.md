---
schema: intent.v1
run_id: run_01M6QYG870B82B527RPZ3MNGXP
step: req
attempt: 1
skill: requirement-review@1.2.0
inputs:
- path: artifacts/SMOKE-1793952585/run_01M6QYG870B82B527RPZ3MNGXP/ticket.md
  sha256: 55c3faa701ece2133f2b689a329ddcaba9821a8fa93bf832f5ca547730773b8e
- path: context/intent-baseline.md
  sha256: 9bfd821aeb2fb9524863e346707e049e4fcfb949a6e55b90223a79194946a215
- path: context/glossary.md
  sha256: 430a90e14c50f969f7f757089c61cf254eb21596ed6e60abb8bf59286822b9ff
pack_sha:
  sample-system: c2709e164850ab7e177e4bccfc42a5dd9b961fdc
claims:
- id: C1
  text: The ticket asks for a `heading`
  cites:
  - artifacts/SMOKE-1793952585/run_01M6QYG870B82B527RPZ3MNGXP/ticket.md#L12
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
  - artifacts/SMOKE-1793952585/run_01M6QYG870B82B527RPZ3MNGXP/ticket.md#L12
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
