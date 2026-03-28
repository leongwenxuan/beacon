---
stepsCompleted:
  - 1
  - 2
  - 3
  - 4
  - 5
  - 6
  - 7
  - 8
inputDocuments:
  - /Users/leongwenxuan/Desktop/beaconmain/_bmad-output/planning-artifacts/prd.md
  - /Users/leongwenxuan/Desktop/beaconmain/idea.md
  - /Users/leongwenxuan/Desktop/beaconmain/beacon_orchestrator_technical_plan.md
workflowType: 'architecture'
project_name: 'beaconmain'
user_name: 'Leongwenxuan'
date: '2026-03-27'
lastStep: 8
status: 'complete'
completedAt: '2026-03-27'
---

# Architecture Decision Document

_This document builds collaboratively through step-by-step discovery. Sections are appended as we work through each architectural decision together._

## Project Context Analysis

### Requirements Overview

**Functional Requirements:**
Beacon must support a complete renter-facing decision workflow rather than isolated search features. Architecturally, the functional requirements imply distinct capability groups for query intake, search session management, multi-source discovery, candidate normalization and ranking, recursive evidence enrichment, recommendation synthesis, inferred visual generation, live progress feedback, and operator visibility. The requirement set also makes clear that the system must preserve session continuity across long-running work and support recommendation delivery even when some evidence paths fail.

The most architecture-shaping functional requirements are the need to discover properties from multiple sources, enrich incomplete listings with external context, reconcile conflicting facts, retain provenance, produce a top-three shortlist, generate a best recommendation, and optionally produce inferred visuals grounded in available evidence. These requirements imply a workflow-oriented architecture with explicit state transitions, separable worker responsibilities, and a source-of-truth data model that can track both completeness and uncertainty.

**Non-Functional Requirements:**
The dominant non-functional drivers are responsiveness, graceful degradation, persistence, provenance integrity, and integration resilience. The architecture must support an early progress signal within 60 seconds, final recommendation delivery within 5 minutes under normal conditions, and a responsive frontend while orchestration continues asynchronously. It must also protect stored session and artifact data, restrict operator access, support multiple concurrent sessions, preserve accessibility in core user flows, and tolerate unreliable external listing and evidence sources.

These NFRs mean the architecture cannot be designed as a simple request-response pipeline. It must support asynchronous processing, progressive updates, explicit failure states, recoverable partial completion, and durable storage of intermediate and final outputs.

**Scale & Complexity:**
Beacon is a high-complexity full-stack web application with AI orchestration at its core. Complexity comes from the interaction of several concerns rather than raw feature count alone: long-running background workflows, external retrieval dependencies, recursive enrichment, recommendation synthesis, artifact generation, and live frontend synchronization.

- Primary domain: full-stack proptech web application with orchestrated AI workflows
- Complexity level: high
- Estimated architectural components: 8-12 major components or subsystems

### Technical Constraints & Dependencies

The architecture must support the product constraints already established in the PRD and supporting documents. Beacon is desktop-first, session-oriented, and built around two connected product surfaces: a live scouting experience and a final recommendation artifact. The system must keep these surfaces synchronized through shared session state.

The architecture must also honor explicit tooling constraints:
- `Convex` is the required backend state and persistence layer.
- `TinyFish` is the required browser-based retrieval layer for listing and evidence collection.
- `fal.ai` is the required visual generation provider for inferred property imagery.

Additional constraints include:
- source data is incomplete, inconsistent, and externally controlled
- recommendation quality depends on evidence completeness rather than listing retrieval alone
- generated visuals must remain clearly distinguishable from retrieved facts
- stale cached evidence must not silently degrade recommendation quality
- live progress must be user-visible while backend workflows continue asynchronously
- discovery speed depends on using source-native filters and explicit landing URLs whenever available instead of making the browser agent rediscover site navigation every run
- browser-based retrieval must detect when navigation is struggling and switch to a more explicit or broader fallback strategy before wasting too much time budget

### Cross-Cutting Concerns Identified

Several concerns will affect nearly every architectural decision in the system.

First, state management is central. Search sessions, candidate units, worker runs, evidence records, recommendation artifacts, and generated images all need durable state and clear lifecycle transitions.

Second, provenance and confidence modeling are first-class concerns. The architecture must preserve where facts came from, how complete each unit is, what was inferred, and where uncertainty remains.

Third, orchestration boundaries matter. Discovery, recursive enrichment, recommendation synthesis, and visual generation are separable but interdependent workflows, so the architecture must support explicit handoffs and failure isolation between them.

Fourth, external dependency resilience is unavoidable. Listing portals and outside evidence sources will be unreliable or inconsistent, so the architecture must degrade gracefully and continue producing usable output when possible.

Fifth, frontend trust depends on backend transparency. The system must expose enough session progress and output lineage for users, demo viewers, and operators to believe the recommendation is the result of real work rather than a static result.

## Starter Template Evaluation

### Primary Technology Domain

Full-stack web application with real-time session updates and asynchronous AI workflow orchestration.

### Starter Options Considered

**Option 1: Next.js App Router + Convex Quickstart**
- Current command path verified through the official Next.js CLI and Convex quickstart documentation.
- Best fit for Beacon because it supports a full-stack web experience, route-based product surfaces, server/client composition, and Convex integration without forcing unrelated SaaS assumptions.
- Keeps the foundation clean while preserving flexibility for later `TinyFish` and `fal.ai` integrations.

**Option 2: Official Convex Next.js App Router Template**
- Viable and maintained.
- Good for rapid setup, but more opinionated as a starting point and less ideal if we want tighter control over folder structure, dependencies, and architecture decisions from day one.

**Option 3: Vite React TypeScript Starter + Convex**
- Strong for pure SPA development.
- Weaker fit for Beacon because the product benefits from a full-stack application structure, route-based surfaces, and a more opinionated production app shell.

### Selected Starter: Next.js App Router + Convex Quickstart

**Rationale for Selection:**
This starter combination gives Beacon the cleanest and most future-proof baseline. It establishes a modern TypeScript web application with routing, Tailwind, ESLint, and App Router conventions, while allowing Convex to become the source of truth for session state, candidate records, evidence, artifacts, and live updates. It avoids unnecessary assumptions about auth, UI libraries, or backend composition while remaining compatible with the required `TinyFish` retrieval layer and `fal.ai` visual generation pipeline.

**Initialization Command:**

```bash
npx create-next-app@latest beaconmain --ts --app --tailwind --eslint --src-dir && cd beaconmain && npm install convex && npx convex dev
```

**Architectural Decisions Provided by Starter:**

**Language & Runtime:**
TypeScript-based Next.js application with App Router as the primary application model.

**Styling Solution:**
Tailwind CSS as the default styling foundation for the web app shell and UI system.

**Build Tooling:**
Next.js build pipeline with App Router conventions and current default development tooling from `create-next-app`.

**Testing Framework:**
No heavy testing framework is forced by the starter itself, which keeps the foundation lightweight and allows testing architecture to be chosen deliberately later.

**Code Organization:**
A route-oriented application structure with `src/` directory organization, clear separation between app routes and backend `convex/` code, and room for architecture-specific domain modules to be layered in cleanly.

**Development Experience:**
Strong local development workflow with TypeScript, ESLint, hot reload, standard Next.js conventions, and Convex local development/bootstrap support.

**Additional Notes:**
- `Convex` should be initialized immediately after app creation so backend state and function organization exist from the start.
- `TinyFish` should be integrated later as an external worker/retrieval subsystem, not inside the starter boundary.
- `fal.ai` should be integrated later as the visual synthesis provider, also outside the starter boundary.
- A more opinionated starter was intentionally avoided to keep core architectural decisions explicit and traceable.

**Note:** Project initialization using this command should be the first implementation story.

## Core Architectural Decisions

### Decision Priority Analysis

**Critical Decisions (Block Implementation):**
- Use `Convex 1.34.0` as the canonical backend, state store, and workflow persistence layer.
- Use `Next.js 16.2` App Router as the primary web application framework.
- Use `TinyFish` as the browser-based retrieval subsystem for listing and evidence collection.
- Use `fal.ai` via `@fal-ai/client 1.9.5` for inferred visual generation.
- Use a session-centric architecture where all discovery, enrichment, synthesis, and visual generation state is persisted against a shared search session.
- Use asynchronous workflow execution with progressive frontend updates rather than a synchronous request-response pipeline.

**Important Decisions (Shape Architecture):**
- Use domain-oriented data modeling in Convex with explicit tables for sessions, candidates, worker runs, recommendation artifacts, and activity logs.
- Use Convex validators plus boundary validation at external integration edges.
- Use an operator-only security model for MVP, with no renter account system required initially.
- Use App Router server-first rendering with client-side islands for live session views and interactive recommendation surfaces.
- Use Vercel for web deployment and Convex managed deployment for backend execution and persistence.
- Use source-specific discovery adapters that compile normalized renter intent into explicit `entryUrl`, `appliedUrl`, filter plans, and fallback execution modes for each provider.
- Use direct-url-first discovery execution, with UI-driven TinyFish interaction reserved for refinement or fallback when explicit source URLs are insufficient.

**Deferred Decisions (Post-MVP):**
- End-user authentication and saved-user preference accounts.
- Fine-grained RBAC beyond operator-only internal access.
- Public API surface for third-party integrations.
- Advanced multi-region scaling and global caching strategy.
- More formalized analytics and product telemetry stack.

### Data Architecture

**Primary Data Store:**
Use `Convex 1.34.0` as the primary application database, backend function runtime, and real-time synchronization layer.

**Data Modeling Approach:**
Use a session-centered domain model with explicit records for:
- `searchSessions`
- `unitRecords`
- `workerRuns`
- `recommendationArtifacts`
- `imageArtifacts`
- `activityLogs`
- visual artifact metadata and storage references

This keeps the architecture aligned with Beacon's actual workflow model: one renter brief drives one long-running search session with evolving state and artifacts.

**Image Storage Strategy:**
Use Convex file storage as the binary store for all retrieved and generated images. Actual image bytes should be stored in Convex `_storage`, while application tables store metadata, provenance, and references to those files through `storageId` values.

Use `imageArtifacts` as the canonical metadata table for:
- listing gallery images retrieved through `TinyFish`
- floor plans and layout images
- exterior and building-context images
- `fal.ai` generated previews and room renders

Each image artifact record should include:
- `sessionId`
- optional `unitId`
- optional `recommendationArtifactId`
- `storageId`
- `kind`
- `sourceType`
- `provider`
- `originalUrl` when applicable
- mime or dimension metadata when available
- provenance and creation timestamps

This means Convex owns both the durable file pointer and the business metadata, while the actual blob lives in Convex storage.

**Validation Strategy:**
Use Convex schema validation and typed function boundaries as the primary validation mechanism for internal data integrity. Add boundary validation for external retrieval and generation payloads before they are merged into canonical session state.

**Migration Approach:**
Use additive schema evolution and backfill actions rather than a separate relational migration system. This matches Convex's operating model and keeps MVP iteration speed high.

**Caching Strategy:**
Use Convex as the canonical cache index for retrieved evidence, normalized facts, worker outputs, and artifact references. Cache only where reuse is safe, and attach freshness metadata so stale evidence does not silently distort future recommendations.

For images specifically:
- cache image metadata and provenance in `imageArtifacts`
- store retrieved or generated image bytes in Convex file storage
- generate access URLs from `storageId` at query time instead of storing permanent public URLs as canonical truth
- treat provider URLs as transient inputs, not durable source-of-truth artifacts

### Authentication & Security

**Authentication Method:**
Do not require renter accounts in MVP. Treat renter sessions as anonymous or session-scoped. Protect operator and troubleshooting surfaces separately.

**Authorization Pattern:**
Use an operator-only access model for internal observability, session inspection, and troubleshooting views. Public users should access only renter-facing search and recommendation flows.

**Security Middleware:**
Use Next.js middleware and server-side route protection for internal/operator paths. Keep internal workflows and provider credentials server-side only.

**Encryption Approach:**
Rely on platform-standard encryption in transit and at rest across Vercel, Convex, and integrated providers. Do not introduce custom cryptographic workflows unless a later requirement justifies them.

**API Security Strategy:**
Avoid exposing a broad public API surface in MVP. Prefer server-side application boundaries and authenticated internal service calls between the web app, Convex actions, TinyFish workflows, and fal.ai generation requests.

### API & Communication Patterns

**Primary Application Pattern:**
Use Next.js App Router plus Convex queries, mutations, and actions as the main application interface rather than designing around an external REST or GraphQL API.

**Communication Model:**
Use asynchronous workflow coordination through persisted Convex state and worker run records. Let the frontend consume session state and derived outputs rather than talking directly to retrieval or generation workers.

For discovery specifically, the orchestrator should compile source-specific plans before execution. Each plan should capture the source, `entryUrl`, `appliedUrl` when available, filter inputs, execution mode such as `directUrl`, `uiFilters`, or `broadBrowse`, and fallback order. TinyFish should start from the most explicit target possible rather than re-navigating from a generic homepage whenever a site supports direct search URLs or source-native filters.

**Error Handling Standard:**
Standardize on explicit status transitions and typed error states for sessions, unit records, and worker runs. Partial failure should be represented in state, not hidden behind generic request failures.

For discovery workflows, treat navigation struggle as a first-class typed condition rather than a generic failure. A run should be markable as degraded when it exceeds a step budget, time budget, repeated-action threshold, or fails to reach an expected page state despite execution.

**Rate Limiting Strategy:**
Apply lightweight throttling at renter query submission boundaries and stricter protection around internal/operator surfaces. External provider protection should be enforced through bounded worker concurrency and retry policy, not only frontend request limits.

**Service Communication:**
- Next.js frontend ↔ Convex for application state and live updates
- Convex actions ↔ TinyFish for retrieval jobs
- Convex actions ↔ fal.ai for visual generation jobs
- Convex remains the source of truth for all resulting state, artifacts, and status transitions
- source adapters inside the Convex orchestration layer compile normalized search intent into provider-specific discovery plans before TinyFish execution
- TinyFish runs should receive explicit target URLs or explicit browser action plans, not vague high-level browsing goals, whenever the provider supports deterministic search entry points

**Artifact Persistence Pattern:**
- `TinyFish` may return image URLs or raw file outputs; Convex actions must fetch and store those image bytes into Convex file storage before writing canonical artifact records.
- `fal.ai` generated images must be fetched and stored into Convex file storage before they are attached to recommendation records.
- frontend code should consume Convex-generated file URLs derived from stored `storageId` values, not provider URLs directly.

### Frontend Architecture

**State Management Approach:**
Use Convex's reactive query model as the primary application state mechanism. Do not add Redux or a global client state framework unless a later complexity threshold justifies it.

**Component Architecture:**
Use domain-oriented feature boundaries aligned to Beacon's product surfaces:
- query intake
- live scouting/session progress
- candidate comparison
- final recommendation brief
- operator inspection views

**Routing Strategy:**
Use Next.js App Router with route-level separation for:
- renter search flow
- live session/result pages
- final recommendation artifact pages
- internal operator/admin surfaces

**Performance Strategy:**
Use server-first rendering where possible, with client components only where live interactivity is required. Defer heavy visual modules until needed and keep provider SDK usage off the client unless there is a direct UX reason.

**Bundle Optimization:**
Keep `TinyFish` and `fal.ai` interactions server-side. Avoid leaking provider logic into the client bundle. Lazy-load rich visualization panels and comparison-heavy UI sections when possible.

### Infrastructure & Deployment

**Hosting Strategy:**
Deploy the web application on Vercel and use Convex managed infrastructure for backend execution, persistence, and real-time updates.

**Environment Configuration:**
Use environment separation across local, preview, and production deployments. Store provider secrets in platform-managed environment variables and keep service credentials inaccessible from client-rendered code.

**Monitoring & Logging:**
Use layered observability:
- Vercel deployment and runtime visibility for web concerns
- Convex dashboard and logs for state and backend execution
- explicit `activityLogs` and `workerRuns` data in Convex for product-level traceability
- provider-specific monitoring for TinyFish and fal.ai execution paths

**Scaling Strategy:**
Rely on Vercel and Convex managed scaling for MVP. Control system stress through bounded workflow concurrency, shortlist size limits, and selective enrichment depth rather than assuming unlimited expansion.

**CI/CD Approach:**
Use Git-based deployment flow with preview environments for frontend changes and corresponding Convex deployment targeting. Keep schema and function changes versioned with application code.

### Decision Impact Analysis

**Implementation Sequence:**
1. Initialize `Next.js 16.2` application shell
2. Initialize `Convex 1.34.0` and create canonical domain tables/functions
3. Implement session lifecycle and query intake flow
4. Integrate `TinyFish` retrieval workflows into Convex-driven worker orchestration
5. Implement candidate normalization, enrichment, and recommendation synthesis state model
6. Integrate `fal.ai` visual generation through server-side Convex actions
7. Build renter-facing live scouting and recommendation surfaces
8. Add operator inspection and troubleshooting views

**Cross-Component Dependencies:**
- Frontend progress views depend on Convex session and worker state design.
- Recommendation quality depends on the unit record schema, provenance model, and enrichment lifecycle.
- Visual generation depends on evidence completeness and artifact persistence design.
- Operator tooling depends on the same state model used for renter-facing progress, so observability cannot be bolted on later.
- TinyFish and fal.ai integrations must terminate into Convex-owned canonical records to prevent split-brain system state.

## Implementation Patterns & Consistency Rules

### Pattern Categories Defined

**Critical Conflict Points Identified:**
5 major areas where AI agents could make incompatible choices without explicit rules:
- naming
- structure
- data and response formats
- workflow and event communication
- loading, error, and retry processes

### Naming Patterns

**Database Naming Conventions:**
Use lower camelCase for Convex table names and fields.

Examples:
- tables: `searchSessions`, `unitRecords`, `workerRuns`, `recommendationArtifacts`, `activityLogs`
- ids and refs: `sessionId`, `unitId`, `workerRunId`
- timestamps: `createdAt`, `updatedAt`, `startedAt`, `completedAt`
- booleans: `isComplete`, `isRecommendationReady`, `hasVisualArtifacts`

Do not mix snake_case and camelCase in Convex documents.

**API Naming Conventions:**
Use domain-oriented action, query, and mutation names with verb-first or intent-first clarity.

Examples:
- queries: `searchSessions.getById`, `unitRecords.listBySession`
- mutations: `searchSessions.create`, `unitRecords.upsertFromDiscovery`
- actions: `orchestrator.runSession`, `tinyfish.runDiscovery`, `visuals.generatePreview`

Route params in Next.js should use folder conventions like `[sessionId]`, `[unitId]`.

**Code Naming Conventions:**
- React components: `PascalCase`
- component files: `PascalCase.tsx`
- utility files: `camelCase.ts` only when exporting helper groups, otherwise match main symbol
- functions and variables: `camelCase`
- constants: `UPPER_SNAKE_CASE` only for true constants
- types, interfaces, and schemas: `PascalCase`

Examples:
- `RecommendationBrief.tsx`
- `LiveSearchPanel.tsx`
- `buildWhySelected.ts`
- `SearchSessionStatus`

### Structure Patterns

**Project Organization:**
Organize frontend code by feature or domain, not by technical layer alone.

Recommended top-level app domains:
- `src/app`
- `src/components`
- `src/features/search`
- `src/features/recommendation`
- `src/features/operator`
- `src/lib`
- `src/types`
- `convex/`

Rules:
- UI for a domain stays inside that domain unless clearly shared
- shared primitives go in `src/components`
- provider wrappers, formatting helpers, and env helpers go in `src/lib`
- canonical backend state logic lives in `convex/`, not duplicated in frontend helpers

**File Structure Patterns:**
- tests should be co-located where practical: `*.test.ts` / `*.test.tsx`
- route-specific server code stays near route boundaries
- static assets live in `public/`
- architecture or planning docs stay in `_bmad-output/` or docs folders, not mixed into app code
- environment access must be centralized through typed helpers, not scattered `process.env` usage

### Format Patterns

**API Response Formats:**
Do not wrap every internal call in custom `{ data, error }` envelopes unless crossing an actual external boundary.

Use:
- direct typed returns for Convex queries, mutations, and actions
- explicit typed error objects only at external provider boundaries or route handlers
- persisted workflow state for long-running job status instead of ad hoc polling payloads

**Data Exchange Formats:**
Use camelCase JSON field names everywhere under our control.

Represent evidence state explicitly:
- `sourceType: "retrieved" | "inferred" | "unknown"`
- `status: "planning" | "discovering" | "shortlisting" | "enriching" | "synthesizing" | "generatingVisuals" | "ready" | "partialReady" | "failed"`

Dates:
- store timestamps as numbers in backend state where possible
- format for display only at the UI layer

Null handling:
- use `null` for known-empty optional values
- use absence only when value is not yet populated and distinction matters semantically

### Communication Patterns

**Event System Patterns:**
Do not introduce a separate event bus unless needed. Treat persisted Convex state transitions as the primary event system.

State transition and event names should use domain verbs:
- `sessionCreated`
- `discoveryQueued`
- `candidatesShortlisted`
- `enrichmentCompleted`
- `recommendationReady`
- `visualGenerationFailed`

Worker records should include:
- `type`
- `status`
- `startedAt`
- `completedAt`
- `errorCode`
- `errorMessage`
- `attemptCount`
- `entryUrl`
- `appliedUrl`
- `executionMode`
- `filterPlan`
- `stepCount`
- `repeatedActionCount`
- `resultCount`

**State Management Patterns:**
Use Convex reactive state as the source of truth.
UI state should be local and ephemeral only for presentation concerns.

Rules:
- backend workflow state lives in Convex
- frontend derives display state from Convex records
- no duplicate client-side canonical store for sessions and candidates
- mutations and actions should be narrow and domain-specific

### Process Patterns

**Error Handling Patterns:**
Separate these clearly:
- user-facing recoverable product errors
- operator-visible diagnostic errors
- provider and integration errors

Rules:
- external integration failures must become typed workflow state, not uncaught exceptions in UI
- user-facing messages should describe impact, not internals
- provider raw errors may be logged for operators but not shown directly to renters

**Loading State Patterns:**
Use consistent loading states at both session and component level.

Rules:
- route and page level: derive from session status
- component level: use `isLoading`, `isSubmitting`, `isRefreshing`
- long-running tasks should prefer progress and status messaging over spinners only
- partial data should render whenever safe instead of blocking the full page

### Enforcement Guidelines

**All AI Agents MUST:**
- use camelCase for application-controlled fields, functions, and state names
- treat Convex as the single source of truth for workflow and domain state
- distinguish retrieved, inferred, and unknown data explicitly
- keep TinyFish and fal.ai integration logic server-side
- represent long-running work through persisted status transitions, not hidden async side effects
- compile provider-specific discovery targets before running TinyFish instead of making the browser agent rediscover filter paths from scratch on every run
- prefer direct URLs and source-native filters before UI traversal, and persist telemetry needed to detect browser struggle

**Pattern Enforcement:**
- validate against these rules during code review and story acceptance
- update this section if a new cross-cutting convention is introduced
- reject PRs and stories that introduce duplicate canonical state or mixed naming conventions

### Pattern Examples

**Good Examples:**
- `searchSessions.create`
- `unitRecords.upsertFromDiscovery`
- `status: "generatingVisuals"`
- `sourceType: "inferred"`
- `RecommendationBrief.tsx`
- `LiveSearchPanel.tsx`

**Anti-Patterns:**
- mixing `unit_records`, `unitRecords`, and `UnitRecords`
- calling TinyFish directly from client components
- storing duplicate candidate truth in React global state and Convex simultaneously
- flattening inferred visuals into the same certainty level as retrieved listing facts
- returning inconsistent error shapes across provider integrations

## Project Structure & Boundaries

### Complete Project Directory Structure

```text
beaconmain/
├── README.md
├── package.json
├── tsconfig.json
├── next.config.ts
├── eslint.config.js
├── postcss.config.js
├── components.json
├── .env.local
├── .env.example
├── .gitignore
├── .github/
│   └── workflows/
│       ├── ci.yml
│       └── deploy.yml
├── public/
│   ├── images/
│   └── icons/
├── src/
│   ├── app/
│   │   ├── globals.css
│   │   ├── layout.tsx
│   │   ├── page.tsx
│   │   ├── search/
│   │   │   ├── page.tsx
│   │   │   └── [sessionId]/
│   │   │       ├── page.tsx
│   │   │       ├── loading.tsx
│   │   │       └── error.tsx
│   │   ├── recommendation/
│   │   │   └── [sessionId]/
│   │   │       ├── page.tsx
│   │   │       ├── loading.tsx
│   │   │       └── error.tsx
│   │   ├── operator/
│   │   │   ├── page.tsx
│   │   │   ├── sessions/
│   │   │   │   └── [sessionId]/page.tsx
│   │   │   └── worker-runs/
│   │   │       └── [workerRunId]/page.tsx
│   │   └── api/
│   │       ├── tinyfish/
│   │       │   └── webhook/route.ts
│   │       ├── fal/
│   │       │   └── webhook/route.ts
│   │       └── health/route.ts
│   ├── components/
│   │   ├── ui/
│   │   ├── layout/
│   │   ├── search/
│   │   ├── recommendation/
│   │   ├── operator/
│   │   └── shared/
│   ├── features/
│   │   ├── search/
│   │   │   ├── components/
│   │   │   ├── hooks/
│   │   │   ├── lib/
│   │   │   └── types.ts
│   │   ├── recommendation/
│   │   │   ├── components/
│   │   │   ├── hooks/
│   │   │   ├── lib/
│   │   │   └── types.ts
│   │   ├── operator/
│   │   │   ├── components/
│   │   │   ├── hooks/
│   │   │   ├── lib/
│   │   │   └── types.ts
│   │   └── visuals/
│   │       ├── components/
│   │       ├── lib/
│   │       └── types.ts
│   ├── lib/
│   │   ├── env.ts
│   │   ├── convex.ts
│   │   ├── utils.ts
│   │   ├── errors.ts
│   │   ├── constants.ts
│   │   ├── formatters/
│   │   ├── validation/
│   │   └── telemetry/
│   ├── types/
│   │   ├── search.ts
│   │   ├── recommendation.ts
│   │   ├── evidence.ts
│   │   ├── visuals.ts
│   │   └── operator.ts
│   └── middleware.ts
├── convex/
│   ├── _generated/
│   ├── schema.ts
│   ├── constants.ts
│   ├── searchSessions/
│   │   ├── queries.ts
│   │   ├── mutations.ts
│   │   └── helpers.ts
│   ├── unitRecords/
│   │   ├── queries.ts
│   │   ├── mutations.ts
│   │   └── helpers.ts
│   ├── workerRuns/
│   │   ├── queries.ts
│   │   ├── mutations.ts
│   │   └── helpers.ts
│   ├── recommendationArtifacts/
│   │   ├── queries.ts
│   │   ├── mutations.ts
│   │   └── helpers.ts
│   ├── imageArtifacts/
│   │   ├── queries.ts
│   │   ├── mutations.ts
│   │   └── helpers.ts
│   ├── activityLogs/
│   │   ├── queries.ts
│   │   └── mutations.ts
│   ├── orchestrator/
│   │   ├── actions.ts
│   │   ├── discovery.ts
│   │   ├── enrichment.ts
│   │   ├── synthesis.ts
│   │   ├── visuals.ts
│   │   └── helpers.ts
│   ├── integrations/
│   │   ├── tinyfish/
│   │   │   ├── actions.ts
│   │   │   ├── mappers.ts
│   │   │   └── types.ts
│   │   └── fal/
│   │       ├── actions.ts
│   │       ├── mappers.ts
│   │       └── types.ts
│   ├── lib/
│   │   ├── status.ts
│   │   ├── provenance.ts
│   │   ├── scoring.ts
│   │   ├── evidence.ts
│   │   ├── storage.ts
│   │   └── validation.ts
│   └── http.ts
├── tests/
│   ├── integration/
│   │   ├── searchSessions/
│   │   ├── recommendation/
│   │   └── operator/
│   ├── e2e/
│   │   ├── renter-flow.spec.ts
│   │   ├── recommendation-flow.spec.ts
│   │   └── operator-flow.spec.ts
│   ├── fixtures/
│   └── utils/
└── docs/
    └── architecture/
```

### Architectural Boundaries

**API Boundaries:**
- Public renter-facing app routes live under `src/app/search` and `src/app/recommendation`
- Internal operator surfaces live under `src/app/operator`
- external-provider callbacks and webhooks terminate only at `src/app/api/tinyfish/webhook` and `src/app/api/fal/webhook`
- canonical application reads and writes flow through Convex functions, not ad hoc route-local state

**Component Boundaries:**
- route shells live in `src/app`
- reusable visual primitives live in `src/components`
- domain-specific UI and hooks live in `src/features/<domain>`
- no direct provider integration from React components

**Service Boundaries:**
- orchestration logic lives in `convex/orchestrator`
- provider-specific logic lives in `convex/integrations/tinyfish` and `convex/integrations/fal`
- canonical business and domain helpers live in `convex/lib`
- Next.js should orchestrate page delivery, not own backend workflow truth

**Data Boundaries:**
- Convex owns canonical workflow state and persistence
- `searchSessions`, `unitRecords`, `workerRuns`, `recommendationArtifacts`, and `imageArtifacts` are source-of-truth entities
- provider payloads must be mapped into internal domain models before persistence
- UI reads derived state; it does not define domain truth
- image binaries are stored in Convex file storage, while tables store metadata and references

### Requirements to Structure Mapping

**Feature Mapping:**
- Query intake and search setup -> `src/app/search`, `src/features/search`, `convex/searchSessions`
- Property discovery and candidate selection -> `convex/orchestrator/discovery.ts`, `convex/unitRecords`, `convex/integrations/tinyfish`
- Evidence enrichment and completion -> `convex/orchestrator/enrichment.ts`, `convex/lib/evidence.ts`, `convex/lib/provenance.ts`
- Recommendation and comparison -> `src/features/recommendation`, `convex/orchestrator/synthesis.ts`, `convex/recommendationArtifacts`
- Visual understanding and inferred representation -> `src/features/visuals`, `convex/orchestrator/visuals.ts`, `convex/integrations/fal`, `convex/imageArtifacts`
- Live search and session feedback -> `src/app/search/[sessionId]`, `src/features/search/hooks`, `convex/searchSessions/queries.ts`
- Persistence, operations, and recovery -> `convex/workerRuns`, `convex/activityLogs`, `src/app/operator`
- Trust and transparency -> `src/features/recommendation`, `convex/lib/provenance.ts`, `convex/lib/status.ts`

**Cross-Cutting Concerns:**
- env and config -> `src/lib/env.ts`, `convex/constants.ts`
- validation -> `src/lib/validation`, `convex/lib/validation.ts`
- scoring, provenance, and status -> `convex/lib/`
- telemetry and logging -> `src/lib/telemetry`, `convex/activityLogs`

### Integration Points

**Internal Communication:**
- Next.js UI reads reactive Convex query state
- user actions trigger Convex mutations and actions
- orchestrator actions update canonical state records
- operator surfaces inspect the same persisted state used by renter flows

**External Integrations:**
- `TinyFish` integration entry: `convex/integrations/tinyfish/actions.ts`
- `fal.ai` integration entry: `convex/integrations/fal/actions.ts`
- provider callbacks and webhooks terminate in `src/app/api/.../webhook/route.ts` when needed
- all provider data is mapped into internal types before storage
- all retrieved or generated images are stored in Convex file storage and indexed in `imageArtifacts`
- source-specific discovery adapters should live alongside or beneath the TinyFish integration boundary so provider URL compilation and fallback logic are not scattered across the app

**Data Flow:**
1. renter submits query
2. Convex creates `searchSession`
3. orchestrator compiles source-specific discovery plans with `entryUrl`, `appliedUrl`, filter inputs, execution mode, and fallback order
4. TinyFish executes the most explicit discovery target first, typically a direct search URL or source-native filtered landing page
5. Convex records discovery telemetry such as step count, duration, repeated actions, expected-page checks, and result counts to detect navigation struggle
6. if direct-url discovery underperforms or the browser agent struggles, orchestrator switches to the next fallback strategy such as UI-assisted filters or broader retrieval
7. TinyFish returns candidate and evidence data, including image URLs or files
8. Convex fetches image bytes, stores them in file storage, and writes canonical `imageArtifacts` records
9. Convex normalizes and persists canonical session, unit, evidence, and discovery-run records
10. synthesis creates recommendation artifact
11. fal.ai generates inferred visuals when triggered
12. Convex stores generated image bytes in file storage and links them to recommendation and image artifact records
13. frontend renders live progress and final recommendation from Convex state and file URLs generated from `storageId`

### File Organization Patterns

**Configuration Files:**
- root config files stay at repo root
- typed env access is centralized in `src/lib/env.ts`
- Convex-specific constants and config stay under `convex/`

**Source Organization:**
- route entrypoints in `src/app`
- shared UI in `src/components`
- domain features in `src/features`
- domain backend in `convex/<domain>`
- cross-domain backend helpers in `convex/lib`

**Test Organization:**
- co-located unit tests where useful
- broader integration tests in `tests/integration`
- end-to-end renter and operator tests in `tests/e2e`

**Asset Organization:**
- static design assets in `public/`
- generated artifact references stored in Convex, not committed into repo

### Development Workflow Integration

**Development Server Structure:**
- Next.js dev server for UI shell
- Convex dev runtime for backend and state workflows
- provider integrations mocked or sandboxed behind boundary modules where possible

**Build Process Structure:**
- web build uses Next.js conventions
- backend workflow logic deploys through Convex
- provider integration code remains isolated so frontend builds are not coupled to retrieval and generation internals

**Deployment Structure:**
- Vercel deploys web app
- Convex deploys backend and state layer
- environment-specific provider keys are injected at deploy time
- operator routes can be protected independently from public renter flows

## Architecture Validation Results

### Coherence Validation ✅

**Decision Compatibility:**
All major technology decisions are compatible and mutually reinforcing. `Next.js 16.2` provides the web application shell, `Convex 1.34.0` provides the canonical backend state and workflow runtime, `TinyFish` provides browser-based retrieval, and `fal.ai` provides inferred visual generation. The system remains coherent because provider integrations terminate into Convex-owned state rather than creating parallel data authority.

**Pattern Consistency:**
The implementation patterns support the architectural decisions cleanly. Naming, structure, state ownership, and external integration boundaries all align with the selected stack. The rules around camelCase naming, Convex as source of truth, server-side provider integration, and explicit retrieved, inferred, and unknown distinctions are consistent with both the data model and the user-facing trust requirements.

**Structure Alignment:**
The proposed project structure supports all major decisions. `src/` is organized around product surfaces and feature boundaries, while `convex/` is organized around domain records, orchestration, and provider integrations. This gives AI agents a clear ownership model and reduces the chance of duplicate logic or split-brain state.

### Requirements Coverage Validation ✅

**Epic/Feature Coverage:**
No epic breakdown exists yet, but all current feature areas from the PRD are architecturally covered:
- query intake
- discovery and shortlist generation
- recursive evidence enrichment
- recommendation synthesis
- inferred visual generation
- live progress tracking
- operator observability

**Functional Requirements Coverage:**
All functional requirement categories are supported by the architecture:
- query intake and search setup map to `searchSessions`
- discovery and candidate selection map to `unitRecords`, `TinyFish`, and orchestration actions
- evidence enrichment maps to enrichment workflows, provenance modeling, and worker state
- recommendation and comparison map to synthesis modules and recommendation artifacts
- visual representation maps to visual evidence, `fal.ai`, and artifact persistence
- live feedback maps to Convex reactive state and App Router session pages
- operations and recovery map to `workerRuns`, `activityLogs`, and operator routes
- trust and transparency map to provenance, confidence, and explicit output labeling

**Non-Functional Requirements Coverage:**
The architecture addresses all major NFR areas:
- performance through asynchronous workflows and progressive updates
- security through operator-only protected internal surfaces and server-side provider boundaries
- scalability through managed Vercel and Convex infrastructure and bounded concurrency
- accessibility through a structured web app architecture that supports consistent UI implementation
- integration resilience through provider isolation, typed boundary mapping, and graceful degradation

### Implementation Readiness Validation ✅

**Decision Completeness:**
Critical architectural decisions are documented with concrete technologies and current verified versions where relevant. Deferred decisions are explicitly identified instead of being left ambiguous.

**Structure Completeness:**
The project tree is concrete enough for implementation. Major routes, feature directories, Convex modules, integration boundaries, and testing locations are all specified.

**Pattern Completeness:**
The implementation rules cover the major areas where AI agents could drift: naming, state ownership, integration boundaries, workflow states, error handling, and loading patterns.

### Gap Analysis Results

**Critical Gaps:**
- None identified.

**Important Gaps:**
- End-user authentication is intentionally deferred, so future account-related work will need a follow-up architecture decision.
- Analytics and telemetry depth are not fully specified yet, but this does not block MVP implementation.
- Exact operator auth mechanism is not selected yet, only the architectural boundary is defined.

**Nice-to-Have Gaps:**
- More detailed testing strategy by layer
- More explicit observability event taxonomy
- Future multi-region or higher-scale caching strategy

### Validation Issues Addressed

No blocking validation issues were found. The architecture is internally coherent, covers the documented requirements, and provides enough consistency rules and structure guidance for AI-assisted implementation.

### Architecture Completeness Checklist

**✅ Requirements Analysis**
- [x] Project context thoroughly analyzed
- [x] Scale and complexity assessed
- [x] Technical constraints identified
- [x] Cross-cutting concerns mapped

**✅ Architectural Decisions**
- [x] Critical decisions documented with versions
- [x] Technology stack fully specified
- [x] Integration patterns defined
- [x] Performance considerations addressed

**✅ Implementation Patterns**
- [x] Naming conventions established
- [x] Structure patterns defined
- [x] Communication patterns specified
- [x] Process patterns documented

**✅ Project Structure**
- [x] Complete directory structure defined
- [x] Component boundaries established
- [x] Integration points mapped
- [x] Requirements to structure mapping complete

### Architecture Readiness Assessment

**Overall Status:** READY FOR IMPLEMENTATION

**Confidence Level:** High

**Key Strengths:**
- clear session-centric architecture
- strong source-of-truth model in Convex
- explicit separation of retrieval, synthesis, and generation concerns
- implementation patterns designed to reduce AI agent drift
- project structure aligned with product surfaces and workflow boundaries

**Areas for Future Enhancement:**
- renter authentication and saved-user workflows
- deeper observability and analytics
- richer scaling and caching policies
- more formal external API decisions if platformization becomes necessary

### Implementation Handoff

**AI Agent Guidelines:**
- Follow all architectural decisions exactly as documented
- Use implementation patterns consistently across all components
- Respect project structure and boundaries
- Refer to this document for all architectural questions

**First Implementation Priority:**
Initialize the project with the chosen `Next.js + Convex` foundation, then establish the canonical Convex schema, session lifecycle, and orchestrator boundaries before building retrieval and recommendation features.
