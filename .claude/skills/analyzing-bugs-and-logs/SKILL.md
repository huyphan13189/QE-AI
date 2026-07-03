---
name: analyzing-bugs-and-logs
description: Use when analyzing an error log, stack trace, or 500-level failure to localize the root cause, triage severity, or write a reproducible bug report; also use when a log is too long to read and needs summarizing. Not for writing automated tests (use writing-automation-tests) or fixing the code once the cause is confirmed.
---

# Analyzing Bugs and Logs with AI

## Overview

**AI summarizes logs and guesses a root cause fast and with total confidence — even when it's wrong.** Its output is a HYPOTHESIS to verify by reproduction, never a conclusion to act on. A confident tone is not evidence: an LLM will commit to a single story from a partial stack trace as readily as from a complete one. Logs also routinely carry secrets and PII, so they must be sanitized before they leave your machine, especially before pasting into an external chatbot.

## The trap this skill exists to prevent

You paste a stack trace and ask "what's wrong?" The AI comes back with one clean, plausible-sounding cause, stated with total confidence. You believe it, patch that spot, and ship a fix for the wrong bug — or write a bug report around a cause nobody confirmed. Failure modes: treating the AI's stated cause as fact; skipping reproduction to confirm it; a bug report missing concrete repro steps or expected-vs-actual; treating a single log line in isolation instead of the surrounding timeline; and pasting raw production logs — with tokens, keys, or user PII intact — into an external tool.

## Process

1. **Sanitize the log first.** Strip tokens, API keys, session IDs, and PII before handing anything to an AI, especially a third-party one. Redact, don't just eyeball — grep for common patterns (`Authorization:`, `password=`, emails, `Bearer `, connection strings).

2. **Ask for a summary plus MULTIPLE hypotheses**, ranked by likelihood — not a single verdict. If the AI gives you one confident cause, explicitly push it to list at least 2-3 alternatives and what would distinguish them.

3. **Verify, don't trust.** Reproduce the failure, or narrow it with concrete data/steps, to confirm or rule out each hypothesis. A human makes the call on which hypothesis is real — the AI's job stops at proposing candidates.

4. **Write the bug report** using the confirmed cause: title, environment, repro steps, expected vs. actual, severity/priority, and redacted supporting log/evidence. Never file a report whose only backing is an unverified AI guess.

5. **Triage.** Rank severity by impact × frequency, and attach only the confirmed suspect area — not every hypothesis the AI floated.

**Example.** A `NullPointerException` on `order.customer.email` at checkout. A quick AI read says "customer is null, add a null check" — sounds clean, one line, done. Pushed for alternatives, it also surfaces: (a) the customer record loads async and the exception fires on the first render before it resolves, (b) a specific guest-checkout path never attaches a customer at all, (c) a migration left `email` null on old records. Reproducing narrows it to (b) — guest checkout — which a blind null-check "fix" would have papered over while leaving the real gap in the order flow.

## Bug report structure

| Section | What it must contain |
|---|---|
| Title | One line: symptom + affected area, specific enough to search for later |
| Environment | Version/build, OS/browser/device, env (prod/staging), relevant flags |
| Preconditions | Account state, data setup, or config needed before the repro starts |
| Repro steps | Numbered, minimal, deterministic — another person can follow them cold |
| Expected vs. actual | What should happen vs. what actually happens, stated separately |
| Severity & priority | Impact × frequency, not gut feeling; note blocking vs. workaround-available |
| Evidence / logs | Redacted excerpt, screenshot, or trace — enough to confirm, not the whole file |

## Red flags

- Accepting the AI's stated root cause without reproducing it first.
- A bug report someone else can't reproduce from the steps alone.
- Only one hypothesis offered, with no way to distinguish it from alternatives.
- A log pasted to an AI still contains secrets, tokens, or PII.
- A root cause concluded from a single log line with no surrounding context.

## Common mistakes

- Treating the AI's hypothesis as the conclusion instead of a lead to test.
- Skipping reproduction because the AI's explanation "sounds right."
- Vague reports like "doesn't work" instead of expected vs. actual with concrete steps.
- Pasting entire production logs, secrets and all, into an external tool.
- Patching the symptom the AI pointed at instead of the confirmed root cause.
