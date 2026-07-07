---
created: 2026-07-07
---

# BRS signal report artifact

A live, shareable dashboard of the [[BRS Scanner — live data|151 scanner sessions]] — the visual front-end of the [[BRS analysis toolkit]].

**URL:** https://claude.ai/code/artifact/319f1e50-87dd-4e85-9dd2-d30ac2ddb354 (default-private to Marlon).

## What it shows
- KPI readout (sessions, median resonance, weakest signal, hot leads).
- **Signal-strength bars** — the 8 dimensions ranked weakest-first, each with the recommended fix.
- **Where sites land** — the four resonance bands (Invisible/Emerging/Competing/Resonating) with an action per band + a daily score trend.
- **Action briefing** — data-derived moves.
- **Priority Leads table** — every scanned site, sorted neediest-first, filterable by band/industry/search, each row carrying the move to make. This is the operational part — sort low → follow up.

## Data & refresh
- Built from a **PII-safe snapshot** (`scripts/analysis/export-json.mjs` → `brs-snapshot.json`) — no email/phone; contact details stay in GHL.
- **To refresh:** re-run `node scripts/analysis/export-json.mjs`, re-inject into the HTML, and redeploy the artifact to the same URL. A monthly cron could regenerate it automatically.
- Design: a "resonance signal console" — monospace readout identity, FixMyMarketing coral accent, theme-aware.
