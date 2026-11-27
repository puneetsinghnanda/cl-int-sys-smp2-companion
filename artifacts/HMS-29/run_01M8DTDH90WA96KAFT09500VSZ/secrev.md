---
schema: secure-review.v1
run_id: run_01M8DTDH90WA96KAFT09500VSZ
step: secrev
attempt: 1
skill: secure-code-review@1.4.0
inputs:
  - path: artifacts/HMS-29/run_01M8DTDH90WA96KAFT09500VSZ/impl.md
    sha256: 1071d8f70af8c5b0b6f2db524634df3c9f76a9a7ebf2a6669cdbe99127208150
  - path: context/conventions.md
    sha256: dc53d94ec34eb193eeb06fdd0db19eb86a24c29f7d3c434e7c212d61038ca852
  - path: context/architecture.md
    sha256: 9faab093d10933a46f6473b25c80728f8745e597100e650cd2537bbafb2b595a
pack_sha:
  sample-system: 7681f87749883d71abb4011d118366ca1242e1cc
claims:
  - id: C1
    text: "The Doctors count link puts the entry name only into an `aria-label` attribute, which React escapes; no raw HTML is rendered."
    cites: ["sample-system:web/src/pages/SettingsPage.tsx#L72"]
  - id: C2
    text: "The link target is the fixed in-app path `/doctors` plus a query string from the `query` helper, so no user text decides where the link goes."
    cites: ["sample-system:web/src/pages/SettingsPage.tsx#L72"]
  - id: C3
    text: "The `query` helper encodes every value with `URLSearchParams`."
    cites: ["sample-system:web/src/api/client.ts#L40-L42"]
  - id: C4
    text: "The accessible name comes from the new helper `doctorsLinkLabel`, which only joins the count and the entry name."
    cites: ["artifacts/HMS-29/run_01M8DTDH90WA96KAFT09500VSZ/impl.md#claim:C2"]
  - id: C5
    text: "The change leaves the endpoint `/api/doctors` as it is and adds no API route."
    cites: ["artifacts/HMS-29/run_01M8DTDH90WA96KAFT09500VSZ/impl.md#claim:C8"]
  - id: C6
    text: "The web app talks to the API only through `web/src/api/client.ts`."
    cites: ["context/architecture.md#components"]
  - id: C7
    text: "In the MVP every request is the actor `staff` and every permission is granted, so this change adds no new trust boundary."
    cites: ["context/architecture.md#identity"]
  - id: C8
    text: "Logs carry method, route and error message only, `never request bodies`, contact details or medical history; the change adds no logging."
    cites: ["context/conventions.md#logging-and-data"]
  - id: C9
    text: "In-app links use `Link` from `react-router-dom`, already a dependency of the web app, so no dependency is added."
    cites: ["sample-system:web/src/pages/SettingsPage.tsx#L2"]
trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8]
known: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8]
assumed: []
unknown: []
questions: []
scans:
  - kind: sast
    report_sha256: 89bc9d78b7be90a3d6e5a7f8f835b2820b24425f94fa622b3019440ee463fa46
  - kind: secrets
    report_sha256: 5a1cea4400bf5f18180b6d2eff35db1be83f6e03aa19603fd628c0484b8d90a3
findings: []
---
# Secure code review: HMS-29

## Summary
The risk is low, and I found no weakness to report. The change is web-only, in
`web/src/pages/SettingsPage.tsx`, plus the unit-test step's test file. It turns a count into an
in-app link.

- **Output encoding:** the staff-entered entry name reaches the page only as an `aria-label`,
  which React escapes (C1, C4).
- **Link target:** the link goes to the fixed path `/doctors`. Its query string is built with
  `URLSearchParams` from a fixed parameter name and the numeric row id (C2, C3), so there is no
  open redirect or URL injection.
- **No new surface:** the change adds no API route, no authorisation path, no logging and no
  dependency (C5 to C9).

The SAST and secrets reports from the verifier are attached by hash, unchanged.

## Findings
None.

## Known
- REQ-1, REQ-2: the link target is a fixed path with an encoded numeric id (C2, C3).
- REQ-3: the link sets no status parameter; nothing in it is security-relevant.
- REQ-4, REQ-5: these only choose between a link and plain text; no new data is shown.
- REQ-6: the accessible name is escaped text (C1, C4).
- REQ-7, REQ-8: no API, logging or authorisation code changed (C5, C7, C8).

## Assumed
None.

## Unknown
None.

## Open questions
None.
