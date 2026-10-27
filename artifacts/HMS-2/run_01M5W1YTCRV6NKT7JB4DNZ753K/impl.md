---
schema: change.v1
run_id: run_01M5W1YTCRV6NKT7JB4DNZ753K
step: impl
attempt: 1
skill: implement@1.0.1
inputs:
  - path: artifacts/HMS-2/run_01M5W1YTCRV6NKT7JB4DNZ753K/triage.md
    sha256: 2c61f0ad50afdc55e27aaecc16365d693e542e59cf47b90732397d1b98161570
  - path: context/conventions.md
    sha256: dc53d94ec34eb193eeb06fdd0db19eb86a24c29f7d3c434e7c212d61038ca852
  - path: context/generated/module-map.json
    sha256: 31d9ca15805fc1b6488e8da89fdb0ee7714adbcdfda176d69581ab8f3d6482da
pack_sha:
  sample-system: 415369398bdad3122ac084a00766a01a3034f900
claims:
  - id: C1
    text: "The triage ranks `PatientsPage` as the high-likelihood cause."
    cites: ["artifacts/HMS-2/run_01M5W1YTCRV6NKT7JB4DNZ753K/triage.md#L40"]
  - id: C2
    text: "The expected display for a patient with no upcoming Scheduled appointment is a `hyphen`."
    cites: ["artifacts/HMS-2/run_01M5W1YTCRV6NKT7JB4DNZ753K/triage.md#L23"]
  - id: C3
    text: "Patients with an upcoming Scheduled appointment must stay `unchanged`."
    cites: ["artifacts/HMS-2/run_01M5W1YTCRV6NKT7JB4DNZ753K/triage.md#L26"]
trace: []
known: [CH-1]
assumed: []
unknown: []
questions: []
code:
  - repo: sample-system
    base_sha: 05da3986a6de089c65145bff45e024df88c5d187
    head_sha: 79a98397fc36fab06392a72b94f2cc194e0a0770
    diff_sha256: 1544db682027ea25e655eafc38c35f6391882e0835f406ac3011c2cd9fee4378
    files: [web/src/pages/PatientsPage.tsx]
summary: "The patient list's Next appointment column shows '-' instead of 'None' when a patient has no upcoming Scheduled appointment; patients with one are unchanged."
---
# Change: HMS-2

## Summary
Fixes the triage's high candidate, `PatientsPage` (C1). When a list item has no next appointment,
the "Next appointment" cell now renders "-" instead of the literal "None" (C2). The branch that
renders an upcoming appointment (start time and doctor, linked to the appointment) is untouched (C3).

## Changes
- `web/src/pages/PatientsPage.tsx`: the empty branch of the Next appointment cell returns `'-'`
  instead of `'None'`. One line; no other file changed. Serves candidate `PatientsPage`.

## Known
- CH-1: the only place that renders "None" for an empty next appointment is this cell; the change
  replaces that text and nothing else (C1, C2, C3).

## Assumed
- None.

## Unknown
- None.

## Open questions
- None.
