---
schema: approvals.v1
run_id: run_01M74D9VA0FDDS3DN77YFEXK7H
---

## G-escalate-req-a1 · required role: tech_lead
kind: escalation
Artifact: artifacts/HMS-9/run_01M74D9VA0FDDS3DN77YFEXK7H/req.md (sha256 d91af6d2d33b27873ad261064f8454e09c4904922e83e8ea0b13ffb56bc031e8)
Reason for gate: overall quality came out low (80% of the writing checks passed)
decision:  approve        # approve | reject | changes | abort
comment: Low band from one soft size check (code notes in Summary); content reviewed 4.4/5, all claims cited.
<!-- signature block written by `tsl approve` - do not edit -->
<!-- tsl:signature-begin G-escalate-req-a1 -->
```json
{
  "action": null,
  "approved_at": "2026-11-11T06:23:45Z",
  "approver": "eng-3c51",
  "artifacts": [
    {
      "attempt": 1,
      "sha256": "d91af6d2d33b27873ad261064f8454e09c4904922e83e8ea0b13ffb56bc031e8",
      "step": "req"
    }
  ],
  "comment_sha256": "2ff04db9c1e7ab191588430692f38857d571fded707d0da5cf0e2f902d4c9077",
  "decision": "approve",
  "gate": "G-escalate-req-a1",
  "key_id": "key_01M4ZAT978AW7WH5Y2GHCA6J7Y",
  "kind": "escalation",
  "role": "tech_lead",
  "run_id": "run_01M74D9VA0FDDS3DN77YFEXK7H",
  "schema": "approval-signature.v1",
  "self_approved": true,
  "signature": "pcBnspCZtoZCyGH33skrIZDk7XCBFevXzth9s7E_BwWemiUDKdmotxJaG_pLp-p5g6yaMb8KpV40KiUNmyKBBw",
  "system": "SYS-SMP2"
}
```
<!-- tsl:signature-end G-escalate-req-a1 -->

## G-escalate-impact-a2 · required role: tech_lead
kind: escalation
Artifact: artifacts/HMS-9/run_01M74D9VA0FDDS3DN77YFEXK7H/impact.md (sha256 6a8917d093b964f8c187482843359ed33a2b14310dd1885d78d828836927e08a)
Reason for gate: overall quality came out low (only 65% of claims are backed by a verified source)
decision:    approve      # approve | reject | changes | abort
comment: Low band from 5 dropped reviewer findings (anchor not found) counted in S1; artifact claims 13/15 verified,
  review 4.6/5, PATCH effect added.
<!-- signature block written by `tsl approve` - do not edit -->
<!-- tsl:signature-begin G-escalate-impact-a2 -->
```json
{
  "action": null,
  "approved_at": "2026-11-11T07:20:21Z",
  "approver": "eng-3c51",
  "artifacts": [
    {
      "attempt": 2,
      "sha256": "6a8917d093b964f8c187482843359ed33a2b14310dd1885d78d828836927e08a",
      "step": "impact"
    }
  ],
  "comment_sha256": "7d8be4585bc26f89ec6ed8d2c388058b9408fae256aced3a50b87735d1ba2584",
  "decision": "approve",
  "gate": "G-escalate-impact-a2",
  "key_id": "key_01M4ZAT978AW7WH5Y2GHCA6J7Y",
  "kind": "escalation",
  "role": "tech_lead",
  "run_id": "run_01M74D9VA0FDDS3DN77YFEXK7H",
  "schema": "approval-signature.v1",
  "self_approved": true,
  "signature": "PbF9FBW9UFmqwv-dgPiTr1IFfcUg_xQQDK-NBLSqnovDw5bLd2Oyv3BnfxSqUQ245DPyjk_0DgVF9tJPoNc9AA",
  "system": "SYS-SMP2"
}
```
<!-- tsl:signature-end G-escalate-impact-a2 -->
