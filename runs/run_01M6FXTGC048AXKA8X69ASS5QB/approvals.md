---
schema: approvals.v1
run_id: run_01M6FXTGC048AXKA8X69ASS5QB
---

## G-escalate-impact-a2 · required role: tech_lead
kind: escalation
Artifact: artifacts/HMS-7/run_01M6FXTGC048AXKA8X69ASS5QB/impact.md (sha256 b05955d7672699dc41d3aaf0289755f88dff65e0aad8b09535a88bca58b74e4d)
Reason for gate: retry budget exhausted (checks)
decision: changes  # approve | reject | changes | abort
comment: Content is fine; shorten resolves[0].how to under 300 characters.
<!-- signature block written by `tsl approve` - do not edit -->
<!-- tsl:signature-begin G-escalate-impact-a2 -->
```json
{
  "action": null,
  "approved_at": "2026-11-03T06:50:41Z",
  "approver": "eng-3c51",
  "artifacts": [
    {
      "attempt": 2,
      "sha256": "b05955d7672699dc41d3aaf0289755f88dff65e0aad8b09535a88bca58b74e4d",
      "step": "impact"
    }
  ],
  "comment_sha256": "1f3721a1ce5108b33808db5a1a67c2b561eb1a5f5ef72419ee50cc940e2f75c2",
  "decision": "changes",
  "gate": "G-escalate-impact-a2",
  "key_id": "key_01M4ZAT978AW7WH5Y2GHCA6J7Y",
  "kind": "escalation",
  "role": "tech_lead",
  "run_id": "run_01M6FXTGC048AXKA8X69ASS5QB",
  "schema": "approval-signature.v1",
  "self_approved": true,
  "signature": "FCr7Vb052aUy2WT0JpWLq7-Q7xRp7vc9T-3PZUKFR5pSIja-IHHxPBIutgTM6iFhaAH7CC8DH30-k6G0rrcvBQ",
  "system": "SYS-SMP2"
}
```
<!-- tsl:signature-end G-escalate-impact-a2 -->

## G-escalate-design-a2 · required role: tech_lead
kind: escalation
Artifact: artifacts/HMS-7/run_01M6FXTGC048AXKA8X69ASS5QB/design.md (sha256 4e47f93fe154e95a25a777f5f6f3389287f759816ccc24ecbd97a6b4efe4f023)
Reason for gate: band Low
decision: changes  # approve | reject | changes | abort
comment: Q-01/Q-02 answered from code (see open-questions.md). Fix the interface to match the code: (1) shared helper = DashboardService's '30d' rule: from = addDays(localDay(now,tz),-29), counted starts_at >= dayBounds(from).start and < dayBounds(today).end, status NO_SHOW. (2) Link reuses the dashboard No-shows tile shape: /appointments?doctorId=<id>&status=NO_SHOW&from=<YYYY-MM-DD>&to=<YYYY-MM-DD>; from/to are hospital-local days, not UTC instants. (3) Use camelCase response fields (e.g. noShowsLast30Days, noShowsWindow {from,to} as local days), matching feePaise/slotMinutes and the dashboard's noShows/period. Drop Q-01, Q-02, Q-04 and "half-open"; keep Q-03.
<!-- signature block written by `tsl approve` - do not edit -->
<!-- tsl:signature-begin G-escalate-design-a2 -->
```json
{
  "action": null,
  "approved_at": "2026-11-03T07:38:41Z",
  "approver": "eng-3c51",
  "artifacts": [
    {
      "attempt": 2,
      "sha256": "4e47f93fe154e95a25a777f5f6f3389287f759816ccc24ecbd97a6b4efe4f023",
      "step": "design"
    }
  ],
  "comment_sha256": "f7df7e8b18f9dbb1bf7743719ea936bcc43e1c55dc13666ef67eb65d95c30e70",
  "decision": "changes",
  "gate": "G-escalate-design-a2",
  "key_id": "key_01M4ZAT978AW7WH5Y2GHCA6J7Y",
  "kind": "escalation",
  "role": "tech_lead",
  "run_id": "run_01M6FXTGC048AXKA8X69ASS5QB",
  "schema": "approval-signature.v1",
  "self_approved": true,
  "signature": "aPYnDjHdEnu4cda1_cxN3fMGRoDAPpPfyuE2Gzv5Pyp0kPv0G_WL6mbH2G2blCfDJZL7JyvsWLQwmL-_vYSzBw",
  "system": "SYS-SMP2"
}
```
<!-- tsl:signature-end G-escalate-design-a2 -->

## G-escalate-design-a3 · required role: tech_lead
kind: escalation
Artifact: artifacts/HMS-7/run_01M6FXTGC048AXKA8X69ASS5QB/design.md (sha256 215531f1ce96acf3cbf36175d23fe9f75a7d793b88397e524abe7591fb3b1056)
Reason for gate: band Low
decision: approve
comment: Design matches the code (review 4.85/5, all 18 claims supported). Low confidence comes from citations tsl cannot
auto-check, not from content.
<!-- signature block written by `tsl approve` - do not edit -->
<!-- tsl:signature-begin G-escalate-design-a3 -->
```json
{
  "action": null,
  "approved_at": "2026-11-03T07:55:21Z",
  "approver": "eng-3c51",
  "artifacts": [
    {
      "attempt": 3,
      "sha256": "215531f1ce96acf3cbf36175d23fe9f75a7d793b88397e524abe7591fb3b1056",
      "step": "design"
    }
  ],
  "comment_sha256": "a5b7ae7ddf660530a0e217dd06f8780d03214825b0a4b4a18ee0337c748e9d9b",
  "decision": "approve",
  "gate": "G-escalate-design-a3",
  "key_id": "key_01M4ZAT978AW7WH5Y2GHCA6J7Y",
  "kind": "escalation",
  "role": "tech_lead",
  "run_id": "run_01M6FXTGC048AXKA8X69ASS5QB",
  "schema": "approval-signature.v1",
  "self_approved": true,
  "signature": "2wAhro3uMIs3JiNuh1iZu78uFiVP-XTmSHHei0iaUzoGdBrOebEK0xjmR5-5YC1MwEJNmwOosFiVWuMJpbRODA",
  "system": "SYS-SMP2"
}
```
<!-- tsl:signature-end G-escalate-design-a3 -->
