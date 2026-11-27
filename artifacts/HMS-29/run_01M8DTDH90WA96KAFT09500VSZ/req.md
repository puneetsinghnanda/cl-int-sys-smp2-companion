---
schema: intent.v1
run_id: run_01M8DTDH90WA96KAFT09500VSZ
step: req
attempt: 1
skill: requirement-review@1.5.0
inputs:
  - path: artifacts/HMS-29/run_01M8DTDH90WA96KAFT09500VSZ/ticket.md
    sha256: c1f22e81f5af870957497135b1f968829146643a6dc710ff3c638e28e774daa5
  - path: context/intent-baseline.md
    sha256: 9bfd821aeb2fb9524863e346707e049e4fcfb949a6e55b90223a79194946a215
  - path: context/glossary.md
    sha256: 430a90e14c50f969f7f757089c61cf254eb21596ed6e60abb8bf59286822b9ff
pack_sha:
  sample-system: 7681f87749883d71abb4011d118366ca1242e1cc
claims:
  - id: C1
    text: "The Settings page lists departments and specializations with their name, `the number of doctors` who use them, their status and the actions."
    cites: ["artifacts/HMS-29/run_01M8DTDH90WA96KAFT09500VSZ/ticket.md#L17-L18"]
  - id: C2
    text: "An entry used by any doctor `cannot be` deleted (REF-2)."
    cites: ["artifacts/HMS-29/run_01M8DTDH90WA96KAFT09500VSZ/ticket.md#L18-L19"]
  - id: C3
    text: "Today the number of doctors on the Settings page is `plain text`."
    cites: ["artifacts/HMS-29/run_01M8DTDH90WA96KAFT09500VSZ/ticket.md#L19"]
  - id: C4
    text: "To see which doctors use an entry, staff set the doctor list filter by hand (`DOC-2`)."
    cites: ["artifacts/HMS-29/run_01M8DTDH90WA96KAFT09500VSZ/ticket.md#L19-L20"]
  - id: C5
    text: "The glossary defines `Front-desk staff` as the hospital staff who use the system."
    cites: ["context/glossary.md#front-desk-staff"]
  - id: C6
    text: "The glossary defines `Inactive` for a department or specialization kept for history."
    cites: ["context/glossary.md#inactive"]
  - id: C7
    text: "Dashboard tiles already follow the pattern of a count `linking to the list it` counts."
    cites: ["context/glossary.md#dashboard"]
  - id: C8
    text: "The baseline goal is that `Staff manage doctors` end to end."
    cites: ["context/intent-baseline.md#goals"]
trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8]
known: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8]
assumed: []
unknown: []
requirements:
  - id: REQ-1
    title: Department count links to the filtered doctor list
    type: functional
    status: known
    statement: "On the Settings page, the number in the `Doctors` column of a department is a link to the doctor list filtered to that department."
    cites:
      - artifacts/HMS-29/run_01M8DTDH90WA96KAFT09500VSZ/ticket.md#L23-L24
      - runs/run_01M8DTDH90WA96KAFT09500VSZ/ide-answers.md#a-02
  - id: REQ-2
    title: Specialization count links to the filtered doctor list
    type: functional
    status: known
    statement: "On the Settings page, the number in the `Doctors` column of a specialization is a link to the doctor list filtered to that specialization."
    cites:
      - artifacts/HMS-29/run_01M8DTDH90WA96KAFT09500VSZ/ticket.md#L23-L25
      - runs/run_01M8DTDH90WA96KAFT09500VSZ/ide-answers.md#a-02
  - id: REQ-3
    title: Linked list includes doctors of any status
    type: functional
    status: known
    statement: "The linked doctor list leaves the status filter at `Any status`, so it shows the same number of doctors as the Settings page count."
    cites:
      - artifacts/HMS-29/run_01M8DTDH90WA96KAFT09500VSZ/ticket.md#L26-L27
  - id: REQ-4
    title: Zero count is plain text
    type: functional
    status: known
    statement: "A count of `0` is shown as plain text, not a link."
    cites:
      - artifacts/HMS-29/run_01M8DTDH90WA96KAFT09500VSZ/ticket.md#L28
  - id: REQ-5
    title: Link works for Active and Inactive entries
    type: functional
    status: known
    statement: "The link works the same for `Active and Inactive` entries."
    cites:
      - artifacts/HMS-29/run_01M8DTDH90WA96KAFT09500VSZ/ticket.md#L29
  - id: REQ-6
    title: Accessible name of the link
    type: functional
    status: known
    statement: "The link's accessible name says what it opens, in the pattern `3 doctors in Cardiology`: \"<n> doctors in <name>\" for both departments and specializations, and \"1 doctor in <name>\" when the count is 1."
    cites:
      - artifacts/HMS-29/run_01M8DTDH90WA96KAFT09500VSZ/ticket.md#L30
      - runs/run_01M8DTDH90WA96KAFT09500VSZ/ide-answers.md#a-01
  - id: REQ-7
    title: Rest of the Settings page unchanged
    type: constraint
    status: known
    statement: "The rest of the `Settings page` is unchanged."
    cites:
      - artifacts/HMS-29/run_01M8DTDH90WA96KAFT09500VSZ/ticket.md#L31
  - id: REQ-8
    title: Doctor list unchanged
    type: constraint
    status: known
    statement: "The `doctor list` is unchanged."
    cites:
      - artifacts/HMS-29/run_01M8DTDH90WA96KAFT09500VSZ/ticket.md#L31
unknowns: []
questions: []
answers:
  - id: A-01
    kind: question
    about: Q-01
    answer: "Same pattern (proposed default)"
    requirements: [REQ-6]
  - id: A-02
    kind: question
    about: Q-02
    answer: "Use code meaning (proposed default)"
    requirements: [REQ-1, REQ-2]
glossary:
  - {term: Front-desk staff, found: true}
  - {term: Inactive, found: true}
  - {term: Department, found: false}
  - {term: Specialization, found: false}
  - {term: Settings page, found: false}
  - {term: Doctor list, found: false}
---
# Requirements review: HMS-29

## Summary
HMS-29 asks that the doctor count for each department and specialization on the Settings page
(C1) becomes a link to the doctor list, filtered to that entry. Today the count is plain text
(C3), and staff have to set the doctor list filter by hand (C4). The link follows the same
pattern as the dashboard tiles, where each count links to the list it counts (C7). The main
risk is a mismatch between the Settings count and the linked list, which REQ-3 handles by
keeping the status filter at "Any status". A-01 settled the accessible name.

## Requirements
- REQ-1 Department count links to the filtered doctor list (known): the Doctors count of a department links to the doctor list filtered to that department.
- REQ-2 Specialization count links to the filtered doctor list (known): the Doctors count of a specialization links to the doctor list filtered to that specialization.
- REQ-3 Linked list includes doctors of any status (known): the status filter stays at "Any status", so the list shows as many doctors as the count.
- REQ-4 Zero count is plain text (known): a count of 0 is not a link.
- REQ-5 Link works for Active and Inactive entries (known): Inactive entries (C6) link the same way.
- REQ-6 Accessible name of the link (known): "<n> doctors in <name>" for both kinds; "1 doctor in <name>" for a count of 1.
- REQ-7 Rest of the Settings page unchanged (known).
- REQ-8 Doctor list unchanged (known).

## Known
- REQ-1, REQ-2: ticket acceptance criterion 1 (L23-L25). A-02 confirms that the terms mean the Settings page tables and the /doctors list in the code.
- REQ-3: ticket acceptance criterion 2 (L26-L27).
- REQ-4: ticket acceptance criterion 3 (L28).
- REQ-5: ticket acceptance criterion 4 (L29).
- REQ-6: ticket acceptance criterion 5 (L30), with the wording decided in A-01.
- REQ-7, REQ-8: ticket acceptance criterion 6 (L31), split into two testable constraints.

## Assumed
None.

## Unknown
None.

## Open questions
None open.

Answered in the IDE:
- A-01: Q-01 asked what the accessible name of a specialization link and of a count of 1 should be. eng-1df5 answered "Same pattern (proposed default)". It decides REQ-6. No mismatch: the ticket gives "3 doctors in Cardiology" only as an example.
- A-02: Q-02 asked whether "Department", "Specialization", "Settings page" and "Doctor list", which are missing from the glossary, can take their meaning from the code. eng-1df5 answered "Use code meaning (proposed default)". It decides REQ-1 and REQ-2. The tech lead still needs to add the glossary entries (non-blocking).

## Out of scope
- Links from other places, such as the doctor detail page or the dashboard: the ticket asks only about the Settings page.
- Changing the rule that an entry in use cannot be deleted (REF-2, C2): the ticket only mentions it as background.
- How the link URL and filter parameters are built: that is for design, not requirements.
