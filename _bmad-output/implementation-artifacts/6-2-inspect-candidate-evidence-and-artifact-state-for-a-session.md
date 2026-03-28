# Story 6.2: Inspect Candidate, Evidence, and Artifact State for a Session

Status: ready-for-dev

## Story

As an operator,
I want to inspect the units, evidence, and artifacts attached to a Beacon session,
So that I can understand what the system actually retrieved, inferred, and persisted.

## Acceptance Criteria

1. **Given** an operator selects a session **When** they open its detail view **Then** Beacon shows shortlisted candidates and related canonical records for that session **And** preserves linkage between session, candidate, recommendation, and artifact state.

2. **Given** the session includes stored evidence or images **When** the operator inspects details **Then** they can view provenance-aware evidence summaries and artifact metadata **And** distinguish retrieved inputs from inferred outputs.

3. **Given** a recommendation artifact exists **When** the operator reviews its supporting state **Then** they can trace it back to the underlying units, evidence, and image artifacts **And** verify that canonical Convex records, not transient provider outputs, back the visible result.

## Tasks / Subtasks

- [ ] Build an operator session-detail view keyed by `sessionId`.
- [ ] Surface canonical session, candidate, evidence, recommendation, and artifact state in one inspectable flow.
- [ ] Preserve traceable linkage between session records, shortlisted candidates, recommendation artifacts, and image artifacts.
- [ ] Show provenance-aware evidence summaries that distinguish retrieved inputs from inferred outputs and unknown gaps.
- [ ] Surface image-artifact metadata and recommendation-artifact lineage from canonical Convex-backed records.
- [ ] Keep operator detail reads grounded in the same backend state used by renter-facing workflows.
- [ ] Maintain operator-only access boundaries while exposing richer diagnostics than renter surfaces.
- [ ] Prepare the detail view to support degraded-session diagnosis and worker-run drill-down in Story `6.3`.

## Dev Notes

- Depends on Story `6.1`, which introduced the protected operator overview and route boundary.
- This story turns the overview from a status board into a true inspectable session/state debugger.
- Previous story intelligence: preserve the canonical linkage built across earlier epics so operator inspection follows real persisted relationships instead of reconstructed snapshots.
- Operators should be able to answer “what does Beacon know, what did it infer, and what artifact backs this UI?” from this view.

### Architecture Guardrails

- Operators can inspect the state of a search session and its shortlisted candidates.
- Operators can inspect generated artifacts and stored evidence associated with a session.
- Convex owns canonical workflow state, candidate records, recommendation artifacts, and image artifacts.
- Provider payloads must already be mapped into internal domain models before operator inspection reads them.
- Keep operator inspection under protected internal routes and do not expose these richer details to renter-facing routes.
- Distinguish retrieved, inferred, and unknown data explicitly in both stored state and operator presentation.

### File Structure Notes

- `src/app/operator/sessions/[sessionId]/page.tsx`
- `src/features/operator/components/`
- `src/features/operator/hooks/`
- `convex/searchSessions/queries.ts`
- `convex/unitRecords/queries.ts`
- `convex/recommendationArtifacts/queries.ts`
- `convex/imageArtifacts/queries.ts`
- `convex/activityLogs/queries.ts`
- `src/types/operator.ts`

### Testing Requirements

- Verify an operator can inspect canonical session, candidate, recommendation, and artifact state from one session detail view.
- Verify evidence summaries distinguish retrieved, inferred, and unknown state.
- Verify recommendation artifacts remain traceable back to supporting candidate/evidence/artifact records.
- Verify operator detail reads stay protected behind the operator-only access model.
- Run lint/typecheck after operator session-detail and query updates.

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
