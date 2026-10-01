---
type: Knowledge
title: Cursor Cloud Agent harness — facts and gotchas
description: How the Cursor Cloud Agent execution harness works (environments, snapshots, start scripts, secrets, subagents, PR flow) and the failure modes observed in practice.
tags: [harness, cursor, cloud-agent, environment, tooling]
timestamp: 2026-09-30T00:00:00Z
status: active
---

# What the harness is

Cursor Cloud Agents run autonomously in a remote VM per run. Key mechanics observed while working on [wealth_tracker](https://github.com/imVincentTan/wealth_tracker):

- **Environment config**: `.cursor/environment.json` defines install/start scripts. The `start` script runs detached on every boot to bring up services (Postgres, dev servers); nothing waits on it, and failures are not surfaced except via logs.
- **Prebuilt snapshots**: VMs boot from a snapshot of a previous environment build. The git **refs** may be newer than the **working tree files**, and long-running processes started at boot serve whatever code was on disk *then*.
- **Logs/state to check first**: `/tmp/cursor/async-install/` (setup status), `/tmp/cursor/start-user/` (start script log + exit status).
- **Secrets**: injected as env vars from the Cursor Dashboard (Cloud Agents > Secrets); values are redacted in tool output.
- **PR flow**: agents push branches and open PRs via the harness's PR tool (not `gh`); PRs default to draft.
- **Subagents**: specialized agents exist for codebase exploration, hypothesis-driven debugging (stateful, auto-resumes), GUI/browser verification (computer use), and video review. GUI verification must go through the computer-use subagent; terminal verification should use plain shell.
- **Walkthrough artifacts**: screenshots/recordings saved under `/opt/cursor/artifacts/` are uploaded and shown to the user. Screen recording is started/stopped explicitly around the manual test.

# Gotchas observed (cost real time)

1. **Stale server after snapshot boot.** Boot start script launched uvicorn against a pre-merge checkout; the running API rejected enum values present in the on-disk code. *Always verify the running service exhibits a behavior unique to the current checkout* (e.g. call an endpoint with a newly-added enum value) before trusting it.
2. **Stale DB schema.** `create_all` creates missing tables but never alters existing ones; a DB created by old code silently lacks new columns/enum values → 500s on insert. Fix: recreate the DB (dev) or run real migrations (prod).
3. **Start scripts rot when the repo layout changes.** The VM start script still launched the deleted Vite `frontend/` after the Next.js merge. After any restructure, update the environment's start script too.
4. **Untracked leftovers survive.** Deleted directories (old `frontend/node_modules`) linger as untracked files and can keep stale servers alive.
5. **`docker` may not exist in the VM** even when the repo's README leads with `docker compose`. Check before assuming; native Postgres + venv is the fallback.

# Related

* [Verify a web app in a cloud VM](/playbooks/verify-web-app-cloud-vm.md) — the playbook derived from these gotchas
