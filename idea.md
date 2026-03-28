PRD: Beacon

1. Executive Summary

Beacon is an AI rental advisor that helps users discover, evaluate, and understand rental apartments through an orchestrated multi-agent workflow.

A user gives Beacon a natural-language housing brief, for example:

Find me a 1-bedroom apartment near Tanjong Pagar under S$3,800 with MRT access and good food nearby.

Beacon then searches broadly across rental sources, gathers supporting evidence for promising units, and returns a clear, visually polished recommendation rather than a generic list of listings.

The product is designed around two connected experiences:
	1.	A live search and evidence-gathering surface that shows Beacon actively scouting and assembling candidates.
	2.	A final single-page recommendation brief that presents the strongest match, explains why it fits, shows tradeoffs, and links the user to the original listing.

Beacon is not a property portal, booking flow, or transactional marketplace. Its primary value is decision support.

⸻

2. Product Vision

Beacon should feel like an intelligent rental scout and advisor.

The user should feel that:
	•	they explain what they want once,
	•	Beacon does the searching and comparison work in the background,
	•	and Beacon returns a composed recommendation artifact that helps them decide quickly.

Beacon should shift the rental experience from:
	•	fragmented browsing,
	•	incomplete listing interpretation,
	•	repeated tab-switching,
	•	separate map checking,
	•	and guesswork,

to:
	•	guided understanding,
	•	curated comparison,
	•	neighborhood-aware recommendation,
	•	and faster decision-making.

⸻

3. Problem Statement

Finding a good rental apartment is difficult because the information needed to make a decision is scattered, incomplete, and inconsistent.

Common user pain points include:
	•	listings are spread across multiple portals,
	•	listing quality varies heavily,
	•	core facts are often missing or buried,
	•	neighborhood context is not packaged clearly,
	•	users cannot compare homes beyond superficial price and photos,
	•	and listing images rarely help users understand what living there might actually feel like.

The hard part is not merely discovering that a unit exists.
The hard part is understanding:
	•	where it is,
	•	what is nearby,
	•	whether it fits the renter’s priorities,
	•	whether the listing is complete enough to trust,
	•	and whether the home is worth pursuing.

⸻

4. Product Positioning

Beacon is:
	•	an AI rental advisor,
	•	a rental scouting system,
	•	a recommendation and decision-support product.

Beacon is not:
	•	a booking platform,
	•	a property marketplace,
	•	a transaction workflow,
	•	a portal clone,
	•	or a full-market inventory platform.

This distinction should be visible in the product design, language, and user flow.

⸻

5. Goals

Primary Goal

Build a product that demonstrates how an orchestrated agent workflow can produce a stronger rental decision experience than browsing raw listing portals.

Secondary Goals
	•	show a credible agent-based evidence-gathering workflow,
	•	make Beacon feel trustworthy and transparent,
	•	create a polished recommendation artifact that feels differentiated,
	•	and support optional visual inference when sufficient visual evidence exists.

Non-Goals
	•	supporting rental transactions end-to-end,
	•	replacing listing portals as the inventory source of truth,
	•	building exact architectural reconstructions,
	•	or providing complete market coverage.

⸻

6. Target User

Primary User

An urban renter who has a reasonably specific housing brief but does not want to manually browse multiple portals, compare listings, and interpret neighborhood context alone.

Example needs:
	•	near a specific MRT station or district,
	•	within a strict budget,
	•	a specific bedroom count,
	•	and lifestyle constraints such as food access, commute convenience, or general livability.

Secondary User

A hackathon judge, early adopter, or technical reviewer who needs to understand both:
	•	the user value of the product,
	•	and the credibility of the orchestration system behind it.

⸻

7. Core User Stories
	•	As a renter, I want to describe what I want in natural language so I do not need to manually translate my needs into multiple portal filters.
	•	As a renter, I want Beacon to gather listing facts and neighborhood evidence on my behalf so I do not have to open multiple sources myself.
	•	As a renter, I want Beacon to tell me which home is the strongest fit and why.
	•	As a renter, I want Beacon to surface tradeoffs clearly, not just present a sales pitch.
	•	As a renter, I want a concise and visually clear recommendation page so I can quickly decide whether to open the original listing.
	•	As a demo viewer, I want to see that Beacon is doing real work in the background, not just rendering a static mock result.

⸻

8. Product Experience Overview

Beacon should be understood as a two-stage product.

Stage A: Search and Evidence Gathering

The user submits a housing brief. Beacon then begins searching, enriching, and comparing candidate units in the background.

During this stage, the user may see a live scouting surface that communicates:
	•	Beacon is searching,
	•	Beacon is gathering evidence,
	•	and Beacon is narrowing toward stronger candidates.

Stage B: Recommendation Artifact

Once the evidence is sufficiently complete, Beacon returns a single-page recommendation brief that presents:
	•	the strongest match,
	•	why it was chosen,
	•	what is nearby,
	•	what tradeoffs exist,
	•	and what to do next.

The recommendation brief is the primary user-facing artifact.

⸻

9. Primary Product Surface: Single-Page Recommendation Brief

Summary

The most important Beacon experience is the final recommendation brief.

This should feel like:
	•	an AI-generated home recommendation memo,
	•	a concise advisor deck,
	•	or a curated decision brief made specifically for the renter’s query.

It should not feel like:
	•	a dashboard,
	•	a browsing page,
	•	a booking funnel,
	•	or a property portal detail screen.

Core Purpose

The recommendation brief should answer:
	•	what Beacon recommends,
	•	why it fits the renter’s brief,
	•	what the local context looks like,
	•	what tradeoffs the renter should know,
	•	and whether the renter should click through to the source listing.

Core Sections

1. Search Brief Recap
A compact restatement of the renter’s original housing brief.

2. Hero Recommendation
Shows:
	•	recommended property,
	•	rent,
	•	key image,
	•	and one-line rationale.

3. Why Beacon Chose This
Highlights the strongest fit reasons, such as:
	•	transit convenience,
	•	budget fit,
	•	food and amenities,
	•	completeness of evidence,
	•	and balanced tradeoffs.

4. Property Visual Read
Uses listing photos and, if available, inferred room understanding to help the renter understand the home better.

5. Neighborhood Context
Shows:
	•	a map or spatial snippet,
	•	nearby essentials,
	•	and a concise neighborhood summary.

6. Honest Tradeoffs
Explicitly calls out weaker or uncertain aspects of the unit.

7. Alternatives
Briefly presents 1–2 alternatives and why they were not selected as the top recommendation.

8. Call to Action
Provides a clean path to the original listing.

Why This Is the Primary Surface

This format best expresses Beacon’s core value: transforming scattered rental evidence into one clear recommendation.

⸻

10. Supporting Product Surface: Live Search and Evidence Page

Summary

Beacon may also present a live search surface while the orchestrator is working.

This page is not the main product artifact. It is a support surface used to:
	•	communicate active search progress,
	•	make the system feel alive and credible,
	•	and show how candidate units are being discovered and enriched.

Core Purpose

The live page should help build trust by showing:
	•	discovery is happening,
	•	evidence is being gathered,
	•	and candidate quality is improving over time.

Conceptual UX

The live page should feel like:
	•	a calm intelligence workspace,
	•	map-first,
	•	progressively updating,
	•	and minimal.

The page should not feel like:
	•	a portal results page,
	•	a booking flow,
	•	or a noisy operations dashboard.

Core Elements
	•	a map as the primary visual anchor,
	•	a side rail of evolving candidate cards,
	•	and a subtle activity or status layer.

Role in Product Flow

This page supports the final recommendation brief, but it is not the destination of the product.

⸻

11. How the Product Works

At a high level, Beacon works by:
	1.	receiving a user’s natural-language rental brief,
	2.	dispatching specialist agents to gather listing and contextual evidence,
	3.	recursively filling missing details for promising units,
	4.	synthesizing the strongest recommendation,
	5.	and optionally generating inferred visual previews if enough evidence exists.

Beacon’s intelligence comes from:
	•	wide parallel search,
	•	structured evidence gathering,
	•	recursive gap filling,
	•	ranking and synthesis,
	•	and careful separation between known facts and inferred outputs.

⸻

12. Agent Architecture Overview

Beacon is powered by an orchestrated multi-agent system.

The product uses:
	•	a top-level orchestrator to manage the session,
	•	specialist evidence-gathering agents,
	•	synthesis logic to compose recommendations,
	•	and an optional visual inference pipeline.

The detailed technical design for this should be referenced from the orchestrator technical plan:

Reference: beacon_orchestrator_technical_plan.md

This PRD does not restate the full orchestration logic in detail. Instead, it assumes the orchestrator technical plan as the source of truth for:
	•	discovery swarms,
	•	recursive completion logic,
	•	worker routing,
	•	synthesis stages,
	•	visual inference flow,
	•	and storage/integration design.

High-Level Agent Roles

At a product level, Beacon relies on these agent responsibilities:
	•	discovery of candidate listings,
	•	extraction of detailed listing facts,
	•	neighborhood and amenity enrichment,
	•	visual evidence collection,
	•	confidence and validation checks,
	•	synthesis of recommendation logic,
	•	and optional visual inference.

⸻

13. Core Product Logic

Beacon’s product logic should follow this pattern:

1. Search widely first

Beacon should search broadly and in parallel to discover many candidate units quickly.

2. Narrow intelligently

Beacon should then shortlist the most promising units rather than deeply processing everything.

3. Fill missing information recursively

For shortlisted units, Beacon should keep collecting targeted evidence until each unit is recommendation-ready or sufficiently complete.

4. Synthesize clearly

Beacon should turn structured evidence into concise recommendation logic, tradeoffs, and final presentation sections.

5. Generate visuals only when justified

If enough floor plan and image evidence exists, Beacon may generate inferred visual outputs — but these should be treated as supporting artifacts, not the core promise.

⸻

14. TinyFish’s Role in the Product

TinyFish acts as Beacon’s browser-based evidence collection layer.

Beacon should use TinyFish for:
	•	navigating listing sites,
	•	applying search filters,
	•	extracting listing results,
	•	opening and reading detailed listing pages,
	•	retrieving image URLs and floor plans,
	•	and gathering external supporting evidence.

Beacon should not rely on TinyFish for:
	•	final ranking,
	•	final recommendation composition,
	•	image generation,
	•	or UI rendering.

TinyFish is the retrieval layer, not the final reasoning or presentation layer.

⸻

15. Data and Backend Overview

Beacon needs a persistent backend state layer to coordinate all agent work and power the frontend.

At a product level, the backend must support:
	•	search session lifecycle,
	•	candidate unit records,
	•	evidence updates,
	•	image and artifact storage,
	•	recommendation output storage,
	•	and frontend synchronization.

The current recommended backend backbone is Convex.

Convex’s Product Role

Convex serves as:
	•	the search session store,
	•	the shared source of truth for unit records,
	•	the worker run tracker,
	•	the artifact store for downloaded and generated images,
	•	and the live state layer that updates the frontend.

The detailed backend and state architecture should also reference:

Reference: beacon_orchestrator_technical_plan.md

⸻

16. Visual Inference Strategy

Summary

Beacon may optionally generate inferred visual previews of the apartment when enough evidence exists.

This can include:
	•	room understanding from listing images,
	•	floor plan to room-photo matching,
	•	and inferred preview images of what the unit may feel like.

Product Requirements

Any generated visual should be:
	•	clearly labeled as inferred or AI-generated,
	•	grounded in retrieved evidence,
	•	and never presented as a guaranteed reconstruction.

Why This Exists

This helps renters better understand a unit when listing images alone are insufficient.

Constraints

Visual inference should not be required for the core recommendation experience.

⸻

17. Functional Requirements

FR1

The system shall allow a user to enter a natural-language rental brief.

FR2

The system shall interpret that brief into a structured rental search intent.

FR3

The system shall search across one or more rental sources to discover candidate units.

FR4

The system shall collect and normalize evidence for shortlisted units.

FR5

The system shall enrich shortlisted units with neighborhood and amenity context.

FR6

The system shall recursively fill missing critical information for promising units until they are recommendation-ready or sufficiently complete.

FR7

The system shall rank shortlisted units according to the renter’s stated priorities.

FR8

The system shall generate a final recommendation brief that clearly explains the strongest match.

FR9

The system shall include honest tradeoffs and confidence notes in its recommendation output.

FR10

The system shall provide a link to the original listing.

FR11

The system should provide a live scouting surface while the system is gathering evidence.

FR12

The system may generate inferred visual outputs when sufficient evidence exists.

⸻

18. Non-Functional Requirements

Clarity

The final output must be easy to understand quickly.

Trustworthiness

The product must distinguish clearly between retrieved facts and inferred outputs.

Responsiveness

Users should see visible progress shortly after submitting a query.

Reliability

Failure in one enrichment path must not crash the entire recommendation flow.

Explainability

The product should explain why a recommendation was chosen in human-readable language.

Graceful Degradation

If some data cannot be retrieved, Beacon should still return a partial but useful recommendation when possible.

⸻

19. UX and Design Direction

Design Principles

Beacon should feel:
	•	minimal,
	•	premium,
	•	calm,
	•	trustworthy,
	•	and spatially aware.

Visual Character

The product should use:
	•	mostly white and soft grey surfaces,
	•	restrained color accents,
	•	strong typography hierarchy,
	•	and minimal chrome.

What It Should Avoid

Beacon should not feel like:
	•	a booking flow,
	•	a portal clone,
	•	a dense dashboard,
	•	or a generic AI gimmick.

Recommendation Brief Style

The main output should feel like:
	•	a recommendation memo,
	•	a curated single-page deck,
	•	or an advisor artifact.

Live Search Style

The live search surface should feel like:
	•	a calm intelligence workspace,
	•	not a portal or operations dashboard.

⸻

20. MVP Scope

Must Have
	•	natural-language rental query input,
	•	broad listing discovery,
	•	candidate shortlisting,
	•	evidence enrichment for top units,
	•	recommendation ranking,
	•	single-page recommendation brief,
	•	and source listing CTA.

Should Have
	•	live scouting surface,
	•	alternatives section,
	•	tradeoff and confidence notes,
	•	image and floor plan evidence collection.

Nice to Have
	•	richer confidence scoring,
	•	multi-source search,
	•	deck-like recommendation styling,
	•	and stronger visual evidence mapping.

Stretch
	•	inferred room previews,
	•	floor plan plus image scene understanding,
	•	and generated visual outputs.

⸻

21. Risks and Mitigations

Risk 1: Product drifts into a portal or booking flow

Mitigation:
	•	keep the recommendation brief as the primary product artifact,
	•	reduce controls after query submission,
	•	and keep the UI oriented around advice, not transaction.

Risk 2: Agent complexity becomes too broad

Mitigation:
	•	keep the MVP focused on discovery, enrichment, and synthesis,
	•	and reference the orchestrator technical plan for deeper execution logic.

Risk 3: Data remains incomplete or inconsistent

Mitigation:
	•	allow recursive evidence filling,
	•	use confidence and completeness thresholds,
	•	and support partial recommendations when necessary.

Risk 4: Generated visuals feel misleading

Mitigation:
	•	require explicit labeling,
	•	keep visual inference optional,
	•	and ground generation only in retrieved evidence.

Risk 5: External site variability affects runs

Mitigation:
	•	support only a small number of sources initially,
	•	keep agent tasks narrow,
	•	and degrade gracefully when some evidence paths fail.

⸻

22. Success Criteria

Beacon is successful if:
	•	a renter can submit a realistic rental brief in plain language,
	•	Beacon can discover and enrich promising units,
	•	Beacon can return a clear recommendation with supporting rationale,
	•	the final output feels meaningfully more useful than a raw portal listing,
	•	and the product communicates a credible multi-agent workflow.

⸻

23. References

Orchestrator Technical Plan

For the detailed orchestration system, recursive gap-filling logic, TinyFish worker design, visual synthesis pipeline, and backend state flow, reference:

beacon_orchestrator_technical_plan.md

This PRD intentionally does not duplicate the full orchestration specification.

⸻

24. Final Summary

Beacon is an AI rental advisor that searches widely, gathers evidence, fills missing details for promising units, and returns one clear recommendation the renter can act on.

Its value is not just in finding listings. Its value is in turning scattered rental evidence into a trustworthy, understandable, and well-presented recommendation.

The product should feel like a system that scouts, compares, and explains homes on the renter’s behalf — then returns a polished recommendation brief instead of making the renter continue the search alone.