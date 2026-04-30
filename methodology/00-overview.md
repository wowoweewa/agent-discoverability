# 7-Pillar AEO/GEO Audit Framework — Overview

The audit measures how visible a client's business is across LLM-powered search and agentic interfaces. Each pillar has its own detailed file (`01-` through `07-`) covering: what it is, why it matters, how to audit, how to fix, and a worked example.

## The Seven Pillars

| # | Pillar | What it answers |
|---|---|---|
| 1 | Structured data | Can AI parse the entities, products, and relationships on this site? |
| 2 | AI crawler access | Are GPTBot, ClaudeBot, PerplexityBot, GoogleOther allowed (or blocked) — intentionally? |
| 3 | `llms.txt` | Is there a clean, AI-readable summary file at the root? |
| 4 | Content extractability | Are facts in plain text and properly chunked, or hidden in images, JS-rendered DOMs, or PDFs? |
| 5 | Agent readiness | Can autonomous shoppers/agents complete tasks (book, buy, request) without human-only flows? |
| 6 | Citation probing | When users ask ChatGPT/Claude/Perplexity/Gemini relevant questions, is the client cited? Compared to whom? |
| 7 | Brand mention monitoring | Tracking citations and sentiment over time; baseline + monthly delta |

## Audit deliverable

`deliverables/audit-template.md` defines the **section structure** (cover, executive summary, methodology, pillar scores, prompt panel results, prioritized fix list, implementation path, appendix). Visual design — typography, layout, score visualization, brand styling — is the responsibility of a separate `designing-audit-pdf` skill (to be built). Section template and design skill stay decoupled so either can change without touching the other.

## Methodology principles

- **Cite real outputs, not abstractions.** Every claim about "your brand isn't visible" must be backed by a screenshotted/saved LLM response.
- **No vendor lock-in language.** Don't pitch tools the client must subscribe to forever; pitch fixes that compound.
- **Score for fixability, not for perfection.** A 4/10 with a clear fix path is more valuable to surface than a 7/10 with diminishing returns.
- **Calgary/Canadian context where it matters.** Local search behavior, French-language considerations, regulatory framing for regulated verticals (legal, financial, healthcare).

## Status

Pillar files (01–07) are scaffolded but not yet written. See the project task list.
