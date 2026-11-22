---
schema: change.v1
run_id: run_01M81PTAY0WBBGAGVYF6QV4PCW
step: impl
attempt: 1
skill: implement@1.3.0
inputs:
  - path: artifacts/HMS-28/run_01M81PTAY0WBBGAGVYF6QV4PCW/design.md
    sha256: 6da6e924c4da45ca038fca32ca4d30d8ecf05f857d56510c1a5fb2e9789a0efc
  - path: artifacts/HMS-28/run_01M81PTAY0WBBGAGVYF6QV4PCW/split.md
    sha256: 2a67d6bac48a0f331a38dd9686cc75735f7f339ab7c2b0be8ed789c19e99fa98
  - path: context/conventions.md
    sha256: dc53d94ec34eb193eeb06fdd0db19eb86a24c29f7d3c434e7c212d61038ca852
  - path: context/generated/module-map.json
    sha256: 4158c8fcd239c22cb8f1029dd11f61d8839867ff8088bd9ab76197e2db96db4b
pack_sha:
  sample-system: 7681f87749883d71abb4011d118366ca1242e1cc
code:
  - repo: sample-system
    base_sha: 7681f87749883d71abb4011d118366ca1242e1cc
    head_sha: d9f36ffeafd3dcae8215bf0ecb1d0a9c4113c567
    diff_sha256: 5681253aebf8ebbcd91ec8202d670da69ac3353ca0ee366fc9566d0d76863326
    files:
      - api/src/repositories/doctorRepo.ts
      - api/src/services/doctorService.ts
      - api/src/types.ts
      - web/src/__tests__/DoctorsPage.test.tsx
      - web/src/api/types.ts
      - web/src/pages/DoctorsPage.tsx
claims:
  - id: C1
    text: "The design changes `DoctorFilter` and DoctorRepo.list to take a weekday filter."
    cites: ["artifacts/HMS-28/run_01M81PTAY0WBBGAGVYF6QV4PCW/design.md#item:CO-1"]
  - id: C2
    text: "The design adds `DoctorRepo.weekdaysByDoctor`."
    cites: ["artifacts/HMS-28/run_01M81PTAY0WBBGAGVYF6QV4PCW/design.md#item:CO-2"]
  - id: C3
    text: "The design changes `DoctorService.list` to parse weekday and add weekdays to rows."
    cites: ["artifacts/HMS-28/run_01M81PTAY0WBBGAGVYF6QV4PCW/design.md#item:CO-3"]
  - id: C4
    text: "The design adds `weekdays: number[]` to the API DoctorListRow type."
    cites: ["artifacts/HMS-28/run_01M81PTAY0WBBGAGVYF6QV4PCW/design.md#item:CO-4"]
  - id: C5
    text: "The design adds `weekdays: number[]` to the web DoctorListRow type."
    cites: ["artifacts/HMS-28/run_01M81PTAY0WBBGAGVYF6QV4PCW/design.md#item:CO-5"]
  - id: C6
    text: "The design changes `DoctorsPage` to add the Works on column and filter."
    cites: ["artifacts/HMS-28/run_01M81PTAY0WBBGAGVYF6QV4PCW/design.md#item:CO-6"]
  - id: C7
    text: "The column and filter use the existing `WEEKDAYS` names."
    cites: ["artifacts/HMS-28/run_01M81PTAY0WBBGAGVYF6QV4PCW/design.md#item:CO-7"]
  - id: C8
    text: "The existing HMS-6 page test pins the exact header list (`'Fee', 'Status'` then the later columns), which the design changes."
    cites: ["sample-system:web/src/__tests__/DoctorsPage.test.tsx#L47"]
  - id: C9
    text: "Invalid input is answered 400 with `ValidationError`."
    cites: ["context/conventions.md#errors"]
trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10, REQ-11, REQ-12, REQ-13, REQ-14, REQ-15]
known: [CO-1, CO-2, CO-3, CO-4, CO-5, CO-6, CO-7, CO-8, EP-1]
assumed: []
unknown: []
questions: []
summary: "Adds the HMS-28 Works on weekday column and filter to the doctor list: an EXISTS weekday filter in DoctorRepo.list, a new DoctorRepo.weekdaysByDoctor, weekday parsing and per-row weekdays in DoctorService.list (with_today only), weekdays on both DoctorListRow types, and the column and filter in DoctorsPage."
---
# Change: HMS-28

## Summary
The change builds the approved design and nothing more:
- CO-1: `DoctorFilter` gets an optional `weekday`. When it is set, `DoctorRepo.list` adds `EXISTS (SELECT 1 FROM availability_block ab WHERE ab.doctor_id = d.id AND ab.weekday = $n)` to the WHERE clause that the count and page queries share. Doctors with two blocks that day are listed and counted once, and the total and pages reflect the filter.
- CO-2: the new `DoctorRepo.weekdaysByDoctor(ids)` runs one `array_agg(DISTINCT weekday ORDER BY weekday)` query per page and returns a Map from doctor id to weekdays. It returns an empty Map for no ids.
- CO-3: `DoctorService.list` reads `weekday` with `f.oneOf('weekday', ['1' … '7'])`, as the status filter does, and converts it with `Number`. Any other value becomes a 400 `ValidationError` on field weekday through the existing `f.done()` (C9). In the `with_today` branch, each row also gets `weekdays` (default []).
- CO-4, CO-5: both `DoctorListRow` types gain `weekdays: number[]`.
- CO-6: `DoctorsPage` reads and writes `weekday` in the search params through the existing `set` helper, which also resets the page. It adds a "Works on" select with "Any day" and Monday to Sunday (values 1 to 7) from `WEEKDAYS` (CO-7). It also adds a "Works on" column between Status and Today, showing short names joined by ", " or "No availability".
- CO-8 and EP-1: the client and router pass the new query parameter through unchanged, so no edit was needed there. EP-1 changes only through CO-1 and CO-3.

## Changes
- api/src/repositories/doctorRepo.ts: CO-1 (`weekday` on DoctorFilter, EXISTS condition) and CO-2 (new `weekdaysByDoctor`).
- api/src/services/doctorService.ts: CO-3 (`WEEKDAY_VALUES`, weekday parsing, `weekdays` on rows with with_today).
- api/src/types.ts: CO-4 (`weekdays: number[]` on DoctorListRow).
- web/src/api/types.ts: CO-5 (`weekdays: number[]` on DoctorListRow).
- web/src/pages/DoctorsPage.tsx: CO-6 (`worksOn` helper, Works on filter, Works on column).
- web/src/__tests__/DoctorsPage.test.tsx: updated the existing HMS-6 header test, because it pins the column list that CO-6 changes (C8). It is renamed "HMS-6 REQ-1, REQ-11, HMS-28 REQ-13: Today stays last, after the Works on column" and now expects Works on before Today. The fixture rows gain `weekdays` so they match the changed type. I wrote no new tests; the unit-test step covers them.

## Known
- CO-1: built as designed (C1).
- CO-2: built as designed (C2).
- CO-3: built as designed (C3); errors follow the conventions (C9).
- CO-4: built as designed (C4).
- CO-5: built as designed (C5).
- CO-6: built as designed (C6), using the existing names (C7).
- CO-7: existing `WEEKDAYS`, used unchanged (C7).
- CO-8: existing pass-through; no edit needed.
- EP-1: changed only through CO-1 and CO-3.

## Assumed
None

## Unknown
None

## Open questions
None
