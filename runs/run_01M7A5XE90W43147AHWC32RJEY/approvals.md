---
schema: approvals.v1
run_id: run_01M7A5XE90W43147AHWC32RJEY
---

## G-escalate-threat-a3 · required role: tech_lead
kind: escalation
Artifact: artifacts/HMSTEST-1/run_01M7A5XE90W43147AHWC32RJEY/threat.md (sha256 37729e8de61b4742d117e068b990c380bfa6239723611c5da65e3482033c0727)
Reason for gate: the step still failed its quality checks after the maximum number of retries
decision: changes
comment: Add one threat for login rate limiting; keep "T-1" wording, cost $0 – café ✓
<!-- signature block written by `tsl gate` - do not edit -->
<!-- tsl:signature-begin G-escalate-threat-a3 -->
```json
{
  "action": null,
  "approved_at": "2026-11-13T11:35:40Z",
  "approver": "eng-3c51",
  "artifacts": [
    {
      "attempt": 3,
      "sha256": "37729e8de61b4742d117e068b990c380bfa6239723611c5da65e3482033c0727",
      "step": "threat"
    }
  ],
  "comment_sha256": "27ece97a9d4080e36dfa67bdb13c980347f048164c9c55ae3357dc2cdff0c34c",
  "decision": "changes",
  "gate": "G-escalate-threat-a3",
  "key_id": "key_01M4ZAT978AW7WH5Y2GHCA6J7Y",
  "kind": "escalation",
  "role": "tech_lead",
  "run_id": "run_01M7A5XE90W43147AHWC32RJEY",
  "schema": "approval-signature.v1",
  "self_approved": true,
  "signature": "Eu5qiHpIPTm-8gEMhXbkKoYGbe0yiysd5UautYRQmiYvaD0s--TnvPN0atkNyIl0yNA_b9qDWRt2a78uUHr6AA",
  "system": "SYS-SMP2"
}
```
<!-- tsl:signature-end G-escalate-threat-a3 -->

## G-escalate-threat-a6 · required role: tech_lead
kind: escalation
Artifact: artifacts/HMSTEST-1/run_01M7A5XE90W43147AHWC32RJEY/threat.md (sha256 609ea127ccfe69d407c35a3e299f35d838b2a3775c4a20d674c4c726cdb1973d)
Reason for gate: the step still failed its quality checks after the maximum number of retries
decision:          # approve | reject | changes | abort
comment:
<!-- signature block written by `tsl gate` - do not edit -->
