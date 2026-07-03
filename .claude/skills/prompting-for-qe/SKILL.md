---
name: prompting-for-qe
description: Use when composing or refining a prompt for a QE task with AI — generating test cases, analyzing a requirement, summarizing test docs, parsing logs — especially when the output comes back generic, rambling, invented, or unusable, or when a prompt that finally worked needs to become a reusable template. Not for the test-case generation workflow itself (use generating-test-cases) or auditing an existing suite (use auditing-test-quality) — this is about prompt construction, applicable across any QE prompting task.
---

# Prompting for QE

## Overview

**Output quality on any QE task is upper-bounded by the quality of the prompt and context you supply — a vague prompt returns something that reads plausible but is unusable.** A prompt that actually works has five parts: a stated role, real task context, a clearly scoped task, a specified output format, and explicit constraints. Skip any of them and the model fills the gap with a guess.

## The trap this skill exists to prevent

"Write test cases for the login feature" looks like a complete instruction. It isn't. The model has no idea what the login feature actually does, so it invents plausible-sounding behavior, picks its own format, and returns something you have to rework by hand instead of use. Typical failure modes:

- **No context → invention.** Without the real requirement, constraints, or sample data, the model fabricates specifics ("locks after 3 attempts") that sound authoritative and are fiction.
- **No output format → manual rework.** Free-form prose or an ad-hoc table means every response needs reformatting before it's usable anywhere.
- **No few-shot examples → drift.** When a consistent shape matters (Gherkin, a fixed JSON schema, a specific severity taxonomy), an unillustrated prompt drifts from it on every run.
- **First draft accepted as final.** No read-through, no correction pass — whatever the model guessed on attempt one ships as-is.

## Process

1. **State the role and the task explicitly.** Tell the model what expertise to bring ("You are a senior QA analyst reviewing an API spec for edge cases") and name the single task, not a vague ask. A named role narrows the space of plausible answers before context even enters.

2. **Supply real, sufficient context — anonymized.** Paste the actual requirement, ticket, API contract, or log excerpt, plus constraints the model can't infer (roles, limits, dependencies, environment). Never paste real PII, credentials, or production secrets to "make it realistic" — use synthetic or scrubbed data that preserves the shape of the real thing.

3. **Specify the output format and constraints.** Say exactly what shape you want back — table, Gherkin, JSON with a given schema — and bound it (how many items, what scope, what to exclude). An unconstrained prompt gets an unconstrained, unpredictable-length answer.

4. **Use few-shot examples when the shape must be consistent.** One or two worked examples of input → desired output lock in format, tone, and level of detail far more reliably than a format description alone, especially across repeated runs by different people.

5. **Iterate before you trust it, then lock in a template.** Read the first output critically, point out exactly what's wrong (invented detail, wrong format, missed case) and ask for a corrected pass. Once a prompt reliably produces usable output, save it as a template for the next person doing the same task — don't rediscover it from scratch each time.

## Prompt anatomy

| Element | What to give | Why |
|---|---|---|
| Role | Who the model should act as (persona, seniority, focus) | Narrows tone and judgment before any content is supplied |
| Context | The real requirement/log/spec, plus constraints, anonymized | Prevents invented behavior filling the gaps |
| Task | One clearly scoped instruction | Multiple tasks in one prompt dilute all of them |
| Format | Exact output shape (table, Gherkin, JSON schema) | Removes reformatting work and ambiguity |
| Constraints | Count, scope, exclusions, length limits | Bounds an otherwise unpredictable response |
| Examples | 1–2 worked input → output pairs (few-shot) | Locks in shape and consistency across runs |

## Weak vs strong, same task

**Weak:** `Write test cases for the login feature.`
Returns invented, all-happy-path cases in an arbitrary format — reworked by hand.

**Strong:**
```
You are a senior QA analyst. Below is the actual login spec:
[paste spec: email+password; locks for 15 min after 5 failed attempts;
password min 8 chars, 1 upper, 1 digit; "remember me" keeps session 30 days].

Produce test cases as a Markdown table: | ID | Type (pos/neg/boundary) | Steps | Expected |.
Cover boundaries (attempt 4/5/6, password length 7/8), negatives (locked account,
wrong password, malformed email) and the remember-me session. Do NOT assert any
behavior not stated above; mark anything you infer as "assumption — confirm".
```
Role, real context, exact format, explicit coverage and constraints — usable as returned.

## Red flags in a QE prompt

- No output format is specified anywhere in the prompt.
- No real context is supplied — the model has to guess at the system under test.
- The first response is used as-is, with no read-through or correction pass.
- Several unrelated tasks are crammed into a single prompt.
- Real PII, credentials, or production data are pasted in for "realism."

**A prompt that runs and returns text isn't the same as a prompt that returns something usable.**

## Common mistakes

- **Treating the model as if it shares your context.** It only knows what's in the prompt — assume nothing carries over from your head.
- **Skipping few-shot when a strict shape is required.** A format description alone under-constrains; one good example does more work than another paragraph of instructions.
- **Never saving a prompt that worked.** If it took three iterations to get right, capture that version as a template instead of re-deriving it next time.
- **Padding the prompt with irrelevant detail.** Extra unrelated background dilutes the actual task and increases the chance the model latches onto the wrong part.
