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
decision: approve
comment: Accept the threat model; H2 design-input check is a known workflow bug
<!-- signature block written by `tsl gate` - do not edit -->
<!-- tsl:signature-begin G-escalate-threat-a6 -->
```json
{
  "action": null,
  "approved_at": "2026-11-13T11:41:15Z",
  "approver": "eng-3c51",
  "artifacts": [
    {
      "attempt": 6,
      "sha256": "609ea127ccfe69d407c35a3e299f35d838b2a3775c4a20d674c4c726cdb1973d",
      "step": "threat"
    }
  ],
  "comment_sha256": "efc4516c5262be7210fdceef4f8191a3b6dc333e1a62dfcf1d4cfdf65cd61eff",
  "decision": "approve",
  "gate": "G-escalate-threat-a6",
  "key_id": "key_01M4ZAT978AW7WH5Y2GHCA6J7Y",
  "kind": "escalation",
  "role": "tech_lead",
  "run_id": "run_01M7A5XE90W43147AHWC32RJEY",
  "schema": "approval-signature.v1",
  "self_approved": true,
  "signature": "fcdpMDkZyEhKr6xW5dJocTo5TqsZgeN3b6x6NDdYLX1Zkk8s8c5oOFqNSa8L5N8hl4JaSdAslO4N1UhCoMd7Ag",
  "system": "SYS-SMP2"
}
```
<!-- tsl:signature-end G-escalate-threat-a6 -->

## G-threat-review · required role: tech_lead
kind: workflow_gate
Artifact: artifacts/HMSTEST-1/run_01M7A5XE90W43147AHWC32RJEY/threat.md (sha256 609ea127ccfe69d407c35a3e299f35d838b2a3775c4a20d674c4c726cdb1973d)
Reason for gate: workflow gate (always)
decision: abort
comment: hands-on test done
<!-- signature block written by `tsl gate` - do not edit -->
<!-- tsl:signature-begin G-threat-review -->
```json
{
  "action": null,
  "approved_at": "2026-11-13T11:42:32Z",
  "approver": "eng-3c51",
  "artifacts": [
    {
      "attempt": 6,
      "sha256": "609ea127ccfe69d407c35a3e299f35d838b2a3775c4a20d674c4c726cdb1973d",
      "step": "threat"
    }
  ],
  "comment_sha256": "3f56158706bd603f743b1ef1288fe12d2bfd8ac5f0a5bd74fe82772a264e904d",
  "decision": "abort",
  "gate": "G-threat-review",
  "key_id": "key_01M4ZAT978AW7WH5Y2GHCA6J7Y",
  "kind": "workflow_gate",
  "role": "tech_lead",
  "run_id": "run_01M7A5XE90W43147AHWC32RJEY",
  "schema": "approval-signature.v1",
  "self_approved": true,
  "signature": "XEeqfCy0bBsJaHoFL8kvPyeiSP-u4235vvmEYEK7n0LMfaoJETzEYnFo4ySyZFEOm3gWG1NI-ajB8D4SztRLAQ",
  "system": "SYS-SMP2"
}
```
<!-- tsl:signature-end G-threat-review -->
