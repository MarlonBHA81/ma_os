---
created: 2026-07-07
---

# BRS analysis toolkit

Read-only scripts that turn [[BRS Scanner — live data|live scanner sessions]] into insight — the working core of the "analyse → improve over time" loop for the [[Comprehensive Marketing Review]].

**Location:** `/Users/mac/brs-scanner/scripts/analysis/` (in the scanner repo).

- `market-report.mjs` — markdown [[BRS Scanner — cross-session insights 2026-07-07|market report]]: score distribution, dimension ranking, industry/country cuts, daily trend. `--out <file>` to save.
- `score-diagnosis.mjs` — scoring **calibration**: per-dimension average + zero-rate (detects "is this low score real or a scanner blind spot?"), schema-drift health, feedback-loop readiness.
- `lib.mjs` — shared env loader, read-only connect, and the **normalize-both-schemas** helper (the reusable fix for the two score shapes).

They never write to MongoDB. Run with plain `node` from the repo root.

## What they've told us so far
- Weakness is **genuine, not a scanner bug** — no dimension has a ≥60% zero-rate (Trust Signals is 36% zeros + 17% avg).
- **professional-services** scores highest (~53), **entertainment** lowest (~19); most sessions are unclassified by industry/country.
- The real **learning loop is still off** — no `scan_feedback` / `recommendation_feedback` captured yet. Wiring that up is the unlock for data-driven scoring tuning.

## Open follow-ups
1. Turn `market-report.mjs` into a monthly job (repo already has `app/api/cron/`).
2. Unify the score schema + backfill (reviewed production change).
3. Capture recommendation/dimension feedback in the report UI to switch the learning loop on.
