---
schema: context-section.v1
section: test-strategy
owner: eng-db6b
origin: human
status: reviewed
verified_at: {sample-system: 7cf61b938808101b7195cc80e8da2238ac370a23}
---

# Test strategy {#test-strategy}

## Layers {#layers}
API tests use Vitest and Supertest against a throwaway PostgreSQL 16. Web tests use Vitest
with jsdom and Testing Library. The db workspace tests the seed and the migrations.
Sources: sample-system:api/vitest.config.mts#L10, sample-system:web/vitest.config.mts#L10

## How to run {#how-to-run}
`npm test` at the repository root runs every workspace.
Sources: sample-system:package.json#L19

## Gaps {#gaps}
The suite covers the main paths only; line coverage is partial.
