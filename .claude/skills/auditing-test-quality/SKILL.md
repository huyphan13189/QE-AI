---
name: auditing-test-quality
description: Use when asked to audit, assess, review, or judge the quality or coverage of an existing test suite ("how good are our tests", "what's missing in tests", "test coverage audit"), or before trusting a green/high-coverage suite. Not for writing new tests (use testing skills for that).
---

# Auditing Test Quality

## Overview

**A green suite and high coverage % prove tests RUN, not that they TEST anything.** An audit hunts for *fake* tests (tautological, false-safety, echo-the-mock) and *un-exercised risk*, not just missing test files. Coverage tools and "all passing" are the starting point, not the answer.

## The trap this skill exists to prevent

A naive audit runs the suite, reads the coverage report, lists low-coverage files, and calls it done. That mechanical pass reliably **misses the judgment parts**:

- **Tautological / fake tests** — a test that asserts a mock was called with the value you just told the mock to return. Passes forever, guards nothing. High coverage, zero protection.
- **False-safety tests** — asserting on i18n *keys* instead of rendered text, `toBeDefined()` placeholders, "renders without crashing", snapshot-only. Green even when the visible behavior breaks.
- **Isolation anti-patterns** — leaking mocks between tests (missing reset), order-dependence, real timers/`Date`/network causing flakiness.
- **Coverage nuance** — a file with 0% *direct* coverage may be fully exercised through its parent (child component, wrapped util). Low coverage ≠ untested; high coverage ≠ well-tested.
- **The missing layer** — a suite that mocks the entire boundary (DB, network) can be 100% green while schema/contract drift ships to prod. "No integration/e2e layer" is often the biggest real gap and no coverage number reveals it.

**Judge what tests PROVE, not what they touch.**

## Process

1. **Inventory + run.** Count test vs source files per area. Run the suite ONCE (record pass/fail + wall time). Run coverage *if the tool already exists* (`--coverage`, vitest `--coverage`) — treat the % as a map of where to look, never as the verdict.

2. **Deterministic gap map.** Enumerate source files with no sibling test via a shell loop (don't eyeball). Exclude the genuinely-fine-untested: thin controllers whose services are tested, DTOs/types, config, barrel `index` files, entrypoints, generated code. Then **risk-rank the remaining gaps** — auth/session/security, money, data-writing, and scoring logic first.

3. **Quality read (the judgment step).** Read a *high-signal sample* of the EXISTING tests — prioritize money/security/scoring/core-flow suites — through the 7 lenses below. Explicitly hunt for fake/tautological/false-safety tests and call them out by `path:line`.

4. **Scale via fan-out.** If the suite is too big to read in one pass, dispatch parallel subagents split BY AREA (e.g. backend / web / each service), each applying all 7 lenses and returning `path:line` evidence + a risk-ranked gap list. Compute the gap map yourself (step 2) and hand it to them as input. Then synthesize.

5. **Synthesize.** Deliver: a one-paragraph honest verdict, per-lens findings with evidence, a unified **risk-ranked gap list**, and recommendations ordered by **value/effort**. Correct your own earlier assumptions when the reading contradicts them.

## The 7 lenses

| # | Lens | What to look for |
|---|------|------------------|
| 1 | Business-logic vs framework | Real behavior/rules asserted vs framework wiring or tautological mock-echoes |
| 2 | Integration / e2e coverage | Is ANY test run against a real DB / real network / booted app? Or is the whole boundary mocked (schema/contract drift ships green)? |
| 3 | Test value | Which tests guard money/security/scoring/data-integrity vs which guard nothing |
| 4 | Coverage gaps on high-risk code | Risk-ranked missing tests — mind covered-via-parent nuance |
| 5 | Isolation / anti-patterns | Shared state, order-dependence, real time/Date/network, missing mock resets |
| 6 | Assertion quality | Meaningful assertions & edge/error/boundary paths vs `toBeDefined`/`toBeInTheDocument`-only |
| 7 | Structure | Pure logic extracted & unit-tested, consistent mocking, harness quality, naming |

## Red flags of a fake test

- Asserts a mock returned what you configured it to return.
- Asserts on translation keys / test-ids / CSS classes instead of user-visible behavior.
- Only assertion is `toBeDefined`, `not.toThrow`, "renders without crashing", or a snapshot.
- Would still pass if the feature it "tests" were deleted.

**A file being green tells you nothing until you've read what it asserts.**

## Common mistakes

- **Trusting the coverage number.** 90% of tautological tests = 90% coverage, 0% protection.
- **Listing low-coverage files as "gaps" without judging risk** — and without checking covered-via-parent.
- **Skipping the run** — always run the suite; broken/orphaned test scripts (a `test:e2e` pointing at a deleted dir) are real findings.
- **Only finding missing tests, never fake ones.** The dangerous tests are the ones that pass while guarding nothing.
- **Reading everything serially** on a large suite instead of fanning out by area.
