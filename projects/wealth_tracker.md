---
type: Project Context
title: wealth_tracker
description: Local web app for CSV bank/credit card statement ingestion, expense categorization, and financial dashboards. Next.js "Tally" report UI on FastAPI + Postgres.
tags: [finance, csv, postgres, fastapi, nextjs]
timestamp: 2026-09-30T00:00:00Z
project: wealth_tracker
resource: https://github.com/imVincentTan/wealth_tracker
status: active
---

# Summary

Personal finance tracker. Upload CSV statements from bank and credit card accounts into Postgres; view income, expenses, categories, and cash-flow dashboards.

The "Tally" archive merge (PR [#2](https://github.com/imVincentTan/wealth_tracker/pull/2), merged 2026-09-21) replaced the old Vite/React `frontend/` with a **Next.js app at the repo root** (`src/`), on top of the existing FastAPI + Postgres backend in `backend/`.

# Architecture (post-merge)

- **Frontend**: Next.js app at repo root (`src/app`, `src/components`, `src/lib`). Key files: `src/lib/api.ts` (backend calls), `src/lib/store.ts` (zustand + import flow), `src/lib/reports.ts` (`buildReport`), `src/components/tally-app.tsx`, `import-dialog.tsx`, `transaction-table.tsx`, charts in `category-chart.tsx` + `trend-chart.tsx`.
- **Backend**: FastAPI in `backend/app/` — `models.py` (Account, Category, CategoryRule, Import(+`raw_csv`), RawImportRow, Transaction(+`merchant`)), `routers/` (accounts, categories, imports, transactions, dashboard), `services/csv_parser.py` (`parse_csv_rows`, `extract_merchant`, `detect_parser_config`), `services/seed.py` (default categories + rules), `parsers/defaults.py` (TD/Amex/Chase).
- **DB**: Postgres 16. Tables created via `Base.metadata.create_all` on startup — **no migrations**; schema drift requires recreating the DB.
- UI talks to `NEXT_PUBLIC_API_URL` (default `http://127.0.0.1:8000`). UI dev port is **43127** (not 3000). API docs at `http://127.0.0.1:8000/docs`.

# Product intent (preserve)

- Store original CSV text (`Import.raw_csv`) + every raw row as JSON (`RawImportRow.raw_data`); preview-then-commit imports; dedup on re-import via `dedup_hash`.
- Saved accounts carry `parser_config` (institution + account type + column mapping).
- Parsers: TD chequing/savings (Withdrawals/Deposits), TD card, Amex, Chase, plus generic Date/Description/Amount detection with column mapping.
- Transfers (card payments, Zelle, ATM cash) are stored but **excluded** from spending totals.
- CAD is the reporting currency.
- Recategorize a transaction, optionally apply to that merchant (creates a CategoryRule). User-created rules must override seeded defaults — see [User rules override seed rules](/knowledge/user-rules-override-seed-rules.md).

# Run

```bash
cp .env.example .env
docker compose up --build                          # Postgres + FastAPI (api on :8000)
npm install
NEXT_PUBLIC_API_URL=http://127.0.0.1:8000 npm run dev   # UI on :43127
```

No Docker: run Postgres yourself, set `DATABASE_URL`, then `cd backend && python -m venv .venv && pip install -r requirements.txt && uvicorn app.main:app --reload --port 8000`.

Verify: `npm run test` (ledger tests), `npm run lint`, `npm run build`. Sample CSVs in `public/samples/` (chase-checking.csv, chase-credit.csv).

# Phase 2 (deferred)

- Investment statements: NBF, Wealthsimple
- Net worth including investment balances

# Related

* Repo: [github.com/imVincentTan/wealth_tracker](https://github.com/imVincentTan/wealth_tracker)
* Plan file: `PLAN.md` in repo root
* [Cloud VM verification playbook](/playbooks/verify-web-app-cloud-vm.md) — used to verify the merge end-to-end
