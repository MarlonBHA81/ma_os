---
created: 2026-07-07
---

# WAT operating layer

Marlon's **agent operating system** — Workflows, Agents, Tools — at `/Users/mac/Documents/GitHub/wat`. The companion to this brain.

> **ma_os = know · WAT = do.** Durable facts, decisions and context live here (in `ma_os`); repeatable procedures and the scripts that run them live in WAT.

## The three layers
- **Workflows** (`wat/workflows/*.md`) — plain-language SOPs (objective, inputs, tools, outputs, edge cases).
- **Agent** — Claude: reads the workflow, runs tools in order, recovers from failure.
- **Tools** (`wat/tools/` + registered tools elsewhere) — deterministic scripts. The [[BRS analysis toolkit]] is registered *in place* (`brs-scanner/scripts/analysis/`), not moved.

## Why it fits
We built the BRS system this way already: tested tools + an orchestrating agent + a fail-loud guardrail ([[BRS monitoring resilience]]). WAT just names it and adds the missing layer — written **workflows** — so the process is repeatable by any agent.

## Current workflows
- `brs-schema-health` — the health gate (run before trusting a report/redeploy).
- `brs-market-report` — generate the [[BRS analysis toolkit|market report]].
- `brs-refresh-dashboard` — refresh the [[BRS signal report artifact|live dashboard]] to the same URL.
- `brs-handle-schema-drift` — recover when the live app changes.

## Conventions worth remembering
- **Secrets stay in the owning app's `.env`** (`brs-scanner/.env.local`) — never in WAT, never in this brain, never in a deliverable. (WAT independently arrived at the same rule I've been following.)
- **Tools are language-agnostic** — the existing ones are Node/`.mjs`; kept as-is rather than ported to Python.
- Deliverables go to where Marlon uses them (this brain, the artifact, Sheets/Slides); `.tmp/` is disposable.
