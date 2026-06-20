---
type: Reference
title: Crew — Project Plan
description: Purpose, OKF conventions, directory layout, and self-improvement cycle for the crew knowledge repo.
tags: [meta, okf, self-improvement]
timestamp: 2026-06-06T00:00:00Z
---

# Purpose

`crew` is a version-controlled knowledge base for building and improving AI agents. It stores:

- **Skills** — reusable procedures and domain know-how
- **Knowledge** — durable facts, preferences, and reference material
- **Playbooks** — step-by-step workflows agents should follow
- **Project context** — per-repo learnings (e.g. [wealth_tracker](/projects/wealth_tracker.md))

Over time, every agent interaction should feed a **self-improvement cycle**: capture what worked, what failed, and update this repo so future sessions start smarter. See [Self-improvement cycle](/playbooks/self-improvement-cycle.md).

# Format standard — Open Knowledge Format (OKF)

All markdown knowledge in this repo uses **OKF v0.1** ([spec](https://github.com/GoogleCloudPlatform/knowledge-catalog/blob/main/okf/SPEC.md)).

## Why OKF

| Criterion | OKF fit |
|-----------|---------|
| Git-native, diffable | Markdown + YAML frontmatter |
| Human-readable | Renders on GitHub, editable in any editor |
| Agent-consumable | No SDK required; `type` field routes concepts |
| Cross-linking | Markdown links form a knowledge graph |
| Change history | Reserved `log.md` at each scope |
| Minimal lock-in | One required field (`type`); extensions allowed |

OKF is a strong default for this repo. Alternatives considered:

| Alternative | Verdict |
|-------------|---------|
| Plain markdown wiki | Fine locally, but no interoperability conventions |
| JSON/YAML knowledge store | Harder for humans to author and review |
| RAG-only (vector DB) | Good for retrieval, poor for curated/versioned knowledge |
| Cursor rules/skills only | Tool-specific; doesn't travel across agents |

**Hybrid note:** Cursor has its own [Agent Skills](https://cursor.com/docs) format (`SKILL.md`). When a skill needs to run inside Cursor, we can **derive** a Cursor skill from an OKF `type: Skill` concept. OKF remains the source of truth in `crew`; Cursor artifacts are exports, not the canonical store.

# Directory layout

```
crew/
├── index.md                 # Bundle root navigation (OKF index)
├── log.md                   # Bundle-wide change log
├── concepts/                # Plans, references, meta
├── skills/                  # Reusable agent skills
├── knowledge/               # Facts, preferences, reference
├── playbooks/               # Workflows (incl. self-improvement)
└── projects/                # Per-project context and lessons
```

Each subdirectory has its own `index.md` (OKF directory listing) and MAY have its own `log.md` for scoped history.

# Concept types (crew vocabulary)

OKF does not register types centrally. This repo uses:

| `type` | Use for |
|--------|---------|
| `Skill` | Reusable agent capability or procedure |
| `Playbook` | Multi-step workflow |
| `Knowledge` | Durable fact or reference |
| `Preference` | User/agent preference learned from sessions |
| `Lesson` | Post-session learning (what worked / failed) |
| `Project Context` | Summary of a codebase or project |
| `Reference` | Meta docs, plans, standards |

Consumers MUST tolerate unknown types (per OKF spec).

# Recommended frontmatter

Beyond required `type`, crew concepts SHOULD include:

```yaml
---
type: Skill
title: Human-readable name
description: One-sentence summary
tags: [tag1, tag2]
timestamp: 2026-06-06T00:00:00Z
# Optional extensions:
project: wealth_tracker      # link to a project scope
source_session: <chat-id>    # where this was learned
status: draft | active | deprecated
---
```

# Phased scope

## Phase 1 — Foundation (now)

- [x] Clone repo, establish OKF bundle structure
- [x] Document plan and self-improvement playbook
- [ ] Add first real skills and knowledge from sessions
- [ ] Link [wealth_tracker](/projects/wealth_tracker.md) project context

## Phase 2 — Self-improvement loop

- [ ] After each significant session: write a `Lesson` or update `Knowledge`
- [ ] Promote repeated lessons into `Skill` or `Playbook`
- [ ] Keep [log.md](/log.md) updated with each change
- [ ] Optional: script to validate OKF conformance (frontmatter + `type`)

## Phase 3 — Tooling integration

- [ ] Export OKF `Skill` concepts to Cursor `SKILL.md` where needed
- [ ] Agent prompt snippet: "read crew index, load relevant project context"
- [ ] Optional: HTML visualizer or search index over frontmatter

# Open questions

- Automate log.md append at end of sessions, or keep manual?
- One `projects/<name>.md` per repo, or a subdirectory per project?
- How aggressively to sync preferences into Cursor user rules vs keep in crew only?
