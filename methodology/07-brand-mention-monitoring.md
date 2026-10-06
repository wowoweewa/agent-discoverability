# Pillar 7 — Off-page Authority

> Filename note: the file remains `07-brand-mention-monitoring.md` for now to avoid breaking references. Rename to `07-off-page-authority.md` in a future cleanup pass.

## What it is

Two coupled measurements that together establish whether the brand has external authority signals AI engines can find and trust:

**(A) Static off-page authority audit** — one-time, run at audit time. Measures the client's presence and quality across the off-page sources that AI engines weight heavily: Reddit, Wikipedia, YouTube, review aggregators (G2/Capterra/Trustpilot), industry directories, news/trade publications, and `sameAs` linkage in schema.

**(B) Citation tracking over time** — recurring, runs on a monthly cadence as part of the retainer. Re-runs the Pillar 6 prompt panel and tracks how citation rate, sentiment, and competitor share shift over time.

Half (A) is the strongest correlate of AI visibility in the current research: Ahrefs' 75K-brand study found unlinked brand mentions correlate with AI visibility at r=0.664 versus backlinks at r=0.218 — roughly 3:1 advantage. Brand mentions outweigh link equity in 2026.

Half (B) is what justifies the retainer — LLM responses drift weekly (model retraining, retrieval index updates, competitor activity), so a one-time audit goes stale fast.

## Why it matters

Pillars 1–5 cover what the client controls on their own site. Pillar 6 measures the outcome (am I cited?). Pillar 7 measures the *external* inputs that drive Pillar 6's outcome — the off-page signals AI engines lean on when ranking and citing.

A client with perfect on-site optimization but no Reddit presence, no Wikipedia entry, no third-party reviews, and no industry directory listings will score high on Pillars 1–5 and still lose to a competitor with weaker site hygiene but stronger off-page presence. This pillar surfaces that gap and gives the operator a roadmap of off-page work to recommend.

## How to audit — Part A (static off-page authority)

### Inputs
- Client brand name (exact and common variations)
- Client category and geography
- Competitor list (3–5 direct competitors, for comparison)

### Checks

**Wikipedia presence**
- [ ] Wikipedia entry exists for the brand
- [ ] If yes: entry accuracy, citations, last edit recency
- [ ] If no: notability-eligibility assessment (does the brand have enough independent secondary-source coverage to qualify under WP:CORP / WP:NOTABILITY?)
- [ ] Wikidata entry linked from Wikipedia entry (if applicable)

**Reddit presence**
- [ ] Brand mentioned in relevant subreddits in last 12 months (search `site:reddit.com "[brand]"`)
- [ ] Sentiment of mentions (positive / neutral / negative / inaccurate)
- [ ] Whether the brand has an active subreddit (optional, for larger brands)
- [ ] Frequency of mentions in category subreddits (e.g., r/realestate, r/personalfinance)

**YouTube presence**
- [ ] Branded channel exists and is active (videos in last 12 months)
- [ ] Brand mentioned in industry/review videos by third parties
- [ ] Video transcripts contain brand-relevant keywords (transcripts are indexed)
- [ ] At minimum: a single high-quality "what is [brand]" video that AI engines can cite

**Review aggregators (vertical-dependent)**
- [ ] B2B SaaS: G2, Capterra, Software Advice, TrustRadius — claimed profile, review count, recent reviews
- [ ] Consumer: Trustpilot, BBB, Google Business Profile, vertical-specific (Yelp, Houzz, Avvo, Zillow)
- [ ] Local services: Google Business Profile claimed with current NAP, photos, recent reviews
- [ ] Review recency (older than 6 months reads as stale to AI engines)

**Industry directories (vertical-dependent)**
- [ ] Client listed in 3+ category-relevant industry directories
- [ ] NAP (name/address/phone) consistent across directories
- [ ] Professional association memberships visible and linked

**News / trade publications**
- [ ] Brand mentioned in tier-1 publications (vertical-relevant; e.g., TechCrunch for SaaS, Globe & Mail / Calgary Herald for local Canadian businesses)
- [ ] Trade press coverage in last 24 months
- [ ] Founder or executive bylines in industry publications

**Schema `sameAs` coverage**
- [ ] `Organization` schema includes `sameAs` linking to: LinkedIn, Twitter/X, Facebook, Wikipedia (if applicable), Wikidata (if applicable), Crunchbase, industry directories, YouTube channel
- [ ] Each `sameAs` URL resolves to a live, claimed profile
- [ ] At least 5 `sameAs` entries (entity grounding)

## How to audit — Part B (citation tracking over time)

This half grades whether monitoring infrastructure exists and is being used, not the citation rate itself (that's Pillar 6).

### Checks
- [ ] Baseline citation rate documented per LLM (from Pillar 6 first-run)
- [ ] Baseline competitor share documented (who's cited when client isn't)
- [ ] Defined cadence for re-runs (weekly, biweekly, or monthly)
- [ ] Same prompt panel used each cycle (so deltas are comparable)
- [ ] Same LLM versions tracked, with version drift logged when models update
- [ ] Sentiment captured per mention (positive / neutral / negative / inaccurate)
- [ ] Delta report produced after each cycle (citation rate change, new mentions, lost mentions)
- [ ] Competitor citation share tracked alongside client share
- [ ] Annotated correlation with client-side changes (when the client shipped a fix, did citations move?)
- [ ] Alerts on inaccurate or negative mentions (so they can be addressed)

## Combined scoring rubric (Parts A + B)

Score is a weighted average: Part A (off-page authority, 70%) + Part B (tracking infrastructure, 30%). Part A dominates because authority is the upstream driver of citations; tracking without authority just measures stagnation.

- **0–2 (Critical)**: Minimal off-page presence (Wikipedia absent, no review aggregator profiles, no Reddit mentions, no directory listings) AND no monitoring
- **3–5 (Poor)**: Presence in 1–2 off-page channels, ad-hoc monitoring only
- **6–7 (Adequate)**: Presence in 3+ relevant off-page channels (reviews, directories, social), monthly monitoring with delta reports
- **8–9 (Strong)**: Wikipedia entry or tier-1 news coverage, claimed and active profiles across all vertical-relevant aggregators, structured monitoring with competitor-share tracking and version drift logging
- **10 (Excellent)**: All of the above plus alerting on inaccuracies, annotated correlation between client-side changes and citation movement, and an active off-page authority growth strategy (PR outreach, directory submissions, review acquisition cadence)

## How to fix

### Quick wins (under 1 hr per item)
- Claim missing review aggregator profiles (G2/Capterra/Trustpilot/Google Business)
- Update NAP across the top 5 vertical directories
- Add comprehensive `sameAs` array to `Organization` schema
- Establish Pillar 6 baseline by running the panel once and saving outputs

### Substantive fixes (1–10 hrs)
- Wikipedia notability assessment: collect 3+ independent secondary sources (news, trade press, books) before attempting a draft
- YouTube: produce a single "what is [brand]" video, ensure transcript is detailed
- Review acquisition cadence: design a post-purchase / post-service review ask that targets the highest-leverage aggregator for the vertical
- Build monitoring spreadsheet: one column per cycle, one row per query; standardize delta output

### Deep fixes (>10 hrs / requires sustained effort or dev)
- Wikipedia article draft + submission (requires established editor history or a Wikipedia consultant; high effort, high return if successful)
- Trade press PR campaign for tier-1 coverage
- Founder thought-leadership program (LinkedIn essays, podcast appearances, conference talks) — most reliable path to durable off-page authority for B2B
- Automate citation tracking via LLM APIs (OpenAI, Anthropic, Perplexity) with results logged to database; build dashboard and alerting

## How this delivers the retainer

The monitoring retainer is justified by Part B — monthly re-runs of the Pillar 6 panel, delta reports, and surfacing of inaccuracies or competitor moves. Without ongoing tracking, the audit's findings go stale within 30–60 days. The retainer is the compound interest on the one-time audit.

Implementation work (schema fixes, content updates, off-page authority pursuit) is scoped separately as one-time engagements rather than bundled into the retainer.

### Monthly delta report contents
- Citation rate this month vs. last month (per LLM, overall)
- New mentions (where client wasn't cited before, now is)
- Lost mentions (where client was cited before, no longer is — usually highest priority to investigate)
- Competitor citation share change
- Notable shifts in *how* the brand is described
- Off-page authority changes (new Reddit threads, new reviews, news mentions)
- Recommended actions (DIY for client; implementation engagement quoted separately if client wants operator-implemented)

## Worked example

[Placeholder — populate from first real retainer cycle]

## Tools to consider

- **Profound, Otterly.AI, AthenaHQ, Peec AI** — purpose-built LLM monitoring SaaS. Good for clients who want the dashboard included; reduces operator workload but increases tooling cost (Profound $399+/mo, Otterly $29+/mo, Peec €89+/mo, Athena $250+/mo).
- **Manual + spreadsheet** — fine for first 5–10 retainers; lets you tune what to track before locking in tooling.
- **Custom via API** — best margin but requires upfront build time. Worth doing once you have 5+ retainer clients (the ROI is clear).

For Phase 1: manual + spreadsheet. Move to tooling when monitoring eats >2 hrs per client per month.

## References
- [Ahrefs — 75K-brand correlation study on AI visibility](https://ahrefs.com/blog/ai-visibility-study) — unlinked brand mentions r=0.664 vs backlinks r=0.218
- [Pillar 6 — Citation Probing](./06-citation-probing.md) (provides the panel methodology Part B tracks over time)
- [Reddit citation collapse Sept 2025 — CMSWire](https://www.cmswire.com/digital-marketing/reddits-rise-in-ai-citations-what-marketers-must-know-about-aeo-strategy/)
- [Wikipedia notability for organizations (WP:CORP)](https://en.wikipedia.org/wiki/Wikipedia:Notability_(organizations_and_companies))
