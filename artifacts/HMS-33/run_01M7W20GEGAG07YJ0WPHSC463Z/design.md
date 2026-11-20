---
schema: design.v1
run_id: run_01M7W20GEGAG07YJ0WPHSC463Z
step: design
attempt: 1
skill: design@1.9.0
inputs:
  - path: artifacts/HMS-33/run_01M7W20GEGAG07YJ0WPHSC463Z/split.md
    sha256: c05e9c99ae954bc1723d7c72ed43e179acd5ddc73fc04085968a1a344df9ef19
  - path: artifacts/HMS-33/run_01M7W20GEGAG07YJ0WPHSC463Z/req.md
    sha256: 8f8b87647a6a511b96c044fc3e961844c5b87b7ac9a1923914a691c76eba9969
  - path: context/intent-baseline.md
    sha256: 9bfd821aeb2fb9524863e346707e049e4fcfb949a6e55b90223a79194946a215
  - path: context/architecture.md
    sha256: 9faab093d10933a46f6473b25c80728f8745e597100e650cd2537bbafb2b595a
  - path: context/generated/module-map.json
    sha256: 4158c8fcd239c22cb8f1029dd11f61d8839867ff8088bd9ab76197e2db96db4b
  - path: context/generated/api-inventory.json
    sha256: ba024ab1b4b4151d26ce1afa77bd2dab1cec645ea737ce37f98e0e8674f795e9
pack_sha:
  sample-system: 7681f87749883d71abb4011d118366ca1242e1cc
claims:
  - id: C1
    text: "The list query already selects `a.cancellation_reason`, so no SQL or migration change is needed."
    cites: ["sample-system:api/src/repositories/appointmentRepo.ts#L23"]
  - id: C2
    text: "`toSummary` builds each list item and leaves the reason out."
    cites: ["sample-system:api/src/repositories/appointmentRepo.ts#L28", "context/generated/module-map.json#sym:sample-system:api/src/repositories/appointmentRepo.ts:toSummary"]
  - id: C3
    text: "The repository's list returns its rows through `rows.map(toSummary)`."
    cites: ["sample-system:api/src/repositories/appointmentRepo.ts#L83"]
  - id: C4
    text: "The list's row type `SummaryRow` has no cancellation reason field."
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/repositories/appointmentRepo.ts:SummaryRow"]
  - id: C5
    text: "The detail type already uses the field name and type `cancellationReason: string | null`."
    cites: ["sample-system:web/src/api/types.ts#L116"]
  - id: C6
    text: "The API declares `AppointmentSummary` in its types file."
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/types.ts:AppointmentSummary"]
  - id: C7
    text: "The web app declares `AppointmentSummary` in its API types file."
    cites: ["context/generated/module-map.json#sym:sample-system:web/src/api/types.ts:AppointmentSummary"]
  - id: C8
    text: "The service's list returns `Page<AppointmentSummary>`."
    cites: ["sample-system:api/src/services/appointmentService.ts#L25"]
  - id: C9
    text: "The web client reads the list as `send<Page<AppointmentSummary>>`."
    cites: ["sample-system:web/src/api/client.ts#L77"]
  - id: C10
    text: "`AppointmentTable` is one shared component used by the list, dashboard, doctor and patient pages."
    cites: ["context/generated/module-map.json#sym:sample-system:web/src/components/AppointmentTable.tsx:AppointmentTable"]
  - id: C11
    text: "The table already takes an optional prop `hide?` that only some pages set."
    cites: ["sample-system:web/src/components/AppointmentTable.tsx#L9"]
  - id: C12
    text: "The list page renders `<AppointmentTable items={list.data.items} />`."
    cites: ["sample-system:web/src/pages/AppointmentsPage.tsx#L49"]
  - id: C13
    text: "The list endpoint is `/api/appointments`."
    cites: ["context/generated/api-inventory.json#http:GET:/api/appointments"]
  - id: C14
    text: "The user chose `List endpoint only` for the new field."
    cites: ["runs/run_01M7W20GEGAG07YJ0WPHSC463Z/ide-answers.md#a-02"]
  - id: C15
    text: "The list shows the reason `as staff entered it`."
    cites: ["artifacts/HMS-33/run_01M7W20GEGAG07YJ0WPHSC463Z/req.md#item:REQ-17", "runs/run_01M7W20GEGAG07YJ0WPHSC463Z/ide-answers.md#a-01"]
trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10, REQ-11, REQ-12, REQ-13, REQ-14, REQ-15, REQ-16, REQ-17]
known: [CO-1, CO-2, CO-3, CO-4, CO-5, CO-6, CO-7, CO-8, EP-1, EP-2, EP-3]
assumed: []
unknown: []
questions: []
components:
  - id: CO-1
    name: "AppointmentRepo.list: add the reason to each list item"
    module: module:sample-system:api
    change: changed
    cites: ["sample-system:api/src/repositories/appointmentRepo.ts#L83", "sample-system:api/src/repositories/appointmentRepo.ts#L23", "runs/run_01M7W20GEGAG07YJ0WPHSC463Z/ide-answers.md#a-02"]
  - id: CO-2
    name: "API type AppointmentListItem (AppointmentSummary plus cancellationReason), in api/src/types.ts"
    module: module:sample-system:api
    change: new
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/types.ts:AppointmentSummary", "runs/run_01M7W20GEGAG07YJ0WPHSC463Z/ide-answers.md#a-02"]
  - id: CO-3
    name: "AppointmentService.list: return Page<AppointmentListItem>"
    module: module:sample-system:api
    change: changed
    cites: ["sample-system:api/src/services/appointmentService.ts#L25", "runs/run_01M7W20GEGAG07YJ0WPHSC463Z/ide-answers.md#a-02"]
  - id: CO-4
    name: "Web type AppointmentListItem in web/src/api/types.ts, and api.appointments in web/src/api/client.ts typed with it"
    module: module:sample-system:web
    change: changed
    cites: ["context/generated/module-map.json#sym:sample-system:web/src/api/types.ts:AppointmentSummary", "sample-system:web/src/api/client.ts#L77", "runs/run_01M7W20GEGAG07YJ0WPHSC463Z/ide-answers.md#a-02"]
  - id: CO-5
    name: "AppointmentTable: optional showCancellationReason prop adding the Cancellation reason column after Status"
    module: module:sample-system:web
    change: changed
    cites: ["context/generated/module-map.json#sym:sample-system:web/src/components/AppointmentTable.tsx:AppointmentTable", "sample-system:web/src/components/AppointmentTable.tsx#L9"]
  - id: CO-6
    name: "AppointmentsPage: turns the column on"
    module: module:sample-system:web
    change: changed
    cites: ["context/generated/module-map.json#sym:sample-system:web/src/pages/AppointmentsPage.tsx:AppointmentsPage", "sample-system:web/src/pages/AppointmentsPage.tsx#L49"]
  - id: CO-7
    name: "Dashboard, doctor and patient pages: keep using AppointmentTable without the new prop"
    module: module:sample-system:web
    change: existing
    cites: ["context/generated/module-map.json#sym:sample-system:web/src/components/AppointmentTable.tsx:AppointmentTable"]
  - id: CO-8
    name: "AppointmentDetailPage: still shows the full reason"
    module: module:sample-system:web
    change: existing
    cites: ["context/generated/module-map.json#sym:sample-system:web/src/pages/AppointmentDetailPage.tsx:AppointmentDetailPage"]
endpoints:
  - id: EP-1
    method: GET
    path: /api/appointments
    change: changed
    fields:
      - name: items[].cancellationReason
        type: string
        nullable: true
        format: "the stored reason text unchanged, up to 300 characters; null when the appointment has no reason (every status except Cancelled)"
        example: "Patient travelling"
        status: known
    cites: ["context/generated/api-inventory.json#http:GET:/api/appointments", "sample-system:web/src/api/types.ts#L116", "runs/run_01M7W20GEGAG07YJ0WPHSC463Z/ide-answers.md#a-02"]
  - id: EP-2
    method: GET
    path: /api/appointments/:id
    change: existing
    fields: []
    cites: ["context/generated/api-inventory.json#http:GET:/api/appointments/:id"]
  - id: EP-3
    method: POST
    path: /api/appointments/:id/cancel
    change: existing
    fields: []
    cites: ["context/generated/api-inventory.json#http:POST:/api/appointments/:id/cancel"]
answers:
  - id: A-02
    kind: question
    about: Q-01
    answer: "List endpoint only (proposed default)"
    decides: [CO-1, CO-2, CO-3, CO-4, EP-1]
---
# Design: HMS-33

## Overview
The reason is already stored and already read by the list query (C1), but `toSummary` leaves it out (C2, C3). The API adds it only to the items of `GET /api/appointments`, as the user chose in A-02 (C14): a new `AppointmentListItem` type is `AppointmentSummary` plus `cancellationReason: string | null`, the same name and type the detail response uses (C5). The dashboard, doctor and patient responses keep `AppointmentSummary` and do not change. Trade-off: one extra type on each side for a payload that stays unchanged everywhere else.

On the web side, the shared `AppointmentTable` (C10) gets an optional `showCancellationReason` prop, following its existing optional `hide?` prop (C11); only the list page sets it (C12), so the other three pages keep their columns. Name default: `showCancellationReason`, following `hide`; trade-off: an opt-in prop rather than a second table component.

Cell rule (S-1 to S-3): if the status is Cancelled and the reason is not null, show it, and when it is longer than 60 characters show its first 60 characters followed by "…"; otherwise show "-". Characters are counted as code points (`Array.from`), so a character outside the basic plane is not split; trade-off: slightly more work than `slice`. The text is rendered as React text, as staff entered it, with no filtering (C15). Shortening happens in the web app, so the API keeps sending the full text; trade-off: a little more data per row, at most 300 characters.

Filters, date range and paging (S-4) are unchanged: the column reads from the same items the list already pages and filters. Opening, cancelling and booking (S-6) use EP-2 and EP-3 and other existing code, all unchanged.

## Components
- CO-1 (changed) AppointmentRepo.list: maps each row to `{ ...toSummary(r), cancellationReason: r.cancellation_reason }`; the list's row type gains `cancellation_reason: string | null`. No SQL change (C1).
- CO-2 (new) API type AppointmentListItem in api/src/types.ts, next to AppointmentSummary (C6).
- CO-3 (changed) AppointmentService.list returns `Page<AppointmentListItem>` (C8).
- CO-4 (changed) Web type AppointmentListItem in web/src/api/types.ts (C7); api.appointments returns `Page<AppointmentListItem>` (C9).
- CO-5 (changed) AppointmentTable: `showCancellationReason?: boolean`, default false; when true, a header "Cancellation reason" right after Status and one cell per row by the cell rule.
- CO-6 (changed) AppointmentsPage: passes `showCancellationReason` to the table.
- CO-7 (existing) Dashboard, doctor and patient pages: unchanged, no new prop.
- CO-8 (existing) AppointmentDetailPage: unchanged; shows the full reason.
- EP-1 (changed) GET /api/appointments: adds `items[].cancellationReason`: string, nullable; the stored text unchanged, up to 300 characters; null when the appointment has no reason; example "Patient travelling". Known: A-02 fixes where, C5 fixes the name and type.
- EP-2 (existing) GET /api/appointments/:id: unchanged.
- EP-3 (existing) POST /api/appointments/:id/cancel: unchanged.

Stories served: S-1, S-2, S-3 by CO-1 to CO-6 and EP-1; S-2 AC3 also by CO-8 and EP-2; S-4 by CO-6 and EP-1; S-5 by CO-5 and CO-7; S-6 by CO-8, EP-2 and EP-3.

## Known
- CO-1 to CO-6 and EP-1: each change point is proven by a sym fact or the product code line it cites, and decided by A-02 where it concerns the contract.
- CO-7, CO-8, EP-2, EP-3: existing parts the stories rely on, left unchanged.

## Assumed
None.

## Unknown
None.

## Open questions
None open.

Answered in the IDE:
- A-02 (Q-01): where should the new `cancellationReason` field be added? Answer: "List endpoint only (proposed default)". Decides CO-1, CO-2, CO-3, CO-4 and EP-1. No mismatch with the inputs.
