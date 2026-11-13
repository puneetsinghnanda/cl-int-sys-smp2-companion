---
schema: intent.v1
run_id: run_01M79P1N889PBTFM8J1EX89TBK
step: req
attempt: 1
skill: requirement-review@1.3.0
inputs:
  - {path: artifacts/HMS-5/run_01M79P1N889PBTFM8J1EX89TBK/ticket.md, sha256: b78f67028d76de344dcdca873ff0f2bc52874b3353478f72549bdc089a370e06}
  - {path: context/intent-baseline.md, sha256: 9bfd821aeb2fb9524863e346707e049e4fcfb949a6e55b90223a79194946a215}
  - {path: context/glossary.md, sha256: 430a90e14c50f969f7f757089c61cf254eb21596ed6e60abb8bf59286822b9ff}
pack_sha: {sample-system: 7681f87749883d71abb4011d118366ca1242e1cc}
claims:
  - {id: C1, text: "The patient page already lists the patient's appointments, `upcoming first, then past`.", cites: ["artifacts/HMS-5/run_01M79P1N889PBTFM8J1EX89TBK/ticket.md#L14"]}
  - {id: C2, text: "The patient page shows a `Visit summary` above the appointments list.", cites: ["artifacts/HMS-5/run_01M79P1N889PBTFM8J1EX89TBK/ticket.md#L16"]}
  - {id: C3, text: "The summary shows the number of the patient's `Completed` appointments.", cites: ["artifacts/HMS-5/run_01M79P1N889PBTFM8J1EX89TBK/ticket.md#L17"]}
  - {id: C4, text: "The summary shows the number of the patient's `No-show` appointments.", cites: ["artifacts/HMS-5/run_01M79P1N889PBTFM8J1EX89TBK/ticket.md#L17-L18"]}
  - {id: C5, text: "`Scheduled and Cancelled` appointments are not counted in either number.", cites: ["artifacts/HMS-5/run_01M79P1N889PBTFM8J1EX89TBK/ticket.md#L19"]}
  - {id: C6, text: "A patient with no Completed or No-show appointments shows `0 for each`.", cites: ["artifacts/HMS-5/run_01M79P1N889PBTFM8J1EX89TBK/ticket.md#L20"]}
  - {id: C7, text: "The rest of the patient page, including the appointments list and `medical history`, is unchanged.", cites: ["artifacts/HMS-5/run_01M79P1N889PBTFM8J1EX89TBK/ticket.md#L21-L22"]}
  - {id: C8, text: "Medical history appears `only on the patient's own page`.", cites: ["context/intent-baseline.md#data-protection"]}
  - {id: C9, text: "A Completed appointment is one the patient attended, closed `once its start time has passed`.", cites: ["context/glossary.md#completed"]}
  - {id: C10, text: "A No-show is a past appointment the patient `did not attend`.", cites: ["context/glossary.md#no-show"]}
  - {id: C11, text: "The glossary Period offers `Last 30 days` and is used by the dashboard tiles and the doctor page's No-shows count.", cites: ["context/glossary.md#period"]}
  - {id: C12, text: "The user chose to count over `All time`.", cites: ["runs/run_01M79P1N889PBTFM8J1EX89TBK/ide-answers.md#a-01"]}
  - {id: C13, text: "Front-desk staff act as `staff` in the MVP.", cites: ["context/glossary.md#front-desk-staff"]}
trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9]
known: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9]
assumed: []
unknown: []
requirements:
  - {id: REQ-1, title: "Visit summary placement", type: functional, status: known, statement: "The patient page shows a `Visit summary` above the appointments list.", cites: ["artifacts/HMS-5/run_01M79P1N889PBTFM8J1EX89TBK/ticket.md#L16"]}
  - {id: REQ-2, title: "Completed count", type: functional, status: known, statement: "The Visit summary shows the number of the patient's `Completed` appointments.", cites: ["artifacts/HMS-5/run_01M79P1N889PBTFM8J1EX89TBK/ticket.md#L17"]}
  - {id: REQ-3, title: "No-show count", type: functional, status: known, statement: "The Visit summary shows the number of the patient's `No-show` appointments.", cites: ["artifacts/HMS-5/run_01M79P1N889PBTFM8J1EX89TBK/ticket.md#L17-L18"]}
  - {id: REQ-4, title: "Other statuses not counted", type: functional, status: known, statement: "`Scheduled and Cancelled` appointments are not counted in either number.", cites: ["artifacts/HMS-5/run_01M79P1N889PBTFM8J1EX89TBK/ticket.md#L19"]}
  - {id: REQ-5, title: "Zero counts", type: functional, status: known, statement: "A patient with no Completed or No-show appointments shows `0 for each`.", cites: ["artifacts/HMS-5/run_01M79P1N889PBTFM8J1EX89TBK/ticket.md#L20"]}
  - {id: REQ-6, title: "Counting window", type: functional, status: known, statement: "Both counts cover `All time`: every Completed and No-show appointment of the patient, with no period picker.", cites: ["runs/run_01M79P1N889PBTFM8J1EX89TBK/ide-answers.md#a-01", "artifacts/HMS-5/run_01M79P1N889PBTFM8J1EX89TBK/ticket.md#L12-L13"]}
  - {id: REQ-7, title: "Appointments list unchanged", type: constraint, status: known, statement: "The patient page's appointments list is `unchanged`.", cites: ["artifacts/HMS-5/run_01M79P1N889PBTFM8J1EX89TBK/ticket.md#L21-L22"]}
  - {id: REQ-8, title: "Medical history unchanged", type: constraint, status: known, statement: "The patient page's `medical history` is unchanged.", cites: ["artifacts/HMS-5/run_01M79P1N889PBTFM8J1EX89TBK/ticket.md#L21-L22", "context/intent-baseline.md#data-protection"]}
  - {id: REQ-9, title: "Rest of page unchanged", type: constraint, status: known, statement: "`The rest of the patient page` is unchanged apart from the new Visit summary.", cites: ["artifacts/HMS-5/run_01M79P1N889PBTFM8J1EX89TBK/ticket.md#L21"]}
unknowns: []
questions: []
answers:
  - {id: A-01, kind: question, about: Q-01, answer: "All time (proposed default)", requirements: [REQ-6]}
glossary:
  - {term: "Visit summary", found: false}
  - {term: "Patient page", found: false}
  - {term: "Completed", found: true}
  - {term: "No-show", found: true}
  - {term: "Scheduled", found: true}
  - {term: "Cancelled", found: true}
  - {term: "Front-desk staff", found: true}
  - {term: "Medical history", found: true}
---
# Requirements review: HMS-5

## Summary
HMS-5 adds a `Visit summary` above the appointments list on the patient page (C2). The summary shows two counts, the patient's `Completed` and `No-show` appointments (C3, C4), and leaves out Scheduled and Cancelled ones (C5). A patient with neither shows 0 for each (C6), and the user chose to count over `All time` (C12). The main risk is touching the patient page, the one page that shows medical history (C8), so the rest of the page must stay unchanged (C7).

## Requirements
- REQ-1 Visit summary placement (known): the patient page shows a Visit summary above the appointments list.
- REQ-2 Completed count (known): the Visit summary shows the number of Completed appointments.
- REQ-3 No-show count (known): the Visit summary shows the number of No-show appointments.
- REQ-4 Other statuses not counted (known): Scheduled and Cancelled appointments are not counted.
- REQ-5 Zero counts (known): a patient with neither shows 0 for each.
- REQ-6 Counting window (known): both counts cover all time, with no period picker.
- REQ-7 Appointments list unchanged (known).
- REQ-8 Medical history unchanged (known).
- REQ-9 Rest of page unchanged (known).

## Known
- REQ-1: ticket L16.
- REQ-2, REQ-3: ticket L17-L18; the glossary defines Completed (C9) and No-show (C10).
- REQ-4: ticket L19.
- REQ-5: ticket L20.
- REQ-6: answer A-01 (All time).
- REQ-7, REQ-8, REQ-9: ticket L21-L22; baseline data protection keeps medical history on the patient's own page.

## Assumed
None.

## Unknown
None. "Visit summary" and "Patient page" are not in the glossary; the ticket defines the Visit summary at L16-L18 and the patient page at L14.

## Open questions
None open.

Answered in the IDE:
- A-01 (Q-01, answered by the user, eng-dbe4): which time window the counts use. Options were all time, a selectable Period (C11) or a fixed last 30 days. Answer: "All time (proposed default)". Decides REQ-6. No mismatch: the ticket gives no window.

## Out of scope
- Rates, percentages or highlighting of patients who often miss appointments: the ticket asks only for the two numbers (L17-L18).
- Following up with patients (user story L12-L13) stays a manual task for staff.
- Labels and layout: by default the counts are labelled with the status names "Completed" and "No-show". Trade-off: this matches the glossary, but the wording may differ from what product prefers.
- No instruction-like text, credentials or personal data were found in the inputs.
