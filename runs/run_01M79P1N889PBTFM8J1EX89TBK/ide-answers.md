# IDE answers: run_01M79P1N889PBTFM8J1EX89TBK

Written by the tsl post-tool hook from AskUserQuestion. Do not edit: artifacts cite these
entries as the user's own answers.

## A-01
- step: req
- attempt: 1
- asked_at: 2026-11-13T05:30:20Z
- answered_by: eng-dbe4
- tool_use_id: toolu_01T2AGDYe795p7ufgYUJpdYt
- question: Q-01: HMS-5 asks the patient page to count the patient's Completed and No-show appointments (ticket L17-L18), but gives no time window. The glossary's "Period" (Today / Last 7 days / Last 30 days) is used by the dashboard and the doctor page's No-shows count. Which window should the Visit summary use?
- options: All time (proposed default) | Selectable Period | Fixed Last 30 days
- answer: All time (proposed default)

## A-02
- step: impact
- attempt: 1
- asked_at: 2026-11-13T05:46:14Z
- answered_by: eng-dbe4
- tool_use_id: toolu_01H5Rs6jNFzAW9zA9CRHv9aF
- question: Q-01: The Visit summary must count all-time Completed and No-show appointments, but the patient page's appointments come from `repo.appointments(id, now, limit = 100)` (api/src/repositories/patientRepo.ts L232), so counting them in the browser undercounts past 100. Where should the patient page get the two counts?
- options: Add to GET /api/patients/:id (proposed default) | New endpoint | Count in the browser
- answer: Add to GET /api/patients/:id (proposed default)

## A-03
- step: design
- attempt: 1
- asked_at: 2026-11-13T05:54:47Z
- answered_by: eng-dbe4
- tool_use_id: toolu_01Rh3MJ4CcjSQMGBgVurnaHU
- question: Q-01: GET /api/patients/:id will return the patient's all-time Completed and No-show counts (A-02). The stories assert exact counts (e.g. Completed 2, No-show 1). PatientDetail fields are camelCase (`medicalHistory`, `emergencyContact` are nested objects). What should the field look like?
- options: visitSummary { completed, noShow } (proposed default) | Flat completedCount + noShowCount | visit_summary { completed, no_show }
- answer: visitSummary { completed, noShow } (proposed default)
