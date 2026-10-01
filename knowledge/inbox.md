---
type: Knowledge
title: Learnings inbox
description: Raw, signal-gated candidate learnings appended by the session loop. Promoted deliberately or deleted — never a queue that must be processed.
tags: [meta, self-improvement, inbox]
timestamp: 2026-09-30T00:00:00Z
status: active
---

# Learnings inbox

Written by the [session loop](/skills/session-loop.md). One line per candidate learning:

```
- YYYY-MM-DD | <project> | <correction|failure|fact> | <one-sentence learning>
```

## Rules

- **TTL**: an entry that survives ~2 review cycles without being promoted is **deleted**, not kept. If it wasn't worth promoting, it wasn't worth keeping.
- **Promotion** follows the [self-improvement cycle](/playbooks/self-improvement-cycle.md): recurs in 2+ sessions → Skill/Playbook; clearly load-bearing → Knowledge/Lesson immediately; project-specific → the project's own repo docs.
- **Measure by reuse.** If entries here never get read by future sessions, tighten the session loop's gate — don't lower the bar for keeping them.

## Entries

(append below, newest last)

- 2026-09-30 | crew | fact | First entry: the inbox itself was created as part of the session-loop setup; format validated by this line.
- 2026-09-30 | wealth_tracker | failure | `.env`'s host-native `DATABASE_URL` (127.0.0.1) leaked into the api container via compose `${VAR:-default}` interpolation — container must always get the in-network `db` hostname; hardcode it in compose.
- 2026-09-30 | harness | failure | Inter-container traffic on the docker bridge is broken in this nested cloud VM (container→container times out; host→container and container→host work). Validate compose stacks here via a host-gateway override, not by changing the committed file.
- 2026-09-30 | wealth_tracker | failure | Next.js reads `.env` at build time: a repo `.env` carrying `NEXT_PUBLIC_API_URL` (dev origin) silently baked into the "same-origin" static bundle. When building same-origin bundles, explicitly empty the var (`NEXT_PUBLIC_API_URL=""`) — process env beats `.env`.
- 2026-09-30 | wealth_tracker | fact | Starlette `StaticFiles(html=True)` serves `404.html` itself instead of raising — SPA fallbacks must intercept the served 404 (check `response.status_code`), not just the raised exception.
