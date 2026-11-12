# IDE answers: run_01M77BM48R93EMY13TZXCDN7PZ

Written by the tsl post-tool hook from AskUserQuestion. Do not edit: artifacts cite these
entries as the user's own answers.

## A-01
- step: req
- attempt: 2
- asked_at: 2026-11-12T07:51:35Z
- answered_by: eng-3fd6
- tool_use_id: toolu_01KgbfRso2agzEUWqRywdLni
- question: REQ-17: The ticket says ties are ordered by doctor name (L30-L31) but not how names compare, or what happens when two doctors share a name. I assumed names compare ignoring case, with doctor id as the final tie-break, the same way the doctor list sorts. Is that right?
- options: Yes, confirm | Case-sensitive, then id
- answer: Yes, confirm

## A-02
- step: impact
- attempt: 1
- asked_at: 2026-11-12T08:04:58Z
- answered_by: eng-3fd6
- tool_use_id: toolu_01LiMPLpHwLsWyNuns9isC8H
- question: Q-01: tsl warns the context pack is 1 commit and 7 files behind sample-system main. That commit is the HMS-9 merge, which changed api/src/services/doctorService.ts, where the day-schedule slot rule lives. Should design and implement treat the current main code as the source of truth?
- options: Use current main code (proposed default) | Refresh the context pack first
- answer: Refresh the context pack first

## A-03
- step: design
- attempt: 1
- asked_at: 2026-11-12T08:29:11Z
- answered_by: eng-3fd6
- tool_use_id: toolu_01EEbbHwo3L3sMiVVS3vnZ2V
- question: Q-01: The stories need each entry's doctor id, code, full name and department name, plus slot {date, start, label} or null. Which JSON shape should GET /api/specializations/:id/earliest-slots return per entry?
- options: Flat (proposed default) | Nested like DoctorSummary
- answer: Flat (proposed default)

## A-04
- step: design
- attempt: 1
- asked_at: 2026-11-12T08:29:11Z
- answered_by: eng-3fd6
- tool_use_id: toolu_01EEbbHwo3L3sMiVVS3vnZ2V
- question: Q-02: Every route declares a permission. The specializations router uses reference:read; the day schedule, which exposes the same slot data, uses appointments:read. Which should the new route require?
- options: appointments:read (proposed default) | reference:read
- answer: appointments:read (proposed default)
