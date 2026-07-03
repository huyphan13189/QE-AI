---
name: writing-automation-tests
description: Use when writing, extending, or maintaining automated tests (API or UI) with AI assistance — generating a test scaffold, converting manual test cases into scripts, adding coverage for a new endpoint or flow. Not for auditing an existing automated suite (use auditing-test-quality), and not for designing test cases from a requirement (use generating-test-cases).
---

# Writing Automation Tests with AI

## Overview

**AI writes automated tests fast, but by default it writes tests that pass FOREVER — not tests that fail when there's a bug.** A test only has value if it can go red at the right moment. Speed from AI is real; the discipline of proving the test actually catches something is what makes that speed safe to use.

## The trap this skill exists to prevent

Ask AI to "write a test for this endpoint" and you get something green on the first run — and that's the problem, not the reassurance. The default output tends to:

- Assert only `status === 200` or "didn't throw," never the actual response shape or business rule.
- Use brittle selectors — visible text, `nth-child`, DOM position — that break on the next copy change.
- Mock a dependency by echoing back the exact value the test just set, so the assertion can never fail.
- Skip the negative case entirely — no bad input, no auth failure, no empty state.
- Depend on execution order or wall-clock timing, so it passes locally and flakes in CI.

A suite full of these is worse than no suite: it reports full coverage while guarding nothing.

## Process

1. **State the expected behavior before generating anything.** Write input → expected output/side-effect in plain language (e.g., "POST with a duplicate email returns 409 and does not create a row"), not just "test this function."

2. **Generate, then READ the assertions line by line.** AI-written tests read as plausible; check what each `expect`/`assert` actually pins down. Replace status-only or type-only checks with checks on the real business rule.

3. **Prove the test can fail — run both directions.** Run it once against correct behavior (green) and once after deliberately breaking the behavior or feeding bad input (must go red). **If you can't make it fail, the test isn't testing anything** — fix the assertion before moving on.

4. **Harden for durability.** Swap brittle selectors for `data-testid`/role-based locators, replace `sleep(n)` with waiting on a condition/event, and isolate state (fresh fixtures/db per test, reset mocks between tests) so order and timing don't matter.

5. **Maintain deliberately.** When a test goes red later, first decide: real bug or brittle test? Ask AI to explain the diff, but the human decides whether to fix the code, fix the test, or fix the selector — never auto-accept an AI's "just update the assertion" fix without checking it doesn't quietly weaken the test into one that always passes.

## Checklist for a test worth trusting

| Check | Why it matters |
|---|---|
| Asserts behavior, not just status/type | `200 OK` proves the route responded, not that it did the right thing |
| Has a negative/edge case, not just the happy path | Most real bugs live in error handling and boundaries |
| Uses stable selectors (`data-testid`, role, API contract field) | Text/position selectors break on unrelated UI or copy changes |
| No hard-coded `sleep`/fixed delay | Timing-based waits are the #1 source of flaky CI |
| State isolated per test (fresh fixtures, reset mocks) | Prevents order-dependence and cross-test pollution |
| You've watched it fail at least once | The only proof the test can catch the bug it claims to guard against |

Example (Playwright + TypeScript) — checking behavior, not just a toast appearing:

```ts
test("duplicate signup email is rejected", async ({ page, request }) => {
  await request.post("/api/signup", { data: { email: "dup@x.com", password: "x" } });

  await page.getByTestId("email-input").fill("dup@x.com");
  await page.getByTestId("password-input").fill("x");
  await page.getByTestId("signup-submit").click();

  await expect(page.getByTestId("form-error")).toHaveText(/already registered/i);
  await expect(page.getByTestId("signup-submit")).toBeEnabled(); // didn't silently succeed
});
```

## Red flags in AI-generated tests

- Only assertion is `toBeDefined()`, `not.toThrow()`, a 200/OK check, or a snapshot.
- No case where the input is wrong, missing, unauthorized, or a boundary value.
- Selector is raw text, CSS class, or `nth-child`/index-based.
- A mock returns exactly the value the test just configured it to return.
- The test has never been run against a deliberately broken version of the code.

## Common mistakes

- **Trusting a green test you've never seen fail.** Green-on-first-run is not evidence — it's the default AI output.
- **Letting AI pick the selector without review.** It reaches for visible text first; you have to redirect it to `data-testid`/role.
- **Using `sleep(ms)` instead of waiting on a condition.** It's the easiest fix to accept and the easiest source of flakiness later.
- **Asking for a batch of shallow tests instead of fewer deep ones.** Ten tests that each check one status code cover less real risk than two that check full behavior plus edge cases.
- **Not extracting pure logic for unit tests.** Business rules buried inside a UI/API handler end up only reachable through slow, brittle end-to-end tests.
