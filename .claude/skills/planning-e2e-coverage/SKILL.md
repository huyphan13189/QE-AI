---
name: planning-e2e-coverage
description: Use when deciding what end-to-end tests a codebase needs — planning e2e coverage for the first time, choosing which user flows/screens to cover and in what order, tiering coverage as low/medium/high, assessing how well an existing e2e suite covers the app's flows/screens (the covered-vs-not baseline), or building a roadmap to grow an e2e suite. Not for writing an individual spec (use writing-e2e-tests), and not for judging whether the existing tests actually prove anything / catch bugs (use auditing-test-quality).
---

# Planning E2E Coverage

## Overview

**E2E coverage is a prioritized roadmap, not an inventory — you decide what to test FIRST and grow outward, you never "cover everything."** E2E is the slowest, most expensive tier; a hundred e2e specs is a liability, not an achievement. The value is ranking flows by risk and starting with a thin smoke gate over the money/main paths, then expanding deliberately.

## The trap this skill exists to prevent

Asked to "plan e2e for this app," a naive pass either lists every screen as equally worth testing, or writes one spec for whatever flow it happened to read first. Both fail the same way — no ranking, no baseline, no roadmap:

- **Flat inventory.** Every route treated as equal priority, so the login page and the payment flow get the same weight and the suite grows without direction.
- **No baseline.** Proposes tests without first checking what e2e already exists — duplicating covered flows while real gaps stay dark.
- **"Cover everything" ambition.** Aims for high e2e count instead of a thin, high-value gate — producing a slow suite that teams start skipping.
- **Main flows not first.** Edge cases and obscure screens get specs before the core revenue/auth journey does.
- **A plan with no growth path.** A one-time list with no sequencing, no "add one per feature merge," no stop condition — so it's never actually maintained.

**A coverage plan earns its name by ordering and cutting — a ranked roadmap with an explicit "not yet," not a checklist of everything.**

## Process

1. **Sweep the app to inventory real user journeys and screens.** Enumerate from ground truth, not memory: the route table (`App.tsx`/router config), the nav/menu, and backend controllers/endpoints. Produce a flat list of distinct *journeys* (a journey = a user goal spanning several screens: "sign up → onboard", "browse → add to cart → checkout"), not individual pages.

2. **Rank by risk — main flows first.** For each journey score **blast radius if it breaks** (money lost, data corrupted, auth bypassed, users locked out) × **traffic/centrality** (does every session pass through it?). The revenue path, authentication, and the app's core loop rank highest; admin corners and static pages rank lowest. This ranking is a human judgment the model informs, not decides.

3. **Baseline: map inventory → existing e2e.** Diff the journey list against the current specs — grep each spec's `page.goto()` routes and `test(...)` titles, and mark every journey **covered / partial / none**. This coverage matrix is the input to everything else: it stops you re-covering and shows the real gaps. **You can run this step standalone** — when someone asks "how well does our e2e cover the app's flows and screens?", steps 1–3 (inventory → rank → matrix) *are* the answer; you don't need the full roadmap. (This baseline answers "what's covered / how much"; to judge whether those covered specs actually *test* anything — fake/tautological specs — hand off to `auditing-test-quality`.)

4. **Tier the gaps as LOW / MEDIUM / HIGH** (see table). LOW = the smoke gate you build first; HIGH = deferred until justified. Assign each uncovered journey a tier from its rank in step 2.

5. **Sequence a growth roadmap with a stop condition.** Ship the LOW tier as the pre-deploy gate now. Then a rule for growth: **add one MEDIUM spec per feature merge that touches its area; defer HIGH until a flow has a production incident or ≥2 stakeholders ask.** State explicitly what's out of scope for now — the plan isn't done until it says what you're *not* testing yet.

6. **Hand each chosen journey to implementation.** For every journey you decide to cover, `writing-e2e-tests` builds the actual spec. This skill decides *what and in what order*; that skill decides *how*.

## Coverage tiers

| Tier | What it covers | Build it when |
|---|---|---|
| **LOW** — smoke gate | The 3–6 highest-risk main journeys, happy path only (auth boundary, the core loop, the revenue/checkout path). One assertion of the real outcome each. | **First, always.** This is the minimum pre-deploy gate; a red one blocks prod. |
| **MEDIUM** — main flows in depth | The same core flows' important branches + the next tier of features (secondary journeys, a key negative path or two per flow). | Incrementally — one spec as each feature area is built or changed. |
| **HIGH** — breadth & edges | Admin/back-office surfaces, rare screens, exhaustive edge/negative/empty states, cross-feature interactions. | Deferred — only when a flow earns it (a real incident, high traffic, or stakeholder demand). Don't front-load. |

## Relationship to neighboring skills

- **`designing-test-strategy`** plans testing for *one feature/release across all test types* (a risk matrix + charter). This skill plans *e2e coverage across the whole app* as a tiered roadmap. Use that for "how do we test this feature"; use this for "which journeys deserve an e2e and in what order."
- **`writing-e2e-tests`** implements a single chosen journey. Plan here, build there.
- **`auditing-test-quality`** judges whether existing tests *prove* anything (fake/tautological tests). This skill only maps *what flows are covered*, not how well.

## Red flags

- The plan lists screens/routes flatly with no risk ranking — login and checkout at the same priority.
- No baseline of what e2e already exists, so the plan risks re-covering flows or missing the real gaps.
- The goal is stated as a coverage % or a target spec count rather than "the main flows are gated."
- Edge cases / admin screens are tiered above the core revenue or auth journey.
- The roadmap is a one-time list with no sequencing and no explicit "not testing yet."
- Every uncovered journey is marked HIGH-priority — nothing was actually cut.

## Common mistakes

- **Inventorying pages instead of journeys** — a per-screen list misses that e2e tests *goals spanning screens*; rank journeys, not routes.
- **Skipping the baseline** — proposing specs without diffing against existing coverage wastes effort on covered flows and hides gaps.
- **Chasing a coverage number** — e2e value is a thin high-value gate, not a big count; more slow specs teams skip is negative value.
- **Front-loading HIGH tier** — writing edge-case and admin specs before the core loop is gated is effort spent where risk is lowest.
- **A plan with no stop condition** — without "defer HIGH until justified," the suite grows unbounded into a slow, flaky liability.
- **Confusing coverage with quality** — "the flow has a spec" ≠ "the spec would catch a bug"; map coverage here, then send the suite to `auditing-test-quality`.
