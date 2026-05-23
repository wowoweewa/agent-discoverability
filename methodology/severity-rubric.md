# Severity Rubric — pillar weights, prerequisite caps, and overall score

The audit produces a single 0–100 overall agent-discoverability score plus per-pillar severity tags that drive fix prioritization. This file is the source of truth for both. When pillar weights change, the overall score formula in `deliverables/audit-template.md` changes with it — version the rubric and note the version in the audit appendix.

Current version: **v1.0** (May 2026)

## Pillar weights

| Pillar | Weight | Rationale |
|---|---|---|
| 1 — Structured data | 15% | High-leverage, deterministic to verify, drives entity grounding across all engines |
| 2 — AI crawler access | 15% **with prerequisite cap** | Necessary precondition. If crawlers can't reach the site, no other work matters. |
| 3 — `llms.txt` | **0% (signal-only)** | Not used by any major AI engine (Mueller, Dec 2024). Reported as Present / Malformed / Absent but does not affect score. |
| 4 — Content extractability | 15% **with prerequisite cap** | Necessary precondition. If content is locked in JS, images, or PDFs, no AI engine can cite it. |
| 5 — Agent readiness | 10% | Important for emerging autonomous-shopper traffic but lower current-state impact than visibility pillars |
| 6 — Citation probing | 25% | Outcome metric. Highest direct weight — this is what the audit ultimately answers. |
| 7 — Off-page authority | 20% | Strongest research-backed correlate of AI visibility (Ahrefs r=0.664 for unlinked brand mentions). |

**Sum of scored weights: 100%.** Pillar 3 is excluded from the weighted sum.

## Prerequisite caps

Pillars 2 and 4 are *structural prerequisites* — if either is critically broken, the site is effectively invisible to AI engines regardless of how strong other pillars are. The cap reflects that reality in the overall score, so the audit deliverable doesn't mislead the client with a high number while their site is silently failing.

| Condition | Overall score cap |
|---|---|
| Pillar 2 score ≤ 2 (Critical) **OR** Pillar 4 score ≤ 2 (Critical) | 30 |
| Pillar 2 score ≤ 2 **AND** Pillar 4 score ≤ 2 | 15 |

The cap replaces the weighted sum, not adds to it. If the weighted sum is below the cap, the lower number wins (no inflation).

## Per-pillar 0–10 → severity tag mapping

Each pillar has its own 0–10 rubric defined in its own file (see `01-structured-data.md` etc). This mapping converts that 0–10 score into a severity tag used in the audit deliverable's Section 4 pillar table and Section 6 prioritized fix list.

| 0–10 score band | Severity tag | What it means for fix prioritization |
|---|---|---|
| 0–2 | **Critical** | Fix immediately. Sorted to top of fix list. Almost always blocks higher pillars from showing impact. |
| 3–5 | **Important** | Fix within 60 days. Sorted below Critical, above Nice-to-have. |
| 6–7 | **Nice-to-have** | Improvement opportunity. Sorted at the bottom of fix list. Surface only if effort is low. |
| 8–10 | *(no action)* | Do not surface in fix list. May be cited as a strength in the executive summary. |

Pillar 3 (`llms.txt`) does not get a severity tag — it's reported as Present / Malformed / Absent in the appendix only.

## Overall score formula

```
weighted_sum = (P1 × 1.5) + (P2 × 1.5) + (P4 × 1.5) + (P5 × 1.0) + (P6 × 2.5) + (P7 × 2.0)

if P2 ≤ 2 AND P4 ≤ 2:
    overall = min(weighted_sum, 15)
elif P2 ≤ 2 OR P4 ≤ 2:
    overall = min(weighted_sum, 30)
else:
    overall = weighted_sum

round to nearest integer (0–100)
```

Max possible weighted_sum with all pillars at 10: 100.

## Worked example

Real-estate brokerage in Calgary, audit conducted May 2026:

| Pillar | Score (0–10) | Severity tag | Weighted contribution |
|---|---|---|---|
| 1 — Structured data | 6 | Nice-to-have | 9.0 |
| 2 — AI crawler access | 8 | *(no action)* | 12.0 |
| 3 — `llms.txt` | *(absent)* | — | — |
| 4 — Content extractability | 7 | Nice-to-have | 10.5 |
| 5 — Agent readiness | 3 | Important | 3.0 |
| 6 — Citation probing | 2 | **Critical** | 5.0 |
| 7 — Off-page authority | 4 | Important | 8.0 |

Weighted sum: 47.5
Prerequisite check: P2 (8) > 2 AND P4 (7) > 2 → no cap applied
**Overall: 48/100**

Fix list ordering (audit template Section 6):
1. Pillar 6 (Critical, score 2) — improve citation probing outcomes
2. Pillar 5 (Important, score 3) — add agent-readable booking/contact flows
3. Pillar 7 (Important, score 4) — claim Reddit/G2 presence, pursue Wikipedia notability
4. Pillar 1 (Nice-to-have, score 6) — add Author and Service schema
5. Pillar 4 (Nice-to-have, score 7) — convert remaining PDF content to HTML

Pillars 2 and 3 are not surfaced as fixes (P2 is strong; P3 is signal-only and reported in the appendix).

## How this rubric connects to the audit deliverable

- **Section 2 (Executive Summary)**: cites the overall score and a one-sentence explanation. If a prerequisite cap was applied, the explanation must say so explicitly — e.g., "Score capped at 30 because AI crawlers are blocked. Lifting that block is the single highest-leverage fix."
- **Section 4 (Pillar Scores)**: each pillar row shows score, severity tag (mapped from this file), and one-line "what we found."
- **Section 6 (Prioritized Fix List)**: ordered by severity tag (Critical → Important → Nice-to-have), then within each tag by effort × impact (audit-runner discretion).
- **Section 8 (Appendix)**: cites the severity rubric version used (currently v1.0) so future audits can be compared apples-to-apples.

## Versioning

When weights or caps change, bump the version and note in the audit appendix. Major version bumps (e.g., adding/removing a pillar, changing prerequisite logic) require regenerating any prior-period comparison charts on retainer dashboards to maintain comparability.

## References
- Per-pillar 0–10 rubrics: see `01-structured-data.md` through `07-brand-mention-monitoring.md`
- Engine substrates context: `engine-substrates.md` — informs why P2 (crawler access) is prerequisite-capped
- Research grounding for P7 weight: Ahrefs 75K-brand study (r=0.664 for unlinked brand mentions vs r=0.218 for backlinks)
