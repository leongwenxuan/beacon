# Story 1.4: Preserve Session Continuity and Revisit In-Progress Work

Status: ready-for-dev

## Story

As a renter,
I want to revisit an active Beacon session and keep any partial progress,
So that long-running search work remains usable across refreshes and later returns.

## Acceptance Criteria

1. **Given** a renter refreshes or reopens a valid session URL **When** the session page loads again **Then** Beacon restores the latest persisted session state from Convex **And** does not create a duplicate session.

2. **Given** partial progress exists for the session **When** the renter revisits it **Then** the page shows the latest available progress, counts, or partial outputs already persisted **And** preserves continuity between in-progress work and later recommendation output.

3. **Given** a session has degraded or only partially completed **When** the renter views it **Then** Beacon surfaces the current state honestly **And** keeps whatever useful partial progress is available instead of collapsing to a generic failure screen.

## Tasks / Subtasks

- [ ] Ensure the session route always resolves from persisted `sessionId` state in Convex and never creates a new session during refresh/revisit flows.
- [ ] Reuse `searchSessions.getById` (or the canonical session live query) as the single read path for revisit behavior.
- [ ] Render the latest persisted workflow status, timestamps, progress metadata, and any already-available partial outputs when the renter returns to a session.
- [ ] Preserve continuity between the live-progress route and the later recommendation route by keeping session-derived state stable across revisit flows.
- [ ] Distinguish valid-but-incomplete sessions from invalid/not-found sessions so renters do not see a false failure state.
- [ ] Add explicit degraded/partial messaging for states such as `partialReady` or `failed` while still rendering whatever useful persisted context exists.
- [ ] Ensure the session route can recover cleanly after browser refresh, new tab open, or later revisit without depending on transient client memory.
- [ ] Keep renter-facing failure messaging product-safe and impact-oriented; raw provider or operator diagnostics must stay out of this surface.
- [ ] Keep the revisit flow compatible with future stories that will add shortlist snapshots, evidence quality, recommendation artifacts, and visual artifacts.

## Dev Notes

- Depends on Story `1.3`, which established the reactive session page and persisted live-status display.
- This story is the continuity layer for Epic 1: it proves Beacon is session-centric rather than a one-shot submit-and-forget experience.
- Previous story intelligence: preserve the same reactive Convex query model from `1.3` and keep the session route grounded in canonical backend state instead of ephemeral client memory.
- Do not solve degraded-state handling with a generic catch-all error page; the PRD explicitly requires partial value and graceful continuity.

### Architecture Guardrails

- Use `Convex 1.34.0` as the canonical backend, state store, workflow persistence layer, and real-time sync layer.
- Convex remains the single source of truth for workflow state and revisit continuity.
- The frontend must derive continuity state from Convex records, not from local-only caches or a duplicate global store.
- Workflow state must continue using explicit statuses such as `planning`, `discovering`, `shortlisting`, `enriching`, `synthesizing`, `generatingVisuals`, `ready`, `partialReady`, and `failed`.
- Partial data should render whenever safe instead of blocking the full page.
- User-facing messages should describe impact and continuity, not expose raw provider or operator diagnostics.
- Route separation between live search/session pages and recommendation pages must remain intact.

### File Structure Notes

- `src/app/search/[sessionId]/page.tsx`
- `src/app/search/[sessionId]/loading.tsx`
- `src/app/search/[sessionId]/error.tsx`
- `src/app/recommendation/[sessionId]/page.tsx`
- `src/features/search/components/`
- `src/features/search/hooks/`
- `convex/searchSessions/queries.ts`
- `convex/searchSessions/helpers.ts`
- `src/types/search.ts`

### Testing Requirements

- Verify refresh and revisit of a valid session URL reload the same persisted session instead of creating a duplicate record.
- Verify already-persisted progress or partial output remains visible after refresh and later revisit.
- Verify degraded or partially completed sessions show honest, non-empty continuity messaging.
- Verify invalid or missing session ids produce a controlled not-found state distinct from incomplete-but-valid sessions.
- Run lint/typecheck after continuity/revisit handling is added.

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
