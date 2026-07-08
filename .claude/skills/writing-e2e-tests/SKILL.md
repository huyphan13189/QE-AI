---
name: writing-e2e-tests
description: Use when writing or fixing a golden-path end-to-end test that drives a real browser through the real backend and database — a full user journey (login, checkout, core flow), or when an existing e2e spec is flaky or slow to set up. Covers state reset, seeding, auth bypass, and gated-entry deep-linking. Not for asserting a single API/UI unit (use writing-automation-tests), and not for designing which journeys to cover (use designing-test-strategy).
---

# Writing E2E Tests

## Overview

**An e2e test's reliability is decided before the first assertion — by how you get the app into the right state.** Most e2e pain is *setup orchestration* (flaky state, real OAuth, gated entry points), not the check at the end. Get the spine right and the test is stable; get it wrong and no amount of clever assertions saves it.

The spine every golden-path spec follows:

```
reset → seed → auth → enter → drive UI → assert visible outcome
```

**Prerequisite — you need the backend's cooperation.** A trustworthy e2e suite needs test-only affordances *in the app*: a reset endpoint, seed endpoints, a dev-login. If you can't touch the backend (a black-box third-party service), you can't build this properly — either get those affordances added, or accept a narrower test (real signup + UI-built state) and name the flakiness/speed cost. Don't pretend a black-box e2e is as solid as one with a real harness. Examples below are framework-agnostic in shape but assume Playwright + a SQL database; translate the concretes (`TRUNCATE`, cookie-planting, `build && preview`) to your stack.

## The trap this skill exists to prevent

Ask AI to "write an end-to-end test for the checkout flow" and it drives the whole thing through the UI from a cold browser — and that spec is slow, flaky, and often can't even run. The default output tends to:

- **Log in through the real Google/SSO button** — which can't run headless, so the test is dead on arrival or hangs.
- **Build state by clicking through the entire UI** (register → verify → create → navigate) instead of seeding — minutes per test, and every unrelated UI change breaks setup.
- **Share state between tests** (no reset) so specs pass alone but flake when run together or reordered.
- **Grind through a gated entry point** (a "reach level 2 first" rule, a wizard) instead of jumping straight to the flow under test.
- **Wait with `sleep`/fixed timeouts** and assert something loose that stays green even when the feature is broken.

A suite like this is slow *and* untrustworthy — the two things e2e most needs not to be.

## Process

1. **Pick ONE journey and its visible end-state.** A single golden path (highest-risk first — see `designing-test-strategy`) with a concrete outcome you can see on screen ("order confirmation shows total 5 items"). Don't test five flows in one spec.

2. **Reset to a known-empty state in `beforeEach`.** Expose a test-only reset (fast `TRUNCATE ... RESTART IDENTITY`) and run a single worker so specs never depend on each other's leftovers. Missing reset is the #1 cause of order-dependent flakes.

3. **Seed the world over an API/test endpoint, not through the UI.** Add test-only seed endpoints (mounted only outside production) that return the ids you need, wrapped in typed helpers. Don't click through setup, and don't hand-write DB inserts in the spec (see `generating-test-data` for building the data itself).

4. **Bypass every real side-effecting external integration — identity first.** Add a dev-only login endpoint that mints a session for a given email, guarded twice (not mounted in prod **and** a runtime prod check); plant the session the way the app cold-boots (e.g. set the session cookie, let the app restore it) and grab an access token for step 5. Never automate the real OAuth screen. **Same reasoning applies to payments, email/SMS, and third-party APIs** — route them to sandbox/test mode or a stub (a checkout e2e must hit the payment provider's *test* mode with a test card, never charge real money or send real mail). Caveat to state openly: planting a session means the **login UI itself is not exercised** — if logging in *is* the journey under test, that's the one case where you drive the login screen for real instead of bypassing it.

5. **Enter the flow — arrange prerequisites over the API, drive the steps-under-test through the UI.** The rule that resolves "UI or API?": anything the user asked you to *verify* runs through the UI; anything that's merely a *precondition* to reach it is arranged over the API. If the entry is gated, use the token to create the prerequisite entity, then navigate straight to its route; the app loads it like a reload. **Never finish the journey over the API** — if you do, you tested the API, not the journey. (So for "log in, add item, checkout": adding the item and checking out are the steps to verify → UI; a pre-existing product/catalog is a precondition → seed it.)

6. **Assert the exact visible outcome, on deterministic data.** Seed data whose correct path is recognizable (e.g. the right option carries a known label) so you can act and assert precisely instead of clicking blindly and matching a loose regex. Prove the assertion can fail — that discipline lives in `writing-automation-tests`; apply it here.

7. **Make it reproducible and gate the deploy with it.** Let the test runner start backend + web (build, don't dev, for build-time-env apps) against a dedicated test DB; capture trace/screenshot only on failure; run a single worker. e2e is the slow tier — run it as a **pre-deploy gate**, not on every PR (unit + integration cover PRs).

## The spine — why each step rots if skipped

| Step | Do | Rots into, if skipped |
|---|---|---|
| **reset** | Truncate all tables per test, single worker | Order-dependent flakes; passes alone, fails in CI |
| **seed** | Build state over test API endpoints | Minutes-long UI setup that breaks on any copy/layout change |
| **auth** | Dev-login endpoint plants a session | Test can't run headless through real OAuth |
| **enter** | Deep-link past gated entry, then drive UI | Grinds prerequisites every run, or can't reach the flow at all |
| **assert** | Exact outcome on deterministic data | Green even when the feature is broken |

## Red flags

- The spec automates the real Google/SSO login screen.
- The test hits a real payment gateway, sends real email/SMS, or calls a live third-party API instead of test-mode/sandbox/stub.
- Setup registers/creates everything by clicking through the UI instead of seeding.
- No `reset` (or equivalent isolation) in `beforeEach`.
- `page.waitForTimeout(ms)` / `sleep()` anywhere.
- The whole journey is executed via API calls with no real UI interaction.
- The final assertion is a loose regex or a bare 200/visible check that can't distinguish right from broken.

## Common mistakes

- **Logging in through the real identity provider** instead of a guarded dev-login endpoint — the test can't run headless.
- **Seeding by clicking through the UI** — slow and brittle; seed over a test API and reserve the UI for the flow under test.
- **Forgetting per-test reset** — the classic "passes locally, flakes in CI" from leaked rows.
- **Deep-linking over the API and then finishing over the API** — you moved the whole test off the UI and stopped testing the journey.
- **Silently dropping a step the user asked to verify.** Auth-bypass removes the login screen from coverage; if "can log in" was a stated requirement, say so and cover it separately (or drive login for real when it *is* the journey). Same for any step you shortcut over the API.
- **Running e2e on every PR** — it's the slow tier; gate the deploy with it and let unit + integration cover PRs.
- **Asserting something loose because the outcome "isn't determinable"** — usually it is once you seed deterministic data; only weaken the assertion when the result genuinely depends on timing, and then assert the weaker-but-true fact honestly.
