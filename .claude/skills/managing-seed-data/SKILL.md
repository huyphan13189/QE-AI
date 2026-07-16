---
name: managing-seed-data
description: Use when building or extending LOCAL seed data as a reusable, idempotent fixtures library shared by a dev seed CLI and e2e — adding a builder/scenario for a new feature's dev data, wiring an HTTP seed slice for e2e, or fixing seed that accumulates rows on rerun. Triggers "add seed data", "seed for screen X", "seed to stress the UI", "make the seed idempotent". Not for generating the diverse data VALUES themselves (edge/unicode/bulk — use generating-test-data), not for prod reference data that ships on deploy, and not for writing the e2e spec that consumes the seed (use writing-e2e-tests) — this is the seeding infrastructure and its lifecycle.
---

# Managing Seed Data

## Overview

**Feature dev/test data belongs in ONE shared fixtures library so the dev seed CLI and e2e both reuse it — a `builder` creates one domain slice, a `scenario` composes builders, and every path runs behind a fail-closed PROD guard and MUST be idempotent (rerun = same state, never accumulate).** Two builders producing the same data two ways is the bug this structure prevents.

**Two seed layers — never mix them:**
- **Prod reference data** (subjects, categories, config) → its own seed file, runs on deploy. Feature/test data NEVER goes here.
- **Local stress/dev data** → the fixtures library + a dev CLI. This is where all feature seed data goes.

## The one contract that bites: idempotency

Every scenario does `clean()` then recreates. `clean()` deletes ONLY tagged entities — rows whose marker column starts with a seed prefix (e.g. users with a `seed-` uid, records with a `[seed] ` name/label, entities with a `seed-` slug).

**So every row a builder creates MUST be removable on rerun — one of:**
1. It carries a seed prefix/marker that `clean()` already deletes, OR
2. It cascade-deletes because its parent is a deleted seed entity (a child row dies when its `seed-` parent is removed via `onDelete: Cascade`).

**THE TRAP — rows attached to the "POV"/persona user.** The persona user you seed data *for* (a real login, or an e2e account) is **un-prefixed and never deleted**. Any row you attach to that user that is NOT cascade-tied to a deleted seed parent will **accumulate +N every rerun**. This is silent — a smoke test that only counts users won't catch it.

**Fix pattern for a persona-owned table with no natural prefix:** pick a marker column (a `source`/`kind`/text field), set it to a seed marker in the builder, and add one `deleteMany({ where: { <marker>: { startsWith: 'seed' } } })` line to `clean()`. Then verify with the smoke test — it must assert the new table's count is EQUAL after a second run.

## Adding seed for a new feature — the touchpoints

| # | Where | What |
|---|-------|------|
| 1 | `builders/<feature>` | New `build<Feature>(db, opts)` — mirror an existing builder |
| 2 | the kitchen-sink scenario | Call the builder so the dev CLI covers it |
| 3 | `clean()` | **If rows aren't prefix/cascade-cleaned**, add a `deleteMany` by marker (see trap) |
| 4 | e2e seed endpoint/controller | A gated `POST seed/<feature>` for e2e; assert test-env FIRST; add the table to the reset/truncate list (FK child before parent) |
| 5 | e2e seed client | A `seed<Feature>()` helper the spec calls |
| 6 | idempotency smoke test | Extend the "count equal after 2nd run" assertion to the new table |

Steps 4–6 are only needed if e2e will use the data; steps 1–3 alone deliver the manual dev-CLI value.

## Edge-case data — always use a shared palette

Pull long/emoji/overflow/empty values from a shared edge-data palette, not inline literals. Cover the families that break UI: **long no-space names** (overflow), **unicode** (emoji / RTL / zero-width / diacritics / newline / empty / single-char), **boundary numbers** (0 / 1 / 9999 / 100000), and at least one **empty entity** (0 children) for empty-state. Names of prefixable entities go through the seed-name helper so `clean()` catches them. Keep the palette **deterministic** (index-based, no `Math.random`/`Date.now` for asserted values) so e2e stays stable — and so an e2e spec can import the SAME value to assert truncation/format.

## Guard — never bypass

A `assertLocalDatabase()`-style guard is the FIRST line of every scenario and the CLI, with independent fail-closed layers (host allowlist, cloud markers, DB-name allowlist, `NODE_ENV≠production`). Do not add a seed path that skips it, and never import the fixtures library into a prod-reachable entrypoint (release commands, deploy hooks). HTTP seed endpoints are separately gated (assert-test-env + only registered when `NODE_ENV==='test'`).

## Verify before done

1. **Typecheck** — field/enum names must match the real schema; this is the gate. Guess against the schema file, then let typecheck confirm.
2. **Idempotency smoke test** — run the kitchen-sink scenario twice, assert every seeded table's count is equal. If a persona-owned table you added isn't asserted, add it, or an accumulation leak ships silently.
3. **A real CLI run** against the local DB.

## Common mistakes

| Mistake | Reality |
|---|---|
| "clean() makes it idempotent" for persona-owned rows | Only if prefixed/cascade-tied. Un-prefixed persona rows accumulate every rerun. Tag a marker + extend `clean()`. |
| Put feature data in the prod reference seed | That ships on deploy. Feature data → fixtures library only. |
| Hard-code long/emoji strings inline | Use the shared edge palette so e2e can import the same values to assert truncation/format. |
| Skip the empty-state entity | Empty states break layouts too — seed a 0-child entity. |
| Guess enum/field names and ship | Confirm against the schema; run typecheck. |
| Forget the reset/truncate entry | e2e reset won't clear the new table → cross-test pollution. |
