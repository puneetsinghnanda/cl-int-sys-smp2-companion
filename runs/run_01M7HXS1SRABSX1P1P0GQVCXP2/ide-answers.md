# IDE answers: run_01M7HXS1SRABSX1P1P0GQVCXP2

Written by the tsl post-tool hook from AskUserQuestion. Do not edit: artifacts cite these
entries as the user's own answers.

## A-01
- step: req
- attempt: 2
- asked_at: 2026-11-16T10:23:27Z
- answered_by: eng-dbe4
- tool_use_id: toolu_015PUezxTx9y3S3LiA2DCF8k
- question: Q-01: HMS-5 asks the patient page to count the patient's Completed and No-show appointments but gives no time window. In the earlier HMS-5 run you chose "All time"; the reviewer wants it confirmed for this run. Which window should the Visit summary use?
- options: All time (proposed default) | Selectable Period | Fixed Last 30 days
- answer: All time (proposed default)

## A-02
- step: impact
- attempt: 1
- asked_at: 2026-11-16T10:31:48Z
- answered_by: eng-dbe4
- tool_use_id: toolu_01W4LmcimHpaoTXnWabf1qzi
- question: Q-01: The Visit summary must count all-time Completed and No-show appointments, but the patient page's appointments list is capped by a default `limit = 100` (api/src/repositories/patientRepo.ts), so counting in the browser undercounts. In the earlier HMS-5 run you chose to add the counts to GET /api/patients/:id. Where should the patient page get the two counts in this run?
- options: Add to GET /api/patients/:id (proposed default) | New endpoint | Count in the browser
- answer: Add to GET /api/patients/:id (proposed default)

## A-03
- step: design
- attempt: 1
- asked_at: 2026-11-16T10:38:12Z
- answered_by: eng-dbe4
- tool_use_id: toolu_01XzoLHEFchRSrzht6qhaVxU
- question: Q-01: GET /api/patients/:id will return the patient's all-time Completed and No-show counts (A-02). The stories assert exact counts (e.g. Completed 2, No-show 1). PatientDetail fields are camelCase (`medicalHistory` is a nested object). In the earlier HMS-5 run you chose `visitSummary { completed, noShow }`. What should the field look like in this run?
- options: visitSummary { completed, noShow } (proposed default) | completedCount, noShowCount
- answer: visitSummary { completed, noShow } (proposed default)
