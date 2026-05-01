# Pillar 6 — Citation Probing

## What it is

Directly querying ChatGPT, Claude, Perplexity, and Gemini with a curated panel of 20+ category-relevant questions and recording (a) whether the client is cited, (b) which competitors are cited instead, (c) the verbatim model output. This pillar is the *outcome metric* — pillars 1–5 are inputs that improve this number.

## Why it matters

Pillars 1–5 are necessary but not sufficient. A perfectly schema'd, crawler-friendly site can still be invisible if it lacks third-party citations, expert authorship, or category authority. This pillar measures the result and surfaces *why* the result is what it is.

## How to audit

### Inputs
- Client's category, geography, and primary services
- Competitor list (3–5 direct competitors)
- LLM access: ChatGPT (Plus or via API), Claude.ai, Perplexity, Gemini

### Build the prompt panel (20–30 queries across categories)
- **Category + location**: "Best [service] in [city]?", "Top-rated [profession] in [city]?"
- **Need + buying intent**: "I need [specific service]; who should I hire?"
- **Comparison**: "[Competitor A] vs [Competitor B] — which is better for [use case]?"
- **Long-tail informational**: "How do I choose a [profession]?", "What should I know before hiring [service]?"
- **Brand-direct**: "Tell me about [Client Name]", "What does [Client Name] do?"
- **Trust/credentials**: "Who are the most reputable [profession] in [city]?"
- **Pricing**: "How much does [service] cost in [city]?"
- **Niche/segment**: "[Service] for [specific customer type] in [city]?"

### Run the panel
- [ ] Each query run on each LLM (4 LLMs × 25 queries = 100 runs)
- [ ] Capture verbatim output, model version, and date
- [ ] Use a fresh chat per query to avoid memory bias
- [ ] Use a logged-out / temporary session where possible to avoid personalization bias
- [ ] Geographic VPN if checking from a different region than the client's market

### Score each response
- **Cited (Yes)**: client named explicitly, with or without link
- **Cited (Indirect)**: client's content quoted or paraphrased without naming, OR client appears as one in a long list (>5)
- **Not cited (Competitor)**: a direct competitor cited instead
- **Not cited (Other)**: irrelevant or generic answer
- **Not cited (Refusal)**: model declined to recommend

### Score the pillar
- Citation rate = (Yes + 0.5×Indirect) / total queries, per LLM and overall
- **0–2 (Critical)**: <5% citation rate; competitors dominate
- **3–5 (Poor)**: 5–15%; mentioned occasionally on long-tail
- **6–7 (Adequate)**: 15–35%; cited on brand-direct queries; missing on category queries
- **8–9 (Strong)**: 35–60%; cited on category + location; missing only on niche segments
- **10 (Excellent)**: >60%; consistent cite across category, comparison, and informational queries

## How to fix

The fix depends on *why* the citation is missing. Diagnose first:

### If competitors are cited and client isn't
- Check what content the cited competitors have that the client lacks (often: industry publication features, third-party reviews, "best of" listicles, podcast appearances)
- Check whether competitors have richer schema and `sameAs` links
- Check whether competitors are cited on Reddit, Wikipedia, or industry directories

### If LLMs hedge ("I don't have specific information about...")
- Knowledge gap — entity not strongly recognized
- Build out: claim Knowledge Panel, ensure consistent NAP (name/address/phone), strengthen `sameAs`, get a Wikipedia-eligible reference if possible
- Publish authoritative content on the brand-name page (an "about" that an LLM can cite)

### If client is cited but inaccurately
- Stale content somewhere LLMs trained on (old website, archived directories)
- Issue takedowns / corrections; publish a clear current-truth page; update schema

### If client is buried in a list of 10+
- Authority signal weak — needs cited reviews, editorial mentions, structured testimonials with `Review` schema, professional association memberships visible in `sameAs`

## Worked example

[Placeholder — populate from first real audit]

## Important methodology notes

- **Run dates matter**: LLM responses change weekly. Always include the run date and model version in the audit deliverable.
- **Personalization bias**: results vary by user history. Use logged-out sessions or temporary chats.
- **Geographic bias**: results vary by IP geolocation. Use a VPN matching the client's target market if auditing for a non-local market.
- **Model version drift**: GPT-4o ≠ GPT-5; Claude Sonnet 4.5 ≠ 4.6. Note version strings.

## References
- [Reddit citation collapse Aug-Nov 2025 — CMSWire](https://www.cmswire.com/digital-marketing/reddits-rise-in-ai-citations-what-marketers-must-know-about-aeo-strategy/)
- [eMarketer AI search adoption stats](https://www.emarketer.com/content/faq-on-geo-aeo--where-ai-search-seo-overlap-2026)
