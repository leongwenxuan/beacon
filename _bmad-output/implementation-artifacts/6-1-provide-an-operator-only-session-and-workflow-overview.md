# Story 6.1: Provide an Operator-Only Session and Workflow Overview

Status: ready-for-dev

## Story

As an operator,
I want a protected internal view of Beacon sessions and workflow runs,
So that I can monitor whether discovery, enrichment, synthesis, and visuals are functioning correctly.

## Acceptance Criteria

1. **Given** operator surfaces exist in the application **When** an internal user accesses them **Then** Beacon restricts access using the MVP operator-only access model **And** renter-facing routes remain separate from internal troubleshooting views.

2. **Given** an operator opens the overview surface **When** sessions and worker runs are listed **Then** they can inspect current and recent sessions with persisted statuses **And** see enough high-level state to identify which stage a session is currently in.

3. **Given** workflow execution degrades or stalls **When** the overview is refreshed reactively or revisited **Then** the operator can see that the session is partial, failed, or delayed **And** can differentiate workflow-stage problems instead of seeing a generic failure bucket.

## Tasks / Subtasks

- [ ] Implement operator-only route protection for internal troubleshooting surfaces.
- [ ] Build an operator overview route that lists current/recent sessions from canonical Convex state.
- [ ] Surface session workflow status, timestamps, and high-level stage information in the overview.
- [ ] Include correlated worker-run summaries so operators can see what stage is currently active or degraded.
- [ ] Keep renter-facing routes and operator routes clearly separated at the route and auth boundary.
- [ ] Make the overview reactive to Convex state so operators can revisit/refresh without losing live workflow visibility.
- [ ] Add degraded/stalled-state visibility that differentiates workflow-stage issues instead of collapsing everything into a generic error.
- [ ] Keep the overview query/model aligned with the same canonical session/worker state used elsewhere in the app.

## Dev Notes

- Depends on Story `5.4`, which completed the graceful-degradation model across recommendation and visual-generation workflows.
- This story starts the operator-only observability layer promised by the PRD and architecture.
- Previous story intelligence: reuse the same persisted session and worker-run state from earlier epics instead of inventing a separate operator-only backend model.
- The overview should answer “what stage is this session in and is it healthy?” quickly without forcing operators into deep detail pages first.

### Architecture Guardrails

- MVP security must follow an operator-only internal access model for observability and troubleshooting.
- Route separation is required between renter search/recommendation surfaces and internal operator surfaces.
- Explicit observability through Convex-backed `workerRuns` and `activityLogs` is required so partial failures and recoveries are inspectable.
- Operator views should inspect the same canonical persisted state used by renter-facing workflows.
- Keep authorization at the route/server boundary and avoid exposing operator-only state on renter-facing surfaces.
- Convex remains the single source of truth for session and workflow inspection.

### File Structure Notes

- `src/app/operator/page.tsx`
- `src/app/operator/layout.tsx`
- `src/features/operator/components/`
- `src/features/operator/hooks/`
- `src/lib/`
- `src/middleware.ts`
- `convex/searchSessions/queries.ts`
- `convex/workerRuns/queries.ts`
- `convex/activityLogs/queries.ts`

### Testing Requirements

- Verify operator routes are protected and renter-facing routes remain separate.
- Verify the overview shows session status and high-level workflow-stage visibility from canonical state.
- Verify degraded, delayed, or partial sessions are distinguishable from healthy sessions.
- Verify operator overview reads update reactively or correctly on revisit/refresh.
- Run lint/typecheck after operator-route and query changes.

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
