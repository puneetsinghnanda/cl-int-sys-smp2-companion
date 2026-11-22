# IDE answers: run_01M820V4J0VSJHKNZZ5VK0BT2Y

Written by the tsl post-tool hook from AskUserQuestion. Do not edit: artifacts cite these
entries as the user's own answers.

## A-01
- step: req
- attempt: 1
- asked_at: 2026-11-22T16:23:23Z
- answered_by: eng-a037
- tool_use_id: toolu_01Mva5yeWkFxm6zVzoHtTNtG
- question: Q-01: HMS-31 says the appointment page shows the doctor's current specialization and department. In the data model, a department or specialization in use can be set Inactive. When that happens, what should the appointment page show?
- options: Name as is (proposed default) | Name plus '(Inactive)'
- answer: Name plus '(Inactive)'

## A-02
- step: design
- attempt: 1
- asked_at: 2026-11-22T16:34:53Z
- answered_by: eng-a037
- tool_use_id: toolu_01SKqZhD9jRX496RZF3z7Cus
- question: Q-01: HMS-31 adds the doctor's specialization, department and fee to `GET /api/appointments/:id`. Today its `doctor` object is `{ id, code, fullName }`, shared with the appointment lists. How should the detail response carry the new fields?
- options: Extend detail doctor (proposed default) | Separate doctorDetails object | Flat top-level fields
- answer: Extend detail doctor (proposed default)
