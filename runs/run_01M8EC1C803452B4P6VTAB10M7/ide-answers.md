# IDE answers: run_01M8EC1C803452B4P6VTAB10M7

Written by the tsl post-tool hook from AskUserQuestion. Do not edit: artifacts cite these
entries as the user's own answers.

## A-01
- step: req
- attempt: 1
- asked_at: 2026-11-27T11:27:56Z
- answered_at: 2026-11-27T11:28:20Z
- answered_by: eng-3c51
- tool_use_id: toolu_01M23RjSWXUid843YxjEoykC
- question: Q-01: Acceptance criterion 1 lists every Active doctor with availability today. Once a doctor's last slot has started, they have 0 free slots and would show "Fully booked" (criterion 4), even if nobody booked them. How should the dashboard treat a doctor whose slots today have all passed?
- options: Keep, show Fully booked (proposed default) | Hide the doctor | Keep, different label
- answer: Keep, show Fully booked (proposed default)

## A-02
- step: req
- attempt: 1
- asked_at: 2026-11-27T11:27:56Z
- answered_at: 2026-11-27T11:28:20Z
- answered_by: eng-3c51
- tool_use_id: toolu_01M23RjSWXUid843YxjEoykC
- question: Q-02: A row shows the doctor's department. If that department is Inactive, should the row add a marker?
- options: Append " (Inactive)" (proposed default) | Name only, no marker
- answer: Append " (Inactive)" (proposed default)

## A-03
- step: design
- attempt: 1
- asked_at: 2026-11-27T11:50:40Z
- answered_at: 2026-11-27T11:50:56Z
- answered_by: eng-3c51
- tool_use_id: toolu_016CeCe1aMy6DxPrLaR6GrZy
- question: Q-01: How should the dashboard page get the "On duty today" rows from the API?
- options: Add onDuty to GET /api/dashboard (proposed default) | New GET /api/dashboard/on-duty
- answer: Add onDuty to GET /api/dashboard (proposed default)

## A-04
- step: design
- attempt: 1
- asked_at: 2026-11-27T11:50:40Z
- answered_at: 2026-11-27T11:50:56Z
- answered_by: eng-3c51
- tool_use_id: toolu_016CeCe1aMy6DxPrLaR6GrZy
- question: Q-02: What shape should each on-duty row have, and how should the next free slot be sent?
- options: nextFree as "HH:MM" label (proposed default) | nextFree as { start, label }
- answer: nextFree as { start, label }
