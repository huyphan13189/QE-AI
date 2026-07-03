---
name: generating-test-data
description: Use when generating test data with AI help — synthetic users/records for UI, API, or DB tests, seed data, fixtures, bulk/load datasets, or any request like "generate 50 test users". Not for designing test cases from a requirement (use generating-test-cases).
---

# Generating Test Data with AI

## Overview

**AI produces realistic-looking test data almost instantly — and "looks realistic" is not the same as "is usable."** The default output clusters around clean, valid, happy-path values, and it will drift from your schema the moment it isn't pinned down. Good test data is judged on three things: does it cover the boundary and invalid cases, not just the pretty ones; does every record actually satisfy the schema and constraints; and is it synthetic all the way through, with zero real customer/PII data anywhere in it.

## The trap this skill exists to prevent

"Generate 50 test users" comes back as 50 records that are all subtly the same: valid emails, plausible names, round numbers, no empty strings, no unicode, nothing over a length limit. It reliably fails in three ways:

- **Happy-values only.** No nulls, no zero/negative/max-int, no empty or over-length strings — the exact inputs bugs hide behind.
- **Silent schema drift.** A field has the wrong type, a required field is missing, a date is in the wrong format, or a foreign-key reference points nowhere — and it isn't caught until the load/insert fails (or worse, doesn't).
- **Reach for real data "to make it realistic."** Someone pastes a prod export or anonymizes it "well enough" to get variety fast — and PII or real customer records end up in a test fixture or, worse, a repo.

**Realistic-looking is not the bar. Schema-valid, boundary-covering, and 100% synthetic is.**

## Process

1. **State the schema, volume, and output format up front.** Fields, types, constraints (required, ranges, formats, uniqueness), relationships (foreign keys, referential integrity), the count needed, and the exact output format (JSON, CSV, SQL insert, etc.) you'll actually load. Vague input produces data you have to hand-fix.
2. **Demand every coverage group, not just valid rows.** Explicitly ask for valid, boundary, invalid, unicode/special-character, and bulk-volume records — see the table below. Left unstated, the model defaults to group 1 only.
3. **Verify against the schema before trusting it.** Validate or load-test a sample; don't take the model's word that types, required fields, and constraints are correct. Delete or fix any record that violates a constraint — a boundary case that's also schema-invalid is a bad test fixture, not a good one.
4. **Keep it synthetic, always.** Never paste in or seed from real customer/prod data "for realism." Use fabricated names, emails, and IDs; flag anywhere compliance/PII rules apply (e.g., a "real-looking" SSN or card number format).
5. **Save the generation prompt.** Once the schema/coverage ask is dialed in, keep it as a reusable template for the next batch of the same shape — regenerating from scratch each time reintroduces the happy-path drift.

## Coverage groups to demand

| Group | Ask for |
|-------|---------|
| Valid | Well-formed records spanning the realistic range of the schema |
| Boundary | min/max, zero, empty string, empty array, just-over/just-under a limit |
| Invalid | wrong type, missing required field, out-of-range value, malformed format |
| Unicode & special chars | non-Latin scripts, emoji, quotes/backslashes, whitespace-only strings |
| Bulk volume | the actual N needed for load/perf testing, not a handful padded up |
| Relational integrity | valid and dangling foreign keys, duplicate vs. unique-constraint values |

## Red flags in AI-generated test data

- Every record is clean, valid, and well within normal ranges — no boundaries, no nulls, no empties.
- Records that violate a stated constraint (wrong type, missing required field, invalid enum value).
- No unicode, special-character, or over-length entries anywhere in the set.
- Anything that looks like it could be a real name, email, phone number, or ID lifted from production.
- Output format doesn't match what your loader/test harness actually consumes.

**A big, tidy batch of records proves the model can pattern-match a schema — not that the data is safe or loadable.**

## Common mistakes

- **Trusting schema-correctness without checking.** Validate a sample against the actual schema or load it; don't assume the model got types and constraints right.
- **Forgetting boundary and invalid cases.** "Generate test data" defaults to happy values unless you name the other groups explicitly.
- **Reaching for prod data "just this once" for realism.** Anonymization is easy to get wrong; synthetic data has zero PII risk by construction.
- **Not specifying output format.** Getting back a shape you then have to manually convert wastes the speed AI was supposed to give you.
- **Generating bulk volume without validating it.** A script that inserts 10,000 rows with a handful of schema violations fails loudly (or silently) at the worst time.
