---
schema: intent.v1
run_id: run_01M60VHZHR79G15R3AKPB0VYDK
step: req
attempt: 1
skill: requirement-review@1.0.0
inputs:
  - path: artifacts/HMS-5/run_01M60VHZHR79G15R3AKPB0VYDK/ticket.md
    sha256: 415bf2334ebfb4189067f9c25f44f104b8f8d30b2115d4dda94e0c99f78b48f4
  - path: context/intent-baseline.md
    sha256: 9bfd821aeb2fb9524863e346707e049e4fcfb949a6e55b90223a79194946a215
  - path: context/glossary.md
    sha256: 7623345ad8bad33b8a14a713b452edb2a36834e8b1c2228b76f3aa154034f16e
pack_sha:
  sample-system: bd98ff08b83f8762c5e5c073ba5fadef88169b33
claims:
  - id: C1
    text: "The patient page already lists the patient's appointments, `upcoming first`, then past."
    cites: ["artifacts/HMS-5/run_01M60VHZHR79G15R3AKPB0VYDK/ticket.md#L14"]
  - id: C2
    text: "`Completed` and No-show are allowed only after the appointment start time."
    cites: ["context/intent-baseline.md#closed-appointments"]
  - id: C3
    text: "Completed, Cancelled and No-show appointments `never change`."
    cites: ["context/intent-baseline.md#closed-appointments"]
  - id: C4
    text: "Medical history appears only on the `patient's own page`, never in lists, the dashboard or logs."
    cites: ["context/intent-baseline.md#data-protection"]
  - id: C5
    text: "In the MVP every user acts as `staff`."
    cites: ["context/glossary.md#front-desk-staff"]
  - id: C6
    text: "A `Scheduled` appointment is booked and still open."
    cites: ["context/glossary.md#scheduled"]
  - id: C7
    text: "A `No-show` is a past appointment the patient did not attend."
    cites: ["context/glossary.md#no-show"]
  - id: C8
    text: "A `Completed` appointment is a past appointment the patient attended."
    cites: ["context/glossary.md#completed"]
  - id: C9
    text: "A `Cancelled` appointment was called off before it took place."
    cites: ["context/glossary.md#cancelled"]
trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9]
known: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8]
assumed: [REQ-9]
unknown: [U-1, U-2]
requirements:
  - id: REQ-1
    title: Visit summary placed above the appointments list
    type: functional
    status: known
    statement: "The patient page shows a `Visit summary` above the appointments list."
    cites: ["artifacts/HMS-5/run_01M60VHZHR79G15R3AKPB0VYDK/ticket.md#L16"]
  - id: REQ-2
    title: Count of Completed appointments
    type: functional
    status: known
    statement: "The Visit summary shows the number of the patient's `Completed` appointments."
    cites: ["artifacts/HMS-5/run_01M60VHZHR79G15R3AKPB0VYDK/ticket.md#L17", "context/glossary.md#completed"]
  - id: REQ-3
    title: Count of No-show appointments
    type: functional
    status: known
    statement: "The Visit summary shows the number of the patient's `No-show` appointments."
    cites: ["artifacts/HMS-5/run_01M60VHZHR79G15R3AKPB0VYDK/ticket.md#L17", "context/glossary.md#no-show"]
  - id: REQ-4
    title: Scheduled appointments excluded
    type: functional
    status: known
    statement: "`Scheduled` appointments are not counted in either number."
    cites: ["artifacts/HMS-5/run_01M60VHZHR79G15R3AKPB0VYDK/ticket.md#L19", "context/glossary.md#scheduled"]
  - id: REQ-5
    title: Cancelled appointments excluded
    type: functional
    status: known
    statement: "`Cancelled` appointments are not counted in either number."
    cites: ["artifacts/HMS-5/run_01M60VHZHR79G15R3AKPB0VYDK/ticket.md#L19", "context/glossary.md#cancelled"]
  - id: REQ-6
    title: Zero counts shown when none exist
    type: functional
    status: known
    statement: "A patient with no Completed or No-show appointments shows `0` for each count."
    cites: ["artifacts/HMS-5/run_01M60VHZHR79G15R3AKPB0VYDK/ticket.md#L20"]
  - id: REQ-7
    title: Appointments list unchanged
    type: constraint
    status: known
    statement: "The `appointments list` on the patient page is unchanged."
    cites: ["artifacts/HMS-5/run_01M60VHZHR79G15R3AKPB0VYDK/ticket.md#L21"]
  - id: REQ-8
    title: Medical history and rest of page unchanged
    type: constraint
    status: known
    statement: "The `medical history` and the rest of the patient page are unchanged."
    cites: ["artifacts/HMS-5/run_01M60VHZHR79G15R3AKPB0VYDK/ticket.md#L21", "context/intent-baseline.md#data-protection"]
  - id: REQ-9
    title: Counts cover all of the patient's appointments
    type: functional
    status: assumed
    statement: "The counts include all of the patient's Completed and No-show appointments, with no date window, so staff see `how often` a patient attended or missed."
    basis: "The ticket states no time window and its goal is to see how often a patient has attended or missed appointments overall."
    cites: ["artifacts/HMS-5/run_01M60VHZHR79G15R3AKPB0VYDK/ticket.md#L12"]
unknowns:
  - id: U-1
    text: "The term `Visit summary` is not in the glossary."
    question: Q-01
  - id: U-2
    text: "The term `patient page` is not in the glossary, and the ticket's reference `PAT-3` (the PRD section describing it) is not among the inputs."
    question: Q-02
questions:
  - id: Q-01
    blocking: false
    ask: product owner
    text: "Should `Visit summary` be added to the glossary as the count of one patient's Completed and No-show appointments?"
    why: Keeps the term consistent between ticket, design and tests.
    evidence: ["artifacts/HMS-5/run_01M60VHZHR79G15R3AKPB0VYDK/ticket.md#L16", "context/glossary.md#glossary"]
    options: ["Add the definition as proposed", "Use a different label or definition"]
    default: "Define Visit summary as the counts of one patient's Completed and No-show appointments."
  - id: Q-02
    blocking: false
    ask: tech lead
    text: "Is the `patient page` of the ticket the patient detail page described in PRD `PAT-3` (the page that also holds the medical history)?"
    why: Design must target the right page; the PRD section is not in the inputs.
    evidence: ["artifacts/HMS-5/run_01M60VHZHR79G15R3AKPB0VYDK/ticket.md#L14", "context/intent-baseline.md#data-protection"]
    options: ["Yes, the patient detail page", "Another page"]
    default: "The patient's own detail page, where medical history is shown."
  - id: Q-03
    blocking: false
    ask: product owner
    text: "Should the counts cover the patient's whole history, or only a recent period (REQ-9)?"
    why: A date window changes the counts staff use to decide on follow-up.
    evidence: ["artifacts/HMS-5/run_01M60VHZHR79G15R3AKPB0VYDK/ticket.md#L12"]
    options: ["All time", "A fixed recent period"]
    default: "All time, as the ticket names no period."
glossary:
  - {term: Visit summary, found: false}
  - {term: patient page, found: false}
  - {term: Completed, found: true}
  - {term: No-show, found: true}
  - {term: Scheduled, found: true}
  - {term: Cancelled, found: true}
  - {term: Front-desk staff, found: true}
  - {term: Medical history, found: true}
---
# Requirements review: HMS-5

## Summary
HMS-5 asks for a "Visit summary" on the patient page, above the appointments list, showing how many
of the patient's appointments are Completed and how many are No-show, so front-desk staff can follow
up with patients who often miss appointments. Scheduled and Cancelled appointments are excluded and
zero counts are shown as 0. The rest of the page, including the appointments list and the medical
history, must stay unchanged. Main risks are small: the term "Visit summary" and "patient page" are
not in the glossary, and the ticket names no time window for the counts (assumed: all time).

## Requirements
- REQ-1 Visit summary placed above the appointments list — known — the patient page shows a `Visit summary` above the appointments list.
- REQ-2 Count of Completed appointments — known — shows the number of the patient's `Completed` appointments.
- REQ-3 Count of No-show appointments — known — shows the number of the patient's `No-show` appointments.
- REQ-4 Scheduled appointments excluded — known — `Scheduled` appointments are not counted.
- REQ-5 Cancelled appointments excluded — known — `Cancelled` appointments are not counted.
- REQ-6 Zero counts shown when none exist — known — shows `0` for each when there are none.
- REQ-7 Appointments list unchanged — known — the `appointments list` is unchanged.
- REQ-8 Medical history and rest of page unchanged — known — the `medical history` and the rest of the page are unchanged.
- REQ-9 Counts cover all of the patient's appointments — assumed — no date window applies.

## Known
- REQ-1: ticket acceptance criterion 1 (L16).
- REQ-2, REQ-3: ticket acceptance criterion 2 (L17–L18); statuses defined in the glossary.
- REQ-4, REQ-5: ticket acceptance criterion 3 (L19).
- REQ-6: ticket acceptance criterion 4 (L20).
- REQ-7, REQ-8: ticket acceptance criterion 5 (L21–L22); medical history stays on the patient's own page per the baseline.

## Assumed
- REQ-9: counts cover the patient's whole history. Basis: the ticket names no period and asks how often a patient has attended or missed (L12). Confirm via Q-03.

## Unknown
- U-1: "Visit summary" is not in the glossary → Q-01.
- U-2: "patient page" is not in the glossary and PRD PAT-3 is not among the inputs → Q-02.

## Open questions
- Q-01 (non-blocking, product owner): add "Visit summary" to the glossary? Default: counts of one patient's Completed and No-show appointments.
- Q-02 (non-blocking, tech lead): is the patient page the patient detail page of PRD PAT-3? Default: yes, the page that holds the medical history.
- Q-03 (non-blocking, product owner): all-time counts or a recent period? Default: all time.

## Out of scope
- The follow-up action itself (contacting patients who often miss appointments) is the user's goal, not a feature asked for in the acceptance criteria.
- Showing the counts anywhere other than the patient page (lists, dashboard) is not asked for.
