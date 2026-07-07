---
created: 2026-07-07
sessions_analysed: 151
---

# BRS Scanner — cross-session insights 2026-07-07

First analysis across **151 real [[BRS Scanner — live data|scanner sessions]]**. This is the seed of the "analyse → improve output over time" loop for the [[Comprehensive Marketing Review]].

## Score distribution (all 151)

| Band | Scans |
|---|---|
| 0–24 | 33 |
| 25–49 | **68** |
| 50–74 | 49 |
| 75–100 | **1** |

Most sites cluster at **25–49**. Almost nobody breaks 75. Reverse-scoring aside, this says the market is genuinely weak online — lots of qualified prospects.

## Dimension weakness ranking (avg %, normalized over ALL 151)

| Dimension | Avg % |
|---|---|
| Trust Signals | **17%** ← weakest |
| Customer/ICP Clarity | 26% |
| Distribution | 31% |
| Measurement | 38% |
| Positioning | 43% |
| Message Resonance | 48% |
| Journey Logic | 55% |
| Experience Quality | **73%** ← strongest |

**This validates the [[Brand Resonance System]] thesis directly:** everyone's *tech/UX is fine* (73%) but *trust, clarity and distribution are broken* (17–31%). The gap isn't technical — it's **authority translation**. That's a headline you can market with, and it tells the [[MMR Template|MMR]]/[[CMR Template|CMR]] where to concentrate recommendations.

## Data-quality finding — root cause: SCHEMA DRIFT (not missing data)

The "86 scans with no `dimensions`" were **not** failures — they store all 8 scores as **flat top-level fields** (`customerClarity`, `trustSignals`…). The other 65 use the newer **nested `scores{}` + `dimensions{}`** shape. **Both shapes are still being written as of today (2026-07-07)** → two live code paths produce different document structures.

- **Fix (recommended):** unify the scanner on one score schema and backfill old docs — otherwise every analysis needs the normalize-both workaround, and the app's own learning layer (`score_distributions`, `scanner_weights_history`) may be reading only one shape.
- Only **10 scans in the last 30 days** — low volume; patterns are directional.
- **~96 of 151 have no industry** (`default`/null) — industry-level cuts are weak until classification improves.

*(Numbers above already corrected to normalize both schemas, so they cover 100% of sessions.)*

## Next
Turn this into a repeatable pass (weights/recommendations tuning, or a monthly market-insight report). Target of the loop is an open decision — see the recommendation in the working notes.
