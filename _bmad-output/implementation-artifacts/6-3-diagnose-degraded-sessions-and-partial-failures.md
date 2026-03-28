# Story 6.3: Diagnose Degraded Sessions and Partial Failures

Status: ready-for-dev

## Story

As an operator,
I want Beacon to expose degradation and partial-failure details clearly,
So that I can recover from issues and trust the system in demos and live runs.

## Acceptance Criteria

1. **Given** a session experiences provider errors, partial retrieval failures, or incomplete downstream work **When** the operator inspects the session **Then** Beacon exposes failure state through persisted workflow and activity records **And** shows which stage or dependency degraded without requiring raw provider logs in the renter UI.

2. **Given** useful partial outputs still exist **When** the operator reviews a degraded session **Then** they can confirm what was successfully preserved versus what failed **And** understand whether the renter-facing flow should still be able to deliver a usable partial result.

3. **Given** troubleshooting surfaces are used during demos or testing **When** issues occur mid-run **Then** the operator can quickly inspect session state, candidate state, and artifact state in one coherent model **And** use that visibility to diagnose rather than guess at system behavior.

## Tasks / Subtasks

- [ ] Surface degraded-session and partial-failure detail from canonical `workerRuns`, `activityLogs`, session state, candidate state, and artifact state.
- [ ] Show which workflow stage or dependency degraded instead of only a generic failed/partial status.
- [ ] Preserve visibility into what candidate, evidence, recommendation, and visual outputs survived after partial failure.
- [ ] Add operator-friendly drill-down into workflow/activity state so troubleshooting does not require raw provider logs in renter surfaces.
- [ ] Keep degraded-session diagnosis grounded in canonical persisted state rather than ad hoc client inference.
- [ ] Make it clear whether the renter-facing flow should still be able to deliver a usable partial result.
- [ ] Support fast diagnosis during demos or tests by keeping session, candidate, evidence, artifact, and failure state connected in one model.
- [ ] Preserve renter-safe messaging boundaries while allowing richer operator diagnosis internally.

## Dev Notes

- Depends on Story `6.2`, which established operator session-detail inspection across sessions, candidates, evidence, recommendations, and artifacts.
- This story is the final observability/troubleshooting layer for the MVP and closes the loop on graceful degradation.
- Previous story intelligence: preserve the operator session-detail relationships from `6.2` and enrich them with explicit degradation/failure interpretation rather than rebuilding a second debug model.
- The goal is not to dump raw provider internals; it is to make Beacon’s persisted workflow state diagnosable enough to recover and trust in demos.

### Architecture Guardrails

- Operators can identify when a session has partially failed or degraded.
- Explicit observability through Convex-backed `workerRuns` and `activityLogs` is required so partial failures and recoveries are inspectable.
- Partial failure should be represented in state, not hidden behind generic request failures.
- Operator tooling depends on the same state model used for renter-facing progress, so observability cannot be bolted on later.
- User-facing messages should describe impact, while richer diagnostic detail remains internal/operator-only.
- Convex remains the single source of truth for degraded state, preserved outputs, and workflow lineage.

### File Structure Notes

- `src/app/operator/sessions/[sessionId]/page.tsx`
- `src/app/operator/worker-runs/[workerRunId]/page.tsx`
- `src/features/operator/components/`
- `src/features/operator/hooks/`
- `convex/workerRuns/queries.ts`
- `convex/activityLogs/queries.ts`
- `convex/searchSessions/queries.ts`
- `convex/unitRecords/queries.ts`
- `convex/imageArtifacts/queries.ts`
- `src/types/operator.ts`

### Testing Requirements

- Verify degraded sessions expose workflow/activity detail that identifies the failed or degraded stage.
- Verify operator views can distinguish preserved partial outputs from failed outputs.
- Verify operator tooling can assess whether renter-facing partial-ready output should still be usable.
- Verify richer diagnostics remain operator-only and do not leak into renter-facing routes.
- Run lint/typecheck after degraded-session diagnosis updates.

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
