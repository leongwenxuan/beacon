# Story 1.2: Create a Persisted Search Session from a Rental Brief

Status: ready-for-dev

## Story

As a renter,
I want to submit a rental brief and get a persisted Beacon session,
So that my search can begin and continue across refreshes or revisits.

## Acceptance Criteria

1. **Given** a renter enters a natural-language brief with location, budget, and preferences **When** they submit the form **Then** Beacon creates a `searchSession` record in Convex **And** stores the original brief plus normalized search-intent fields needed for downstream work.

2. **Given** a session is created **When** submission succeeds **Then** the renter is routed to a session-specific page using a stable `sessionId` **And** the session starts in an explicit persisted workflow state such as `planning` or `discovering`.

3. **Given** the brief is incomplete or malformed **When** the renter submits it **Then** Beacon returns actionable validation feedback **And** no invalid session record is created.

## Tasks / Subtasks

- [ ] Define the initial Convex schema for `searchSessions` with canonical fields for the original rental brief, normalized search intent, workflow status, and timestamps.
- [ ] Add Convex validators so the session write boundary rejects malformed brief payloads before they become canonical state.
- [ ] Implement `searchSessions.create` as the canonical mutation for session creation.
- [ ] Implement `searchSessions.getById` as the canonical query for the session route and later live-progress reads.
- [ ] Wire the renter brief form from the search entry surface into the session-creation mutation and redirect to `src/app/search/[sessionId]/` on success.
- [ ] Persist a stable workflow status such as `planning` at creation time so later orchestrator stages can advance from canonical state.
- [ ] Store normalized search-intent fields that downstream discovery planning can consume without reparsing raw UI copy every time.
- [ ] Return renter-safe validation messages for malformed or incomplete briefs and ensure failed submissions do not insert partial/invalid rows.
- [ ] Keep session creation logic narrow and synchronous; do not start TinyFish or `fal.ai` work in this story.

## Dev Notes

- Depends on Story `1.1`, which established the starter app shell, Convex wiring, and the route boundaries this story now activates.
- This story creates the session-centric backbone used by every later workflow stage: discovery, shortlisting, enrichment, recommendation, visuals, and operator inspection.
- Reuse the route/file conventions established in Story `1.1` instead of inventing new search/session boundaries.
- Previous story intelligence: keep the architecture folder split from `1.1` intact, especially `src/features/search` for renter flow UI and `convex/` for canonical backend state.

### Architecture Guardrails

- Use `Convex 1.34.0` as the canonical backend, state store, workflow persistence layer, and real-time sync layer.
- Convex must remain the single source of truth for workflow state; frontend code must derive session state from Convex rather than duplicate it in client stores.
- Use explicit workflow statuses from the architecture vocabulary such as `planning`, `discovering`, `shortlisting`, `enriching`, `synthesizing`, `generatingVisuals`, `ready`, `partialReady`, and `failed`.
- Use camelCase field names and typed validators for Convex documents and function boundaries.
- Keep provider integrations and credentials server-side only; TinyFish and `fal.ai` are not part of this story.
- Store timestamps as canonical backend values and format them only at the UI layer.

### File Structure Notes

- `convex/schema.ts`
- `convex/searchSessions/queries.ts`
- `convex/searchSessions/mutations.ts`
- `convex/searchSessions/helpers.ts`
- `src/app/search/page.tsx`
- `src/app/search/[sessionId]/page.tsx`
- `src/features/search/`
- `src/types/search.ts`

### Testing Requirements

- Verify valid brief submission creates exactly one `searchSessions` row and redirects to the matching `sessionId` route.
- Verify a refresh or revisit of the created session route loads the persisted session via `searchSessions.getById`.
- Verify malformed or incomplete briefs do not create a Convex session row.
- Verify the initial persisted status is explicit and readable by later session UI code.
- Run lint/typecheck after adding schema, mutations, queries, and route wiring.

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
