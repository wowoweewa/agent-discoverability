---
name: running-an-audit
description: Runs a complete 7-pillar agent-discoverability audit for a client website. Use when the user says "run an audit on https://example.com", "audit this client", "do a full agent-discoverability audit", "/run-audit", "audit X for me", "check how visible Y is to AI agents", or provides any client URL with intent to produce the $1,500 audit deliverable. Validates inputs, invokes the auditor subagent, runs a QA review pass on the draft, and saves to `clients/<slug>/draft-audit-<YYYY-MM-DD>.md`. Methodology lives in `methodology/`; never duplicate its content into this skill.
---

# Running an Audit

Thin orchestration layer for the agent-discoverability audit. The `auditor` subagent does the technical work against the methodology. This skill handles input validation, dispatch, QA review, and operator hand-off.

## Context Required

Read these before starting (so the QA review pass has the right reference points):

- `methodology/00-overview.md` — framework summary
- `methodology/severity-rubric.md` — scoring formula and severity bands
- `deliverables/audit-template.md` — section structure the draft must follow

The auditor subagent reads the per-pillar methodology files itself; you don't need to load them in main context.

## Steps

### Step 1 — Gather inputs

Confirm or collect:

- **URL** (required) — primary client site
- **Brand name** — exact spelling for citation panel
- **Category** — e.g., "Calgary real estate brokerage"
- **Geography** — primary market
- **Competitors** — 3–5 direct competitors (ask if not provided; required for the citation panel)
- **Client slug** — short kebab-case identifier (e.g., `acme-realty`). Default to a slug derived from the domain.

If any of these are missing and the operator didn't provide them, ask once. Do not invent competitor lists or brand spellings.

### Step 2 — Set up the client folder

```bash
mkdir -p clients/<slug>
```

Check whether a draft already exists for today (`clients/<slug>/draft-audit-<YYYY-MM-DD>.md`). If it does, ask the operator whether to overwrite or pick up where it left off. Do not silently overwrite a draft.

### Step 3 — Pre-flight check

Confirm the URL is reachable before dispatching the auditor.

```bash
curl -sIL -A "Mozilla/5.0" <URL> | head -5
```

If the response is 4xx/5xx or times out, surface to the operator before invoking the auditor. The auditor will fail or produce misleading findings on an unreachable site.

### Step 4 — Invoke the auditor subagent

Dispatch the `auditor` subagent with all collected inputs. Use a prompt of the form:

```
Audit this client:
- URL: <URL>
- Brand: <Brand Name>
- Category: <Category>
- Geography: <Geography>
- Competitors: <Competitor 1>, <Competitor 2>, <Competitor 3>
- Slug: <slug>
- Run date: <YYYY-MM-DD>

Follow your standard process. Return the draft path plus a 3-bullet top-findings summary.
```

The auditor will:
1. Run substrate + per-pillar checks
2. Execute Pillar 6 inline per `methodology/06-citation-probing.md` (Mode A if API keys are present, Mode B fallback prints the panel for manual operator run)
3. Score per `severity-rubric.md` (with prerequisite caps)
4. Draft to `clients/<slug>/draft-audit-<YYYY-MM-DD>.md`
5. Return the path + top findings

The standalone `probing-citations` skill is for retainer-cycle re-runs from main context — not invoked from inside the auditor.

### Step 5 — QA review pass

Read the draft. Do NOT just trust it. Specifically check:

- **Section 2 (Executive Summary)**: overall score arithmetic matches `severity-rubric.md` formula. If a prerequisite cap was applied, is it stated?
- **Section 4 (Pillar Scores)**: every pillar has a score AND severity tag. Pillar 3 is marked "signal-only" (no tag). Severity tags match the score bands (0–2 Critical, 3–5 Important, 6–7 Nice-to-have, 8–10 no tag).
- **Section 5 (Prompt Panel Results)**: outputs are verbatim, run date present, model version present. If the panel was deferred to manual run, the section is clearly marked "PENDING — operator runs the panel before send."
- **Section 6 (Prioritized Fix List)**: sorted Critical → Important → Nice-to-have. Pillar 3 absent (signal-only).
- **Section 8 (Appendix)**: severity rubric version (v1.0) and methodology last-updated date are cited.

If any check fails, edit the draft directly to fix or dispatch the auditor with a follow-up prompt naming the issue. Don't ship a draft with arithmetic errors or missing severity tags.

### Step 6 — Surface findings to the operator

Output a short message to the operator with:

- Draft path: `clients/<slug>/draft-audit-<YYYY-MM-DD>.md`
- Overall score (with cap callout if applicable)
- Top 3 priority fixes from Section 6
- QA pass status (any issues you found and fixed)
- Whether Pillar 6 panel is run or pending

Stop here. The operator reviews and decides whether to ship.

## Output Format

The deliverable is `clients/<slug>/draft-audit-<YYYY-MM-DD>.md` produced by the auditor subagent following `deliverables/audit-template.md`. This skill does NOT produce the audit itself — it produces the operator-facing summary message.

Your final message to the operator:

```markdown
**Audit draft ready for review.**

- Draft: `clients/<slug>/draft-audit-<YYYY-MM-DD>.md`
- Overall score: X/100  [Capped at Y because <reason>]
- Pillar 6 panel: [Run | Pending — operator runs manually]
- QA pass: [Clean | Fixed N issues: <brief list>]

**Top 3 priority fixes (Critical first):**
1. <Fix>
2. <Fix>
3. <Fix>

Open the draft to review the full report before any client communication.
```

## Gotchas

- **Don't skip the URL pre-flight.** A failing site produces a misleading "Pillar 4 is 0" finding when the real issue is a 503 outage. Verify reachability first.
- **Don't accept the auditor's draft uncritically.** Arithmetic errors in Section 2 (overall score) are the most common quality issue and ruin credibility. The QA pass exists because Sonnet occasionally miscomputes weighted sums.
- **Don't overwrite existing drafts silently.** The operator may have hand-edited a prior draft. Always confirm before overwriting.
- **Don't ship the draft directly to the client.** This skill produces `draft-audit-*.md`. Final client send is a separate operator step after manual review. For the first 3–5 client audits, the operator hand-edits the draft before send (draft-mode mitigation per PROGRESS.md).
- **Don't dispatch the auditor without competitors.** Pillar 6 (citation probing) requires a competitor list to produce comparative findings. Missing competitors leaves Section 5 thin.
- **Slug collisions matter.** Two clients with similar names (`acme-co` and `acme-corp`) can produce confusion. Use a longer slug if needed.
- **Pricing language.** Section 7 of the draft mentions "$400/mo monitoring retainer" — this is canonical per `methodology/scoping-framework.md`. Two-tier pricing is a future expansion, not current. Don't substitute in $500/$2,000 if the auditor's draft uses the older language; correct to $400/mo single tier.
- **Don't dispatch the auditor twice for the same client on the same day.** It will overwrite the draft. If you need to re-run, archive the prior draft first (`mv draft-audit-YYYY-MM-DD.md draft-audit-YYYY-MM-DD-v1.md`).

## Constraints

- **First 5 client audits go to draft, not direct-ship.** Per PROGRESS.md Path A: hand-review every draft before send. After 5 clean direct-ship-ready drafts, retire draft-mode.
- **Never invent client data.** Brand spellings, competitor lists, category framing must come from the operator or from the client's site. Do not infer.
- **Skill does not duplicate methodology.** This file orchestrates; the auditor + methodology files do the audit logic. If you find yourself writing audit checks in this skill, move them to the auditor subagent.
- **No client-facing output from this skill.** Final client-bound audit (PDF, email, etc.) is the operator's job, optionally with a future `designing-audit-pdf` skill.
