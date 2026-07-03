---
name: designing-test-strategy
description: Use when planning testing for a feature, epic, or release, writing a test charter for an exploratory session, or deciding what to test first and what to skip. Not for generating detailed test cases (use generating-test-cases).
---

# Designing Test Strategy

## Overview

**A good test strategy is a decision about what to test first and what to skip — not an inventory of everything that could be tested.** AI is extremely good at listing test types (functional, boundary, security, performance, accessibility, load, ...); that list is not a strategy. The value is in ranking by risk and being explicit about what gets left out, and that ranking is a human call the model can inform but not make.

## The trap this skill exists to prevent

A naive prompt — "test strategy for feature X" — returns a sprawling checklist covering every test category in existence, with no ordering, no connection to what the feature actually does wrong when it breaks, and no statement of scope. It reads as thorough and is unusable for planning:

- **Unprioritized sprawl.** Twenty test types listed side by side, no signal on which matters most for this feature.
- **Generic risk.** "Security", "performance", "data loss" named without ever touching the feature's actual behavior — could be pasted onto any ticket unchanged.
- **No out-of-scope.** Never says what's being deliberately skipped or deferred, so nothing was actually decided.
- **Missing charter basics.** A "test charter" that's just a case list — no session objective, no defined scope boundary, no oracle for judging results.

**A strategy earns its name by cutting, not by covering.**

## Process

1. **Frame the feature.** Describe what the feature does, its context (who uses it, what's upstream/downstream), and **what's scariest if it breaks** — money lost, data corrupted, security exposed, reputation damaged. This framing is what keeps the next step from being generic.

2. **Brainstorm risks, then build a risk matrix.** Ask the model to brainstorm concrete failure modes for *this* feature, then organize them into a matrix of likelihood × impact. Reject any risk phrased generically enough to apply to a different feature unchanged — send it back for specifics.

3. **Derive test types and charters from the top risks.** For each high-likelihood/high-impact risk, work out which test types actually address it (functional, boundary, exploratory, security, performance, ...) and draft a test charter: session objective, scope, starting test ideas.

4. **Cut.** State explicitly what's out of scope and what's deferred to later. This is the step AI cannot do for you — the model can surface risks, but ranking them and deciding what to drop is a human judgment call, not the AI's to make unsupervised.

5. **Close it out as a short, usable document.** Ranked risks + charters + explicit scope. If it's longer than a page and still reads like a checklist, it isn't done cutting.

## Components of a good test charter

| Component | What to state |
|---|---|
| Mission | The one-sentence goal of this session — what you're trying to learn or break |
| Scope (in/out) | What area is covered and, explicitly, what's excluded from this charter |
| Risks & rating | The specific risks this charter addresses, with likelihood × impact |
| Test types | Which kinds of testing apply (functional, boundary, exploratory, security, perf...) |
| Test ideas | Concrete starting points for the session, not a full case list |
| Oracle | How you'll know if what you see is a bug — spec, comparable feature, common sense |

## Red flags in an AI-generated test strategy

- No risk ranking — every item presented as equally important.
- No stated out-of-scope; the strategy tries to cover everything.
- Risks are generic ("security", "performance") with no tie to the feature's specific behavior.
- A "charter" that's actually a test-case list — missing mission or scope.

**A strategy that doesn't say what to skip hasn't decided anything.**

## Common mistakes

- **Letting AI list instead of prioritize.** More test types is not a better strategy; ranking the ones that matter is.
- **Mistaking breadth for quality.** Covering every category on paper is not the same as covering the risk that actually matters.
- **Skipping the human risk-ranking step.** The model can surface risks; deciding which ones drive the plan is not its call.
- **Writing a charter at the wrong altitude.** A charter listing every test case has collapsed into a case list — it should read like a mission brief, not a script.
