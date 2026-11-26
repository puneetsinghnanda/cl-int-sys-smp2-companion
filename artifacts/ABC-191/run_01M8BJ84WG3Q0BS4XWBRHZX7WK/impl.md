---
schema: change.v1
run_id: run_01M8BJ84WG3Q0BS4XWBRHZX7WK
step: impl
attempt: 2
skill: implement@1.4.0
inputs:
  - {path: artifacts/ABC-191/run_01M8BJ84WG3Q0BS4XWBRHZX7WK/triage.md, sha256: 7c81d414265f679723c175c00feb5fcdfd139a35f4cab38eb1d4702f2a0fa6f2}
  - {path: context/conventions.md, sha256: dc53d94ec34eb193eeb06fdd0db19eb86a24c29f7d3c434e7c212d61038ca852}
  - {path: context/generated/module-map.json, sha256: 4158c8fcd239c22cb8f1029dd11f61d8839867ff8088bd9ab76197e2db96db4b}
pack_sha: {sample-system: 7681f87749883d71abb4011d118366ca1242e1cc}
code:
  - repo: sample-system
    base_sha: 7681f87749883d71abb4011d118366ca1242e1cc
    head_sha: 4fe520fef7e907f91028ee1d64333250375381ab
    diff_sha256: eb8cb458b752590cbab556b2c09400eda9fca67c3582ad5ac81d3ea446be794d
    files: ["api/src/lib/periods.ts"]
claims:
  - id: C1
    text: "`lastDays` now throws before computing the window when `days` is not a whole number of at least 1."
    cites: ["sample-system:api/src/lib/periods.ts#L15-L18"]
  - id: C2
    text: "The thrown error is a `RangeError` whose message includes the rejected value."
    cites: ["sample-system:api/src/lib/periods.ts#L17"]
  - id: C3
    text: "The ticket expects a `RangeError` that names the value."
    cites: ["artifacts/ABC-191/run_01M8BJ84WG3Q0BS4XWBRHZX7WK/ticket.md#L17"]
trace: [ABC-191]
known: [U-0]
assumed: []
unknown: []
questions: []
summary: "lastDays rejects a day count that is not a whole number of at least 1 with a RangeError naming the value; valid counts behave as before."
---
# Change: ABC-191
## Summary
This fixes the triage candidate `module:sample-system:api`. `lastDays` now rejects an invalid day count instead of returning an inverted window (C1, C2), as the ticket expects (C3). Valid counts follow the same path as before. The ticket limits the scope to `api/src/lib/periods.ts`, so no test file was changed.

## Changes
- `api/src/lib/periods.ts`: added a guard at the start of `lastDays` (api module).

## Known
- U-0: the triage's reproducible defect (a call with a day count of 0), now rejected with a `RangeError` (C3).

## Assumed
None.

## Unknown
None.

## Open questions
None.
