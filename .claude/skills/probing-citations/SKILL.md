---
name: probing-citations
description: Generates and scores the Pillar 6 citation panel — 25 category-relevant queries run against ChatGPT, Claude, Perplexity, and Gemini to measure whether a client is cited and where. Use when the user says "probe citations for [client]", "run the prompt panel on X", "check Pillar 6 for Y", "do a citation tracking cycle", "/probe-citations", or fires for monthly retainer re-runs that benchmark citation rate over time. Generates the prompt set per `methodology/06-citation-probing.md`, captures verbatim responses per LLM, and scores citation rate vs. competitors. (Audit-cycle Pillar 6 runs inline inside the `auditor` subagent — this standalone skill is for retainer cycles in the operator's main context.)
---

# Probing Citations

Single-purpose skill for executing Pillar 6 of the audit methodology. Used standalone in the operator's main context for monthly retainer cycles tracking citation rate drift over time. (Audit-cycle Pillar 6 runs inline inside the `auditor` subagent — same methodology, different invocation context.)

## Context Required

- `methodology/06-citation-probing.md` — full Pillar 6 rubric, query categories, scoring formula
- `methodology/07-brand-mention-monitoring.md` (Part B section) — for retainer-cycle delta reports

## Steps

### Step 1 — Gather inputs

- **Brand name** — exact spelling, including stylization (`HubSpot` not `Hubspot`)
- **Category** — for the "Best [category]" queries
- **Geography** — city/region for local queries
- **Competitors** — 3–5 direct competitors (for comparison queries)
- **Cycle context** — is this a fresh audit (baseline) or a retainer re-run (delta vs. last cycle)?

If this is a retainer re-run, also read the prior cycle's results from `clients/<slug>/citation-panel-<prior-date>.json` so you can produce a delta report.

### Step 2 — Generate the 25-query prompt panel

Generate queries in the 8 categories from `methodology/06-citation-probing.md`:

1. **Category + location** (3 queries) — `Best <category> in <city>?`, `Top-rated <category> in <city>?`, `Most reputable <category> serving <region>?`
2. **Need + buying intent** (4 queries) — `I need <specific service>; who should I hire?`, `Where can I get <specific service> in <city>?`, etc.
3. **Comparison** (3 queries) — `<Competitor A> vs <Competitor B> — which is better for <use case>?`
4. **Long-tail informational** (3 queries) — `How do I choose a <category>?`, `What should I know before hiring <category>?`
5. **Brand-direct** (3 queries) — `Tell me about <Brand>`, `What does <Brand> do?`, `Is <Brand> reputable?`
6. **Trust/credentials** (3 queries) — `Who are the most reputable <category> in <city>?`, `Best-reviewed <category> for <use case>?`
7. **Pricing** (3 queries) — `How much does <service> cost in <city>?`, `Typical pricing for <category>?`
8. **Niche/segment** (3 queries) — `<category> for <specific customer type> in <city>?`

Total: 25 queries. If a vertical is unusual (e.g., very niche B2B), adapt the templates but keep the 8-category structure so cycle-over-cycle comparison stays apples-to-apples.

### Step 3 — Run the panel

Three modes, in order of preference:

**Mode A — API automation (preferred when keys are configured):**
- Anthropic (Claude): use this Claude Code session's existing access; for diversity, also run via the Anthropic API with a different system prompt to simulate end-user behavior
- OpenAI (ChatGPT): requires `OPENAI_API_KEY` env var; use `gpt-4o` and current default model
- Perplexity: requires `PERPLEXITY_API_KEY`; use `sonar` model (their search-grounded option)
- Gemini: requires `GOOGLE_API_KEY`; use `gemini-2.5-flash` or current default

For each LLM, run each query in a fresh session (no chat history). Capture: query, verbatim response, model version, date, geographic IP.

**Mode B — Manual operator run (fallback):**
If keys aren't configured or the operator prefers manual control, output the 25 queries as a markdown file at `clients/<slug>/panel-prompts-<YYYY-MM-DD>.md` plus an empty capture template at `clients/<slug>/panel-results-<YYYY-MM-DD>.md`. The operator runs each query in logged-out / incognito sessions on each LLM, pastes verbatim into the capture template, and re-invokes this skill to score.

**Mode C — Hybrid:**
Run the Anthropic + OpenAI APIs automatically (if keys present), output the Perplexity + Gemini queries for manual run. Common path when only some keys exist.

### Step 4 — Score each response

For each of the 100 query × LLM combinations (25 × 4), tag as:

- **Cited (Yes)** — client named explicitly, with or without link
- **Cited (Indirect)** — client's content quoted/paraphrased without naming, OR client appears in a long list (>5)
- **Not cited (Competitor)** — a direct competitor cited instead (record which)
- **Not cited (Other)** — irrelevant or generic answer
- **Not cited (Refusal)** — model declined to recommend

### Step 5 — Compute citation rate

Per `methodology/06-citation-probing.md`:

```
citation_rate = (Yes + 0.5 × Indirect) / total_queries
```

Compute per-LLM and overall. Apply the Pillar 6 rubric:

- 0–2 (Critical): <5%
- 3–5 (Poor): 5–15%
- 6–7 (Adequate): 15–35%
- 8–9 (Strong): 35–60%
- 10 (Excellent): >60%

### Step 6 — Save structured + render markdown

Save raw structured data:

```json
clients/<slug>/citation-panel-<YYYY-MM-DD>.json
{
  "run_date": "2026-05-17",
  "brand": "Acme Realty",
  "category": "Calgary real estate brokerage",
  "geography": "Calgary, AB",
  "competitors": ["..."],
  "queries": [
    {
      "id": 1,
      "category": "category+location",
      "text": "Best Calgary real estate brokerage?",
      "responses": [
        {"llm": "ChatGPT", "model": "gpt-4o-2025-08", "verbatim": "...", "cited": "No-Competitor", "competitor_cited": "..."},
        ...
      ]
    }
  ],
  "scores": {
    "ChatGPT": {"citation_rate": 0.12, "score_0_10": 3},
    "Claude": {...},
    "overall": {"citation_rate": 0.08, "score_0_10": 3}
  }
}
```

Render markdown for the audit deliverable's Section 5:

```markdown
clients/<slug>/panel-results-<YYYY-MM-DD>.md

## Pillar 6 — Citation Probing Results

Run date: 2026-05-17 | Models: ChatGPT gpt-4o-2025-08, Claude claude-opus-4-7, Perplexity sonar, Gemini 2.5-flash

### Citation rate
- ChatGPT: 12% (Poor, 3/10)
- Claude: 8% (Critical, 2/10)
- Perplexity: 16% (Adequate, 6/10)
- Gemini: 4% (Critical, 1/10)
- Overall: 10% (Poor, 3/10)

### Verbatim responses
[per query, per LLM, verbatim]
```

### Step 7 — If retainer re-run, produce delta

Compare to the prior cycle's JSON. Output to `clients/<slug>/delta-<YYYY-MM-DD>.md`:

- Citation rate this cycle vs. last (per LLM, overall)
- New mentions (where the client wasn't cited before, now is)
- Lost mentions (where the client was cited before, now isn't — highest priority)
- Competitor share movement
- Notable shifts in how the brand is described

## Output Format

Two files per run:

1. `clients/<slug>/citation-panel-<YYYY-MM-DD>.json` — structured data for cycle-over-cycle comparison
2. `clients/<slug>/panel-results-<YYYY-MM-DD>.md` — markdown for Section 5 of the audit deliverable

For retainer cycles, also:

3. `clients/<slug>/delta-<YYYY-MM-DD>.md` — month-over-month delta report

## Gotchas

- **Personalization bias is silent.** If you run ChatGPT in a logged-in session with chat history, the results reflect the user's history, not the model's default behavior. Always use logged-out / temporary / API sessions.
- **Geographic bias is real.** Running from Vancouver IP for a Calgary client produces local-results skew. If running via API, the API call's edge location often handles this — note the geography in the run metadata.
- **Model version drift kills cycle comparison.** GPT-4o ≠ GPT-5 ≠ GPT-5.3. If a model updates between cycles, note the version drift in the delta report and flag that the comparison is not strictly apples-to-apples.
- **Don't paraphrase responses.** Section 5 is verbatim — this is non-negotiable. The audit's credibility rests on the client being able to reproduce the LLM output themselves.
- **Refusals are signal.** If a model refuses to recommend ("I don't make business recommendations"), record as "Refusal" — don't treat as failure to be cited. Some models refuse on certain query types (legal, medical) and that's a structural fact, not a client problem.
- **Brand-direct queries are easier.** A client cited 80% on "Tell me about [Brand]" but 5% on "Best [category]" has a category-authority problem, not a brand-recognition problem. The rubric weights all queries equally, but the diagnostic difference matters for fix prioritization.
- **Competitor lists rot.** If using a competitor list from 6 months ago for a retainer cycle, verify it's still current. New entrants may matter more than the old #3.
- **Save raw data, always.** The JSON file is the source of truth. The markdown is derived. Never ship an audit without the JSON saved.

## Constraints

- **Verbatim outputs only in Section 5.** No paraphrasing, no summary.
- **Always capture run date and model version.** A panel without these is unverifiable.
- **Logged-out / fresh sessions only.** Personalization corrupts the dataset.
- **Same 25-query structure across cycles.** Operator may swap individual query text for vertical fit, but the 8 categories and 25-query count stay fixed so cycles compare cleanly.
- **Score with the rubric, don't eyeball it.** `citation_rate = (Yes + 0.5 × Indirect) / total` then map to the 0–10 band per `methodology/06-citation-probing.md`. No vibes scoring.
