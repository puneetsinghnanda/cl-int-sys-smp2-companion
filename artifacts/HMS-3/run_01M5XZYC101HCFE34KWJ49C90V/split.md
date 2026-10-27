---
schema: stories.v1
run_id: run_01M5XZYC101HCFE34KWJ49C90V
step: split
attempt: 1
skill: story-split@1.0.1
inputs:
  - path: artifacts/HMS-3/run_01M5XZYC101HCFE34KWJ49C90V/req.md
    sha256: 50be375fd64aa765d6499709646b264f9c247964fabbc6dc3fdcf28aee13fa9c
  - path: context/glossary.md
    sha256: 9bfd556283382d3d3835bf3b12514acc4f6941dc24f21cba0c8eaa4cd658a36c
pack_sha:
  sample-system: 415369398bdad3122ac084a00766a01a3034f900
claims:
  - id: C1
    text: "The dashboard gets a tile titled `No-shows`."
    cites: ["artifacts/HMS-3/run_01M5XZYC101HCFE34KWJ49C90V/req.md#L41"]
  - id: C2
    text: "The selected period is one of `Today`, Last 7 days or Last 30 days."
    cites: ["artifacts/HMS-3/run_01M5XZYC101HCFE34KWJ49C90V/req.md#L53"]
  - id: C3
    text: "No-shows are counted like the `Completed and Cancelled` tiles."
    cites: ["artifacts/HMS-3/run_01M5XZYC101HCFE34KWJ49C90V/req.md#L59"]
  - id: C4
    text: "A `No-show` is a past appointment the patient did not attend."
    cites: ["context/glossary.md#no-show"]
trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10]
known: [S-1, S-2, S-3]
assumed: []
unknown:
  - {id: U-1, blocking: false}
questions:
  - id: Q-01
    blocking: false
    ask: tech lead
    text: "Do the existing Completed and Cancelled tiles count by appointment start time in the hospital time zone, so S-1 AC5 can be checked against them?"
    why: "S-1 AC5 says 'the same way' as those tiles; the requirements carry this open (their Q-01)."
    evidence: ["artifacts/HMS-3/run_01M5XZYC101HCFE34KWJ49C90V/req.md#L59"]
    options: ["Copy the existing tiles' counting exactly", "Define the No-shows count separately"]
    default: "Copy the existing Completed and Cancelled tiles exactly."
stories:
  - id: S-1
    title: See how many No-shows there were in the period
    trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5]
    acceptance_criteria:
      - {id: AC1, text: "Given staff open the dashboard, when it loads, then a tile titled \"No-shows\" is shown."}
      - {id: AC2, text: "Given No-show appointments that start in the selected period, when the dashboard loads, then the No-shows tile shows how many there are."}
      - {id: AC3, text: "Given No-show appointments that start outside the selected period, when the dashboard loads, then they are not counted in the No-shows tile."}
      - {id: AC4, text: "Given the period is Today, when staff change it to Last 7 days or Last 30 days, then the No-shows count updates to that period."}
      - {id: AC5, text: "Given any selected period, when the dashboard loads, then No-shows are counted the same way as the Completed and Cancelled tiles count theirs."}
  - id: S-2
    title: Open the No-show appointments from the tile
    trace: [REQ-6, REQ-7]
    acceptance_criteria:
      - {id: AC1, text: "Given the dashboard is open, when staff select the No-shows tile, then the appointment list opens filtered to No-show."}
      - {id: AC2, text: "Given a selected period, when staff select the No-shows tile, then the appointment list is filtered to that selected period."}
  - id: S-3
    title: The rest of the dashboard stays as it was
    trace: [REQ-8, REQ-9, REQ-10]
    acceptance_criteria:
      - {id: AC1, text: "Given the No-shows tile is added, when the dashboard loads, then the other tiles and their counts are unchanged."}
      - {id: AC2, text: "Given the dashboard is open, when it loads, then no medical history appears on it."}
      - {id: AC3, text: "Given the dashboard is open, when it loads, then no contact details appear on it."}
---
# Stories: HMS-3

## Stories
- S-1 See how many No-shows there were in the period (REQ-1 to REQ-5)
  - AC1: Given staff open the dashboard, when it loads, then a tile titled "No-shows" is shown (C1).
  - AC2: Given No-show appointments that start in the selected period, when the dashboard loads, then the tile shows how many there are (C4).
  - AC3: Given No-show appointments that start outside the selected period, when the dashboard loads, then they are not counted.
  - AC4: Given the period is Today, when staff change it to Last 7 days or Last 30 days, then the count updates to that period (C2).
  - AC5: Given any selected period, when the dashboard loads, then No-shows are counted the same way as the Completed and Cancelled tiles (C3).
- S-2 Open the No-show appointments from the tile (REQ-6, REQ-7)
  - AC1: Given the dashboard is open, when staff select the No-shows tile, then the appointment list opens filtered to No-show.
  - AC2: Given a selected period, when staff select the tile, then the list is filtered to that period.
- S-3 The rest of the dashboard stays as it was (REQ-8, REQ-9, REQ-10)
  - AC1: Given the tile is added, when the dashboard loads, then the other tiles and their counts are unchanged.
  - AC2: Given the dashboard is open, when it loads, then no medical history appears on it.
  - AC3: Given the dashboard is open, when it loads, then no contact details appear on it.

## Known
- S-1: built from known REQ-1 to REQ-5 only.
- S-2: built from known REQ-6 and REQ-7 only.
- S-3: built from known REQ-8 to REQ-10 only.

## Assumed
- None.

## Unknown
- U-1 (non-blocking): how the existing Completed and Cancelled tiles count is not in the inputs, so S-1 AC5 is checked against those tiles as they are (Q-01).

## Open questions
- Q-01 (non-blocking, tech lead): do the existing tiles count by start time in the hospital time zone? Default: copy them exactly.
