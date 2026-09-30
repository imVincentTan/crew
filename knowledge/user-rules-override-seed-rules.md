---
type: Lesson
title: User-created rules must override seeded defaults
description: In first-match-wins rule engines, load/evaluate user-created rules before seeded defaults, or user corrections silently never apply.
tags: [design-pattern, rules-engine, seed-data, ux]
timestamp: 2026-09-30T00:00:00Z
project: wealth_tracker
source_session: tally-merge-verification
status: active
---

# What happened

wealth_tracker's `apply_category_rules()` returns the **first** matching rule, and rules were loaded in insertion order. Seeded rules (e.g. `trader joe` → Groceries) are inserted at startup, so they always had the lowest ids and always won. Recategorizing a transaction with "apply to merchant" created a user rule (`Trader Joe S` → Shopping) that **silently never applied** to future imports — the exact feature the product promises.

Fixed in [PR #3](https://github.com/imVincentTan/wealth_tracker/pull/3) by loading rules newest-first (`CategoryRule.id.desc()`), so the most recent user correction wins.

# The generalizable pattern

Any system with **seeded defaults + user overrides + first-match-wins evaluation** has this trap. Invariants to enforce:

1. **User data beats seed data.** Evaluation order must try user-created entries before built-in ones.
2. **Most recent correction wins** among user entries (or surface conflicts explicitly).
3. **Test the override path**, not just rule creation: creating a rule is not proof it will fire. Verify by re-running the matching input through the engine.
4. Silent precedence bugs look like "feature works" in demos — the rule is created, the current row is updated, only *future* matches are wrong.

# Applies to

- Category/rule engines, router configs, linter/formatter overrides, template resolution, feature-flag defaults
- Anywhere `INSERT`-ordered rows feed a `first match returns` loop
