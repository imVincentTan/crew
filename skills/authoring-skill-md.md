---
type: Skill
title: Author a Cursor SKILL.md from an OKF Skill concept
description: How to write an effective Cursor agent skill file (SKILL.md), and how it relates to OKF Skill concepts in this repo.
tags: [skills, cursor, okf, meta]
timestamp: 2026-09-30T00:00:00Z
status: active
---

# Source of truth vs export

Per the [repo plan](/concepts/plan.md): OKF `type: Skill` concepts in `crew/skills/` are canonical. A Cursor `SKILL.md` is an **export/derivation** for the Cursor harness, not the canonical store. When a procedure needs to run inside Cursor, derive a SKILL.md from the OKF concept and keep both in sync.

# SKILL.md anatomy (observed from harness-bundled skills)

```markdown
---
name: short-kebab-name
description: "When to use this skill. Written as a trigger: scenarios, not a summary."
environments: [cloud]        # optional scoping
---

# Title

## When to Use / When NOT to Use
## The procedure (numbered steps, exact commands, exact paths)
## Rubric / quality bar (what good output looks like, with examples)
## Critical rules (hard constraints, phrased as MUST/NEVER)
```

# What makes a skill effective

- **Description is a router.** The agent decides to load the skill from the description alone. Write it as "Use when X, Y, Z" — not "This skill is about X".
- **Imperative procedure.** Exact commands, paths, and tool names. No prose that the model must reinterpret.
- **Rubric with examples.** Show good vs bad outputs explicitly (e.g. "good artifact sets" vs "do NOT upload") — this is what steers behavior at decision points.
- **Hard rules called out.** Constraints like "never kill processes by name" or "always end the recording right after the test" get their own MUST/NEVER lines.
- **Short.** A skill is read into context every time it triggers; keep it under ~100 lines.

# Anti-patterns

- Narrative explanations of *why* the skill exists (put that in the OKF concept).
- Vague steps ("verify the app works") without the *how*.
- Duplicating repo docs wholesale — link instead.
