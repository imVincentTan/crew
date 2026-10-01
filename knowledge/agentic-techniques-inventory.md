---
type: Knowledge
title: Agentic techniques inventory
description: Living inventory of AI agent techniques and harness capabilities — what each is for, and whether crew has validated it in a real session.
tags: [ai, agents, techniques, harness, living-doc]
timestamp: 2026-09-30T00:00:00Z
status: active
---

# Purpose

One place to track "anything AI" worth reusing: harness capabilities, agent techniques, and workflow patterns. Entries are added when **used or validated in a real session** (marked ✅) or spotted as worth trying (marked 👀). Prefer adding a ✅ entry with a link to the session/project over collecting unvetted hype.

# Validated in our sessions

| Technique | What it is / when it helped | Where |
|-----------|------------------------------|-------|
| Snapshot-based cloud VMs | Prebuilt environment snapshots make agent VMs boot fast, but running services can serve stale code — verify before trusting | [harness notes](/knowledge/cursor-cloud-agent-harness.md) |
| Specialized subagents | Explore (codebase search), debug (stateful, hypothesis-driven, instruments code), computer-use (GUI verification), video-review (checks recordings). Delegating GUI work keeps the main agent terminal-focused | tally merge verification |
| Artifact-based proof | Screen recordings + screenshots under an artifacts dir, uploaded with the run; forces "evidence, not claims" for UI work | tally merge verification |
| Preview-then-commit flows | Two-phase mutations (preview → user/agent review → commit) make imports/migrations safe and dedup-able | [wealth_tracker](https://github.com/imVincentTan/wealth_tracker) |
| Self-improvement loop | End-of-session capture of lessons into a versioned knowledge repo (this one) | [playbook](/playbooks/self-improvement-cycle.md) |
| OKF knowledge bundles | Git-native markdown + frontmatter as an agent-consumable, tool-agnostic memory layer; tool-specific formats (Cursor SKILL.md) are exports | [plan](/concepts/plan.md) |

# Watchlist (not yet validated here)

- **MCP servers as tool sources** — dynamic tool namespaces per task; worth documenting per-server auth gotchas as we adopt them
- **Environment builds as CI for agent setup** — treat the dev environment itself as a buildable artifact with logs and drafts
- **Best-of-N parallel attempts** — isolated worktree subagents racing the same task; evaluate for ambiguous refactors
- **Automated OKF conformance check** — small script validating frontmatter/`type` across the bundle (Phase 3 item in the [plan](/concepts/plan.md))

# How to update

Add a row when a technique is used for real (✅ + link) or deliberately queued (👀). Promote techniques with 2+ uses into a `Skill` or `Playbook` per the [self-improvement cycle](/playbooks/self-improvement-cycle.md).
