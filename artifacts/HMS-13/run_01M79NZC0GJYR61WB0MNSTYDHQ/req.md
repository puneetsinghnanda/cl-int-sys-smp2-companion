---
schema: intent.v1
run_id: run_01M79NZC0GJYR61WB0MNSTYDHQ
step: req
attempt: 1
skill: requirement-review@1.2.0
inputs:
  - path: artifacts/HMS-13/run_01M79NZC0GJYR61WB0MNSTYDHQ/ticket.md
    sha256: d8ba0257e6ce19a42d701334cb1bad5947f544f505e2f7ebb4c3410a1a0a0a9f
  - path: context/intent-baseline.md
    sha256: 9bfd821aeb2fb9524863e346707e049e4fcfb949a6e55b90223a79194946a215
  - path: context/glossary.md
    sha256: 430a90e14c50f969f7f757089c61cf254eb21596ed6e60abb8bf59286822b9ff
pack_sha:
  sample-system: 7681f87749883d71abb4011d118366ca1242e1cc
claims:
  - id: C1
    text: "The doctor page's DOC-7 rule is `withinAvailability`, applied to not-yet-started Scheduled appointments."
    cites: ["runs/run_01M79NZC0GJYR61WB0MNSTYDHQ/ide-answers.md#a-02"]
  - id: C2
    text: "The doctor page flags only Scheduled appointments that have not started yet (`starts_at >= now`)."
    cites: ["runs/run_01M79NZC0GJYR61WB0MNSTYDHQ/ide-answers.md#a-01"]
  - id: C3
    text: "The doctor page checks only its first 50 upcoming Scheduled appointments (`limit 50`)."
    cites: ["runs/run_01M79NZC0GJYR61WB0MNSTYDHQ/ide-answers.md#a-02"]
  - id: C4
    text: "An Inactive doctor is one kept for history but `not offered for new use`."
    cites: ["context/glossary.md#inactive"]
  - id: C5
    text: "Completed, Cancelled and No-show appointments `never change`."
    cites: ["context/intent-baseline.md#closed-appointments"]
  - id: C6
    text: "The hospital time zone is `Asia/Kolkata`."
    cites: ["context/intent-baseline.md#constraints"]
  - id: C7
    text: "An Inactive doctor's Scheduled appointments stay and need rescheduling or cancelling (`DOC-6`)."
    cites: ["artifacts/HMS-13/run_01M79NZC0GJYR61WB0MNSTYDHQ/ticket.md#L20-L22"]
trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9]
known: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9]
assumed: []
unknown: []
requirements:
  - id: REQ-1
    title: Outside-availability notice
    type: functional
    status: known
    statement: "A Scheduled appointment that falls outside its doctor's current weekly availability shows the notice `This appointment is outside the doctor's current availability. Reschedule or cancel it.` above the actions."
    cites: ["artifacts/HMS-13/run_01M79NZC0GJYR61WB0MNSTYDHQ/ticket.md#L25-L27"]
  - id: REQ-2
    title: Same rule as the doctor page's DOC-7 flags
    type: functional
    status: known
    statement: "Outside availability is decided by the same rule the doctor page uses for its DOC-7 flags (`withinAvailability`)."
    cites: ["artifacts/HMS-13/run_01M79NZC0GJYR61WB0MNSTYDHQ/ticket.md#L28-L30", "runs/run_01M79NZC0GJYR61WB0MNSTYDHQ/ide-answers.md#a-02"]
  - id: REQ-3
    title: Only not-yet-started appointments get the availability notice
    type: functional
    status: known
    statement: "The outside-availability notice applies only to Scheduled appointments whose start time has not passed, as on the doctor page (`starts_at >= now`); a Scheduled appointment whose start time has passed shows no outside-availability notice."
    cites: ["runs/run_01M79NZC0GJYR61WB0MNSTYDHQ/ide-answers.md#a-01"]
  - id: REQ-4
    title: No 50-appointment cap on the check
    type: functional
    status: known
    statement: "The appointment page applies the rule to its own appointment whatever its position in the doctor's upcoming list; the doctor page's `50-appointment limit` is a list size and is not copied."
    cites: ["runs/run_01M79NZC0GJYR61WB0MNSTYDHQ/ide-answers.md#a-02"]
  - id: REQ-5
    title: Inactive-doctor notice
    type: functional
    status: known
    statement: "A Scheduled appointment whose doctor is Inactive shows the notice `The doctor is Inactive.` Cancel this appointment or book the patient with another doctor. above the actions."
    cites: ["artifacts/HMS-13/run_01M79NZC0GJYR61WB0MNSTYDHQ/ticket.md#L31-L32"]
  - id: REQ-6
    title: Order of the two notices
    type: functional
    status: known
    statement: "When both notices apply, the outside-availability notice comes first and the Inactive notice second, both above the actions (`Yes, confirm`)."
    cites: ["artifacts/HMS-13/run_01M79NZC0GJYR61WB0MNSTYDHQ/ticket.md#L33", "runs/run_01M79NZC0GJYR61WB0MNSTYDHQ/ide-answers.md#a-04"]
  - id: REQ-7
    title: Both notices shown when both apply
    type: functional
    status: known
    statement: "When both conditions apply, `both notices are shown`; the outside-availability notice still shows when the doctor is Inactive."
    cites: ["artifacts/HMS-13/run_01M79NZC0GJYR61WB0MNSTYDHQ/ticket.md#L33", "runs/run_01M79NZC0GJYR61WB0MNSTYDHQ/ide-answers.md#a-03"]
  - id: REQ-8
    title: Closed appointments show no notice
    type: functional
    status: known
    statement: "`Completed, Cancelled and No-show appointments show neither notice`."
    cites: ["artifacts/HMS-13/run_01M79NZC0GJYR61WB0MNSTYDHQ/ticket.md#L34"]
  - id: REQ-9
    title: Rest of the page unchanged
    type: constraint
    status: known
    statement: "`The rest of the appointment page is unchanged`: when, doctor, patient, reason for visit, actions and history stay as they are."
    cites: ["artifacts/HMS-13/run_01M79NZC0GJYR61WB0MNSTYDHQ/ticket.md#L35"]
unknowns: []
questions: []
answers:
  - id: A-01
    kind: question
    about: Q-01
    answer: "No, follow the doctor page (proposed default)"
    requirements: [REQ-3]
  - id: A-02
    kind: question
    about: Q-02
    answer: "Use the same rule (withinAvailability on not-yet-started Scheduled appointments), but don't copy the doctor page's 50-appointment limit; that's a list size, not the rule"
    requirements: [REQ-2, REQ-4]
    mismatch:
      text: "The ticket says an appointment not flagged on the doctor page is not flagged here; the user decided the doctor page's 50-appointment limit is not copied, so an appointment beyond the doctor page's first 50 upcoming ones is flagged here although the doctor page does not flag it."
      cites: ["artifacts/HMS-13/run_01M79NZC0GJYR61WB0MNSTYDHQ/ticket.md#L28-L30"]
  - id: A-03
    kind: question
    about: Q-03
    answer: "Yes, both notices (proposed default)"
    requirements: [REQ-7]
  - id: A-04
    kind: assumption
    about: REQ-6
    answer: "Yes, confirm"
    requirements: [REQ-6]
glossary:
  - {term: Scheduled, found: true}
  - {term: Inactive, found: true}
  - {term: Completed, found: true}
  - {term: Cancelled, found: true}
  - {term: No-show, found: true}
  - {term: Availability block, found: true}
  - {term: Front-desk staff, found: true}
---
# Requirements review: HMS-13

## Summary
HMS-13 asks the appointment page to warn front-desk staff when a Scheduled appointment no longer fits its doctor. There are two cases: the appointment is outside the doctor's current weekly availability (using the same rule as the doctor page's DOC-7 flags), or the doctor is Inactive. Each case has a fixed notice text shown above the actions. Both notices can show together, and closed appointments show neither. The main risk is parity with the doctor page: that page checks only Scheduled appointments that haven't started yet, up to 50 per doctor (C2, C3). The user kept the not-yet-started condition and dropped the 50 cap.

## Requirements
- REQ-1 Outside-availability notice (known): a Scheduled appointment outside its doctor's current weekly availability shows "This appointment is outside the doctor's current availability. Reschedule or cancel it." above the actions.
- REQ-2 Same rule as the doctor page's DOC-7 flags (known): outside availability is decided by `withinAvailability`, the doctor page's rule.
- REQ-3 Only not-yet-started appointments get the availability notice (known): a Scheduled appointment whose start time has passed shows no outside-availability notice.
- REQ-4 No 50-appointment cap on the check (known): the rule is applied to the appointment itself, whatever its position in the doctor's upcoming list.
- REQ-5 Inactive-doctor notice (known): a Scheduled appointment whose doctor is Inactive shows "The doctor is Inactive. Cancel this appointment or book the patient with another doctor." above the actions.
- REQ-6 Order of the two notices (known): the outside-availability notice comes first, then the Inactive notice, both above the actions.
- REQ-7 Both notices shown when both apply (known): the outside-availability notice still shows when the doctor is Inactive.
- REQ-8 Closed appointments show no notice (known): Completed, Cancelled and No-show appointments show neither notice.
- REQ-9 Rest of the page unchanged (known, constraint): when, doctor, patient, reason for visit, actions and history stay as they are.

## Known
- REQ-1: ticket acceptance criterion 1 (L25-L27).
- REQ-2: ticket acceptance criterion 2 (L28-L30); the rule is `withinAvailability` (C1); confirmed in A-02.
- REQ-3: the doctor page checks only not-yet-started appointments (C2); the user chose to follow it in A-01.
- REQ-4: the doctor page caps its list at 50 (C3); the user chose not to copy the cap in A-02.
- REQ-5: ticket acceptance criterion 3 (L31-L32).
- REQ-6: ticket acceptance criterion 4 (L33); order confirmed in A-04.
- REQ-7: ticket acceptance criterion 4 (L33); confirmed for Inactive doctors in A-03.
- REQ-8: ticket acceptance criterion 5 (L34); consistent with closed appointments never changing (C5).
- REQ-9: ticket acceptance criterion 6 (L35).

## Assumed
None. The user confirmed the only assumption (REQ-6) in A-04.

## Unknown
None. Every question was answered in the IDE.

## Open questions
None open.

Answered in the IDE:
- A-01 (Q-01, answered by eng-1df5): should a Scheduled appointment whose start time has passed show the outside-availability notice? Answer: "No, follow the doctor page (proposed default)". Decides REQ-3. No mismatch.
- A-02 (Q-02, answered by eng-1df5): should the rule apply regardless of the doctor page's 50-appointment limit? Answer: "Use the same rule (withinAvailability on not-yet-started Scheduled appointments), but don't copy the doctor page's 50-appointment limit; that's a list size, not the rule". Decides REQ-2 and REQ-4. Mismatch: ticket L28-L30 says an appointment not flagged on the doctor page is not flagged here. Under this answer, an appointment beyond the doctor page's first 50 upcoming ones is flagged here even though the doctor page does not flag it.
- A-03 (Q-03, answered by eng-1df5): should the outside-availability notice also show for an Inactive doctor's appointment? Answer: "Yes, both notices (proposed default)". Decides REQ-7. No mismatch.
- A-04 (REQ-6, answered by eng-1df5): the outside-availability notice first, then the Inactive notice? Answer: "Yes, confirm". Decides REQ-6. No mismatch.

## Out of scope
- How the appointment page gets the doctor's status and the availability result is left to design.
- The doctor page's own DOC-7 flags and its notice stay as they are. The ticket asks only for a change to the appointment page.
- The reschedule and cancel flows are unchanged. The notices only point staff to the existing actions.
- No instruction-like text, credential or personal data was found in the inputs.
