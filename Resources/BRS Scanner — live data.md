---
created: 2026-07-07
---

# BRS Scanner — live data

Where real [[BRS Scanner]] **sessions** live and how to reach them. This closes the "where's the session data?" question from [[Comprehensive Marketing Review]].

## The app

- **Repo:** `/Users/mac/brs-scanner` — a **Next.js app on Vercel** (project "brs-scanner").
- **Live scanner** crawls a site (Firecrawl + PageSpeed) and scores it with the Anthropic API, then syncs contacts to **GoHighLevel** (`GHL_API_KEY`, `GHL_LOCATION_ID`) and emails via Resend.
- **Database:** MongoDB Atlas — db **`brand_resonance_scanner`**. Credentials live in `/Users/mac/brs-scanner/.env.local` (`MONGO_URL`, `DB_NAME`). **Never copy secrets into this vault.** Access read-only with `mongosh` for analysis.

## Key collections

- **`scans`** — one document per session. Fields: `id`, `url`, `totalScore` (0–100), `createdAt`, `country`, `industry`, `userId`, `publicSlug`, `scores{}`, `dimensions{}`. Anonymous scans auto-expire after 180 days (`retentionExpiresAt`).
- `scan_feedback`, `recommendation_feedback` — user feedback per dimension / recommendation (the raw material for tuning).
- `scanner_weights_history`, `score_distributions` — the app already versions its scoring weights and tracks score drift (a built-in learning layer).
- `users`, `purchases`, `signups`, `coupons` — auth + monetisation.

## The 8 scored dimensions (this IS the automated CMR)

Same shape as the manual [[CMR Template]]: `{label, score, max, percentage, band}`.

| Dimension | Max |
|---|---|
| ICP Clarity | 15 |
| Positioning | 15 |
| Message Resonance | 15 |
| Journey | 15 |
| Trust Signals | 10 |
| Experience Quality | 10 |
| Measurement | 10 |
| Distribution | 10 |

**Big insight:** the automated scanner and the manual CMR measure the *same eight things*. That's the bridge — the scanner *is* an automated [[MMR Template|MMR]]/[[CMR Template|CMR]], and its cross-session data can sharpen both. See [[BRS Scanner — cross-session insights 2026-07-07]].
