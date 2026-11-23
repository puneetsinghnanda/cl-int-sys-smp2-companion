---
schema: intent.v1
run_id: run_01M83R9SM0CVRW4RSHWJJBEGQM
step: req
attempt: 1
skill: requirement-review@1.4.0
inputs:
  - path: artifacts/HMS-33/run_01M83R9SM0CVRW4RSHWJJBEGQM/ticket.md
    sha256: bb69112a6fa3eaeea0781fd7250b0cae09b46b21b5e146a98fcf21cf26008252
  - path: context/intent-baseline.md
    sha256: 9bfd821aeb2fb9524863e346707e049e4fcfb949a6e55b90223a79194946a215
  - path: context/glossary.md
    sha256: 430a90e14c50f969f7f757089c61cf254eb21596ed6e60abb8bf59286822b9ff
pack_sha:
  sample-system: 7681f87749883d71abb4011d118366ca1242e1cc
claims:
  - id: C1
    text: "A cancelled appointment's reason is up to `300 characters`, entered when it is cancelled."
    cites: ["artifacts/HMS-33/run_01M83R9SM0CVRW4RSHWJJBEGQM/ticket.md#L14"]
  - id: C2
    text: "The reason is stored in the nullable text column `cancellation_reason` of the appointment table."
    cites: ["context/generated/db-schema.json#column:appointment.cancellation_reason"]
  - id: C3
    text: "The appointment list page is the `/appointments` UI route."
    cites: ["context/generated/api-inventory.json#ui:/appointments"]
  - id: C4
    text: "The list is served by the appointments API route file `api/src/routes/appointments.ts`."
    cites: ["context/generated/api-inventory.json#http:GET:/api/appointments"]
  - id: C5
    text: "`Medical history` never appears in lists, the dashboard or logs."
    cites: ["context/intent-baseline.md#data-protection"]
  - id: C6
    text: "A Cancelled appointment is one called off before it took place, `with a reason`."
    cites: ["context/glossary.md#cancelled"]
  - id: C7
    text: "Completed, Cancelled and No-show appointments `never change`, so the list shows a closed appointment's reason as stored."
    cites: ["context/intent-baseline.md#closed-appointments"]
trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10, REQ-11, REQ-12, REQ-13]
known: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10, REQ-11, REQ-12, REQ-13]
assumed: []
unknown: []
requirements:
  - id: REQ-1
    title: New column after Status
    type: functional
    status: known
    statement: "The appointment list has a column `Cancellation reason` placed directly after the Status column."
    cites: ["artifacts/HMS-33/run_01M83R9SM0CVRW4RSHWJJBEGQM/ticket.md#L17"]
  - id: REQ-2
    title: Reason shown for Cancelled rows
    type: functional
    status: known
    statement: "For a `Cancelled` appointment the column shows its cancellation reason."
    cites: ["artifacts/HMS-33/run_01M83R9SM0CVRW4RSHWJJBEGQM/ticket.md#L18"]
  - id: REQ-3
    title: Placeholder for other statuses
    type: functional
    status: known
    statement: "For Scheduled, Completed and `No-show` appointments the column shows \"-\"."
    cites: ["artifacts/HMS-33/run_01M83R9SM0CVRW4RSHWJJBEGQM/ticket.md#L19"]
  - id: REQ-4
    title: Placeholder for a Cancelled row with no stored reason
    type: functional
    status: known
    statement: "For a Cancelled appointment whose `cancellation_reason` is empty (NULL), the column shows \"-\"."
    cites: ["context/generated/db-schema.json#column:appointment.cancellation_reason", "runs/run_01M83R9SM0CVRW4RSHWJJBEGQM/ide-answers.md#a-01"]
  - id: REQ-5
    title: Long reasons are cut at 60 characters
    type: functional
    status: known
    statement: "A reason longer than `60 characters` shows its first 60 characters followed by \"…\"; a reason of 60 characters or fewer is shown whole."
    cites: ["artifacts/HMS-33/run_01M83R9SM0CVRW4RSHWJJBEGQM/ticket.md#L20"]
  - id: REQ-6
    title: Full reason stays on the appointment page
    type: functional
    status: known
    statement: "The full reason is still shown on the `appointment's own page`."
    cites: ["artifacts/HMS-33/run_01M83R9SM0CVRW4RSHWJJBEGQM/ticket.md#L21"]
  - id: REQ-7
    title: No tooltip in the list
    type: functional
    status: known
    statement: "The list shows no tooltip or hover text with the full reason; a cut reason is read in full only on the appointment's own page (`No tooltip`)."
    cites: ["artifacts/HMS-33/run_01M83R9SM0CVRW4RSHWJJBEGQM/ticket.md#L21", "runs/run_01M83R9SM0CVRW4RSHWJJBEGQM/ide-answers.md#a-02"]
  - id: REQ-8
    title: Column follows filter, date range and paging
    type: functional
    status: known
    statement: "The column follows the list's `status filter`, date range and paging: each shown row carries its own reason or \"-\"."
    cites: ["artifacts/HMS-33/run_01M83R9SM0CVRW4RSHWJJBEGQM/ticket.md#L22"]
  - id: REQ-9
    title: Cancelled filter shows a reason on every row
    type: functional
    status: known
    statement: "When the list is filtered by Cancelled, every row shows a reason (`filtering by Cancelled`)."
    cites: ["artifacts/HMS-33/run_01M83R9SM0CVRW4RSHWJJBEGQM/ticket.md#L22-L23"]
  - id: REQ-10
    title: Other appointment tables unchanged
    type: constraint
    status: known
    statement: "The dashboard, the `doctor page` and the patient page keep their appointment tables unchanged and get no new column."
    cites: ["artifacts/HMS-33/run_01M83R9SM0CVRW4RSHWJJBEGQM/ticket.md#L24-L25"]
  - id: REQ-11
    title: Existing appointment actions unchanged
    type: constraint
    status: known
    statement: "Opening, cancelling and `booking` appointments work as before."
    cites: ["artifacts/HMS-33/run_01M83R9SM0CVRW4RSHWJJBEGQM/ticket.md#L26"]
  - id: REQ-12
    title: No medical history in the list
    type: constraint
    status: known
    statement: "No `medical history` is added to the appointment list."
    cites: ["artifacts/HMS-33/run_01M83R9SM0CVRW4RSHWJJBEGQM/ticket.md#L27", "context/intent-baseline.md#data-protection"]
  - id: REQ-13
    title: No contact details in the list
    type: constraint
    status: known
    statement: "No `contact details` are added to the appointment list."
    cites: ["artifacts/HMS-33/run_01M83R9SM0CVRW4RSHWJJBEGQM/ticket.md#L27"]
unknowns: []
questions: []
answers:
  - id: A-01
    kind: question
    about: Q-01
    answer: "Show \"-\" (proposed default)"
    requirements: [REQ-4]
    mismatch:
      text: "The ticket says a cancelled appointment always has a reason, but the column is nullable; the user decided a Cancelled row with no stored reason shows \"-\"."
      cites: ["artifacts/HMS-33/run_01M83R9SM0CVRW4RSHWJJBEGQM/ticket.md#L14"]
  - id: A-02
    kind: question
    about: Q-02
    answer: "No tooltip (proposed default)"
    requirements: [REQ-7]
glossary:
  - {term: Cancelled, found: true}
  - {term: Scheduled, found: true}
  - {term: Completed, found: true}
  - {term: No-show, found: true}
  - {term: Front-desk staff, found: true}
  - {term: Dashboard, found: true}
  - {term: Medical history, found: true}
---
# Requirements review: HMS-33

## Summary
HMS-33 adds a `Cancellation reason` column after Status on the appointment list (`/appointments`), so front-desk staff can see why an appointment was cancelled without opening it. Reasons over 60 characters are cut with "…" and the full text stays on the appointment's own page. The dashboard, doctor page and patient page tables must not change. The main risk is the shared appointment table: if those pages use one table component, the new column has to be added only for the list (REQ-10). A smaller risk is that `cancellation_reason` is nullable even though the ticket says every cancellation has one, and the user decided a missing reason shows "-" (REQ-4).

## Requirements
- REQ-1 New column after Status (known): the appointment list has a `Cancellation reason` column directly after Status.
- REQ-2 Reason shown for Cancelled rows (known): a Cancelled appointment shows its reason.
- REQ-3 Placeholder for other statuses (known): Scheduled, Completed and No-show rows show "-".
- REQ-4 Placeholder for a Cancelled row with no stored reason (known): a NULL `cancellation_reason` shows "-".
- REQ-5 Long reasons are cut at 60 characters (known): over 60 characters shows the first 60 + "…".
- REQ-6 Full reason stays on the appointment page (known).
- REQ-7 No tooltip in the list (known).
- REQ-8 Column follows filter, date range and paging (known).
- REQ-9 Cancelled filter shows a reason on every row (known).
- REQ-10 Other appointment tables unchanged (known): dashboard, doctor page and patient page get no new column.
- REQ-11 Existing appointment actions unchanged (known): opening, cancelling and booking work as before.
- REQ-12 No medical history in the list (known).
- REQ-13 No contact details in the list (known).

## Known
- REQ-1, REQ-2, REQ-3, REQ-5, REQ-6, REQ-8, REQ-9, REQ-10, REQ-11, REQ-13: stated in the ticket's acceptance criteria (lines 17 to 27).
- REQ-12: stated in the ticket (line 27) and in the baseline's data-protection rule.
- REQ-4: decided by the user in A-01.
- REQ-7: decided by the user in A-02.

## Assumed
None

## Unknown
None

## Open questions
None open.

Answered in the IDE:
- A-01 (Q-01): what a Cancelled row with no stored reason shows. Answer: Show "-" (proposed default), answered by eng-dbe4. Decides REQ-4. Mismatch: the ticket (line 14) says a cancelled appointment always has a reason, but the column `cancellation_reason` is nullable. The user decided that a missing reason shows "-".
- A-02 (Q-02): whether a cut reason shows the full text on hover. Answer: No tooltip (proposed default), answered by eng-dbe4. Decides REQ-7. Matches the ticket.

## Out of scope
- Sorting or searching the list by cancellation reason: the ticket does not ask for it.
- Changing how a reason is entered or how long it can be (`300 characters`, ticket line 14): the ticket describes this as how things work today, not as a change.
- Whether the list API already returns the reason: this is a fact for the design step, which will check `api/src/routes/appointments.ts`.
