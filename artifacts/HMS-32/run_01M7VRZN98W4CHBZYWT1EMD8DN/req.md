---
schema: intent.v1
run_id: run_01M7VRZN98W4CHBZYWT1EMD8DN
step: req
attempt: 1
skill: requirement-review@1.4.0
inputs:
  - path: artifacts/HMS-32/run_01M7VRZN98W4CHBZYWT1EMD8DN/ticket.md
    sha256: 671bea3d44a1be395bb4d091e17738aa36ef8f4b829e9bfbe5f222312b499cdc
  - path: context/intent-baseline.md
    sha256: 9bfd821aeb2fb9524863e346707e049e4fcfb949a6e55b90223a79194946a215
  - path: context/glossary.md
    sha256: 430a90e14c50f969f7f757089c61cf254eb21596ed6e60abb8bf59286822b9ff
pack_sha:
  sample-system: 7681f87749883d71abb4011d118366ca1242e1cc
claims:
  - id: C1
    text: "Each doctor's fee is stored and sent as `integer paise`."
    cites: ["artifacts/HMS-32/run_01M7VRZN98W4CHBZYWT1EMD8DN/ticket.md#L14"]
  - id: C2
    text: "The hospital currency is `INR`."
    cites: ["context/intent-baseline.md#constraints"]
  - id: C3
    text: "`Paise` are one hundredth of a rupee, and fees are stored and sent in paise."
    cites: ["context/glossary.md#paise"]
  - id: C4
    text: "`Medical history` appears only on the patient's own page."
    cites: ["context/intent-baseline.md#data-protection"]
  - id: C5
    text: "A slot is free only inside the doctor's `availability`, in the future and not overlapping a Scheduled appointment."
    cites: ["context/intent-baseline.md#booking-rules"]
trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10, REQ-11, REQ-12, REQ-13, REQ-14]
known: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10, REQ-11, REQ-12, REQ-13, REQ-14]
assumed: []
unknown: []
requirements:
  - id: REQ-1
    title: Show the consultation fee once a doctor is chosen
    type: functional
    status: known
    statement: "On the book appointment page, once a doctor is chosen, the page shows the label \"Consultation fee\" with that doctor's fee."
    cites: ["artifacts/HMS-32/run_01M7VRZN98W4CHBZYWT1EMD8DN/ticket.md#L16-L17"]
  - id: REQ-2
    title: Fee format
    type: functional
    status: known
    statement: "The fee is shown in rupees with the ₹ sign, Indian digit grouping and two decimals; 150000 paise shows as ₹1,500.00."
    cites: ["artifacts/HMS-32/run_01M7VRZN98W4CHBZYWT1EMD8DN/ticket.md#L18-L19", "context/glossary.md#paise"]
  - id: REQ-3
    title: Fee follows the chosen doctor
    type: functional
    status: known
    statement: "Choosing a different doctor updates the fee straight away, without reloading the page."
    cites: ["artifacts/HMS-32/run_01M7VRZN98W4CHBZYWT1EMD8DN/ticket.md#L20"]
  - id: REQ-4
    title: No fee before a doctor is chosen
    type: functional
    status: known
    statement: "Before a doctor is chosen, no fee is shown."
    cites: ["artifacts/HMS-32/run_01M7VRZN98W4CHBZYWT1EMD8DN/ticket.md#L21"]
  - id: REQ-5
    title: Zero fee
    type: functional
    status: known
    statement: "A fee of 0 paise shows as ₹0.00."
    cites: ["artifacts/HMS-32/run_01M7VRZN98W4CHBZYWT1EMD8DN/ticket.md#L22"]
  - id: REQ-6
    title: Fee is read-only on this page
    type: constraint
    status: known
    statement: "The fee is shown only; staff cannot change it on the book appointment page."
    cites: ["artifacts/HMS-32/run_01M7VRZN98W4CHBZYWT1EMD8DN/ticket.md#L23"]
  - id: REQ-7
    title: Booking request unchanged
    type: constraint
    status: known
    statement: "The booking request sent when staff confirm is unchanged; it carries no fee."
    cites: ["artifacts/HMS-32/run_01M7VRZN98W4CHBZYWT1EMD8DN/ticket.md#L23-L24"]
  - id: REQ-8
    title: Slot list unchanged
    type: constraint
    status: known
    statement: "The slot list on the book appointment page is unchanged."
    cites: ["artifacts/HMS-32/run_01M7VRZN98W4CHBZYWT1EMD8DN/ticket.md#L25"]
  - id: REQ-9
    title: Fee that cannot be loaded
    type: functional
    status: known
    statement: "If the doctor's fee cannot be loaded, the page shows no fee and staff can still book."
    cites: ["artifacts/HMS-32/run_01M7VRZN98W4CHBZYWT1EMD8DN/ticket.md#L23", "runs/run_01M7VRZN98W4CHBZYWT1EMD8DN/ide-answers.md#a-02"]
  - id: REQ-10
    title: Fee for a doctor taken from the URL
    type: functional
    status: known
    statement: "When the page opens with a doctor already filled in from the URL, that doctor counts as chosen and its fee is shown as for a doctor picked in the Doctor field."
    cites: ["artifacts/HMS-32/run_01M7VRZN98W4CHBZYWT1EMD8DN/ticket.md#L16-L17", "runs/run_01M7VRZN98W4CHBZYWT1EMD8DN/ide-answers.md#a-01"]
  - id: REQ-11
    title: Booking rules unchanged
    type: constraint
    status: known
    statement: "The booking rules are unchanged."
    cites: ["artifacts/HMS-32/run_01M7VRZN98W4CHBZYWT1EMD8DN/ticket.md#L25", "context/intent-baseline.md#booking-rules"]
  - id: REQ-12
    title: Rest of the page unchanged
    type: constraint
    status: known
    statement: "Apart from the fee, the rest of the book appointment page is unchanged."
    cites: ["artifacts/HMS-32/run_01M7VRZN98W4CHBZYWT1EMD8DN/ticket.md#L25"]
  - id: REQ-13
    title: No medical history added
    type: constraint
    status: known
    statement: "The change adds no medical history to the book appointment page."
    cites: ["artifacts/HMS-32/run_01M7VRZN98W4CHBZYWT1EMD8DN/ticket.md#L26", "context/intent-baseline.md#data-protection"]
  - id: REQ-14
    title: No contact details added
    type: constraint
    status: known
    statement: "The change adds no contact details to the book appointment page."
    cites: ["artifacts/HMS-32/run_01M7VRZN98W4CHBZYWT1EMD8DN/ticket.md#L26"]
unknowns: []
questions: []
answers:
  - id: A-01
    kind: question
    about: Q-01
    answer: "Yes, show it (proposed default)"
    requirements: [REQ-10]
  - id: A-02
    kind: assumption
    about: REQ-9
    answer: "Yes, confirm"
    requirements: [REQ-9]
glossary:
  - {term: Front-desk staff, found: true}
  - {term: Paise, found: true}
  - {term: Slot, found: true}
  - {term: Medical history, found: true}
---
# Requirements review: HMS-32

## Summary
Front-desk staff want to see the chosen doctor's consultation fee on the book appointment page, so they can tell the patient the cost before confirming (C1, C2). The change is display-only: fees are already stored in paise (C3), and nothing about them is edited or sent. The main risks are a doctor pre-filled from the URL, now settled by A-01, and a fee that fails to load, settled by A-02. The booking request, booking rules (C5) and slot list must not change.

## Requirements
- REQ-1 Show the consultation fee once a doctor is chosen (known): the page shows "Consultation fee" with the chosen doctor's fee.
- REQ-2 Fee format (known): ₹ sign, Indian digit grouping, two decimals; 150000 paise shows as ₹1,500.00.
- REQ-3 Fee follows the chosen doctor (known): choosing a different doctor updates the fee without a reload.
- REQ-4 No fee before a doctor is chosen (known).
- REQ-5 Zero fee (known): 0 paise shows as ₹0.00.
- REQ-6 Fee is read-only on this page (known).
- REQ-7 Booking request unchanged (known): it carries no fee.
- REQ-8 Slot list unchanged (known).
- REQ-9 Fee that cannot be loaded (known): no fee is shown and staff can still book.
- REQ-10 Fee for a doctor taken from the URL (known): a pre-filled doctor counts as chosen.
- REQ-11 Booking rules unchanged (known).
- REQ-12 Rest of the page unchanged (known).
- REQ-13 No medical history added (known).
- REQ-14 No contact details added (known).

## Known
- REQ-1 to REQ-8, REQ-11, REQ-12: ticket acceptance criteria 1 to 7.
- REQ-13, REQ-14: ticket acceptance criterion 8, in line with the baseline data-protection rule (C4).
- REQ-9: confirmed by the user in A-02.
- REQ-10: decided by the user in A-01.

## Assumed
None. The one assumption (REQ-9) was confirmed in A-02.

## Unknown
None.

## Open questions
None open.

Answered in the IDE:
- A-01 (Q-01): should the fee also show for a doctor pre-filled from the URL? Answer: "Yes, show it (proposed default)", by eng-dbe4. Decides REQ-10. No mismatch with the ticket.
- A-02 (REQ-9): if the fee cannot be loaded, show no fee and still allow booking? Answer: "Yes, confirm", by eng-dbe4. Decides REQ-9. No mismatch with the ticket.

## Out of scope
- Where on the page the fee sits. Default: directly below the Doctor field. Trade-off: it sits next to the choice it depends on, but not next to the Book button. Left to design.
- Changing or storing fees, and fees in other currencies: the ticket asks for display only (REQ-6).
- "Consultation fee" is not a glossary term; the Paise entry covers fees (C3), so no new term is needed.
