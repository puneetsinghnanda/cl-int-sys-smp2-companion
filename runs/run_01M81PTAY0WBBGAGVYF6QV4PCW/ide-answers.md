# IDE answers: run_01M81PTAY0WBBGAGVYF6QV4PCW

Written by the tsl post-tool hook from AskUserQuestion. Do not edit: artifacts cite these
entries as the user's own answers.

## A-01
- step: req
- attempt: 1
- asked_at: 2026-11-22T13:27:04Z
- answered_by: eng-1df5
- tool_use_id: toolu_011FZH7YtqcYboVJcCJYEjLq
- question: Q-01: The doctor list today has columns Code, Name, Specialization, Department, Fee, Status and Today (added by HMS-6; the ticket doesn't mention it). Where should the new "Works on" column go?
- options: After Status, before Today (proposed default) | Last, after Today
- answer: After Status, before Today (proposed default)

## A-02
- step: req
- attempt: 1
- asked_at: 2026-11-22T13:27:04Z
- answered_by: eng-1df5
- tool_use_id: toolu_011FZH7YtqcYboVJcCJYEjLq
- question: REQ-14: I assumed the "Works on" filter options use full weekday names ("Any day", "Monday" … "Sunday"), because the ticket says "one weekday, Monday to Sunday", while the column itself uses short names (Mon … Sun). Is that right?
- options: Yes, confirm | Use short names
- answer: Yes, confirm

## A-03
- step: design
- attempt: 1
- asked_at: 2026-11-22T13:45:17Z
- answered_by: eng-1df5
- tool_use_id: toolu_01QoHrhMvDSMs3HMYs89Jsmh
- question: Q-01: Each doctor row from GET /api/doctors needs the weekdays the doctor works so the list can show "Mon, Wed, Sat". Which field name and format should it use?
- options: weekdays: int[] 1-7 (proposed default) | works_on: string[] short names
- answer: weekdays: int[] 1-7 (proposed default)

## A-04
- step: design
- attempt: 1
- asked_at: 2026-11-22T13:45:17Z
- answered_by: eng-1df5
- tool_use_id: toolu_01QoHrhMvDSMs3HMYs89Jsmh
- question: Q-02: When should GET /api/doctors include the new weekdays field?
- options: Only with with_today=true (proposed default) | Always
- answer: Only with with_today=true (proposed default)

## A-05
- step: design
- attempt: 1
- asked_at: 2026-11-22T13:45:17Z
- answered_by: eng-1df5
- tool_use_id: toolu_01QoHrhMvDSMs3HMYs89Jsmh
- question: Q-03: Which query parameter (API and page address) should carry the chosen weekday?
- options: weekday=1..7 (proposed default) | weekday=mon..sun
- answer: weekday=1..7 (proposed default)
