---
stepsCompleted:
  - step-01-validate-prerequisites
  - step-02-design-epics
  - step-03-create-stories
inputDocuments:
  - /Users/leongwenxuan/Desktop/beaconmain/_bmad-output/planning-artifacts/prd.md
  - /Users/leongwenxuan/Desktop/beaconmain/_bmad-output/planning-artifacts/architecture.md
---

# beaconmain - Epic Breakdown

## Overview

This document provides the complete epic and story breakdown for beaconmain, decomposing the requirements from the PRD, UX Design if it exists, and Architecture requirements into implementable stories.

## Requirements Inventory

### Functional Requirements

FR1: Renters can describe their rental needs in natural language.
FR2: Renters can submit a rental brief that includes location, budget, property preferences, and lifestyle priorities.
FR3: The system can interpret a renter's brief into a structured search intent.
FR4: The system can create a search session for each submitted rental brief.
FR5: The system can preserve the state of an in-progress search session.
FR6: Renters can revisit the current state of an active search session.
FR7: The system can discover candidate rental properties from multiple sources.
FR8: The system can consolidate discovered candidates into a shared shortlist for a search session.
FR9: The system can identify and remove duplicate or overlapping candidate properties.
FR10: The system can narrow discovered candidates into a ranked top-three shortlist.
FR11: The system can update the shortlist as better evidence becomes available.
FR12: The system can associate each shortlisted property with its original source listing.
FR13: The system can collect property details beyond the original listing page.
FR14: The system can gather building-level and neighborhood-level context for shortlisted properties.
FR15: The system can retrieve supporting evidence from sources beyond the original listing page when important information is missing.
FR16: The system can identify missing information for shortlisted properties.
FR17: The system can continue enriching shortlisted properties until they are sufficiently complete for recommendation.
FR18: The system can reconcile conflicting information gathered from different sources.
FR19: The system can retain provenance for important evidence used in recommendations.
FR20: The system can distinguish between retrieved facts, inferred outputs, and unknown information.
FR21: The system can rank shortlisted properties against the renter's stated priorities.
FR22: The system can identify a best overall recommendation from the shortlisted properties.
FR23: The system can present the top three shortlisted properties in a comparison-ready format.
FR24: The system can explain why each shortlisted property was selected.
FR25: The system can identify strengths and weaknesses for each shortlisted property.
FR26: The system can present tradeoffs across shortlisted properties.
FR27: The system can communicate uncertainty or incomplete evidence within recommendation outputs.
FR28: Renters can access the original source listing for a shortlisted property.
FR29: The system can return a usable recommendation even when no perfect match exists.
FR30: The system can collect visual evidence for shortlisted properties and persist it as session-linked artifacts.
FR31: The system can use available visual and layout evidence to infer a plausible representation of a recommended property.
FR32: The system can present an inferred visual output as part of the recommendation experience.
FR33: The system can indicate that generated visuals are inferred rather than exact reconstructions.
FR34: The system can produce a recommendation even when visual evidence is weak or incomplete.
FR35: The system can associate inferred visual outputs with the property evidence, provenance, and persisted artifact records used to generate them.
FR36: Renters can see that Beacon is actively working on their search session.
FR37: Renters can view progress as properties are discovered, enriched, and narrowed.
FR38: Renters can see evolving candidate quality during an in-progress session.
FR39: The system can surface partial results before the final recommendation is complete.
FR40: The system can preserve continuity between live search progress and the final recommendation experience.
FR41: The system can persist candidate, evidence, recommendation, and artifact state for a session.
FR42: The system can reuse previously gathered evidence when it is safe and appropriate to do so.
FR43: Operators can inspect the state of a search session and its shortlisted candidates.
FR44: Operators can inspect generated artifacts and stored evidence associated with a session.
FR45: Operators can identify when a session has partially failed or degraded.
FR46: The system can continue delivering a usable output when one or more evidence retrieval paths fail.
FR47: The system can preserve partial progress when a full search workflow does not complete successfully.
FR48: Renters can understand whether a recommendation is based on strong, partial, or uncertain evidence.
FR49: Demo viewers can observe that the recommendation is the result of a real multi-stage workflow rather than a static output.
FR50: The system can present recommendation outputs in a way that reflects how evidence was gathered, enriched, and synthesized.

### NonFunctional Requirements

NFR1: The system shall surface an initial progress signal or early shortlist indication within 60 seconds for a typical rental query under normal operating conditions.
NFR2: The system shall produce a final recommendation within 5 minutes for a typical rental query under normal demo conditions.
NFR3: The user interface shall remain responsive while long-running discovery, enrichment, and visual synthesis tasks are in progress.
NFR4: The system shall support progressive result updates so users can receive partial value before a session is fully complete.
NFR5: The system shall protect stored session data, candidate data, evidence records, and generated artifacts from unauthorized access.
NFR6: The system shall restrict operational and troubleshooting visibility to authorized operators only.
NFR7: The system shall preserve the integrity of provenance, confidence, and recommendation data so that outputs cannot be silently corrupted or misrepresented.
NFR8: The system shall protect data in transit and at rest using industry-standard security practices appropriate for a web application MVP.
NFR9: The system shall support multiple concurrent search sessions without causing unacceptable degradation to the core recommendation flow.
NFR10: The system shall degrade gracefully when demand exceeds ideal processing capacity, prioritizing usable partial results over total failure.
NFR11: The system shall support incremental expansion of source coverage, session volume, and artifact storage without requiring a full product redesign.
NFR12: Core user flows shall support keyboard-accessible interaction.
NFR13: The interface shall use semantic structure, accessible labels, and status messaging sufficient for assistive technology support in core flows.
NFR14: Recommendation, progress, and comparison content shall remain readable through adequate contrast, hierarchy, and text clarity.
NFR15: Accessibility support shall apply to both the live search experience and the final recommendation experience.
NFR16: The system shall tolerate partial unreliability from external listing and evidence sources without collapsing the full session.
NFR17: The system shall maintain clear separation between external source data, inferred data, and internally synthesized recommendation outputs.
NFR18: The system shall support persistence and retrieval of session, candidate, evidence, and visual artifact data, including durable storage references for retrieved and generated images, across the end-to-end workflow.
NFR19: The system shall detect and handle stale or incomplete external evidence in a way that reduces the risk of distorted recommendations.

### Additional Requirements

- Epic 1 Story 1 must initialize the project using the selected starter: `Next.js App Router + Convex Quickstart`.
- The initialization command established by architecture is `npx create-next-app@latest beaconmain --ts --app --tailwind --eslint --src-dir && cd beaconmain && npm install convex && npx convex dev`.
- `Convex 1.34.0` is the required canonical backend, state store, workflow persistence layer, and real-time sync layer.
- `TinyFish` is the required browser-based retrieval subsystem for listing and external evidence collection.
- `fal.ai` via `@fal-ai/client 1.9.5` is the required provider for inferred property visual generation.
- The system must use a session-centric data model with explicit records for `searchSessions`, `unitRecords`, `workerRuns`, `recommendationArtifacts`, `imageArtifacts`, and `activityLogs`.
- All discovery, enrichment, synthesis, and visual generation workflows must execute asynchronously with persisted status transitions instead of a synchronous request-response pipeline.
- Convex must remain the single source of truth for workflow state; frontend code must derive state from Convex rather than duplicate canonical state in client stores.
- Retrieved and generated images must be fetched and stored into Convex file storage, with metadata and provenance linked through canonical artifact records using `storageId`.
- The implementation must distinguish `retrieved`, `inferred`, and `unknown` data explicitly in stored state and UI presentation.
- Workflow state must use explicit statuses such as `planning`, `discovering`, `shortlisting`, `enriching`, `synthesizing`, `generatingVisuals`, `ready`, `partialReady`, and `failed`.
- Boundary validation is required at external integration edges before TinyFish or fal.ai payloads are merged into canonical session state.
- MVP security must follow an operator-only internal access model for observability and troubleshooting, with no renter account system required initially.
- Provider integrations and credentials must stay server-side; `TinyFish` and `fal.ai` logic must not leak into client bundles.
- The app must be deployed with Vercel for the web tier and Convex managed infrastructure for backend execution and persistence.
- The architecture requires route separation for renter search, live session pages, recommendation pages, and internal operator surfaces.
- The architecture requires explicit observability through Convex-backed `workerRuns` and `activityLogs` so partial failures and recoveries are inspectable.
- Caching is allowed only where reuse is safe and must include freshness metadata so stale evidence does not silently distort recommendations.
- The implementation sequence must establish the app shell and Convex schema before retrieval, recommendation synthesis, and visual generation work.
- Discovery must prefer source-native filters, explicit search URLs, and direct landing pages before UI-driven browser traversal when a source supports them.
- Each source integration should store a stable `entryUrl` and compile a fresh `appliedUrl` or explicit browser plan from normalized renter intent at runtime instead of replaying stale raw links.
- Discovery runs must persist the executed filter plan, applied URL, mode, result counts, and navigation telemetry so the orchestrator can detect when TinyFish is struggling and switch strategies quickly.
- TinyFish browser navigation should be treated as a fallback or refinement path after direct-url attempts, not the default strategy for reaching the relevant search state on every run.

### UX Design Requirements

No UX design document was provided, so there are no extracted UX-specific implementation requirements yet.

### FR Coverage Map

FR1: Epic 1 - Natural-language query input
FR2: Epic 1 - Structured renter brief submission
FR3: Epic 1 - Search intent interpretation
FR4: Epic 1 - Search session creation
FR5: Epic 1 - In-progress session persistence
FR6: Epic 1 - Revisit active search session
FR7: Epic 2 - Multi-source property discovery
FR8: Epic 2 - Candidate consolidation into a shared shortlist
FR9: Epic 2 - Duplicate and overlap removal
FR10: Epic 2 - Ranked top-three shortlist generation
FR11: Epic 2 - Shortlist updates as evidence improves
FR12: Epic 2 - Source listing association
FR13: Epic 3 - Property detail collection beyond the listing page
FR14: Epic 3 - Building-level and neighborhood-level context gathering
FR15: Epic 3 - Retrieval of supporting evidence from additional sources
FR16: Epic 3 - Missing-information detection
FR17: Epic 3 - Ongoing enrichment until recommendation-ready
FR18: Epic 3 - Cross-source conflict reconciliation
FR19: Epic 3 - Evidence provenance retention
FR20: Epic 3 - Explicit retrieved vs inferred vs unknown distinction
FR21: Epic 2 - Ranking against renter priorities
FR22: Epic 4 - Best overall recommendation selection
FR23: Epic 4 - Comparison-ready top-three presentation
FR24: Epic 4 - Why-selected explanation
FR25: Epic 4 - Strengths and weaknesses presentation
FR26: Epic 4 - Tradeoff communication
FR27: Epic 4 - Uncertainty and incomplete-evidence communication
FR28: Epic 4 - Source listing clickthrough
FR29: Epic 4 - Usable recommendation without a perfect match
FR30: Epic 5 - Visual evidence collection with durable artifact persistence
FR31: Epic 5 - Inferred visual representation generation
FR32: Epic 5 - Inferred visual presentation in the recommendation experience
FR33: Epic 5 - Clear labeling of generated visuals as inferred
FR34: Epic 5 - Recommendation delivery under weak or incomplete visual evidence
FR35: Epic 5 - Visual artifact linkage to evidence, provenance, and persisted records
FR36: Epic 1 - Visible active work state during a search session
FR37: Epic 1 - Progress updates through discovery, enrichment, and narrowing
FR38: Epic 1 - Visibility into evolving candidate quality
FR39: Epic 1 - Partial results before final recommendation completion
FR40: Epic 1 - Continuity between live progress and final recommendation
FR41: Epic 1 - Baseline persistence for session, candidate, evidence, recommendation, and artifact state
FR42: Epic 3 - Safe reuse of previously gathered evidence
FR43: Epic 6 - Operator inspection of sessions and shortlisted candidates
FR44: Epic 6 - Operator inspection of generated artifacts and stored evidence
FR45: Epic 6 - Operator visibility into degraded or partially failed sessions
FR46: Epic 3 - Usable output despite evidence retrieval failures
FR47: Epic 3 - Preservation of partial progress after incomplete workflow runs
FR48: Epic 4 - Clear evidence-strength communication
FR49: Epic 4 - User-visible credibility of the real multi-stage workflow
FR50: Epic 4 - Recommendation outputs that reflect evidence gathering and synthesis lineage

## Epic List

### Epic 1: Search Session Kickoff & Live Progress
Renters can submit a natural-language brief, start a persistent search session, and see Beacon actively working with early partial value and continuity into the final recommendation.
**FRs covered:** FR1, FR2, FR3, FR4, FR5, FR6, FR36, FR37, FR38, FR39, FR40, FR41

### Epic 2: Candidate Discovery & Top-3 Shortlisting
Renters can have Beacon search across multiple sources using source-native filters and direct URLs first, consolidate candidate properties, remove duplicates, and surface a ranked top-three shortlist linked to source listings.
**FRs covered:** FR7, FR8, FR9, FR10, FR11, FR12, FR21

### Epic 3: Evidence Completion, Reconciliation & Resilience
Renters can get stronger recommendations because Beacon fills missing facts, reconciles conflicting evidence, tracks provenance, safely reuses evidence, and continues working under partial source failure.
**FRs covered:** FR13, FR14, FR15, FR16, FR17, FR18, FR19, FR20, FR42, FR46, FR47

### Epic 4: Recommendation, Comparison & Trustworthy Output
Renters can review a comparison-ready top three, understand strengths, weaknesses, tradeoffs, and uncertainty, receive a clear best recommendation, and click through with confidence.
**FRs covered:** FR22, FR23, FR24, FR25, FR26, FR27, FR28, FR29, FR48, FR49, FR50

### Epic 5: Inferred Visuals & Durable Artifact Handling
Renters can see an inferred visual representation of the recommended property that is clearly labeled as inferred and durably linked to the evidence and artifact records used to produce it.
**FRs covered:** FR30, FR31, FR32, FR33, FR34, FR35

### Epic 6: Operator Inspection & Troubleshooting
Operators can inspect runs, shortlisted units, evidence, artifacts, and degraded states so Beacon remains debuggable, recoverable, and demo-safe.
**FRs covered:** FR43, FR44, FR45

<!-- Repeat for each epic in epics_list (N = 1, 2, 3...) -->

## Epic 1: Search Session Kickoff & Live Progress

Renters can submit a natural-language brief, start a persistent search session, and see Beacon actively working with early partial value and continuity into the final recommendation.

<!-- Repeat for each story (M = 1, 2, 3...) within epic N -->

### Story 1.1: Set Up Initial Project from Starter Template and Search Entry

As a renter,
I want Beacon to load a working search entry experience,
So that I can start a rental search from a stable app shell.

**Acceptance Criteria:**

**Given** the repo is initialized for Beacon
**When** the application is created
**Then** it uses the approved `Next.js App Router + Convex Quickstart` foundation
**And** Convex is connected as the backend state layer for the app

**Given** a renter visits the app
**When** the root or search route loads
**Then** they see a desktop-first Beacon app shell with a natural-language rental brief input surface
**And** the page is keyboard accessible with semantic labels and status regions

**Given** the initial app shell is implemented
**When** the codebase is structured
**Then** route boundaries exist for renter search and session views
**And** no provider-specific retrieval or visual generation logic is exposed in client code

### Story 1.2: Create a Persisted Search Session from a Rental Brief

As a renter,
I want to submit a rental brief and get a persisted Beacon session,
So that my search can begin and continue across refreshes or revisits.

**Acceptance Criteria:**

**Given** a renter enters a natural-language brief with location, budget, and preferences
**When** they submit the form
**Then** Beacon creates a `searchSession` record in Convex
**And** stores the original brief plus normalized search-intent fields needed for downstream work

**Given** a session is created
**When** submission succeeds
**Then** the renter is routed to a session-specific page using a stable `sessionId`
**And** the session starts in an explicit persisted workflow state such as `planning` or `discovering`

**Given** the brief is incomplete or malformed
**When** the renter submits it
**Then** Beacon returns actionable validation feedback
**And** no invalid session record is created

### Story 1.3: Show Reactive Live Progress for an Active Session

As a renter,
I want to see Beacon actively progressing through my search session,
So that I trust the system is doing real work before the final recommendation is ready.

**Acceptance Criteria:**

**Given** a valid session exists
**When** the renter opens the session page
**Then** the UI reads session state reactively from Convex
**And** displays the current workflow status using explicit persisted labels such as `planning`, `discovering`, `shortlisting`, `enriching`, or `partialReady`

**Given** the session state changes in Convex
**When** status, timestamps, or progress metadata update
**Then** the session page reflects the new state without a full reload
**And** keeps the interface responsive during long-running work

**Given** progress is still early or incomplete
**When** there is not yet a final recommendation
**Then** the UI shows meaningful progress messaging rather than a dead-end blank state
**And** exposes accessible status updates for assistive technology

### Story 1.4: Preserve Session Continuity and Revisit In-Progress Work

As a renter,
I want to revisit an active Beacon session and keep any partial progress,
So that long-running search work remains usable across refreshes and later returns.

**Acceptance Criteria:**

**Given** a renter refreshes or reopens a valid session URL
**When** the session page loads again
**Then** Beacon restores the latest persisted session state from Convex
**And** does not create a duplicate session

**Given** partial progress exists for the session
**When** the renter revisits it
**Then** the page shows the latest available progress, counts, or partial outputs already persisted
**And** preserves continuity between in-progress work and later recommendation output

**Given** a session has degraded or only partially completed
**When** the renter views it
**Then** Beacon surfaces the current state honestly
**And** keeps whatever useful partial progress is available instead of collapsing to a generic failure screen

## Epic 2: Candidate Discovery & Top-3 Shortlisting

Renters can have Beacon search across multiple sources using source-native filters and direct URLs first, consolidate candidate properties, remove duplicates, and surface a ranked top-three shortlist linked to source listings.

### Story 2.1: Compile Source-Specific Discovery Plans and Explicit Target URLs

As a renter,
I want Beacon to translate my search intent into source-specific discovery plans before browsing,
So that discovery starts from the most relevant search state instead of wasting time navigating manually.

**Acceptance Criteria:**

**Given** a valid `searchSession` exists with normalized search intent
**When** discovery planning starts
**Then** Beacon compiles a source-specific plan for each supported rental source
**And** each plan records the source, mode, filter inputs, and strictness level to apply

**Given** a source supports explicit query URLs or source-native filters
**When** Beacon compiles that source plan
**Then** it stores a stable `entryUrl` and a freshly generated `appliedUrl` or explicit browser action plan
**And** avoids relying on stale pasted raw links as canonical discovery inputs

**Given** discovery plans have been compiled
**When** they are persisted
**Then** Beacon creates a discovery-oriented `workerRun` or equivalent workflow record in Convex
**And** advances the session into an explicit persisted state such as `discovering`

### Story 2.2: Execute Direct-URL-First Discovery with Struggle Detection and Fallback

As a renter,
I want Beacon to start discovery from explicit source URLs and recover quickly if browser navigation is struggling,
So that relevant candidate properties are gathered faster.

**Acceptance Criteria:**

**Given** a persisted source plan includes an `appliedUrl`
**When** discovery executes for that source
**Then** Beacon attempts the direct URL or explicit landing path before any broad UI traversal
**And** invokes `TinyFish` only through server-side Convex actions

**Given** TinyFish takes too many steps, exceeds a time budget, repeats failed actions, or does not reach the expected page state
**When** Beacon evaluates discovery telemetry
**Then** the run is marked as navigation struggle or degraded execution
**And** Beacon switches to the next fallback strategy such as UI-assisted filters or broader retrieval

**Given** an external source is partially unavailable or one discovery strategy fails
**When** fallback evaluation completes
**Then** Beacon records the degraded outcome in persisted workflow state
**And** continues the session where partial discovery is still possible instead of blocking the full source pass

### Story 2.3: Persist Discovery Telemetry and Raw Candidates with Source Linkage

As a renter,
I want discovered properties and the way Beacon found them to be stored against my session,
So that the system can build a shortlist from real source-backed candidates and improve its discovery behavior over time.

**Acceptance Criteria:**

**Given** discovery returns candidate properties from one or more sources
**When** Beacon receives those results
**Then** it stores candidate records in Convex linked to the owning `searchSession`
**And** each candidate retains its source listing reference, provider-origin metadata, and the source plan or run that produced it

**Given** a discovery attempt completes or fails
**When** Beacon persists run outputs
**Then** it stores the executed `entryUrl`, `appliedUrl`, filter plan, execution mode, navigation telemetry, and result counts
**And** that state can later be inspected for debugging recall, speed, and browser struggle

**Given** a candidate is persisted
**When** required listing facts are incomplete
**Then** Beacon still stores the candidate in a usable partial form
**And** marks missing or unknown fields explicitly instead of fabricating completeness

### Story 2.4: Normalize Candidate Sets and Produce an Evolving Ranked Top-3 Shortlist

As a renter,
I want Beacon to merge overlapping listings and surface a ranked top-three shortlist from discovered candidates,
So that I can see the strongest current options before deeper enrichment is finished.

**Acceptance Criteria:**

**Given** multiple discovered candidates may refer to the same property or unit
**When** Beacon processes the candidate set
**Then** it applies deterministic normalization and duplicate-detection rules
**And** consolidates overlapping candidates into a single canonical unit record where confidence is sufficient

**Given** duplicate confidence is weak or ambiguous
**When** Beacon cannot safely merge candidates
**Then** it keeps them separate
**And** preserves enough state for later refinement instead of making an unsafe merge

**Given** a session has a normalized candidate set with enough information for initial scoring
**When** shortlist generation runs
**Then** Beacon ranks candidates against the renter's stated priorities and persists a top-three shortlist linked to the session
**And** each shortlisted property includes linkage to its original source listing

**Given** new discovery results or better candidate data arrive
**When** Beacon recomputes shortlist quality
**Then** it can update the ranked top-three shortlist in persisted state
**And** preserves continuity so the renter sees that the shortlist is evolving rather than resetting arbitrarily

## Epic 3: Evidence Completion, Reconciliation & Resilience

Renters can get stronger recommendations because Beacon fills missing facts, reconciles conflicting evidence, tracks provenance, safely reuses evidence, and continues working under partial source failure.

### Story 3.1: Model Evidence Completeness, Provenance, and Unknowns

As a renter,
I want Beacon to track what it knows, what it inferred, and what is still missing for each candidate,
So that later recommendations can be trustworthy instead of pretending certainty.

**Acceptance Criteria:**

**Given** candidate records exist for a session
**When** Beacon prepares them for enrichment
**Then** each candidate supports explicit evidence-state fields that distinguish `retrieved`, `inferred`, and `unknown` data
**And** retains provenance metadata for important facts that may influence ranking or explanation

**Given** a candidate has incomplete property information
**When** Beacon evaluates it
**Then** missing information is represented explicitly in persisted state
**And** the system can identify what enrichment work remains before recommendation synthesis

**Given** evidence-state modeling is in place
**When** downstream workflows read candidate data
**Then** they can rely on canonical Convex state instead of duplicating ad hoc completeness logic elsewhere
**And** recommendation and operator surfaces can later expose evidence quality consistently

### Story 3.2: Enrich Shortlisted Candidates with External Supporting Evidence

As a renter,
I want Beacon to gather building, layout, and neighborhood context beyond the source listing,
So that shortlisted properties become decision-useful even when original listings are incomplete.

**Acceptance Criteria:**

**Given** a shortlist exists for a session
**When** enrichment is launched
**Then** Beacon creates persisted enrichment workflow records for shortlisted candidates
**And** uses external retrieval paths beyond the original listing page to gather supporting evidence

**Given** new supporting evidence is found
**When** Beacon stores enrichment outputs
**Then** candidate records are updated with additional property, building, or contextual facts
**And** the original source references and provenance are preserved alongside newly retrieved information

**Given** one candidate remains sparse after initial enrichment
**When** Beacon determines key recommendation inputs are still missing
**Then** the system can continue enrichment iteratively within bounded workflow rules
**And** records the candidate's remaining gaps explicitly rather than silently stopping

### Story 3.3: Reconcile Conflicting Facts and Persist Confidence-Aware Candidate State

As a renter,
I want Beacon to handle conflicting facts across sources transparently,
So that the shortlist becomes more trustworthy instead of flattening disagreement into false certainty.

**Acceptance Criteria:**

**Given** two or more sources disagree on an important fact for a candidate
**When** Beacon evaluates the conflict
**Then** it applies explicit reconciliation rules or confidence-aware selection logic
**And** preserves enough provenance to explain which fact was chosen or why uncertainty remains

**Given** conflict resolution cannot safely produce a single authoritative answer
**When** the candidate is updated
**Then** Beacon stores the field in a partial or uncertain state
**And** avoids presenting the result as fully resolved truth

**Given** candidate records have reconciled outputs
**When** later ranking or recommendation workflows consume them
**Then** they can account for evidence strength and conflict state
**And** continue operating without requiring every field to be perfectly complete

### Story 3.4: Reuse Safe Evidence and Preserve Useful Output Through Partial Failure

As a renter,
I want Beacon to avoid wasting safe prior work and still return value when some retrieval paths fail,
So that the search remains useful under imperfect external conditions.

**Acceptance Criteria:**

**Given** cached evidence or prior artifacts exist for relevant entities
**When** Beacon evaluates whether to reuse them
**Then** it only reuses evidence that satisfies freshness and safety rules
**And** records freshness metadata so stale information does not silently distort recommendations

**Given** one or more enrichment or retrieval paths fail
**When** the session continues
**Then** Beacon records the degraded state in canonical workflow records
**And** preserves already-completed discovery or enrichment outputs instead of discarding them

**Given** a session cannot fully complete all intended enrichment work
**When** downstream flows read the session state
**Then** they can still access usable partial candidate outputs
**And** the renter-facing flow remains capable of producing a partial-ready recommendation path

## Epic 4: Recommendation, Comparison & Trustworthy Output

Renters can review a comparison-ready top three, understand strengths, weaknesses, tradeoffs, and uncertainty, receive a clear best recommendation, and click through with confidence.

### Story 4.1: Synthesize a Best Recommendation and Comparison Artifact

As a renter,
I want Beacon to turn enriched shortlisted candidates into one ranked recommendation artifact,
So that I can evaluate the best options without manually stitching evidence together.

**Acceptance Criteria:**

**Given** a session has a shortlist with enough evidence for synthesis
**When** recommendation generation runs
**Then** Beacon creates a persisted recommendation artifact in Convex for the session
**And** identifies one best overall recommendation plus the remaining ranked shortlist entries

**Given** the recommendation artifact is created
**When** it is stored
**Then** it remains linked to the underlying session, shortlisted units, and important evidence references
**And** can be re-read by renter-facing and operator-facing surfaces from canonical state

**Given** evidence quality is imperfect
**When** synthesis still has enough signal to proceed
**Then** Beacon can create a usable recommendation artifact without waiting for perfect completeness
**And** preserves the state needed for later partial-ready presentation

### Story 4.2: Present Strengths, Weaknesses, and Tradeoffs for the Top Three

As a renter,
I want Beacon to explain why each shortlisted property is strong or weak,
So that I can compare tradeoffs instead of only seeing opaque scores.

**Acceptance Criteria:**

**Given** a recommendation artifact exists
**When** the renter opens the recommendation experience
**Then** Beacon presents the top three shortlisted properties in a comparison-ready format
**And** each property includes strengths, weaknesses, and key tradeoff notes

**Given** multiple shortlisted properties satisfy different renter priorities
**When** Beacon renders the comparison
**Then** the UI makes the main tradeoffs legible across the top three
**And** keeps the explanation anchored to persisted candidate and recommendation data

**Given** the best recommendation outranks the others
**When** the renter views the final output
**Then** Beacon explains why that option was selected over the alternatives
**And** distinguishes recommendation reasoning from raw source facts

### Story 4.3: Communicate Evidence Strength, Uncertainty, and Workflow Credibility

As a renter or demo viewer,
I want Beacon to show how strong or incomplete the evidence is and that the result came from a real workflow,
So that I can trust the output without assuming false precision.

**Acceptance Criteria:**

**Given** the recommendation experience is rendered
**When** evidence is strong, partial, conflicting, or uncertain
**Then** Beacon communicates that evidence strength explicitly in the UI
**And** avoids presenting uncertain outputs as equally certain facts

**Given** the recommendation was produced through multi-stage workflow execution
**When** the result is displayed
**Then** the output reflects the lineage of discovery, enrichment, and synthesis
**And** makes it clear that Beacon performed real orchestration rather than serving a static mock result

**Given** the recommendation includes inferred or synthesized content
**When** the renter or demo viewer reviews it
**Then** Beacon distinguishes those outputs from directly retrieved facts
**And** keeps trust and explainability central to the experience

### Story 4.4: Support Source Clickthrough and No-Perfect-Match Outcomes

As a renter,
I want to click through to source listings and still get a useful recommendation when no option is perfect,
So that Beacon remains actionable in real housing search conditions.

**Acceptance Criteria:**

**Given** a shortlisted property appears in the recommendation output
**When** the renter chooses to inspect it further
**Then** Beacon provides clickthrough access to the original source listing
**And** preserves the mapping between the displayed recommendation entry and its source record

**Given** no shortlisted property is a perfect match
**When** Beacon produces the recommendation
**Then** it still returns the best available option and the top-three comparison
**And** frames the output as a tradeoff-aware recommendation rather than a false exact match

**Given** source or evidence quality is uneven across the shortlist
**When** the renter uses the final output
**Then** Beacon remains actionable and understandable
**And** does not collapse the recommendation surface simply because the data is imperfect

## Epic 5: Inferred Visuals & Durable Artifact Handling

Renters can see an inferred visual representation of the recommended property that is clearly labeled as inferred and durably linked to the evidence and artifact records used to produce it.

### Story 5.1: Persist Retrieved Visual Evidence as Canonical Artifacts

As a renter,
I want Beacon to preserve listing and layout imagery as durable session-linked artifacts,
So that visual evidence remains available for recommendation and downstream generation.

**Acceptance Criteria:**

**Given** discovery or enrichment retrieves listing images, floor plans, or building visuals
**When** Beacon ingests those files or URLs
**Then** it fetches and stores the actual image bytes in Convex file storage
**And** writes canonical `imageArtifacts` metadata records linked to the session and related candidate where applicable

**Given** an image artifact is persisted
**When** metadata is stored
**Then** the record includes provenance, provider information, and `storageId`-based linkage
**And** provider URLs are treated as transient inputs rather than durable application truth

**Given** visual artifacts are stored
**When** later workflows need them
**Then** they are retrievable from canonical Convex-backed records
**And** the application does not depend on unstable third-party image links remaining live

### Story 5.2: Generate an Inferred Visual Preview for the Best Recommendation

As a renter,
I want Beacon to generate a plausible visual preview of the recommended property from available evidence,
So that I can form a stronger mental model even when source imagery is incomplete.

**Acceptance Criteria:**

**Given** a best recommendation exists with usable visual or layout evidence
**When** inferred-visual generation is triggered
**Then** Beacon calls `fal.ai` through a server-side integration path
**And** stores workflow state for the generation request in canonical session records

**Given** `fal.ai` returns generated visual output
**When** Beacon ingests the result
**Then** it stores the generated image bytes in Convex file storage
**And** links the resulting artifact to the recommendation and underlying evidence records used to produce it

**Given** the available evidence is partial
**When** visual generation still proceeds
**Then** Beacon can create a plausible inferred preview from the best available inputs
**And** preserves the distinction between generated output and retrieved listing imagery

### Story 5.3: Present Inferred Visuals with Clear Labeling and Evidence Context

As a renter,
I want generated property visuals to be shown with honest labeling and context,
So that they help me understand the recommendation without misleading me into thinking they are exact reconstructions.

**Acceptance Criteria:**

**Given** an inferred visual artifact exists for the recommendation
**When** the renter views the recommendation experience
**Then** Beacon presents the generated visual as part of the recommendation flow
**And** labels it clearly as inferred rather than exact

**Given** both retrieved imagery and generated visuals may exist
**When** Beacon renders them
**Then** the UI distinguishes source imagery from inferred previews
**And** keeps the relationship to evidence and provenance understandable

**Given** the generated visual is derived from limited evidence
**When** it is shown
**Then** Beacon communicates that it is a grounded synthesis rather than a guaranteed reconstruction
**And** preserves trust in the broader recommendation artifact

### Story 5.4: Keep Recommendation Delivery Useful When Visual Generation Is Weak or Fails

As a renter,
I want Beacon to remain useful even if visual evidence is sparse or inferred generation is unsuccessful,
So that the recommendation does not depend on a perfect image-generation outcome.

**Acceptance Criteria:**

**Given** visual evidence is weak or generation confidence is low
**When** Beacon evaluates whether to show an inferred preview
**Then** it can still proceed with the recommendation flow without blocking on a perfect visual result
**And** records the visual limitation explicitly in session or recommendation state

**Given** visual generation fails for a session
**When** the final recommendation is rendered
**Then** Beacon still returns the recommendation artifact and any retrieved visual evidence that exists
**And** avoids turning a visual-generation failure into a full recommendation failure

**Given** a generated visual is unavailable or partial
**When** the renter reviews the output
**Then** the interface remains coherent and actionable
**And** preserves the evidence and recommendation context already available

## Epic 6: Operator Inspection & Troubleshooting

Operators can inspect runs, shortlisted units, evidence, artifacts, and degraded states so Beacon remains debuggable, recoverable, and demo-safe.

### Story 6.1: Provide an Operator-Only Session and Workflow Overview

As an operator,
I want a protected internal view of Beacon sessions and workflow runs,
So that I can monitor whether discovery, enrichment, synthesis, and visuals are functioning correctly.

**Acceptance Criteria:**

**Given** operator surfaces exist in the application
**When** an internal user accesses them
**Then** Beacon restricts access using the MVP operator-only access model
**And** renter-facing routes remain separate from internal troubleshooting views

**Given** an operator opens the overview surface
**When** sessions and worker runs are listed
**Then** they can inspect current and recent sessions with persisted statuses
**And** see enough high-level state to identify which stage a session is currently in

**Given** workflow execution degrades or stalls
**When** the overview is refreshed reactively or revisited
**Then** the operator can see that the session is partial, failed, or delayed
**And** can differentiate workflow-stage problems instead of seeing a generic failure bucket

### Story 6.2: Inspect Candidate, Evidence, and Artifact State for a Session

As an operator,
I want to inspect the units, evidence, and artifacts attached to a Beacon session,
So that I can understand what the system actually retrieved, inferred, and persisted.

**Acceptance Criteria:**

**Given** an operator selects a session
**When** they open its detail view
**Then** Beacon shows shortlisted candidates and related canonical records for that session
**And** preserves linkage between session, candidate, recommendation, and artifact state

**Given** the session includes stored evidence or images
**When** the operator inspects details
**Then** they can view provenance-aware evidence summaries and artifact metadata
**And** distinguish retrieved inputs from inferred outputs

**Given** a recommendation artifact exists
**When** the operator reviews its supporting state
**Then** they can trace it back to the underlying units, evidence, and image artifacts
**And** verify that canonical Convex records, not transient provider outputs, back the visible result

### Story 6.3: Diagnose Degraded Sessions and Partial Failures

As an operator,
I want Beacon to expose degradation and partial-failure details clearly,
So that I can recover from issues and trust the system in demos and live runs.

**Acceptance Criteria:**

**Given** a session experiences provider errors, partial retrieval failures, or incomplete downstream work
**When** the operator inspects the session
**Then** Beacon exposes failure state through persisted workflow and activity records
**And** shows which stage or dependency degraded without requiring raw provider logs in the renter UI

**Given** useful partial outputs still exist
**When** the operator reviews a degraded session
**Then** they can confirm what was successfully preserved versus what failed
**And** understand whether the renter-facing flow should still be able to deliver a usable partial result

**Given** troubleshooting surfaces are used during demos or testing
**When** issues occur mid-run
**Then** the operator can quickly inspect session state, candidate state, and artifact state in one coherent model
**And** use that visibility to diagnose rather than guess at system behavior
