---
schema: secure-review.v1
run_id: run_01M5XZYC101HCFE34KWJ49C90V
step: secrev
attempt: 1
skill: secure-code-review@1.0.1
inputs:
  - path: artifacts/HMS-3/run_01M5XZYC101HCFE34KWJ49C90V/impl.md
    sha256: 494b207ab62d6a2b4728b9c8b2f3c794f95f70ea156045649073f55375b2ba54
  - path: context/conventions.md
    sha256: dc53d94ec34eb193eeb06fdd0db19eb86a24c29f7d3c434e7c212d61038ca852
  - path: context/architecture.md
    sha256: 9faab093d10933a46f6473b25c80728f8745e597100e650cd2537bbafb2b595a
pack_sha:
  sample-system: 415369398bdad3122ac084a00766a01a3034f900
claims:
  - id: C1
    text: "In the MVP every request is the actor `staff` and every permission is granted."
    cites: ["context/architecture.md#identity"]
  - id: C2
    text: "Logs never carry request bodies, contact details or `medical history`."
    cites: ["context/conventions.md#logging-and-data"]
  - id: C3
    text: "The change returns the No-show count as `totals.noShows`."
    cites: ["artifacts/HMS-3/run_01M5XZYC101HCFE34KWJ49C90V/impl.md#L54"]
  - id: C4
    text: "The changed files are the dashboard service, the `DashboardPage`, the web types and two tests."
    cites: ["artifacts/HMS-3/run_01M5XZYC101HCFE34KWJ49C90V/impl.md#L52"]
  - id: C5
    text: "Routes declare a permission with `requirePermission`; sign-in replaces only the identity functions."
    cites: ["context/architecture.md#identity"]
trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10]
known: [SF-1]
assumed: []
unknown:
  - {id: U-1, blocking: false}
questions:
  - id: Q-01
    blocking: false
    ask: tech lead
    text: "Before release, will GET /api/dashboard be limited to signed-in staff or a staff-only network?"
    why: "In the MVP every request is staff and every permission is granted (C1), so the dashboard, now with the No-show count, is readable by anyone who can reach the API."
    evidence: ["context/architecture.md#identity", "context/architecture.md#deployment-shape"]
    options: ["Staff-only network", "Add sign-in before release", "Accept for the MVP"]
    default: "Accept for the MVP as for the existing dashboard, and enforce the route's permission when sign-in replaces identify and requirePermission."
scans:
  - {kind: sast, report_sha256: f17a899f72eba1c660afd6b6e0b8b5646ed7f3c32f50a7126702023832a1bb81}
  - {kind: secrets, report_sha256: 5a1cea4400bf5f18180b6d2eff35db1be83f6e03aa19603fd628c0484b8d90a3}
findings:
  - id: SF-1
    severity: minor
    text: "The dashboard response, now including the No-show count, is readable by any caller because the MVP grants every request; fix direction: enforce the dashboard route's real permission check when sign-in replaces the MVP identity (Q-01)."
    cites: ["sample-system:api/src/services/dashboardService.ts#L53", "context/architecture.md#identity"]
---
# Secure code review: HMS-3

## Summary
Overall risk is low. The SAST scan reports four hits, all in files this change does not touch (a CI
workflow, `api/src/lib/codes.ts` and `api/src/middleware/errorHandler.ts`); the secrets scan reports
none. Reading the diff (C4): the new count at `sample-system:api/src/services/dashboardService.ts#L53`
uses the status `NO_SHOW` as a fixed literal and the existing bound parameters `$4` and `$2` for the
period, so no request input reaches the SQL; the period itself is still validated before use. It
returns one number (C3) at `#L73`, reads no patient columns, logs nothing new (C2), and adds no route,
dependency or error path. The tile at `sample-system:web/src/pages/DashboardPage.tsx#L58` builds its
link from the fixed path, the literal status and the period dates the API returned, through the page's
query helper, and React renders the label and count as text, so nothing is injected. The only finding,
SF-1, is the existing MVP exposure: anyone who can reach the API can read the dashboard (C1).

## Findings
- SF-1 (minor): the dashboard, now with the No-show count, is open to any caller in the MVP; enforce the route's permission once sign-in exists (C1, C5) — `sample-system:api/src/services/dashboardService.ts#L53`.

## Known
- SF-1: the MVP identity model grants every request (C1), and the change adds one count to the dashboard response (C3).

## Assumed
- None.

## Unknown
- U-1 (non-blocking): whether access to the dashboard is limited before release (Q-01).

## Open questions
- Q-01 (non-blocking, tech lead): staff-only network, sign-in before release, or accept for the MVP? Default: accept for the MVP and enforce the permission with the sign-in work.
