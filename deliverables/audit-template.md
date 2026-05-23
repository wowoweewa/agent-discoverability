# Audit Deliverable — Section Template

Section structure only. No design, no formatting choices, no copy that locks visual identity. A separate `designing-audit-pdf` skill (to be created) consumes this template plus the audit-runner output and produces the final styled PDF.

---

## Section 1 — Cover

- Client name
- Site audited (URL, audit date)
- Auditor name + contact
- One-line audit headline (filled in by audit-writer based on top finding)

## Section 2 — Executive Summary

- Overall agent-discoverability score (0–100, computed from pillar scores per `methodology/severity-rubric.md`)
- If a prerequisite cap was applied (Pillar 2 or 4 critically broken), state this explicitly in the summary
- Three-bullet "what this means in plain language"
- Top 3 priority actions (effort × impact ranked)
- One-sentence answer to: "When a potential customer asks ChatGPT/Claude/Perplexity about [client's category], does the AI recommend you?"

## Section 3 — Methodology

- One paragraph: what was tested, across which LLMs, against which queries
- List of LLMs probed (ChatGPT, Claude, Perplexity, Gemini, plus model versions on audit date)
- Number of prompt panels run
- Note: this section is fixed boilerplate — design skill will lay it out compactly

## Section 4 — Pillar Scores

For each of the 7 pillars (structured data, AI crawler access, llms.txt *[signal-only, not scored]*, content extractability, agent readiness, citation probing, off-page authority):

- Pillar name
- Score (0–10)
- One-line "what good looks like"
- One-line "what we found"
- Severity tag: Critical (0–2) / Important (3–5) / Nice-to-have (6–7) / no tag (8–10) — per `methodology/severity-rubric.md`

## Section 5 — Prompt Panel Results

For each query category (e.g., "Best [client category] in [city]"):

- The query text (verbatim)
- LLM responses (verbatim, per LLM, with date stamp)
- Whether the client was cited (Yes / No / Indirect)
- Competitors cited instead

## Section 6 — Prioritized Fix List

Table or sequence:

- Fix description
- Pillar it improves
- Effort (S/M/L)
- Impact (Low/Med/High)
- Whether the client can do it themselves or needs implementation help

## Section 7 — Implementation Path

Three options the client can pick:

- DIY using the fix list (no upsell)
- Implementation engagement (one-time, scoped from fix list)
- Monitoring retainer ($400/mo — citation tracking + monthly delta + advisory)

## Section 8 — Appendix

- Raw LLM outputs not surfaced in Section 5
- Schema/code snippets recommended
- Tools used during audit (cited so client can verify)
- Methodology version + audit date
- Severity rubric version (currently v1.0) — see `methodology/severity-rubric.md`

---

## What goes here vs. in the design skill

| Concern | This template | `designing-audit-pdf` skill |
|---|---|---|
| Section order, content per section | ✅ | ❌ |
| Word count guidance | ✅ | ❌ |
| Tone of voice | ✅ | ❌ |
| Typography, colors, layout, page breaks | ❌ | ✅ |
| Cover styling, charts, score visualization | ❌ | ✅ |
| Brand identity application | ❌ | ✅ |

Don't put visual-design decisions in this file. Don't put section structure in the design skill.
