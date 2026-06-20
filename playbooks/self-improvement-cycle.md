---
type: Playbook
title: Self-improvement cycle
description: End-of-session workflow to capture learnings into the crew knowledge bundle.
tags: [meta, self-improvement, okf]
timestamp: 2026-06-06T00:00:00Z
---

# Trigger

Run this playbook at the end of any significant agent session — especially after building, debugging, or making architectural decisions.

# Steps

## 1. Decide if anything is worth keeping

Capture when at least one applies:

- A **preference** was stated or discovered (stack, style, naming)
- A **mistake** was made and corrected (worth documenting so it isn't repeated)
- A **pattern** emerged that will recur across projects
- **Project-specific context** was established (architecture, conventions, phased plan)

Skip if the session was trivial (quick question, no new durable knowledge).

## 2. Choose the right concept type

| What happened | Write as |
|---------------|----------|
| User preference or default choice | `type: Preference` in [knowledge/](/knowledge/) |
| Reusable how-to procedure | `type: Skill` in [skills/](/skills/) |
| Multi-step workflow | `type: Playbook` in [playbooks/](/playbooks/) |
| Session post-mortem | `type: Lesson` in [knowledge/](/knowledge/) or project dir |
| Project plan or architecture | `type: Project Context` in [projects/](/projects/) |
| Factual reference | `type: Knowledge` in [knowledge/](/knowledge/) |

## 3. Write or update an OKF concept

- Use YAML frontmatter with required `type` plus `title`, `description`, `tags`, `timestamp`
- Prefer structured body: headings, lists, tables, code blocks
- Cross-link related concepts with bundle-relative links (e.g. `/projects/wealth_tracker.md`)
- Update existing concepts rather than duplicating when the topic already exists

## 4. Update indexes and log

- Add entry to the relevant subdirectory [index.md](/index.md)
- Append to [log.md](/log.md) (and subdirectory `log.md` if scoped)

Use ISO `YYYY-MM-DD` date headings; newest first.

## 5. Promote when patterns repeat

If the same lesson appears in 2+ sessions:

1. Consolidate into a single `Skill` or `Playbook`
2. Mark redundant `Lesson` concepts `status: deprecated` in frontmatter
3. Link deprecated lessons to the promoted concept

# Example log entry

```markdown
## 2026-06-06
* **Creation**: Added [wealth_tracker project context](/projects/wealth_tracker.md) after planning bank/CC CSV ingestion.
* **Update**: Recorded preference for CAD base currency in wealth_tracker context.
```

# Examples

End of a wealth_tracker planning session → create/update `/projects/wealth_tracker.md` and log it.

End of a debugging session → `type: Lesson` with root cause and fix.

Repeated CSV parser pattern → promote to `type: Skill` in `/skills/csv-parser-pattern.md`.
