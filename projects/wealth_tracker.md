---
type: Project Context
title: wealth_tracker
description: Local web app for CSV bank/credit card statement ingestion, expense categorization, and financial dashboards.
tags: [finance, csv, postgres, fastapi]
timestamp: 2026-06-06T00:00:00Z
project: wealth_tracker
resource: https://github.com/imVincentTan/wealth_tracker
status: planning
---

# Summary

Personal finance tracker. Upload CSV statements from bank and credit card accounts into Postgres; view income, expenses, categories, and cash-flow dashboards.

Full phased plan lives in the wealth_tracker repo (`PLAN.md`). This concept captures cross-session context for agents working on that project.

# Phase 1 scope (active)

- Local web app (Docker: Postgres + FastAPI + frontend)
- CSV ingestion only — **no investments yet**
- Institutions: **TD** (bank + CC), **Amex** (CC)
- Upload flow: label account on first import → reuse from dropdown
- Categories: structured list + string-matching rules (not pure freeform)
- Transfers: a category; exclude from expense charts via checkbox
- Currency: **CAD** base, multi-currency storage

# Phase 2 (deferred)

- Investment statements: NBF, Wealthsimple
- Net worth including investment balances

# Related

* Repo: [github.com/imVincentTan/wealth_tracker](https://github.com/imVincentTan/wealth_tracker)
* Plan file: `PLAN.md` in repo root
