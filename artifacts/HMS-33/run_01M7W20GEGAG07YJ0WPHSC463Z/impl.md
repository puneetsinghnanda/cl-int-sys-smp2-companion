---
schema: change.v1
run_id: run_01M7W20GEGAG07YJ0WPHSC463Z
step: impl
attempt: 1
skill: implement@1.3.0
inputs:
  - path: artifacts/HMS-33/run_01M7W20GEGAG07YJ0WPHSC463Z/design.md
    sha256: 22c08a21691b01c96d632f5ec14f1a83fb7dc942ef0d08e5ad4dbd6e9bb90d49
  - path: artifacts/HMS-33/run_01M7W20GEGAG07YJ0WPHSC463Z/split.md
    sha256: c05e9c99ae954bc1723d7c72ed43e179acd5ddc73fc04085968a1a344df9ef19
  - path: context/conventions.md
    sha256: dc53d94ec34eb193eeb06fdd0db19eb86a24c29f7d3c434e7c212d61038ca852
  - path: context/generated/module-map.json
    sha256: 4158c8fcd239c22cb8f1029dd11f61d8839867ff8088bd9ab76197e2db96db4b
pack_sha:
  sample-system: 7681f87749883d71abb4011d118366ca1242e1cc
claims:
  - id: C1
    text: "The design adds the new list item type `AppointmentListItem` on both sides."
    cites: ["artifacts/HMS-33/run_01M7W20GEGAG07YJ0WPHSC463Z/design.md#item:CO-2", "artifacts/HMS-33/run_01M7W20GEGAG07YJ0WPHSC463Z/design.md#item:CO-4"]
  - id: C2
    text: "The repository list reads the reason the query already selects, `a.cancellation_reason`."
    cites: ["sample-system:api/src/repositories/appointmentRepo.ts#L23"]
  - id: C3
    text: "The design names the table prop `showCancellationReason`."
    cites: ["artifacts/HMS-33/run_01M7W20GEGAG07YJ0WPHSC463Z/design.md#item:CO-5"]
  - id: C4
    text: "The status value of a cancelled appointment is `CANCELLED`."
    cites: ["sample-system:web/src/api/types.ts#L3"]
  - id: C5
    text: "Only the list route calls the service's `list`, so the other responses keep their shape."
    cites: ["sample-system:api/src/routes/appointments.ts#L14"]
  - id: C6
    text: "The new field is added for the `List endpoint only`."
    cites: ["runs/run_01M7W20GEGAG07YJ0WPHSC463Z/ide-answers.md#a-02"]
trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10, REQ-11, REQ-12, REQ-13, REQ-14, REQ-15, REQ-16, REQ-17]
known: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10, REQ-11, REQ-12, REQ-13, REQ-14, REQ-15, REQ-16, REQ-17]
assumed: []
unknown: []
questions: []
code:
  - repo: sample-system
    base_sha: 7681f87749883d71abb4011d118366ca1242e1cc
    head_sha: 8605a786e9ae6d542099750a9fbf1c443f1c3e48
    diff_sha256: b211736eb74ca43ff1728630a3ea30e377d32bffeb94e0c7b973fc7dbb424169
    files:
      - api/src/repositories/appointmentRepo.ts
      - api/src/services/appointmentService.ts
      - api/src/types.ts
      - web/src/api/client.ts
      - web/src/api/types.ts
      - web/src/components/AppointmentTable.tsx
      - web/src/pages/AppointmentsPage.tsx
summary: "GET /api/appointments items now carry cancellationReason (string or null); the shared appointment table gains an opt-in Cancellation reason column after Status, set only by the appointment list, showing a Cancelled appointment's reason (first 60 characters plus an ellipsis when longer) and \"-\" otherwise."
---
# Change: HMS-33

## Summary
The API's list now returns each appointment's cancellation reason (CO-1 to CO-3, EP-1): the repository maps the `a.cancellation_reason` the query already reads (C2) into `cancellationReason` on a new `AppointmentListItem` type (C1). Only the list route uses this method (C5), so the dashboard, doctor and patient responses are unchanged, as A-02 decided (C6). The web client types the list with the same new type (CO-4). The shared `AppointmentTable` takes an optional `showCancellationReason` prop (C3, CO-5); when set it adds a "Cancellation reason" header after Status and a cell that shows the reason for a `CANCELLED` appointment (C4), shortened to its first 60 characters plus "…" when longer (counted with `Array.from`), and "-" for every other status or a missing reason. Only `AppointmentsPage` sets the prop (CO-6). No SQL, migration or test change; tests are the unit-test step's job, and the design asks for no regression test here.

## Changes
- api/src/types.ts: new `AppointmentListItem` (CO-2).
- api/src/repositories/appointmentRepo.ts: `list` returns `AppointmentListItem[]` with `cancellationReason` (CO-1).
- api/src/services/appointmentService.ts: `list` returns `Page<AppointmentListItem>` (CO-3).
- web/src/api/types.ts: new `AppointmentListItem` (CO-4).
- web/src/api/client.ts: `api.appointments` returns `Page<AppointmentListItem>` (CO-4).
- web/src/components/AppointmentTable.tsx: `showCancellationReason` prop, header and cell rule (CO-5).
- web/src/pages/AppointmentsPage.tsx: passes `showCancellationReason` (CO-6).

## Known
- REQ-1 to REQ-17: each is built as the approved design maps it; REQ-1, REQ-2 by the header after Status; REQ-3, REQ-6, REQ-17 by the cell showing the text as entered; REQ-4 by "-"; REQ-5 and REQ-16 by the 60-character cut; REQ-7 and REQ-13 by leaving the detail, cancel and booking code untouched; REQ-8 and REQ-9 by reading the same paged, filtered items; REQ-10 to REQ-12 because only the list page sets the prop and only the list route returns the field; REQ-14 and REQ-15 because the only field added is the reason.

## Assumed
None.

## Unknown
None.

## Open questions
None.
