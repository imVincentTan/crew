---
type: Skill
title: Session loop
description: Use for any non-trivial task (code changes, debugging, multi-step work). Wraps the work with lightweight context-loading up front and a signal-gated learning capture at the end. Skip entirely for trivial questions.
tags: [meta, self-improvement, workflow]
timestamp: 2026-09-30T00:00:00Z
status: active
---

# Session loop

The always-on companion procedure for non-trivial tasks. Triggered by a user rule; details live here so the rule stays two lines.

**Cursor export**: `exports` of this concept are derived, not canonical — see [Authoring SKILL.md](/skills/authoring-skill-md.md). The derived `SKILL.md` must stay under ~50 lines and mirror the gates below.

## Gate — decide in 5 seconds

Skip this loop entirely when the prompt is a quick question, a one-line change, or pure conversation. When in doubt, skip. The loop exists to serve the work, never to become the work.

## Pre-work (under a minute)

1. Identify which project the task touches.
2. If that project has a context doc (`docs/project-context.md`, README, or AGENTS.md), read it before editing anything.
3. Glance at the crew [skills](/skills/index.md) and [knowledge](/knowledge/index.md) indexes; open an entry only if it's obviously relevant to the task.
4. Note the project's stated invariants before changing code that could violate them.

Do not bulk-read the knowledge base. Context loading is targeted, not exhaustive.

## Do the work

Normal task execution. Nothing changes here.

## Post-work — the signal gate

Ask exactly three questions:

1. **Did the user correct me?** (a preference, a convention, a "no, not that")
2. **Did something fail or surprise me?** (a command that should have worked, an assumption that turned out wrong)
3. **Did I discover a durable fact?** (an environment quirk, an API contract, a non-obvious dependency)

All three no → **write nothing**. Most turns write nothing.

Any yes → append **one line** to [knowledge/inbox.md](/knowledge/inbox.md):

```
- YYYY-MM-DD | <project> | <correction|failure|fact> | <one-sentence learning>
```

## Hard rules

- One line per learning, no essays. Capture is cheap; curation happens elsewhere.
- No generic reflections ("remember to verify your work"). If it isn't specific and durable, don't write it.
- Never extend a task significantly to capture. If there's no time, skip.
- Promotion out of the inbox is NOT this skill's job — it follows the [self-improvement cycle](/playbooks/self-improvement-cycle.md) (recurs in 2+ sessions, or clearly load-bearing).
- Project-specific learnings go to the project's own repo docs, per the boundary rule in the [plan](/concepts/plan.md); the inbox is for cross-project material or pointers.
