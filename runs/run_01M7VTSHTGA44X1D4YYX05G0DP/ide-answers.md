# IDE answers: run_01M7VTSHTGA44X1D4YYX05G0DP

Written by the tsl post-tool hook from AskUserQuestion. Do not edit: artifacts cite these
entries as the user's own answers.

## A-01
- step: req
- attempt: 1
- asked_at: 2026-11-20T06:40:00Z
- answered_by: eng-dffd
- tool_use_id: toolu_012oZgkzero69mqNPo13Vi4P
- question: REQ-9: I assumed the summary shows the next appointment's date and time and the last visit's date in the hospital time zone (Asia/Kolkata), because the baseline fixes the hospital time zone as Asia/Kolkata and appointments are stored in UTC. Is that right?
- options: Yes, confirm | Browser local time
- answer: Yes, the hospital time zone, but read it from Settings (useSettings().timeZone, default Asia/Kolkata, PRD NFR-2), as the doctor and booking pages do. Do not hard-code Asia/Kolkata.

## A-02
- step: design
- attempt: 1
- asked_at: 2026-11-20T06:49:57Z
- answered_by: eng-dffd
- tool_use_id: toolu_017Y4TFm43E3NVJxZfAv6eMF
- question: Q-01: The stories test next appointment, last visit and three counts for a patient. How should the API deliver them to the patient page?
- options: visits on patient detail (proposed default) | New visits endpoint | Compute in the web app
- answer: visits on patient detail (the proposed default), but compute next, last and the three counts with their own queries over all of the patient's appointments, not from the 100-row appointments list (patientRepo.appointments has limit = 100), so AC4 holds for any patient.

## A-03
- step: design
- attempt: 1
- asked_at: 2026-11-20T06:49:57Z
- answered_by: eng-dffd
- tool_use_id: toolu_017Y4TFm43E3NVJxZfAv6eMF
- question: Q-02: Last visit shows a date only, but web/src/lib/format.ts `dateTime` prints a time. How should the date be formatted?
- options: New `date` helper (proposed default) | Inline Intl format
- answer: New `date` helper (proposed default)
