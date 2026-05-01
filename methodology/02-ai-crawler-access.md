# Pillar 2 — AI Crawler Access

## What it is

The site's `robots.txt`, meta robots tags, server-level bot rules (Cloudflare, Akamai, AWS WAF), and HTTP response codes — and whether they allow or block the crawlers that feed AI search engines. In 2023–2024 many sites blocked AI crawlers in panic over IP scraping. By 2026 those same sites need visibility back, and most don't realize they're still blocking.

## Why it matters

If GPTBot, ClaudeBot, PerplexityBot, or Google-Extended can't fetch the site, the LLMs they feed simply can't cite it — no amount of schema or content quality compensates. This is the single most common silent failure mode.

## How to audit

### Inputs
- Site URL
- Direct fetch of `/robots.txt`
- Browser DevTools or curl to inspect headers
- Where possible: 30 days of server access logs filtered by crawler user-agent

### Checks
- [ ] `/robots.txt` exists and is parseable
- [ ] `GPTBot` (OpenAI training): allow / block / unspecified — document intent
- [ ] `OAI-SearchBot` (ChatGPT search) — should be allowed for visibility
- [ ] `ChatGPT-User` (user-triggered fetches) — should be allowed
- [ ] `ClaudeBot` (Anthropic) — should be allowed
- [ ] `Anthropic-AI` (legacy) — allowed or unspecified
- [ ] `PerplexityBot` — should be allowed
- [ ] `Perplexity-User` — should be allowed
- [ ] `Google-Extended` (Gemini training) — separate from regular Googlebot
- [ ] `Applebot-Extended` (Apple Intelligence) — should be allowed
- [ ] `Bytespider` (TikTok) — separate decision
- [ ] `meta name="robots"` and `X-Robots-Tag` headers don't override allow directives
- [ ] No blanket `Disallow: /` for any AI crawler unless intentional
- [ ] CDN/WAF rules (Cloudflare bot fight, Akamai bot manager) don't block known AI crawlers
- [ ] Server returns 200 to allowed crawlers (not 403, 429, or 503)
- [ ] Server logs show actual hits from these crawlers in last 30 days (proves the allow is working)

### Scoring rubric
- **0–2 (Critical)**: Blanket block on all AI crawlers (`Disallow: /` for GPTBot etc.)
- **3–5 (Poor)**: Some major AI crawlers blocked, others unspecified; intent unclear
- **6–7 (Adequate)**: Major AI crawlers allowed in robots.txt but server logs show no hits (likely WAF blocking)
- **8–9 (Strong)**: All major AI crawlers explicitly allowed; logs confirm regular fetches
- **10 (Excellent)**: All of the above plus a documented policy in robots.txt comments explaining intent (e.g., "we allow AI crawlers for search visibility and disallow training")

## How to fix

### Quick wins (under 1 hr)
- Add explicit `User-agent: GPTBot` / `User-agent: ClaudeBot` / etc. blocks with `Allow: /` for crawlers wanted
- Remove or scope down any `Disallow: /` rules
- Comment the policy decisions in robots.txt for future operators

### Substantive fixes (1–5 hrs)
- Audit Cloudflare / WAF rules for bot management; create an explicit allowlist
- Set up server log monitoring to verify crawler hits ongoing
- Distinguish training crawlers (GPTBot for training) from search/answer crawlers (OAI-SearchBot, ChatGPT-User) — many businesses want different policies for each

### Deep fixes (>5 hrs / requires dev)
- Programmatic robots.txt management as part of deploy pipeline (avoid stale rules across environments)
- Crawler hit dashboard (which crawlers visit which pages how often)

## Worked example

[Placeholder — populate from first real audit]

## References
- [OpenAI GPTBot documentation](https://platform.openai.com/docs/gptbot)
- [Anthropic ClaudeBot](https://www.anthropic.com/claudebot)
- [Perplexity crawler docs](https://docs.perplexity.ai/guides/bots)
- [Google AI crawlers](https://developers.google.com/search/docs/crawling-indexing/overview-google-crawlers)
- [Apple Intelligence crawler](https://support.apple.com/en-us/119829)
