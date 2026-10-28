---
schema: stories.v1
run_id: run_01M60VHZHR79G15R3AKPB0VYDK
step: split
attempt: 1
skill: story-split@1.0.1
inputs:
  - path: artifacts/HMS-5/run_01M60VHZHR79G15R3AKPB0VYDK/req.md
    sha256: 7163b98904b9c026b5fe2fca4f8b92bd8e20975a6e468d30003d1f0f96c67a46
  - path: context/glossary.md
    sha256: 7623345ad8bad33b8a14a713b452edb2a36834e8b1c2228b76f3aa154034f16e
pack_sha:
  sample-system: bd98ff08b83f8762c5e5c073ba5fadef88169b33
claims:
  - id: C1
    text: "A `Completed` appointment is a past appointment the patient attended."
    cites: ["context/glossary.md#completed"]
  - id: C2
    text: "A `No-show` is a past appointment the patient did not attend."
    cites: ["context/glossary.md#no-show"]
  - id: C3
    text: "A `Scheduled` appointment is booked and still open."
    cites: ["context/glossary.md#scheduled"]
  - id: C4
    text: "A `Cancelled` appointment was called off before it took place."
    cites: ["context/glossary.md#cancelled"]
  - id: C5
    text: "Medical history covers blood group, `allergies`, chronic conditions, current medications and notes of one patient."
    cites: ["context/glossary.md#medical-history"]
trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9]
known: [S-2, S-3]
assumed: [S-1]
unknown: []
questions:
  - id: Q-01
    blocking: false
    ask: product owner
    text: "Should `Visit summary` be added to the glossary as the counts of one patient's Completed and No-show appointments? (intent U-1)"
    why: Keeps the term consistent between stories, design and tests.
    evidence: ["artifacts/HMS-5/run_01M60VHZHR79G15R3AKPB0VYDK/req.md#unknown", "context/glossary.md#glossary"]
    options: ["Add the definition as proposed", "Use a different label or definition"]
    default: "Define Visit summary as the counts of one patient's Completed and No-show appointments."
  - id: Q-02
    blocking: false
    ask: tech lead
    text: "Is the `patient page` the patient detail page of PRD PAT-3, where medical history is shown? (intent U-2)"
    why: S-1 to S-3 all target this page.
    evidence: ["artifacts/HMS-5/run_01M60VHZHR79G15R3AKPB0VYDK/req.md#unknown", "context/glossary.md#medical-history"]
    options: ["Yes, the patient detail page", "Another page"]
    default: "The patient's own detail page, where medical history is shown."
  - id: Q-03
    blocking: false
    ask: product owner
    text: "Should the counts cover the patient's whole history, or only a recent period? (intent REQ-9, used by S-1 AC4)"
    why: A date window changes the counts staff use to decide on follow-up.
    evidence: ["artifacts/HMS-5/run_01M60VHZHR79G15R3AKPB0VYDK/req.md#assumed"]
    options: ["All time", "A fixed recent period"]
    default: "All time, as the ticket names no period."
stories:
  - id: S-1
    title: See a patient's attended and missed appointment counts
    trace: [REQ-1, REQ-2, REQ-3, REQ-9]
    acceptance_criteria:
      - id: AC1
        text: "Given a patient, when front-desk staff open the patient page, then a Visit summary is shown above the appointments list."
      - id: AC2
        text: "Given a patient with Completed appointments, when staff open the patient page, then the Visit summary shows the number of that patient's Completed appointments."
      - id: AC3
        text: "Given a patient with No-show appointments, when staff open the patient page, then the Visit summary shows the number of that patient's No-show appointments."
      - id: AC4
        text: "Given a patient with Completed and No-show appointments on dates far apart in the past (assumed REQ-9), when staff open the patient page, then every one of them is included in the counts, with no date window."
  - id: S-2
    title: Count only closed attended and missed appointments
    trace: [REQ-4, REQ-5, REQ-6]
    acceptance_criteria:
      - id: AC1
        text: "Given a patient with Scheduled appointments, when staff open the patient page, then the Scheduled appointments are counted in neither the Completed nor the No-show number."
      - id: AC2
        text: "Given a patient with Cancelled appointments, when staff open the patient page, then the Cancelled appointments are counted in neither the Completed nor the No-show number."
      - id: AC3
        text: "Given a patient with no Completed and no No-show appointments, when staff open the patient page, then the Visit summary shows 0 for each count."
      - id: AC4
        text: "Given a patient with Completed appointments but no No-show appointments, when staff open the patient page, then the No-show count shows 0."
      - id: AC5
        text: "Given a patient with No-show appointments but no Completed appointments, when staff open the patient page, then the Completed count shows 0."
  - id: S-3
    title: Keep the rest of the patient page unchanged
    trace: [REQ-7, REQ-8]
    acceptance_criteria:
      - id: AC1
        text: "Given a patient with appointments, when staff open the patient page, then the appointments list shows the same appointments in the same order as before the change."
      - id: AC2
        text: "Given a patient with a recorded medical history, when staff open the patient page, then the medical history is shown as before the change."
      - id: AC3
        text: "Given any patient, when staff open the patient page, then every section other than the new Visit summary is shown as before the change."
---
# Stories: HMS-5

## Stories

### S-1 See a patient's attended and missed appointment counts
Traces: REQ-1, REQ-2, REQ-3, REQ-9
- AC1: Given a patient, when front-desk staff open the patient page, then a Visit summary is shown above the appointments list.
- AC2: Given a patient with Completed appointments, when staff open the patient page, then the Visit summary shows the number of that patient's Completed appointments.
- AC3: Given a patient with No-show appointments, when staff open the patient page, then the Visit summary shows the number of that patient's No-show appointments.
- AC4: Given a patient with Completed and No-show appointments on dates far apart in the past (assumed REQ-9), when staff open the patient page, then every one of them is included in the counts, with no date window.

### S-2 Count only closed attended and missed appointments
Traces: REQ-4, REQ-5, REQ-6
- AC1: Given a patient with Scheduled appointments, when staff open the patient page, then the Scheduled appointments are counted in neither number.
- AC2: Given a patient with Cancelled appointments, when staff open the patient page, then the Cancelled appointments are counted in neither number.
- AC3: Given a patient with no Completed and no No-show appointments, when staff open the patient page, then the Visit summary shows 0 for each count.
- AC4: Given a patient with Completed but no No-show appointments, when staff open the patient page, then the No-show count shows 0.
- AC5: Given a patient with No-show but no Completed appointments, when staff open the patient page, then the Completed count shows 0.

### S-3 Keep the rest of the patient page unchanged
Traces: REQ-7, REQ-8
- AC1: Given a patient with appointments, when staff open the patient page, then the appointments list shows the same appointments in the same order as before.
- AC2: Given a patient with a recorded medical history, when staff open the patient page, then the medical history is shown as before.
- AC3: Given any patient, when staff open the patient page, then every section other than the new Visit summary is shown as before.

## Known
- S-2: built only from known REQ-4, REQ-5 and REQ-6 (ticket acceptance criteria 3 and 4).
- S-3: built only from known REQ-7 and REQ-8 (ticket acceptance criterion 5).

## Assumed
- S-1: relies on assumed REQ-9, that the counts cover the patient's whole history with no date window. Basis: the ticket names no period and asks how often a patient attended or missed. Confirm via Q-03.

## Unknown
No requirement is unknown, so no story is held back. The intent's two non-blocking glossary gaps are carried as questions: "Visit summary" is not in the glossary (Q-01), and "patient page" is not in the glossary, with PRD PAT-3 not among the inputs (Q-02). The stories use the ticket's wording until they are answered.

## Open questions
- Q-01 (non-blocking, product owner): add "Visit summary" to the glossary? Default: the counts of one patient's Completed and No-show appointments.
- Q-02 (non-blocking, tech lead): is the patient page the patient detail page of PRD PAT-3? Default: yes, the page where medical history is shown.
- Q-03 (non-blocking, product owner): all-time counts or a recent period? Default: all time.
