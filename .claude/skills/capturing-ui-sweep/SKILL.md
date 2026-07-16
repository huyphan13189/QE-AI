---
name: capturing-ui-sweep
description: Use when you want to screenshot the whole app's UI to self-review for broken/misaligned layout, missing data, ugly empty-states, and UX to improve — a visual QA sweep that groups shots by feature and runs one AI pass to flag issues. Triggers "capture the whole UI", "visual QA sweep", "screenshot every screen and review", "snapshot the app grouped by feature". Not for judging whether existing tests actually catch bugs (use auditing-test-quality) or planning which journeys deserve e2e (use planning-e2e-coverage) — this is a report-only visual pass, it never edits UI code.
---

# Capturing UI Sweep

## Overview

**A visual QA sweep captures every curated feature/state as an image, groups the shots by feature into one index page, then runs a SINGLE AI pass to flag layout/data/empty-state/UX problems — and it NEVER touches UI code.** The output is a report for a human to review, not a fix.

The value is catching what unit and e2e assertions can't see: a heading that overflows on mobile, a card that collapses when a name is long, an empty-state that looks broken, spacing that's off. You look at the whole app at once, grouped, in two viewports.

## The trap this skill exists to prevent

A naive "screenshot the app and check it" attempt fails in four predictable ways:

- **Ad-hoc, non-reproducible shots.** Screenshots taken against the dev database with whatever data happens to be there — so the sweep can't be rerun and the "bugs" are just today's stale data.
- **Capture logic tangled into each shot.** Every screen gets its own bespoke script instead of one generic runner driven by a registry, so maintenance explodes and adding a state is a rewrite.
- **Reviewing while capturing (or fixing while reviewing).** Mixing the three concerns — capture, index, review — means shots get skipped, findings get lost, and the agent starts "helpfully" editing UI mid-sweep, corrupting the very thing it's inspecting.
- **Skeletons and one-viewport-only.** Shooting before the page paints captures loading skeletons as if they were the UI; shooting one viewport misses the responsive breakage that's the whole point.

**A sweep earns its keep by being rerunnable against a reset+seeded test database and separating capture → index → review into three isolated steps.**

## The three pieces (architecture)

Keep these separated — a change to one shouldn't force a change in another:

| Piece | Role |
|-------|------|
| A **registry** (`FeatureBlock[]`) | The ONLY thing you maintain as UI changes: each block declares a feature, its states, and a `setup()` that seeds + navigates. |
| A **generic capture runner** | Iterates the registry, drives the browser, writes one PNG per shot + a `manifest.json`. No per-screen logic lives here. |
| A **build-index step** | Turns `manifest.json` into a single `index.html` grouped by feature, and embeds AI findings if a `review.json` exists. |

## Run process

1. **Capture** against a **reset + seeded dedicated test database** — never the real dev DB (you'd lose reproducibility). Runner produces PNGs + `manifest.json`.
2. **Build the index page** from the manifest.
3. **AI review pass — this is the agent's job, not a script:**
   - Read `manifest.json`.
   - For EACH feature (run independent features in parallel via subagents): Read that feature's PNGs (**both viewports**), compare against UI/design guidelines, and list suspicions: **horizontal overflow / misalignment / missing data / ugly empty-state / spacing / UX to improve**.
   - Write findings to `review.json` keyed by feature slug: `{ "<feature-slug>": "- finding 1\n- finding 2" }`.
4. **Rebuild the index** so it embeds the findings.
5. **Tell the user to open the index page** and summarize the heaviest suspicions. Do NOT fix anything.

## Adding a new FeatureBlock (maintenance)

Append one object to the registry. Each `setup()`:
- **Seed only the SUPPLEMENT** the state needs — the runner already reset + seeded the kitchen-sink once; don't re-reset. Pick by state:
  - `full`/`empty`: deep-link an existing route using refs from the seeded data.
  - `edge`: seed long/emoji/overflow names (use your seed edge palette — see managing-seed-data).
  - `modal`/`toast`: navigate → trigger it → `await` it visible.
  - `loading`: capture right BEFORE network-idle to catch the skeleton.
- **`await` the main element painted** (heading/CTA) before shooting — don't capture a skeleton (except the `loading` state).
- **Use durable selectors** (`getByRole`/`getByText` by accessible name), not brittle CSS classes.
- **Suppress first-run interstitials** (onboarding sheets, "what's new" overlays) that block pointer events, or clicks in `setup()` will fail silently.
- Re-run capture + build-index to confirm the new shot produces an image and lands in the index.

## Do not

- Do **not** edit UI code during a sweep — report only.
- Do **not** capture against the real dev DB (kills reproducibility).
- Do **not** treat pixel snapshot-diff as a prerequisite — this is a curated human-reviewed sweep, not automated diffing.

## Common mistakes

| Mistake | Reality |
|---|---|
| Screenshot against dev DB | Not reproducible; "bugs" are just today's data. Reset + seed a test DB. |
| One bespoke script per screen | Doesn't scale. Drive everything from one registry + generic runner. |
| Fix UI while reviewing | Corrupts the thing you're inspecting. Sweep reports; a human decides. |
| Capture before paint | You snapshot skeletons. `await` the main element (except the `loading` state). |
| Only one viewport | Misses responsive breakage — the main reason to sweep. Shoot both. |
