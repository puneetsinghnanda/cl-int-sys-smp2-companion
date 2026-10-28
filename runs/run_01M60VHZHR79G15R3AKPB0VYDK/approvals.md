---
schema: approvals.v1
run_id: run_01M60VHZHR79G15R3AKPB0VYDK
---

## G-escalate-ut-a1 · required role: tech_lead
kind: escalation
Artifact: artifacts/HMS-5/run_01M60VHZHR79G15R3AKPB0VYDK/ut.md (sha256 82b325a6d4e64430304b86d0bab852d64d7f467d40303c40e298f8dd71f566c7)
Reason for gate: band Low
decision: changes
comment: Add a test for a No-show-only patient expecting { completed: 0, no_show: 1 } (S-2 AC5). Also assert appointment order (F-2), assert the PATCH status (F-3), close appointments far apart in date for REQ-9 (F-4), and fix the key-set wording (F-5).
<!-- signature block written by `tsl approve` - do not edit -->
<!-- tsl:signature-begin G-escalate-ut-a1 -->
```json
{
  "action": null,
  "approved_at": "2026-10-28T09:41:43Z",
  "approver": "eng-dbe4",
  "artifacts": [
    {
      "attempt": 1,
      "sha256": "82b325a6d4e64430304b86d0bab852d64d7f467d40303c40e298f8dd71f566c7",
      "step": "ut"
    }
  ],
  "comment_sha256": "621f711e1c4d695631692a11ee4b1b0565273e40a5254d945ace3fe610f22b88",
  "decision": "changes",
  "gate": "G-escalate-ut-a1",
  "key_id": "key_01M4Z5DDZ8324GN4PB350G4J9Q",
  "kind": "escalation",
  "role": "tech_lead",
  "run_id": "run_01M60VHZHR79G15R3AKPB0VYDK",
  "schema": "approval-signature.v1",
  "self_approved": true,
  "signature": "xqVbwhEWbUBCYOj4uOUy1Y6tUf51oBOM0TiL0TASNVz3rY3qIfr7fVMFVTRhlPabycjPVii2_SwY5q-QzcLkBg",
  "system": "SYS-SMP2"
}
```
<!-- tsl:signature-end G-escalate-ut-a1 -->
