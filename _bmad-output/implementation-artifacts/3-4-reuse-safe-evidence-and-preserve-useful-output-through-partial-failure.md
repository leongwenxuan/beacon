# Story 3.4: Reuse Safe Evidence and Preserve Useful Output Through Partial Failure

Status: ready-for-dev

## Story

As a renter,
I want Beacon to avoid wasting safe prior work and still return value when some retrieval paths fail,
So that the search remains useful under imperfect external conditions.

## Acceptance Criteria

1. **Given** cached evidence or prior artifacts exist for relevant entities **When** Beacon evaluates whether to reuse them **Then** it only reuses evidence that satisfies freshness and safety rules **And** records freshness metadata so stale information does not silently distort recommendations.

2. **Given** one or more enrichment or retrieval paths fail **When** the session continues **Then** Beacon records the degraded state in canonical workflow records **And** preserves already-completed discovery or enrichment outputs instead of discarding them.

3. **Given** a session cannot fully complete all intended enrichment work **When** downstream flows read the session state **Then** they can still access usable partial candidate outputs **And** the renter-facing flow remains capable of producing a partial-ready recommendation path.

## Tasks / Subtasks

- [ ] Define safe-reuse/freshness metadata for cached evidence and artifacts in canonical Convex state.
- [ ] Implement reuse rules that only allow prior evidence/artifacts to be reused when safety and freshness conditions are satisfied.
- [ ] Preserve already-completed discovery/enrichment outputs when a later retrieval or enrichment path fails.
- [ ] Persist degraded/partial-failure state in workflow records instead of discarding progress or masking the failure.
- [ ] Keep downstream candidate/recommendation readers able to access usable partial outputs even when full completion is not possible.
- [ ] Align reuse rules with the evidence/provenance/conflict-state model from earlier Epic 3 stories.
- [ ] Record enough workflow/activity metadata for operators to see what was reused, what failed, and what remained partial.
- [ ] Ensure partial-ready delivery remains possible without silently trusting stale or unsafe evidence.

## Dev Notes

- Depends on Story `3.3`, which established confidence-aware reconciliation and explicit uncertain candidate state.
- This story is the resilience layer that lets recommendation synthesis proceed under partial failure without lying about evidence quality.
- Previous story intelligence: reuse must preserve the evidence/provenance/conflict semantics already established in Epic 3 instead of flattening cached data into opaque truth.
- Safe reuse is a performance and resilience tool, not a shortcut around freshness or trust.

### Architecture Guardrails

- Caching is allowed only where reuse is safe and must include freshness metadata so stale evidence does not silently distort recommendations.
- Convex is the canonical cache index for retrieved evidence, normalized facts, worker outputs, and artifact references.
- Partial failure must be represented in canonical state, not hidden behind generic request failures.
- Preserve useful partial outputs rather than collapsing the session when one retrieval path fails.
- Recommendation flows should be able to operate on partial-ready state when there is enough trustworthy signal.
- Keep reuse/freshness rules in backend helpers and canonical workflow logic, not scattered through frontend code.

### File Structure Notes

- `convex/lib/evidence.ts`
- `convex/lib/provenance.ts`
- `convex/lib/status.ts`
- `convex/orchestrator/enrichment.ts`
- `convex/orchestrator/helpers.ts`
- `convex/workerRuns/mutations.ts`
- `convex/activityLogs/mutations.ts`
- `convex/searchSessions/mutations.ts`

### Testing Requirements

- Verify stale or unsafe evidence is not reused as canonical truth.
- Verify completed discovery/enrichment outputs remain available after a later retrieval path fails.
- Verify partial-ready/degraded session state remains readable by downstream workflows.
- Verify workflow/activity state records what was reused, what failed, and what partial outputs survive.
- Run lint/typecheck after reuse/failure-handling updates.

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
