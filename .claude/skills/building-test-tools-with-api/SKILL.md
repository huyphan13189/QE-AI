---
name: building-test-tools-with-api
description: Use when automating a repetitive QE task by calling the Claude API/SDK from code — a script or pipeline that reads a requirement and generates test cases, reads an API spec and generates a checklist, or triages logs/results in bulk. Not for using an interactive AI coding assistant (that's a different workflow), and not for choosing an off-the-shelf AI test tool.
---

# Building Test Tools with the Claude API

## Overview

**Calling an LLM from code gives you real automation power — and three things quietly kill a "real" tool: a leaked API key, a hard-coded model ID that goes stale, and uncontrolled cost or reliability.** A tool that works in your first manual run but breaks on the first batch, the first network blip, or the first `git push` isn't a tool — it's a demo. Read the API key from the environment, choose the model and parameters from current documentation instead of hard-coding an ID, and build in error handling plus token/cost awareness from the start.

## The trap this skill exists to prevent

It's fast to write a script that hard-codes the API key and a model ID straight into the source, skips error handling, and fires off a loop over a hundred inputs. It runs once, looks great, ships. Then:

- The key gets committed to git the first time someone forgets it's in the file — now it's rotated, revoked, and possibly abused before anyone notices.
- The hard-coded model ID becomes outdated as providers ship newer models; the tool silently keeps using a worse or deprecated one.
- One transient network error or rate limit kills the whole batch halfway through, with no way to know what succeeded.
- The token bill for a "quick test" turns out to be nothing like what was expected, because nobody estimated it before running on the full dataset.
- The tool trusts the model's raw text output and feeds it straight into a QE process, so a malformed or hallucinated response corrupts downstream results.

**Judge a tool by whether it survives a bad day (leaked laptop, network blip, provider model update, bulk run) — not by whether the demo run worked.**

## Process

1. **Security first.** Read the API key from an environment variable (or a secrets manager), never a string literal. Add `.env` (or equivalent) to `.gitignore` before the first commit. Never log the key, even in debug output.
2. **Choose the model and parameters from current documentation** — look up the Claude API docs or use the `claude-api` skill for the current model lineup, context limits, and pricing. Store the model name as a config value or environment variable, not a hard-coded string, so it can be updated without touching code.
3. **Structure the request.** Use a system prompt that states the tool's role and constraints, and a user prompt that carries the actual context (spec, requirement, log excerpt). Constrain the output format (e.g., JSON with a defined schema) so the response is machine-parseable, not free text you have to guess at.
4. **Handle errors and cost.** Wrap calls in try/catch with a timeout; retry transient failures (rate limits, network errors) with backoff, but fail loud on non-retryable errors. Read token usage from the response to estimate cost before and during a bulk run, and cap batch size so a bug doesn't burn the whole budget in one run.
5. **Verify the output like any AI output.** Parse defensively, validate against the expected schema, and reject/flag responses that don't fit before they enter a QE pipeline. Never let unvalidated model output flow straight into a report, a test suite, or a triage decision.
6. **Package it for reuse.** Wrap the logic in a CLI or script with parameters (input path, batch size, model override) instead of a one-off notebook cell, so the next person can run it without reading the source.

## Example

```python
import os
import json
from anthropic import Anthropic

# Model comes from config/env, not a hard-coded literal —
# look up the current model id in the Claude API docs / claude-api skill.
MODEL = os.environ["QE_TOOL_MODEL"]

client = Anthropic(api_key=os.environ["ANTHROPIC_API_KEY"])

def generate_checklist(spec_text: str) -> list[dict]:
    try:
        response = client.messages.create(
            model=MODEL,
            max_tokens=1024,
            system="You are a QE assistant. Output only valid JSON: a list of "
                   '{"item": str, "risk": "low"|"medium"|"high"}.',
            messages=[{"role": "user", "content": spec_text}],
            timeout=30,
        )
    except Exception as exc:
        # Distinguish retryable (rate limit/timeout) from fatal in real code.
        raise RuntimeError(f"API call failed: {exc}") from exc

    print(f"tokens used: in={response.usage.input_tokens} out={response.usage.output_tokens}")

    try:
        items = json.loads(response.content[0].text)
    except (json.JSONDecodeError, IndexError) as exc:
        raise ValueError(f"model returned unparseable output: {exc}") from exc

    if not isinstance(items, list):
        raise ValueError("expected a JSON list of checklist items")
    return items
```

## Checklist for a trustworthy API-based tool

| Check | Why it matters |
|-------|-----------------|
| Key read from ENV, never hard-coded | Prevents leaks on commit, sharing, or screen-share |
| Model is config, not a hard-coded ID | Avoids silently running a stale/deprecated model |
| Output format constrained (e.g., JSON) | Makes the response reliably parseable, not guessed at |
| Error handling & retry with backoff | One transient failure doesn't kill a whole batch |
| Token usage estimated & batch capped | No surprise bill from a bulk run |
| Output validated before use | Stops a malformed/hallucinated response from corrupting QE data |

## Red flags

- API key written in source, in a notebook cell, or found in a commit diff.
- Model ID hard-coded as a literal string anywhere in the code.
- No try/catch, no timeout, no retry around the API call.
- No check of token usage or cost before running on the full dataset.
- Model output parsed with blind trust (`json.loads` with no try/except, no schema check) and passed straight into a report or pipeline.

## Common mistakes

- Hard-coding the key and/or model "just for now" — it always ends up in the commit history.
- Skipping error handling so one bad response kills the entire batch with no partial results saved.
- Not constraining the output format, then writing fragile regex to scrape a JSON blob out of free text.
- Trusting AI output as fact and feeding it into a QE decision without validating it first.
- Running a bulk job on the full dataset without estimating token cost on a small sample first.
