---
schema: stories.v1
run_id: run_01M79P1N889PBTFM8J1EX89TBK
step: split
attempt: 1
skill: story-split@1.3.0
inputs:
  - {path: artifacts/HMS-5/run_01M79P1N889PBTFM8J1EX89TBK/req.md, sha256: a03c0b2820af5070279380915fbacc62e5ebb19c702b593d4bd9e20385a7a8f6}
  - {path: context/glossary.md, sha256: 430a90e14c50f969f7f757089c61cf254eb21596ed6e60abb8bf59286822b9ff}
pack_sha: {sample-system: 7681f87749883d71abb4011d118366ca1242e1cc}
claims:
  - {id: C1, text: "The patient page shows a `Visit summary` above the appointments list.", cites: ["artifacts/HMS-5/run_01M79P1N889PBTFM8J1EX89TBK/req.md#item:REQ-1"]}
  - {id: C2, text: "The Visit summary shows the number of the patient's `Completed` appointments.", cites: ["artifacts/HMS-5/run_01M79P1N889PBTFM8J1EX89TBK/req.md#item:REQ-2"]}
  - {id: C3, text: "The Visit summary shows the number of the patient's `No-show` appointments.", cites: ["artifacts/HMS-5/run_01M79P1N889PBTFM8J1EX89TBK/req.md#item:REQ-3"]}
  - {id: C4, text: "`Scheduled and Cancelled` appointments are not counted in either number.", cites: ["artifacts/HMS-5/run_01M79P1N889PBTFM8J1EX89TBK/req.md#item:REQ-4"]}
  - {id: C5, text: "A patient with no Completed or No-show appointments shows `0 for each`.", cites: ["artifacts/HMS-5/run_01M79P1N889PBTFM8J1EX89TBK/req.md#item:REQ-5"]}
  - {id: C6, text: "Both counts cover `All time`, with no period picker.", cites: ["artifacts/HMS-5/run_01M79P1N889PBTFM8J1EX89TBK/req.md#item:REQ-6", "runs/run_01M79P1N889PBTFM8J1EX89TBK/ide-answers.md#a-01"]}
  - {id: C7, text: "The patient page's appointments list is `unchanged`.", cites: ["artifacts/HMS-5/run_01M79P1N889PBTFM8J1EX89TBK/req.md#item:REQ-7"]}
  - {id: C8, text: "The patient page's `medical history` is unchanged.", cites: ["artifacts/HMS-5/run_01M79P1N889PBTFM8J1EX89TBK/req.md#item:REQ-8"]}
  - {id: C9, text: "`The rest of the patient page` is unchanged apart from the new Visit summary.", cites: ["artifacts/HMS-5/run_01M79P1N889PBTFM8J1EX89TBK/req.md#item:REQ-9"]}
  - {id: C10, text: "The appointments list shows appointments `upcoming first, then past`.", cites: ["artifacts/HMS-5/run_01M79P1N889PBTFM8J1EX89TBK/req.md#claim:C1"]}
  - {id: C11, text: "A No-show is a past appointment the patient `did not attend`.", cites: ["context/glossary.md#no-show"]}
  - {id: C12, text: "The glossary Period includes `Last 30 days`.", cites: ["context/glossary.md#period"]}
trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9]
known: [S-1, S-2, S-3]
assumed: []
unknown: []
questions: []
stories:
  - id: S-1
    title: "See a patient's Completed and No-show counts"
    trace: [REQ-1, REQ-2, REQ-3, REQ-6]
    acceptance_criteria:
      - {id: AC1, text: "Given a patient with 2 Completed appointments and 1 No-show appointment, when staff open the patient page, then a Visit summary appears above the appointments list with a Completed count of 2 and a No-show count of 1."}
      - {id: AC2, text: "Given a patient with 3 No-show appointments and no other appointments, when staff open the patient page, then the No-show count is 3 and the Completed count is 0, so the two counts are not swapped or merged."}
      - {id: AC3, text: "Given a patient whose only Completed appointment started on 2 January 2025, well outside any Last 30 days period, when staff open the patient page, then the Completed count is 1, because the counts cover All time."}
      - {id: AC4, text: "Given any patient, when staff open the patient page, then the Visit summary offers no period picker (Today, Last 7 days or Last 30 days)."}
  - id: S-2
    title: "Only Completed and No-show appointments are counted"
    trace: [REQ-4, REQ-5]
    acceptance_criteria:
      - {id: AC1, text: "Given a patient with 1 Completed appointment, 1 Scheduled appointment and 1 Cancelled appointment, where the Cancelled one is the most recent, when staff open the patient page, then the Completed count is 1 and the No-show count is 0."}
      - {id: AC2, text: "Given a patient with only Scheduled and Cancelled appointments, when staff open the patient page, then the Visit summary shows 0 for each."}
      - {id: AC3, text: "Given a patient with no appointments at all, when staff open the patient page, then the Visit summary shows 0 for each."}
  - id: S-3
    title: "The rest of the patient page is unchanged"
    trace: [REQ-7, REQ-8, REQ-9]
    acceptance_criteria:
      - {id: AC1, text: "Given a patient with one upcoming Scheduled appointment and one past Completed appointment, when staff open the patient page, then the appointments list shows them upcoming first, then past, exactly as before the change."}
      - {id: AC2, text: "Given a patient with only past appointments and no upcoming one, when staff open the patient page, then the appointments list shows the same rows in the same order as before the change."}
      - {id: AC3, text: "Given a patient with medical history recorded, when staff open the patient page, then the medical history shows the same fields and values as before the change."}
      - {id: AC4, text: "Given any patient, when staff open the patient page, then every part of the page other than the new Visit summary shows the same content and layout as before the change."}
---
# Stories: HMS-5

## Stories
**S-1 See a patient's Completed and No-show counts** (REQ-1, REQ-2, REQ-3, REQ-6)
- AC1: Given a patient with 2 Completed appointments and 1 No-show appointment, when staff open the patient page, then a Visit summary appears above the appointments list with a Completed count of 2 and a No-show count of 1.
- AC2: Given a patient with 3 No-show appointments and no other appointments, when staff open the patient page, then the No-show count is 3 and the Completed count is 0, so the two counts are not swapped or merged.
- AC3: Given a patient whose only Completed appointment started on 2 January 2025, well outside any Last 30 days period, when staff open the patient page, then the Completed count is 1, because the counts cover All time.
- AC4: Given any patient, when staff open the patient page, then the Visit summary offers no period picker (Today, Last 7 days or Last 30 days).

**S-2 Only Completed and No-show appointments are counted** (REQ-4, REQ-5)
- AC1: Given a patient with 1 Completed appointment, 1 Scheduled appointment and 1 Cancelled appointment, where the Cancelled one is the most recent, when staff open the patient page, then the Completed count is 1 and the No-show count is 0.
- AC2: Given a patient with only Scheduled and Cancelled appointments, when staff open the patient page, then the Visit summary shows 0 for each.
- AC3: Given a patient with no appointments at all, when staff open the patient page, then the Visit summary shows 0 for each.

**S-3 The rest of the patient page is unchanged** (REQ-7, REQ-8, REQ-9)
- AC1: Given a patient with one upcoming Scheduled appointment and one past Completed appointment, when staff open the patient page, then the appointments list shows them upcoming first, then past, exactly as before the change.
- AC2: Given a patient with only past appointments and no upcoming one, when staff open the patient page, then the appointments list shows the same rows in the same order as before the change.
- AC3: Given a patient with medical history recorded, when staff open the patient page, then the medical history shows the same fields and values as before the change.
- AC4: Given any patient, when staff open the patient page, then every part of the page other than the new Visit summary shows the same content and layout as before the change.

## Known
- S-1: built from known REQ-1, REQ-2, REQ-3 and REQ-6 (REQ-6 decided by answer A-01, All time).
- S-2: built from known REQ-4 and REQ-5.
- S-3: built from known REQ-7, REQ-8 and REQ-9.

## Assumed
None.

## Unknown
None. Every known REQ of the intent is covered, and the intent has no unknowns.

## Open questions
None.
