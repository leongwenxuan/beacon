# Story 2.2: Execute Direct-URL-First Discovery with Struggle Detection and Fallback

Status: ready-for-dev

## Story

As a renter,
I want Beacon to start discovery from explicit source URLs and recover quickly if browser navigation is struggling,
So that relevant candidate properties are gathered faster.

## Acceptance Criteria

1. **Given** a persisted source plan includes an `appliedUrl` **When** discovery executes for that source **Then** Beacon attempts the direct URL or explicit landing path before any broad UI traversal **And** invokes `TinyFish` only through server-side Convex actions.

2. **Given** TinyFish takes too many steps, exceeds a time budget, repeats failed actions, or does not reach the expected page state **When** Beacon evaluates discovery telemetry **Then** the run is marked as navigation struggle or degraded execution **And** Beacon switches to the next fallback strategy such as UI-assisted filters or broader retrieval.

3. **Given** an external source is partially unavailable or one discovery strategy fails **When** fallback evaluation completes **Then** Beacon records the degraded outcome in persisted workflow state **And** continues the session where partial discovery is still possible instead of blocking the full source pass.

## Tasks / Subtasks

- [ ] Implement direct-url-first discovery execution using the persisted source plan from Story `2.1`.
- [ ] Call TinyFish only from server-side Convex actions and pass explicit `appliedUrl`, `entryUrl`, or structured browser-plan inputs rather than vague browsing goals.
- [ ] Capture execution telemetry needed for struggle detection, including time budget, step count, repeated failures, and expected-page-state checks.
- [ ] Define typed struggle/degraded conditions in canonical workflow state instead of treating them as generic exceptions.
- [ ] Implement fallback sequencing so discovery can switch from direct URL to UI-assisted filtering or broader retrieval when needed.
- [ ] Persist per-source degraded outcomes to `workerRuns`, session state, and operator-visible activity logs.
- [ ] Ensure a single source failure does not collapse the entire session when other discovery paths can continue.
- [ ] Keep this story focused on execution/fallback mechanics; candidate persistence belongs to Story `2.3`.
- [ ] Keep execution outputs structured so later telemetry inspection and ranking stories can consume them deterministically.

## Dev Notes

- Depends on Story `2.1`, which persisted the source-specific discovery plans and explicit target URLs.
- This story operationalizes the architecture rule that direct URLs and source-native filters come before browser wandering.
- Previous story intelligence: preserve the exact plan shape from `2.1` so fallback behavior, telemetry, and operator observability all reason over the same persisted inputs.
- Treat struggle detection as a first-class workflow concept rather than an implementation detail hidden inside integration code.

### Architecture Guardrails

- TinyFish runs only through server-side Convex actions and integration boundaries.
- Discovery must attempt direct URLs or explicit landing paths before broad UI traversal when a provider supports them.
- Browser struggle should be detected using persisted telemetry such as step count, repeated actions, time budget, and expected-page-state checks.
- Degraded or partial failure must be represented in canonical state, not hidden behind uncaught request errors.
- Session progress should continue where partial discovery is still possible rather than collapsing to total failure.
- Keep provider integrations and credentials out of client bundles and renter-facing UI code.

### File Structure Notes

- `convex/orchestrator/discovery.ts`
- `convex/integrations/tinyfish/actions.ts`
- `convex/integrations/tinyfish/mappers.ts`
- `convex/integrations/tinyfish/types.ts`
- `convex/workerRuns/mutations.ts`
- `convex/activityLogs/mutations.ts`
- `src/app/api/tinyfish/webhook/route.ts`
- `convex/lib/status.ts`

### Testing Requirements

- Verify a supported source attempts `appliedUrl`/direct-url execution before fallback traversal.
- Verify struggle thresholds trigger degraded state and advance to the next fallback strategy.
- Verify partial discovery continues when one source or execution path fails.
- Verify TinyFish execution is only reachable through server-side Convex or API integration boundaries.
- Run lint/typecheck after discovery execution and fallback handling changes.

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
