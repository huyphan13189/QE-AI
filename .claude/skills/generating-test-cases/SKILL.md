---
name: generating-test-cases
description: Use when turning a requirement, user story, ticket, or API/UI spec into test cases with AI help — designing test scenarios, "generate test cases for this feature", expanding coverage, or reviewing an AI-produced case list. Not for auditing an existing automated suite (use auditing-test-quality), and not for general prompt-construction technique when the issue is a rambling/unusable prompt rather than coverage (use prompting-for-qe).
---

# Generating Test Cases with AI

## Overview

**AI produces a plausible, tidy list of test cases in seconds — and that speed is the trap.** The default output skews happy-path, silently invents behavior the spec never stated, and misses the boundary/negative/state cases where real bugs live. This skill drives the model toward *risk and boundary coverage* and forces every generated case to be verified against the actual spec or app before it's trusted.

## The trap this skill exists to prevent

A naive prompt — "generate test cases for this story" — returns 8–12 cases that *look* complete: create, edit, delete, valid login. It reliably **misses the judgment parts**:

- **Happy-path bias.** Mostly positive flows; few negatives, almost no boundaries or error paths.
- **Invented requirements.** The model asserts specific behavior ("shows error after 3 attempts") the story never defined. Looks authoritative, is fiction until confirmed.
- **Redundant filler.** Five variations of the same equivalence class, padding the count without adding coverage.
- **Missing dimensions.** No permission/role cases, no concurrent/state transitions, no data-variation (unicode, length, format), no what-happens-after-failure.
- **Untraceable.** Cases not tied back to a requirement line, so you can't tell coverage from noise.

**Judge cases by the risk they cover, not by how many the model produced.**

## Process

1. **Ground before generating.** Give the model the *actual* requirement text plus explicit constraints (roles, limits, formats, dependencies). First ask it to **list ambiguities and questions**, not cases — the gaps it surfaces are as valuable as the cases, and answering them prevents invented behavior. **If the spec is too thin to answer those questions — most cases would be "assumptions" — stop and get answers before generating a full suite.** A risk-ranked pile of guesses reads as coverage but isn't.

2. **Force the coverage dimensions.** Don't accept a flat list. Require the model to organize cases by the dimensions below and mark each as positive / negative / boundary. Explicitly demand negative, boundary, and error paths — that's where the default underperforms.

3. **Cut invention and duplication.** For every case, check: is this behavior actually stated in the spec, or inferred? Mark inferred ones as "assumption — confirm" instead of fact. Collapse cases that hit the same equivalence class to one representative + its boundaries.

4. **Verify against the real thing.** Walk a high-signal sample against the app/API (or the spec if pre-build). Delete cases the system doesn't support; add cases the walkthrough reveals. AI-generated ≠ correct until exercised.

5. **Risk-rank and format.** Order by impact (money, auth/permission, data integrity, then the rest). Output in the format your test management actually uses (table, Gherkin, CSV), each case traceable to a requirement.

## Coverage dimensions to demand

| Dimension | Ask for |
|-----------|---------|
| Equivalence classes | One representative per class, not five near-duplicates |
| Boundaries | min, max, zero, empty, just-over/just-under, off-by-one |
| Negative / invalid | wrong type, missing required, malformed, out-of-range |
| Error & recovery | what happens on failure, retry, timeout, partial success |
| State & sequence | order-dependent flows, concurrent actions, transitions |
| Roles & permissions | each role, unauthorized access, privilege boundaries |
| Data variation | unicode, very long, special chars, locale, format edge cases |

## Red flags in an AI-generated case list

- All or nearly all cases are positive/happy-path.
- A case asserts a specific behavior (limit, message, timing) not present in the requirement.
- Multiple cases differ only by an interchangeable value in the same equivalence class.
- No boundary or error-path cases for a feature that clearly has limits.
- Cases can't be traced back to any requirement line.

**A neat list of cases proves the model can write — not that the cases cover the risk.**

## Common mistakes

- **Accepting the first list.** The first pass is a draft; the coverage and cutting steps are where quality comes from.
- **Treating inferred behavior as spec.** If the story didn't say it, mark it an assumption and confirm — don't ship it as a requirement.
- **Counting cases as coverage.** 30 happy-path cases cover less than 8 that hit boundaries and errors.
- **Skipping the walkthrough.** Never trust generated cases you haven't checked against the spec or the running app.
- **Pasting sensitive data into the prompt** to "make it realistic" — use anonymized/synthetic values.
- **Risk-ranking a suite that's mostly assumptions.** When the spec is too thin to verify against, more cases just means more guesses — get the requirement clarified first.
