# 7-Pillar AEO/GEO Audit Framework — Overview

The audit measures how visible a client's business is across LLM-powered search and agentic interfaces. Each pillar has its own detailed file (`01-` through `07-`) covering: what it is, why it matters, how to audit, how to fix, and a worked example.

## The Seven Pillars

| # | Pillar | What it answers |
|---|---|---|
| 1 | Structured data | Can AI parse the entities, products, and relationships on this site? |
| 2 | AI crawler access | Are GPTBot, ClaudeBot, PerplexityBot, GoogleOther allowed (or blocked) — intentionally? |
| 3 | `llms.txt` *(signal-only, not scored)* | Is there a clean, AI-readable summary file at the root? Reported as Present / Malformed / Absent; does not affect the overall audit score. |
| 4 | Content extractability | Are facts in plain text and properly chunked, or hidden in images, JS-rendered DOMs, or PDFs? |
| 5 | Agent readiness | Can autonomous shoppers/agents complete tasks (book, buy, request) without human-only flows? |
| 6 | Citation probing | When users ask ChatGPT/Claude/Perplexity/Gemini relevant questions, is the client cited? Compared to whom? |
| 7 | Off-page authority | Static off-page presence audit (Reddit, Wikipedia, reviews, directories, news) + ongoing citation tracking with monthly delta |

## Audit deliverable

`deliverables/audit-template.md` defines the **section structure** (cover, executive summary, methodology, pillar scores, prompt panel results, prioritized fix list, implementation path, appendix). Visual design — typography, layout, score visualization, brand styling — is the responsibility of a separate `designing-audit-pdf` skill (to be built). Section template and design skill stay decoupled so either can change without touching the other.

## Engine substrates

Most AI engines retrieve from an upstream search index, not their own. ChatGPT Search and Copilot read Bing; Gemini/AI Mode/AI Overviews read Google; Claude reads Brave; Perplexity runs its own index. This shapes fix prioritization — substrate indexing comes before AI-specific tactics. See [engine-substrates.md](./engine-substrates.md) for the full map and per-substrate verification checks.

## Scoring

Each pillar produces a 0–10 score per its own rubric. The overall 0–100 audit score is a weighted sum with prerequisite caps applied when Pillar 2 (crawler access) or Pillar 4 (content extractability) is critically broken. Per-pillar severity tags (Critical / Important / Nice-to-have) drive fix list prioritization in the deliverable. See [severity-rubric.md](./severity-rubric.md) for weights, cap logic, formula, and worked example.

## Methodology principles

- **Cite real outputs, not abstractions.** Every claim about "your brand isn't visible" must be backed by a screenshotted/saved LLM response.
- **No vendor lock-in language.** Don't pitch tools the client must subscribe to forever; pitch fixes that compound.
- **Score for fixability, not for perfection.** A 4/10 with a clear fix path is more valuable to surface than a 7/10 with diminishing returns.
- **Calgary/Canadian context where it matters.** Local search behavior, French-language considerations, regulatory framing for regulated verticals (legal, financial, healthcare).

## Status

Pillar files (01–07) are scaffolded but not yet written. See the project task list.
