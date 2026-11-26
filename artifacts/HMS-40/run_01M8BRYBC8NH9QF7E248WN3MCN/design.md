---
schema: design.v1
run_id: run_01M8BRYBC8NH9QF7E248WN3MCN
step: design
attempt: 1
skill: design@1.9.0
inputs:
  - {path: artifacts/HMS-40/run_01M8BRYBC8NH9QF7E248WN3MCN/split.md, sha256: 7ce601066fce0ccfb2969ed8f41153375268416bfc548e75869166c890cafee0}
  - {path: artifacts/HMS-40/run_01M8BRYBC8NH9QF7E248WN3MCN/req.md, sha256: 47ca1b8e9da969fec60ebc20f1098937b1760993517868ad4317e127dc147e4a}
  - {path: context/intent-baseline.md, sha256: 9bfd821aeb2fb9524863e346707e049e4fcfb949a6e55b90223a79194946a215}
  - {path: context/architecture.md, sha256: 9faab093d10933a46f6473b25c80728f8745e597100e650cd2537bbafb2b595a}
  - {path: context/generated/module-map.json, sha256: 4158c8fcd239c22cb8f1029dd11f61d8839867ff8088bd9ab76197e2db96db4b}
  - {path: context/generated/api-inventory.json, sha256: ba024ab1b4b4151d26ce1afa77bd2dab1cec645ea737ce37f98e0e8674f795e9}
pack_sha: {sample-system: 7681f87749883d71abb4011d118366ca1242e1cc}
claims:
  - {id: C1, text: "GET /api/settings is served from `api/src/routes/health.ts`.", cites: ["context/generated/api-inventory.json#http:GET:/api/settings"]}
  - {id: C2, text: "The router is `healthRouter`.", cites: ["context/generated/module-map.json#sym:sample-system:api/src/routes/health.ts:healthRouter"]}
  - {id: C3, text: "The helper `localDay` is in the time library.", cites: ["context/generated/module-map.json#sym:sample-system:api/src/lib/time.ts:localDay"]}
  - {id: C4, text: "The helper `localDay` takes an instant and a time zone and returns the local day.", cites: ["sample-system:api/src/lib/time.ts#L62-L64"]}
  - {id: C5, text: "The route reads `timeZone`, `currency` and the page size from the request context config.", cites: ["sample-system:api/src/routes/health.ts#L21"]}
  - {id: C6, text: "The application context carries an injectable `clock`.", cites: ["sample-system:api/src/services/index.ts#L21"]}
trace: [REQ-1, REQ-2, REQ-3, REQ-4]
known: [S-1]
assumed: []
unknown: []
questions: []
components:
  - {id: CO-1, name: "healthRouter settings handler (api/src/routes/health.ts)", module: "module:sample-system:api", change: changed, cites: ["context/generated/module-map.json#sym:sample-system:api/src/routes/health.ts:healthRouter"]}
  - {id: CO-2, name: "localDay helper (api/src/lib/time.ts)", module: "module:sample-system:api", change: existing, cites: ["context/generated/module-map.json#sym:sample-system:api/src/lib/time.ts:localDay"]}
endpoints:
  - id: EP-1
    method: GET
    path: /api/settings
    change: changed
    fields:
      - {name: today, type: string, nullable: false, format: "YYYY-MM-DD, calendar day in the hospital time zone", example: "2026-03-10", status: known}
    cites: ["context/generated/api-inventory.json#http:GET:/api/settings", "artifacts/HMS-40/run_01M8BRYBC8NH9QF7E248WN3MCN/req.md#item:REQ-1"]
---
# Design: HMS-40

## Overview
The existing settings handler also returns `today`, computed on each request as `localDay(clock(), timeZone)` from the application context's clock and the configured hospital time zone.
- Trade-off: using the context `clock` instead of `new Date()` lets tests fix the instant, at no cost in production.
- Trade-off: `today` is computed per request, so a page that stays open past hospital midnight keeps the old value until it reloads settings.
- Trade-off: the web `Settings` type is left unchanged because no web page uses `today` yet; the next web change adds it.

## Components
- CO-1, changed: settings handler in `api/src/routes/health.ts` adds `today`. The other response fields and the health handler stay as they are.
- CO-2, existing: `localDay` in `api/src/lib/time.ts`, reused as is.
- EP-1, changed: GET /api/settings (the ticket's `GET /settings`). New field `today`: string, not null, `YYYY-MM-DD` in the hospital time zone, example `2026-03-10`.
- S-1 is served by CO-1, CO-2 and EP-1.

## Known
- S-1: every criterion is met by the changed handler and the existing helper.

## Assumed
None

## Unknown
None

## Open questions
None
