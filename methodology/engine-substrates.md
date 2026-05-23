# Engine Substrates — what each AI engine actually retrieves from

Most AI engines do not run their own web index. They retrieve from an upstream search index and re-rank/summarize the results. This map tells the auditor *where to look first* when a client is invisible in a given engine — because the upstream substrate is usually the bottleneck, not the AI layer.

## Substrate map (May 2026)

| Engine | Retrieval substrate | Implication for visibility |
|---|---|---|
| **ChatGPT Search** | Bing index (with OpenAI's own re-ranking) | If client isn't in Bing, ChatGPT can't cite them. **Submit to Bing Webmaster Tools first.** |
| **Microsoft Copilot** | Bing index (via Microsoft's Prometheus RAG layer) | Same as ChatGPT. Bing is the gate. |
| **Claude web search** | Brave Search API | Submit to Brave Search ingestion. Smaller index, less mature tooling. |
| **Google Gemini / AI Mode / AI Overviews** | Google Search index | Google Search Console is the gate. **There is no opt-out for AI Overviews** — `Google-Extended` only blocks standalone Gemini *training*, not AIO surfacing. |
| **Perplexity** | Own ~200B-URL index (launched Sept 2025) | Allowlist `PerplexityBot` in robots.txt and ensure server returns 200 to it. Perplexity is the only major engine running its own index at scale. |

## What this changes about the audit

The audit should diagnose by engine, not in aggregate. If Pillar 6 shows the client cited in Perplexity but invisible in ChatGPT, the answer is almost never "improve your schema" — it's "you're not in Bing's index, or you're indexed poorly." Substrate-level diagnosis is the first cut.

### Diagnostic order when client is invisible in a specific engine

1. **Confirm substrate indexing first.** Search the client's brand name + a known service in the upstream substrate (Bing for ChatGPT/Copilot, Google for Gemini/AIO, Brave for Claude). If the client doesn't appear in the substrate's organic results, AI layer fixes are wasted effort.
2. **Confirm crawler access.** Pillar 2 audit — is the engine's crawler being blocked? Both the AI-specific crawler (GPTBot/ClaudeBot/etc.) AND the substrate's crawler (Bingbot, Googlebot, Brave) need to be allowed.
3. **Then move to retrievability and authority.** Schema, content extractability, off-page authority — these are necessary but only matter once substrate indexing is confirmed.

## Verification checks (per substrate)

### Bing (for ChatGPT, Copilot)
- [ ] Site indexed in Bing (`site:clientdomain.com` in bing.com returns pages)
- [ ] Client registered in Bing Webmaster Tools
- [ ] Bingbot allowed in robots.txt (separate from GPTBot)
- [ ] Bing's IndexNow protocol implemented if the site updates frequently (optional, low effort)

### Google (for Gemini, AI Mode, AI Overviews)
- [ ] Site indexed in Google (`site:clientdomain.com` in google.com)
- [ ] Client verified in Google Search Console
- [ ] Googlebot allowed (separate from Google-Extended)
- [ ] Note: `Google-Extended: Disallow` blocks Gemini training but does NOT block AI Overviews

### Brave (for Claude web search)
- [ ] Site appears in Brave Search results (search.brave.com)
- [ ] No specific Brave allowlist tooling exists yet — focus on general crawlability

### Perplexity (own index)
- [ ] `PerplexityBot` allowed in robots.txt
- [ ] Server returns 200 to `PerplexityBot` user-agent (check 30-day server logs)
- [ ] No Cloudflare/WAF block on `PerplexityBot`

## Crawler-vs-substrate distinction (common operator confusion)

Two layers of access need to be open:

1. **Substrate crawler** (Bingbot, Googlebot, Brave's crawler) — indexes the site into the upstream search index. Without this, the AI engine can't retrieve the page at all.
2. **AI-specific crawler** (GPTBot, OAI-SearchBot, ChatGPT-User, ClaudeBot, Claude-SearchBot, Claude-User, PerplexityBot, Google-Extended) — used for training, search-index supplementation, and live user-triggered fetches.

A site can be indexed in Bing (substrate open) but blocked to GPTBot (AI crawler closed) — and still surface in ChatGPT Search because OpenAI re-ranks Bing's results. Conversely, a site can allow GPTBot but be deindexed from Bing — and ChatGPT will not cite it.

**The substrate layer dominates.** Get substrate indexing right first; AI crawler hygiene second.

## Sources
- [Vercel + MERJ — How AI crawlers see the web (500M-fetch analysis)](https://vercel.com/blog/the-rise-of-the-ai-crawler) — confirms no JS rendering across major AI crawlers
- [OpenAI — GPTBot and OAI-SearchBot documentation](https://platform.openai.com/docs/gptbot)
- [Anthropic — Claude crawlers](https://www.anthropic.com/claudebot) — ClaudeBot (training), Claude-SearchBot (search), Claude-User (live fetch)
- [Perplexity — bot documentation](https://docs.perplexity.ai/guides/bots) — own index, Sept 2025 launch
- [Google — AI crawler overview](https://developers.google.com/search/docs/crawling-indexing/overview-google-crawlers) — Google-Extended scope clarification
- Cloudflare default-block on new zones since July 1, 2025
