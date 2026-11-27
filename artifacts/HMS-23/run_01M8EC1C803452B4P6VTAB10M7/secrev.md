---
schema: secure-review.v1
run_id: run_01M8EC1C803452B4P6VTAB10M7
step: secrev
attempt: 1
skill: secure-code-review@1.4.0
inputs:
  - {path: artifacts/HMS-23/run_01M8EC1C803452B4P6VTAB10M7/impl.md, sha256: 5f1a08cc4132d0ed0cd78adf43f0da120afc8fc78663562f2bfafcff1252965f}
  - {path: context/conventions.md, sha256: dc53d94ec34eb193eeb06fdd0db19eb86a24c29f7d3c434e7c212d61038ca852}
  - {path: context/architecture.md, sha256: 9faab093d10933a46f6473b25c80728f8745e597100e650cd2537bbafb2b595a}
pack_sha: {sample-system: 7681f87749883d71abb4011d118366ca1242e1cc}
claims:
  - {id: C1, text: "The dashboard route keeps its permission check `dashboard:read`; no new route was added.", cites: ["sample-system:api/src/routes/dashboard.ts#L10"]}
  - {id: C2, text: "The new doctor query binds the weekday as a parameter: `b.weekday = $1`.", cites: ["sample-system:api/src/repositories/doctorRepo.ts#L146"]}
  - {id: C3, text: "The weekday comes from the server clock via `isoWeekday(today)`, not from the request.", cites: ["sample-system:api/src/services/doctorService.ts#L170"]}
  - {id: C4, text: "Each row copies the doctor's `fullName` and department only; phone and email are not mapped.", cites: ["sample-system:api/src/services/doctorService.ts#L179"]}
  - {id: C5, text: "The page renders `r.fullName` as React text, so it is escaped.", cites: ["sample-system:web/src/pages/DashboardPage.tsx#L89"]}
  - {id: C6, text: "Logs never carry request bodies or `contact details`; the change adds no logging.", cites: ["context/conventions.md#logging-and-data"]}
  - {id: C7, text: "In the MVP every request is the actor `staff` with every permission.", cites: ["context/architecture.md#identity"]}
  - {id: C8, text: "The new API test fixtures use invented `example.com` addresses.", cites: ["sample-system:api/test/dashboard-on-duty.test.ts#L64"]}
  - {id: C9, text: "`onDutyToday()` runs on every dashboard request, whatever the period.", cites: ["sample-system:api/src/services/dashboardService.ts#L92"]}
  - {id: C10, text: "Per on-duty doctor the service runs `occupyingBetween` plus an availability query.", cites: ["sample-system:api/src/services/doctorService.ts#L171"]}
trace: [REQ-17]
known: [SF-1]
assumed: []
unknown: []
questions: []
scans:
  - {kind: sast, report_sha256: 87b5638cac37cd59c76dc7b71d9857db6c88e9ab26ceb5821fe172378b0d4539}
  - {kind: secrets, report_sha256: 44db21a7136e1c4b99facc372571da85962dce6bfd05d6bdf4ade5a119c96131}
findings:
  - {id: SF-1, severity: minor, text: "GET /api/dashboard now runs two queries per on-duty doctor on every load, so database work grows with the number of doctors; batch the availability and appointment reads into one query each.", cites: ["sample-system:api/src/services/doctorService.ts#L170-L171"]}
---
# Secure code review: HMS-23

## Summary
Overall risk is low. The verifier's SAST and secrets scans both passed on the change. No new route or input was added: the dashboard route keeps `dashboard:read` (C1). The new SQL binds its only value as a parameter (C2), and that value comes from the server clock rather than the request (C3). Rows carry no phone or email (C4), which meets REQ-17 and DSH-9. The page renders the names as escaped React text (C5), and nothing new is logged (C6).

The one finding, SF-1 (minor), is the query count per dashboard load (C9, C10). It matters more because every request is currently a fully permitted `staff` actor (C7).

## Findings
- SF-1 (minor): GET /api/dashboard now runs two queries per on-duty doctor on every load, so database work grows with the number of doctors. Fix direction: batch the availability and appointment reads into one query each. Cite: sample-system:api/src/services/doctorService.ts#L170-L171.

Scans attached unchanged:
- sast: report_sha256 87b5638cac37cd59c76dc7b71d9857db6c88e9ab26ceb5821fe172378b0d4539.
- secrets: report_sha256 44db21a7136e1c4b99facc372571da85962dce6bfd05d6bdf4ade5a119c96131.

Checked with nothing to report:
- **Authorisation:** unchanged (C1).
- **Injection:** the SQL is parameterised (C2).
- **Output encoding:** handled by React (C5).
- **Error leakage:** no new error paths.
- **Logging:** nothing new is logged (C6).
- **Dependencies:** no package or lock file changed.
- **Test data:** the new test fixtures use invented example.com addresses (C8) and phone numbers in the unassigned range that the data rules allow.

## Known
- SF-1: read from the changed loop in `onDutyToday()` (C10), which runs on every dashboard request (C9).

## Assumed
None.

## Unknown
None.

## Open questions
None from this step. The run's existing open questions (runs/run_01M8EC1C803452B4P6VTAB10M7/open-questions.md#q-01 and #q-02) are unchanged.
