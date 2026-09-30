---
type: Playbook
title: Verify a web app end-to-end in a cloud agent VM
description: Boot-to-browser verification workflow for a full-stack app in a Cursor cloud VM, including snapshot-staleness traps.
tags: [verification, cloud-agent, testing, fullstack]
timestamp: 2026-09-30T00:00:00Z
status: active
---

# When

After a big merge/restructure, on a fresh cloud VM, or any time "it worked before" needs to become "it works here, now".

# Steps

## 1. Establish ground truth before testing anything

- `git fetch origin <branch>` for the branch you care about — snapshot checkouts can be stale.
- Read `/tmp/cursor/start-user/start-user.log` and `/tmp/cursor/async-install/install-user.status` to learn what the environment actually started.
- List listening ports and match each process to a code path (`readlink /proc/<pid>/cwd`). A running server may be serving an **older checkout** than what's on disk now.

## 2. Make running services match the checkout

- Restart backends/frontends that started before the current checkout. Kill by PID, never `pkill -f`.
- If the DB was created by older code and the app has no migrations (`create_all`-style), recreate the dev database rather than debugging insert 500s.
- If the repo layout changed, update the environment start script so the *next* boot is correct too.

## 3. Static gates

Run the repo's own checks: unit tests, lint, production build. All three must pass before manual verification.

## 4. Exercise the primary user flow through the API

Drive the real contract with curl/script, not just `/health`. For each product invariant, assert it explicitly. Example set from [wealth_tracker](https://github.com/imVincentTan/wealth_tracker): create resource → preview → commit → re-import (dedup must skip 100%) → aggregate totals (excluded categories must be absent) → user correction (must persist and override defaults on *new* input).

## 5. UI walkthrough with artifacts

- Start the dev server in a persistent tmux session so it survives the agent's shell.
- Drive a browser via the computer-use subagent; verify rendered numbers against the API responses (don't trust either alone).
- Record the walkthrough (start recording immediately before, stop immediately after) and save screenshots/recordings to the artifacts dir.
- Cross-check video-review claims against a high-res screenshot before quoting numbers — video OCR can misread figures.

## 6. Leave it running

Don't stop services after verification; the user may want to click around. Commit/push and open/update the PR before summarizing.

# Evidence to report

Test/lint/build output, API assertions with actual values, screenshot(s) of the rendered UI, one walkthrough video, and the PR link.
