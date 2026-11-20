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
