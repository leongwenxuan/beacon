# Story 1.1: Set Up Initial Project from Starter Template and Search Entry

Status: ready-for-dev

## Story

As a renter,
I want Beacon to load a working search entry experience,
So that I can start a rental search from a stable app shell.

## Acceptance Criteria

1. **Given** the repo is initialized for Beacon **When** the application is created **Then** it uses the approved `Next.js App Router + Convex Quickstart` foundation **And** Convex is connected as the backend state layer for the app.

2. **Given** a renter visits the app **When** the root or search route loads **Then** they see a desktop-first Beacon app shell with a natural-language rental brief input surface **And** the page is keyboard accessible with semantic labels and status regions.

3. **Given** the initial app shell is implemented **When** the codebase is structured **Then** route boundaries exist for renter search and session views **And** no provider-specific retrieval or visual generation logic is exposed in client code.

## Tasks / Subtasks

- [ ] Initialize the project from the approved starter path: `npx create-next-app@latest beaconmain --ts --app --tailwind --eslint --src-dir`, then add `convex` and run local Convex bootstrap.
- [ ] Verify the repo is using the architecture-approved stack foundation: Next.js App Router, TypeScript, Tailwind, ESLint, and Convex as the canonical backend/state layer.
- [ ] Build the first renter-facing app shell at `src/app/page.tsx` and/or `src/app/search/page.tsx` with Beacon branding and a natural-language rental brief entry surface.
- [ ] Scaffold route boundaries for `src/app/search/[sessionId]/` and `src/app/recommendation/[sessionId]/` so session and recommendation flows have clear App Router ownership.
- [ ] Keep provider integrations out of client-rendered code; no TinyFish or `fal.ai` logic should appear in `src/app`, `src/features`, or client components in this story.
- [ ] Add semantic labels, heading structure, keyboard-friendly focus order, and a status region/live region placeholder for the search entry surface.
- [ ] Preserve the architecture folder strategy by keeping shared UI in `src/components`, search-specific UI in `src/features/search`, and backend logic in `convex/`.

## Dev Notes

- This is the first implementation story in the architecture sequence and should land before schema-heavy workflow work.
- It establishes the baseline app shell and route boundaries that later stories will extend for session persistence, live progress, recommendation, and operator tooling.
- No previous story exists in this epic; this story defines the baseline patterns that Story `1.2` and beyond should reuse.

### Architecture Guardrails

- Use `Convex 1.34.0` as the canonical backend, state store, workflow persistence layer, and real-time sync layer.
- Use the selected starter foundation exactly: `Next.js App Router + Convex Quickstart`.
- Keep route separation aligned with the architecture: renter search, live session pages, recommendation pages, and internal operator surfaces.
- Treat Convex as the single source of truth for workflow/domain state from the beginning; do not add a duplicate client-side canonical store.
- Keep TinyFish and `fal.ai` server-side only when they are introduced in later stories.
- Follow the documented project structure under `src/app`, `src/components`, `src/features`, `src/lib`, `src/types`, and `convex/`.

### File Structure Notes

- `src/app/layout.tsx`
- `src/app/page.tsx`
- `src/app/search/page.tsx`
- `src/app/search/[sessionId]/page.tsx`
- `src/app/recommendation/[sessionId]/page.tsx`
- `src/components/`
- `src/features/search/`
- `convex/`

### Testing Requirements

- Smoke test that the app boots successfully with the starter stack and Convex wiring in place.
- Verify the root/search route renders the Beacon shell and natural-language brief input.
- Verify the first screen is keyboard accessible and uses semantic labels/status messaging.
- Run lint/typecheck after bootstrap so the starter baseline is clean before later stories build on it.

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
