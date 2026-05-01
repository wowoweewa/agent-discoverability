# Pillar 7 — Brand Mention Monitoring

## What it is

A standing process that re-runs the citation prompt panel on a defined cadence (weekly or monthly) and tracks the citation rate, sentiment, and competitor share over time. Pillar 6 is a snapshot; Pillar 7 is the trend line.

## Why it matters

LLM responses drift weekly — model retraining, retrieval index updates, new ranking signals, competitor activity. Without monitoring, the client doesn't know whether last month's fixes worked or whether they're losing ground. Monitoring is also the deliverable that justifies the recurring retainer (vs. one-time audit).

## How to audit

This pillar grades whether monitoring infrastructure exists, not whether visibility itself is good (that's Pillar 6).

### Inputs
- Existing monitoring tools or processes the client uses (if any)
- Baseline citation report (from Pillar 6 first-run)

### Checks
- [ ] Baseline citation rate documented per LLM
- [ ] Baseline competitor share documented (who's cited when client isn't)
- [ ] Defined cadence for re-runs (weekly, biweekly, or monthly)
- [ ] Same prompt panel used each cycle (so deltas are comparable)
- [ ] Same LLM versions tracked, with version drift logged when models update
- [ ] Sentiment captured per mention (positive / neutral / negative / inaccurate)
- [ ] Delta report produced after each cycle (citation rate change, new mentions, lost mentions)
- [ ] Competitor citation share tracked alongside client share
- [ ] Annotated correlation with client-side changes (when the client shipped a fix, did citations move?)
- [ ] Alerts on inaccurate or negative mentions (so they can be addressed)

### Scoring rubric
- **0–2 (Critical)**: No monitoring; baseline never established
- **3–5 (Poor)**: Ad-hoc one-time check; no defined cadence
- **6–7 (Adequate)**: Monthly re-run with same panel; delta report produced
- **8–9 (Strong)**: All of the above plus competitor share tracking and version drift logging
- **10 (Excellent)**: All of the above plus alerting on inaccuracies and annotated correlation with client-side changes

## How to fix

### Quick wins (under 1 hr)
- Establish baseline by running Pillar 6 panel once and saving the verbatim outputs
- Schedule a monthly recurring calendar block to re-run

### Substantive fixes (1–5 hrs)
- Build a monitoring spreadsheet template: one column per cycle, one row per query
- Standardize the cycle output: client citation rate, competitor share, delta, notable changes
- Define escalation triggers (e.g., "if citation rate drops 20% MoM, investigate immediately")

### Deep fixes (>5 hrs / requires dev)
- Automate the panel runs via the LLM APIs (OpenAI, Anthropic, Perplexity) with results logged to a database
- Build a dashboard showing citation rate trend, competitor share, and per-query history
- Integrate alerts (Slack, email) for significant deltas
- Diff verbatim outputs cycle-over-cycle to detect changes in how the LLM describes the brand

## How this delivers the retainer

The $500/mo assisted retainer = monthly re-run + delta report + DIY recommendations. The $2,000/mo managed retainer = monthly re-run + delta report + implementation work + advisory.

### Monthly delta report contents
- Citation rate this month vs. last month (per LLM, overall)
- New mentions (where client wasn't cited before, now is)
- Lost mentions (where client was cited before, no longer is — usually highest priority to investigate)
- Competitor citation share change
- Notable shifts in *how* the brand is described
- Recommended actions (assisted: DIY; managed: scoped for operator)

## Worked example

[Placeholder — populate from first real retainer cycle]

## Tools to consider (vs. building from scratch)

- **Profound, Otterly.AI, AthenaHQ, Peec AI** — purpose-built LLM monitoring SaaS. Good for clients who want the dashboard included; reduces operator workload but increases tooling cost.
- **Manual + spreadsheet** — fine for first 5–10 retainers; lets you tune what to track before locking in tooling.
- **Custom via API** — best margin but requires upfront build time. Worth doing once you have 5+ retainer clients (the ROI is clear).

For Phase 1: manual + spreadsheet. Move to tooling when monitoring eats >2 hrs per client per month.

## References
- [Pillar 6 — Citation Probing](./06-citation-probing.md) (provides the panel methodology this pillar tracks over time)
