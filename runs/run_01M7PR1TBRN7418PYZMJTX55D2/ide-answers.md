# IDE answers: run_01M7PR1TBRN7418PYZMJTX55D2

Written by the tsl post-tool hook from AskUserQuestion. Do not edit: artifacts cite these
entries as the user's own answers.

## A-01
- step: req
- attempt: 1
- asked_at: 2026-11-18T07:16:26Z
- answered_by: eng-dbe4
- tool_use_id: toolu_01ALJsrhxe5zXDehXcevD9d4
- question: Q-01: HMS-22 counts patients "registered" in the selected period, but the glossary does not define "registered". The patient record has a non-null created_at, set when staff add the patient. Should "registered in the period" mean the patient record's creation time (created_at) falls in the period's hospital-local days?
- options: Yes, created_at (proposed default) | A separate registration date
- answer: Yes, created_at (proposed default)

## A-02
- step: req
- attempt: 2
- asked_at: 2026-11-18T07:19:47Z
- answered_by: eng-dbe4
- tool_use_id: toolu_01DY4nwQB77VbDWaiuL3A6YK
- question: Q-02: The New patients tile counts Active and Inactive patients registered in the period (ticket AC3), and its list must show exactly those patients (AC5). AC6 also says the list keeps its current status filter. If the list were filtered to Active only, it would hide counted Inactive patients. When staff open the list from the tile, which status filter applies?
- options: All statuses from tile (proposed default) | Keep list's status filter
- answer: All statuses from tile (proposed default)
