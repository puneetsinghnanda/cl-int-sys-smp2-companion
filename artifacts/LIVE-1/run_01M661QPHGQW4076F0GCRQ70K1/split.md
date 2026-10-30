---
schema: stories.v1
run_id: run_01M661QPHGQW4076F0GCRQ70K1
step: split
attempt: 1
skill: story-split@1.0.1
inputs:
  - path: artifacts/LIVE-1/run_01M661QPHGQW4076F0GCRQ70K1/req.md
    sha256: 9514b56924da6973313b7c5448cdd9f9f0335306e31e8a0294dab29fad5a138b
  - path: context/glossary.md
    sha256: 7623345ad8bad33b8a14a713b452edb2a36834e8b1c2228b76f3aa154034f16e
pack_sha:
  sample-system: bd98ff08b83f8762c5e5c073ba5fadef88169b33
claims:
  - id: C1
    text: "The users of the patient list are `Front-desk staff`."
    cites: ["context/glossary.md#front-desk-staff"]
trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5]
known: [S-2]
assumed: [S-1]
unknown:
  - id: U-1
    blocking: false
questions:
  - id: Q-01
    blocking: false
    ask: product owner
    text: "Should the number count every matching patient, or only the patients on the current page?"
    why: "S-1 AC3 relies on the assumed REQ-5; the intent raised this as its Q-01."
    evidence: ["artifacts/LIVE-1/run_01M661QPHGQW4076F0GCRQ70K1/req.md#L59"]
    options: ["Every matching patient", "Only the patients on the current page"]
    default: "Every matching patient, as the intent's REQ-5 assumes."
stories:
  - id: S-1
    title: See how many patients match the search
    trace: [REQ-1, REQ-2, REQ-5]
    acceptance_criteria:
      - id: AC1
        text: "Given the patient list with a search that 12 patients match, when the list is shown, then it shows `12 patients`."
      - id: AC2
        text: "Given the patient list showing a count, when the search text changes, then the count changes to the number of patients that match the new search text."
      - id: AC3
        text: "Given more patients match than the list shows at once, when the list is shown, then the count is the number of every patient that matches the search (assumed, REQ-5)."
  - id: S-2
    title: See zero when no patient matches
    trace: [REQ-3, REQ-4]
    acceptance_criteria:
      - id: AC1
        text: "Given a search that no patient matches, when the list is shown, then it shows `0 patients`."
      - id: AC2
        text: "Given a search that no patient matches, when the list is shown, then the existing empty message is still shown."
---
# Stories: LIVE-1

## Stories
**S-1 See how many patients match the search** (REQ-1, REQ-2, REQ-5)
- AC1: Given a search that 12 patients match, when the list is shown, then it shows "12 patients".
- AC2: Given a count is shown, when the search text changes, then the count follows the new search.
- AC3: Given more matches than the list shows at once, when the list is shown, then the count is of every match (assumed, REQ-5).

**S-2 See zero when no patient matches** (REQ-3, REQ-4)
- AC1: Given no patient matches, when the list is shown, then it shows "0 patients".
- AC2: Given no patient matches, when the list is shown, then the existing empty message is still shown.

## Known
- S-2: built only from the known REQ-3 and REQ-4.

## Assumed
- S-1: AC3 relies on the assumed REQ-5 (count every match, not one page); basis in the intent.

## Unknown
- U-1 (non-blocking): whether the count is of every match or of one page if the list is paged (Q-01).

## Open questions
- Q-01 (non-blocking, product owner): count every match or only the current page? Default: every match.
