# Story 2.1: Compile Source-Specific Discovery Plans and Explicit Target URLs

Status: ready-for-dev

## Story

As a renter,
I want Beacon to translate my search intent into source-specific discovery plans before browsing,
So that discovery starts from the most relevant search state instead of wasting time navigating manually.

## Acceptance Criteria

1. **Given** a valid `searchSession` exists with normalized search intent **When** discovery planning starts **Then** Beacon compiles a source-specific plan for each supported rental source **And** each plan records the source, mode, filter inputs, and strictness level to apply.

2. **Given** a source supports explicit query URLs or source-native filters **When** Beacon compiles that source plan **Then** it stores a stable `entryUrl` and a freshly generated `appliedUrl` or explicit browser action plan **And** avoids relying on stale pasted raw links as canonical discovery inputs.

3. **Given** discovery plans have been compiled **When** they are persisted **Then** Beacon creates a discovery-oriented `workerRun` or equivalent workflow record in Convex **And** advances the session into an explicit persisted state such as `discovering`.

## Tasks / Subtasks

- [ ] Build source-specific discovery-plan compilation on top of the normalized search intent created in Story `1.2`.
- [ ] Define the canonical plan shape in Convex, including `sourceId`, `entryUrl`, `appliedUrl`, `executionMode`, fallback order, filter inputs, and strictness metadata.
- [ ] Implement provider-aware plan builders that compile fresh discovery inputs from current session intent rather than replaying stale raw links.
- [ ] Persist discovery planning output into canonical Convex state so later execution can consume it without rebuilding search intent.
- [ ] Create a discovery `workerRuns` record tied to the session and the compiled source plans.
- [ ] Advance `searchSessions` into a persisted discovery-oriented state such as `discovering` once planning is complete.
- [ ] Add an activity-log/event record so operators can inspect when planning completed and what sources/modes were prepared.
- [ ] Keep plan compilation bounded to deterministic planning work only; do not run TinyFish execution in this story.
- [ ] Ensure plan generation is compatible with later struggle detection, telemetry persistence, and fallback execution stories.

## Dev Notes

- Depends on Story `1.4`, which finalized stable session continuity and ensures discovery planning can always reload from canonical session state.
- This story is the bridge from Epic 1 session setup into Epic 2 discovery orchestration.
- Previous story intelligence: keep all workflow state persisted in Convex and continue using the explicit status vocabulary already established in Epic 1.
- Discovery planning should be deterministic and inspectable so later execution and operator tooling can reason about exactly what Beacon attempted.

### Architecture Guardrails

- Convex remains the single source of truth for workflow state, discovery planning state, and worker lifecycle metadata.
- Discovery must prefer source-native filters, explicit search URLs, and direct landing pages before UI-driven browser traversal when a source supports them.
- Each source integration should store a stable `entryUrl` and compile a fresh `appliedUrl` or explicit browser plan from normalized renter intent at runtime.
- TinyFish browser navigation should be treated as a fallback or refinement path after direct-url attempts, not the default strategy for every run.
- Use camelCase field names and domain-oriented action/query/mutation naming.
- Keep provider-specific logic inside orchestration/integration boundaries, not in renter-facing UI code.

### File Structure Notes

- `convex/orchestrator/discovery.ts`
- `convex/workerRuns/mutations.ts`
- `convex/workerRuns/helpers.ts`
- `convex/searchSessions/mutations.ts`
- `convex/activityLogs/mutations.ts`
- `convex/integrations/tinyfish/`
- `convex/lib/validation.ts`
- `src/types/search.ts`

### Testing Requirements

- Verify the same normalized session intent deterministically produces the same per-source discovery plans.
- Verify compiled plans persist canonical `entryUrl`/`appliedUrl` or explicit browser-plan data for each supported source.
- Verify discovery planning creates a `workerRun` and updates the session to a persisted discovery-oriented state.
- Verify raw pasted links are not treated as canonical search truth when fresh provider-specific plans can be compiled.
- Run lint/typecheck after schema/helper/orchestrator updates.

### References

- `_bmad-output/planning-artifacts/epics.md`
- `_bmad-output/planning-artifacts/prd.md`
- `_bmad-output/planning-artifacts/architecture.md`

## Dev Agent Record

### Agent Model Used

_(fill on completion)_

### Debug Log References

_(fill on completion)_

### Completion Notes List

- _(empty)_

### File List

- _(empty)_
