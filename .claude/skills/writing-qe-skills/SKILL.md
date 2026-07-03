---
name: writing-qe-skills
description: Use when packaging a repeated QE technique or process into a new shared skill for this repo, or when editing an existing one — adding a skill, splitting one, or fixing why a skill never fires. Not for using a skill's process on an actual QE task (use the skill itself).
---

# Writing QE Skills

## Overview

**A good skill is a reusable REFERENCE — a proven technique, process, or checklist — not a story about the one time you solved a problem.** And the `description` field decides whether the skill gets *activated* at the right moment: it must state WHEN to use the skill, not summarize what the skill does. Get the description wrong and the skill is invisible or gets skimmed instead of followed.

## The trap this skill exists to prevent

A naive skill write-up narrates a single incident — "here's how I fixed the flaky login test last Tuesday" — dressed up with a `SKILL.md` header. It reliably **misses the judgment parts**:

- **Narrative, not reference.** Anecdote-shaped content can't be applied to a different but structurally similar problem. A skill must generalize past the one case that inspired it.
- **Description that summarizes instead of triggers.** "Helps you write better tests" tells an AI nothing about *when* to reach for it, and invites it to paraphrase the summary instead of opening the file.
- **Description that leaks the process.** If the description itself contains the how-to, the calling AI reads it, feels informed, and skips the full body — the exact shortcut failure this skill exists to prevent.
- **Orphaned original.** Editing only `.claude/skills/<name>/SKILL.md` and forgetting the adapters means Codex, Gemini, Cursor, Windsurf, and Copilot users never see the skill exists.
- **Vague noun name.** `testing-helper` or `qa-utils` doesn't say what action it covers and collides with other vague names later.

**A skill earns its place by being looked up in a different situation than the one that inspired it — if it only replays one story, it isn't a skill yet.**

## Process

1. **Decide the skill deserves to exist.** It must be a *repeated* technique, *non-obvious* (a naive pass gets it wrong in a specific, describable way), and *broadly applicable* — not a one-off fix or a convention specific to a single project. If you can't name the trap a naive attempt falls into, it's not ready to be a skill.

2. **Name it as an action.** Gerund or verb phrase, lower-case-hyphenated: `auditing-test-quality`, `generating-test-cases`, not `test-quality` or `qa-utils`.

3. **Write the `description` as "Use when ..."**, third person, listing triggers/symptoms only — never a summary of the process. Add a `Not for ...` clause naming the nearest neighboring skill so the two don't get confused or double-invoked.

4. **Write a dense body**: core principle (bold, one line), the trap, numbered process, one reference table, red flags, common mistakes. Pack in the keywords someone would actually search for — the body is what gets read once the description correctly triggers it.

5. **Generate every adapter.** Add a pointer entry in `AGENTS.md`, `GEMINI.md`, and `.github/copilot-instructions.md`; create `.cursor/rules/<name>.mdc` and `.windsurf/rules/<name>.md`; add a row to the skill table in `README.md`. One touch point missed means one tool silently can't see it.

6. **Test it on a real agent.** Hand a fresh agent an actual task and see if it finds and correctly follows the skill unprompted. Patch whatever was ambiguous — a skill that fails its own first real use isn't done.

## Touch points when adding one skill

| File | What changes |
|------|--------------|
| `.claude/skills/<name>/SKILL.md` | The source of truth — frontmatter `name` + `description`, full body |
| `AGENTS.md` | Pointer entry for Codex, trigger description + link to SKILL.md |
| `GEMINI.md` | Pointer entry for Gemini CLI, same pattern |
| `.github/copilot-instructions.md` | Pointer entry for Copilot |
| `.cursor/rules/<name>.mdc` | Thin adapter: Vietnamese trigger description + link + core principle |
| `.windsurf/rules/<name>.md` | Same as Cursor, with `trigger: model_decision` instead of `globs`/`alwaysApply` |
| `README.md` skill table | One row: name, link, one-line "when to use" |

## Red flags in a draft skill

- The description explains *what* the skill does rather than *when* to reach for it — an AI will read it and shortcut the body.
- The body reads as a single incident report, not a technique that transfers.
- An adapter for one tool (Cursor, Windsurf, AGENTS.md, GEMINI.md, or Copilot) is missing while others exist.
- The name is a vague noun (`helper`, `utils`, `qa-tools`) instead of an action.
- The new skill overlaps an existing one closely enough that an AI can't tell which to invoke — no `Not for ...` clause to disambiguate.

## Common mistakes

- **Stuffing "what it does" into the description** instead of "when to use it" — kills activation and invites shortcuts.
- **Updating the original SKILL.md and forgetting one adapter** — the skill silently doesn't exist for that tool's users.
- **Writing long and explanatory instead of dense and scannable** — a skill is a reference to look up mid-task, not an essay to read once.
- **Never testing the skill on a real agent** — ambiguity you can't see yourself is exactly what an agent trips over.
- **Creating a skill for a one-off** — if it won't recur across different tasks, it's a note, not a skill.
