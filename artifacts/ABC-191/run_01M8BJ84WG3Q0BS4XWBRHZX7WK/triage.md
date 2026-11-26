---
schema: triage.v1
run_id: run_01M8BJ84WG3Q0BS4XWBRHZX7WK
step: triage
attempt: 1
skill: defect-triage@1.2.0
inputs:
  - {path: artifacts/ABC-191/run_01M8BJ84WG3Q0BS4XWBRHZX7WK/ticket.md, sha256: 97c8f71bec565a697b32e3a1e7ce7959d1270a4eeb5ce8c65327538207b35a16}
  - {path: context/generated/module-map.json, sha256: 4158c8fcd239c22cb8f1029dd11f61d8839867ff8088bd9ab76197e2db96db4b}
  - {path: context/architecture.md, sha256: 9faab093d10933a46f6473b25c80728f8745e597100e650cd2537bbafb2b595a}
  - {path: context/data-model.md, sha256: 2386ae151d54a73cdbe4fd9e0b69f8e8539c2870f5e52c12e5ed0191cfa95e55}
pack_sha: {sample-system: 7681f87749883d71abb4011d118366ca1242e1cc}
claims:
  - id: C1
    text: "`lastDays(now, tz, days)` is the shared period rule for the dashboard and the doctor page."
    cites: ["artifacts/ABC-191/run_01M8BJ84WG3Q0BS4XWBRHZX7WK/ticket.md#L13"]
  - id: C2
    text: "With `days = 0` or a negative number the window's `from` day is after its `to` day."
    cites: ["artifacts/ABC-191/run_01M8BJ84WG3Q0BS4XWBRHZX7WK/ticket.md#L15"]
  - id: C3
    text: "The expected behaviour is rejection with a `RangeError` that names the value."
    cites: ["artifacts/ABC-191/run_01M8BJ84WG3Q0BS4XWBRHZX7WK/ticket.md#L17"]
  - id: C4
    text: "The symbol `lastDays` is defined in the api module."
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/lib/periods.ts:lastDays"]
trace: []
known: [U-0]
assumed: []
unknown: []
questions: []
candidates:
  - component: module:sample-system:api
    likelihood: high
    cites:
      - "artifacts/ABC-191/run_01M8BJ84WG3Q0BS4XWBRHZX7WK/ticket.md#L13"
      - "context/generated/module-map.json#sym:sample-system:api/src/lib/periods.ts:lastDays"
---
# Defect triage: ABC-191
## Symptoms
- C1: `lastDays(now, tz, days)` is the shared period rule for the dashboard and the doctor page (ticket L13).
- C2: With `days = 0` or a negative number, the `from` day is after the `to` day and `start` is after `end`, so queries count nothing (ticket L15).
- C3: The expected behaviour is a `RangeError` naming the value, for any day count that is not a whole number of at least 1 (ticket L17).

## Candidates
- `module:sample-system:api`, likelihood high. C4: the symbol `lastDays` is defined in `api/src/lib/periods.ts`. The ticket limits the scope to that file (ticket L19).
- Next checks: confirm that `lastDays` does not validate `days`, and add tests for 0, a negative number and a fraction. Valid counts must behave as before (ticket L19).

## Known
- U-0: the defect is reproducible by calling `lastDays` with `days = 0` (ticket L15).

## Assumed
None.

## Unknown
None.

## Open questions
None.
