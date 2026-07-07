---
created: 2026-07-07
---

# BRS monitoring resilience

How the [[BRS analysis toolkit]] and [[BRS signal report artifact]] keep working when the **live scanner app changes** — so a schema tweak never silently breaks (or worse, silently falsifies) the numbers.

## The risk
The `scans` collection already stores dimension scores in **three shapes**: flat top-level (`trustSignals`), nested (`scores.trustSignals`), and a `dimensions{}` object that uses *different keys* (`icpClarity`, `journey`). Any further app change — a renamed dimension, a new one, a new score shape, a dropped field — could break analysis.

## The safeguards (all read-only)
1. **Alias-aware normalization.** Each dimension in `lib.mjs` carries an `aliases` list covering every key the app has used. A rename is a one-line fix in that list; the report, export and dashboard all follow.
2. **A health monitor that fails loud.** `schema-health.mjs` audits every scan against the known schema and exits **2 (alert)** on unreadable scans, partial resolution (a renamed/removed dimension), unknown new dimension keys, or bad `totalScore`. Suitable for a cron/CI alarm — it turns a silent break into a loud one.
3. **A self-monitoring dashboard.** `export-json.mjs` embeds the health verdict in the snapshot; the artifact header pill flips **HEALTHY → CHECK → DRIFT** so a stale/drifted feed is visible at a glance.
4. **Proven by test.** `selftest.mjs` (no DB) simulates each app change and confirms the alarms fire — currently 13/13 passing; live feed is **HEALTHY** (all 8 dimensions resolve 100% across 151 scans).

## The one dependency I can't self-heal
Access relies on `MONGO_URL` / `DB_NAME` in `/Users/mac/brs-scanner/.env.local`. If those are rotated or the DB is renamed, the monitor reports a clear connection/collection failure (not a crash) — the fix is to refresh `.env.local`.

## Standing habit
Run `node scripts/analysis/schema-health.mjs` before trusting a fresh report or redeploying the [[BRS signal report artifact|dashboard]]. Green = safe; amber/red = adapt an alias before publishing.

## Scheduled monitor — ACTIVE (2026-07-07)
Local `launchd` jobs run the guardrail **daily 08:00** and a market report **monthly** (see [[WAT operating layer]] · workflow `brs-scheduled-monitoring`). Because macOS **TCC** blocks background agents from `~/Documents`, the runner + outputs live in `~/Library/Application Support/wat-monitor/` (`monitor.log`, `alerts.md`, `reports/`). Drift → macOS notification. Writes into this brain are **best-effort** (only with Full Disk Access), so **each month: ask Claude to sync the report in + redeploy the dashboard**.
