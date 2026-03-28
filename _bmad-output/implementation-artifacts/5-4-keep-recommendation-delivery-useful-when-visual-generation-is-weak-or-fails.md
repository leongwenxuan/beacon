# Story 5.4: Keep Recommendation Delivery Useful When Visual Generation Is Weak or Fails

Status: ready-for-dev

## Story

As a renter,
I want Beacon to remain useful even if visual evidence is sparse or inferred generation is unsuccessful,
So that the recommendation does not depend on a perfect image-generation outcome.

## Acceptance Criteria

1. **Given** visual evidence is weak or generation confidence is low **When** Beacon evaluates whether to show an inferred preview **Then** it can still proceed with the recommendation flow without blocking on a perfect visual result **And** records the visual limitation explicitly in session or recommendation state.

2. **Given** visual generation fails for a session **When** the final recommendation is rendered **Then** Beacon still returns the recommendation artifact and any retrieved visual evidence that exists **And** avoids turning a visual-generation failure into a full recommendation failure.

3. **Given** a generated visual is unavailable or partial **When** the renter reviews the output **Then** the interface remains coherent and actionable **And** preserves the evidence and recommendation context already available.

## Tasks / Subtasks

- [ ] Persist explicit visual-generation status/limitation state in canonical session and/or recommendation records.
- [ ] Ensure recommendation delivery can complete even when inferred-visual generation is weak, partial, or failed.
- [ ] Preserve retrieved/source imagery and recommendation context when generated visuals are unavailable.
- [ ] Keep the recommendation UI coherent and actionable without broken visual sections or dead-end failure screens.
- [ ] Record visual-generation failure or weakness in workflow/activity state for later operator inspection.
- [ ] Distinguish visual failure from recommendation failure so Beacon can remain useful under imperfect conditions.
- [ ] Keep visual-degradation messaging honest and renter-safe without exposing raw provider diagnostics.
- [ ] Align degraded visual behavior with earlier partial-ready and graceful-degradation semantics from prior epics.

## Dev Notes

- Depends on Story `5.3`, which introduced renter-facing inferred-visual presentation with clear labeling and evidence context.
- This story is the graceful-degradation layer for visuals and should protect the recommendation experience from a single weak provider step.
- Previous story intelligence: keep inferred-vs-retrieved distinctions intact even when generated visuals are missing or partial.
- The recommendation is the core product output; visuals are important but must not become a single point of failure.

### Architecture Guardrails

- The system can produce a recommendation even when visual evidence is weak or incomplete.
- Partial failure must be represented in canonical state rather than collapsing the workflow into a generic error.
- Retrieved and generated images should both remain linked through canonical artifact state so the UI can fall back cleanly.
- Recommendation output must remain usable under degraded conditions and partial-ready states.
- Keep provider-specific failure details server-side and expose only renter-safe impact messaging in the final UI.
- Convex remains the source of truth for session status, recommendation status, artifact linkage, and degraded-state communication.

### File Structure Notes

- `src/app/recommendation/[sessionId]/page.tsx`
- `src/features/recommendation/components/`
- `src/features/visuals/components/`
- `convex/orchestrator/visuals.ts`
- `convex/workerRuns/mutations.ts`
- `convex/activityLogs/mutations.ts`
- `convex/searchSessions/mutations.ts`
- `convex/recommendationArtifacts/mutations.ts`

### Testing Requirements

- Verify recommendation output still renders when inferred-visual generation fails.
- Verify retrieved/source imagery remains available when generated visuals are missing.
- Verify visual-generation weakness/failure is represented explicitly in canonical workflow or recommendation state.
- Verify the final recommendation UI remains coherent and actionable without broken visual sections.
- Run lint/typecheck after degraded visual-flow updates.

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
