---
schema: secure-review.v1
run_id: run_01M6GRSJH8H42FGV284MDTDAZY
step: secrev
attempt: 1
skill: secure-code-review@1.1.0
inputs:
- path: artifacts/HMS-4/run_01M6GRSJH8H42FGV284MDTDAZY/impl.md
  sha256: 76ccc918ec49f6c63a2895b0ee2a9e14e10e11da16a4b7788892428ba3d7b0d4
- path: context/conventions.md
  sha256: dc53d94ec34eb193eeb06fdd0db19eb86a24c29f7d3c434e7c212d61038ca852
- path: context/architecture.md
  sha256: 9faab093d10933a46f6473b25c80728f8745e597100e650cd2537bbafb2b595a
pack_sha:
  sample-system: bd98ff08b83f8762c5e5c073ba5fadef88169b33
claims:
- id: C1
  text: "Every API request passes through `identify`, which sets the actor."
  cites: ["context/architecture.md#identity"]
- id: C2
  text: "In the MVP every request is the actor `staff` and every permission is granted."
  cites: ["context/architecture.md#identity"]
- id: C3
  text: "Logs never carry `request bodies`."
  cites: ["context/conventions.md#logging-and-data"]
- id: C4
  text: "SQL is kept in `api/src/repositories`."
  cites: ["context/conventions.md#code-layout"]
- id: C5
  text: "The new lateral join filters on the literal status `COMPLETED` and takes no request input."
  cites: ["sample-system:api/src/repositories/patientRepo.ts#L139"]
- id: C6
  text: "The Last visit cell renders the `startsAt` output as a JSX text child."
  cites: ["sample-system:web/src/pages/PatientsPage.tsx#L87"]
trace: [REQ-1, REQ-2, REQ-7, REQ-8, REQ-10, REQ-13]
known: [REQ-1, REQ-2, REQ-7, REQ-8, REQ-10, REQ-13]
assumed: []
unknown:
- {id: U-1, blocking: false}
questions:
- id: Q-01
  blocking: false
  ask: tech lead
  text: Four SAST hits predate this change and sit outside its diff (CI action tags, a non-literal RegExp in codes.ts, a log format string in errorHandler.ts); should they be tracked as a separate ticket?
  why: They are not caused by HMS-4 and the change limits forbid touching CI files, but they remain in the scan report attached here.
  evidence: ["runs/run_01M6GRSJH8H42FGV284MDTDAZY/verifier/impl/a1/sast.sarif#L27"]
  options: [open a separate ticket, accept as known]
  default: open a separate ticket
scans:
- {kind: sast, report_sha256: 5ba604a57065cec63c57d0357b94dab57f2b8cff1e1c9db7fc47463c19441b87}
- {kind: secrets, report_sha256: 5a1cea4400bf5f18180b6d2eff35db1be83f6e03aa19603fd628c0484b8d90a3}
findings:
- id: SF-1
  severity: minor
  text: "lastVisitAt adds each patient's visit history to the list response, and in the MVP every caller is staff with every permission, so no per-user check limits who reads it; when sign-in lands, gate the list route with a real permission (accepted for MVP in threat Q-01)."
  cites: ["sample-system:api/src/repositories/patientRepo.ts#L153"]
---
# Secure code review: HMS-4

## Summary
Overall risk is low. The change adds one read-only, parameter-free lateral join (C4, C5) and one nullable field that is rendered as text (C6). It adds no route, no new input, no logging and no dependency.
- The only finding is minor (SF-1). Because the MVP has no real sign-in, visit history now reaches anyone who can call the list. The threat model already accepts this exposure for the MVP.
- The secrets scan has no results.
- The SAST scan reports four hits, all in files outside this diff: .github/workflows/ci.yml, api/src/lib/codes.ts and api/src/middleware/errorHandler.ts. They pre-exist and are raised as Q-01, not as findings on this change.

## Findings
- SF-1 (minor), `sample-system:api/src/repositories/patientRepo.ts#L153`: `lastVisitAt` exposes visit history in the list without a per-user permission in the MVP (C1, C2). Fix direction: gate the list route with a real permission when sign-in is added.

## Known
- REQ-1: the new column renders `startsAt` output as a JSX text child, never as raw HTML, so there is no markup injection (C6).
- REQ-2: the latest-visit lookup sits in the repository layer and uses no request input (C4, C5).
- REQ-7: the status filter is the fixed literal `COMPLETED`, not a parameter (C5).
- REQ-8: a null value is mapped to null and shown as "-", with no error path that could leak data.
- REQ-10: search, status, page and limit are still bound parameters. The new subquery adds none.
- REQ-13: the list's column list is unchanged and selects no medical-history column. No logging was added, and logs never carry request bodies (C3).

## Assumed
None.

## Unknown
- U-1 (non-blocking): four pre-existing SAST hits outside the diff remain open (Q-01).

## Open questions
- Q-01 (non-blocking, tech lead): Track the four pre-existing SAST hits (CI action tags, the non-literal RegExp in codes.ts, the log format string in errorHandler.ts) as a separate ticket? Default: open a separate ticket.
