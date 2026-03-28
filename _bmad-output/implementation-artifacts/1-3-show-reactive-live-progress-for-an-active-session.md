# Story 1.3: Show Reactive Live Progress for an Active Session

Status: ready-for-dev

## Story

As a renter,
I want to see Beacon actively progressing through my search session,
So that I trust the system is doing real work before the final recommendation is ready.

## Acceptance Criteria

1. **Given** a valid session exists **When** the renter opens the session page **Then** the UI reads session state reactively from Convex **And** displays the current workflow status using explicit persisted labels such as `planning`, `discovering`, `shortlisting`, `enriching`, or `partialReady`.

2. **Given** the session state changes in Convex **When** status, timestamps, or progress metadata update **Then** the session page reflects the new state without a full reload **And** keeps the interface responsive during long-running work.

3. **Given** progress is still early or incomplete **When** there is not yet a final recommendation **Then** the UI shows meaningful progress messaging rather than a dead-end blank state **And** exposes accessible status updates for assistive technology.

## Tasks / Subtasks

- [ ] Extend the session read model so `searchSessions.getById` (or a session-specific live query) returns the persisted workflow status plus any lightweight progress metadata needed by the session UI.
- [ ] Build the live session surface in `src/app/search/[sessionId]/page.tsx` and/or `src/features/search/` so it subscribes reactively to Convex session state.
- [ ] Render explicit persisted statuses from canonical state instead of client-derived fake progress.
- [ ] Add meaningful in-progress messaging for early states like `planning` and `discovering` so the renter sees active work before recommendation output exists.
- [ ] Keep the interface responsive while state updates stream in; long-running orchestration should not block the route shell or freeze controls.
- [ ] Add an accessible status region or live region so workflow-state changes are announced to assistive technology.
- [ ] Support loading, not-found, and error states for the session route without collapsing valid partial progress.
- [ ] Keep the live-progress UI compatible with later stories that will add shortlist counts, evidence quality, recommendation readiness, and degraded states.
- [ ] Do not add a separate global client store for canonical session progress; derive it from Convex queries.

## Dev Notes

- Depends on Story `1.2`, which introduced the canonical `searchSessions` schema, `searchSessions.create`, and `searchSessions.getById`.
- This story turns the persisted session model from `1.2` into a reactive renter-facing experience and establishes the live-status pattern that later discovery/enrichment stories will populate.
- Previous story intelligence: preserve the same session route ownership from `1.2` and continue treating Convex as the only source of workflow truth.
- At this stage, the underlying workflow can still be simple or partially stubbed, but the page must be built around persisted Convex state rather than temporary client-only timers or mock state.

### Architecture Guardrails

- Use `Convex 1.34.0` as the canonical backend, state store, workflow persistence layer, and real-time sync layer.
- Convex reactive queries are the primary application state mechanism for live session views.
- Workflow state must use explicit statuses such as `planning`, `discovering`, `shortlisting`, `enriching`, `synthesizing`, `generatingVisuals`, `ready`, `partialReady`, and `failed`.
- Long-running work should be represented through persisted status transitions, not hidden async side effects or opaque spinners only.
- The UI must stay responsive while orchestration continues asynchronously in the backend.
- Keep TinyFish and `fal.ai` logic server-side; this story is about reading and presenting canonical progress, not calling providers from the client.
- Partial data should render whenever safe instead of blocking the entire page.

### File Structure Notes

- `src/app/search/[sessionId]/page.tsx`
- `src/app/search/[sessionId]/loading.tsx`
- `src/app/search/[sessionId]/error.tsx`
- `src/features/search/components/`
- `src/features/search/hooks/`
- `convex/searchSessions/queries.ts`
- `convex/searchSessions/helpers.ts`
- `src/types/search.ts`

### Testing Requirements

- Verify the session page reacts to persisted Convex status changes without a full page reload.
- Verify valid early/incomplete states render meaningful progress messaging instead of an empty shell.
- Verify the status region/live region exposes workflow updates to assistive technology.
- Verify loading and not-found states are controlled and do not create duplicate session records.
- Run lint/typecheck after adding the live-progress route and reactive query wiring.

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
