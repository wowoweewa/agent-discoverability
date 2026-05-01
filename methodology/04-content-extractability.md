# Pillar 4 — Content Extractability

## What it is

Whether the substance of a page — facts, prices, hours, claims, answers — is in plain HTML text accessible without JavaScript execution, OCR, or PDF parsing. Most LLM crawlers fetch raw HTML; content rendered only by client-side JavaScript, locked in images, or buried in PDFs without text layers is effectively invisible to them.

## Why it matters

A site can have perfect schema and full crawler access and still be invisible if the actual content arrives via React after page load with no SSR. This pillar is the single biggest gap on modern marketing sites built with Next.js, Remix, Astro, or framework SPAs that haven't been configured for SSR/SSG.

## How to audit

### Inputs
- Site URL + 5–10 representative pages
- `curl -A "Mozilla/5.0 (compatible; ClaudeBot/1.0)" <url>` to fetch raw HTML
- Comparison: same URL rendered in browser

### Checks
- [ ] Raw HTML (curl/wget) contains the substantive page content
- [ ] Headline (H1), key paragraphs, and CTAs visible without JS execution
- [ ] Pricing visible in raw HTML where prices are public
- [ ] Service/product descriptions in plain text, not embedded in image-only graphics
- [ ] Heading hierarchy clean: one H1 per page, H2 for major sections, H3 for sub-sections, no skipped levels
- [ ] Lists use `<ul>` / `<ol>` not styled divs
- [ ] Tables use `<table>` semantics, not images of tables
- [ ] Images have descriptive `alt` attributes (not empty, not "image1.png")
- [ ] Video content has transcripts or captions in HTML
- [ ] PDFs (if used for important content) have extractable text layer (test by selecting text in browser)
- [ ] Important PDFs are also published as HTML (don't trap key content in PDF-only)
- [ ] No content gated behind a login when the page is supposed to be public-facing
- [ ] Cookie/consent overlays don't return placeholder HTML to crawlers (some libraries do this)

### Scoring rubric
- **0–2 (Critical)**: Raw HTML is mostly empty (`<div id="root"></div>` SPA); main content invisible
- **3–5 (Poor)**: Some content in raw HTML but most key facts client-rendered or in images
- **6–7 (Adequate)**: Most content extractable; some weak alt text and table-as-image issues
- **8–9 (Strong)**: All key content in raw HTML with clean semantics
- **10 (Excellent)**: All of the above plus transcripts on video, HTML versions of PDFs, semantic markup throughout

## How to fix

### Quick wins (under 1 hr)
- Fix obvious empty alt attributes on key images
- Convert image-of-text headlines to actual H1/H2 text
- Add transcripts to embedded videos via the platform's existing tools

### Substantive fixes (1–5 hrs)
- Convert image-of-table content to actual HTML tables
- Replace tracking-pixel-only image hero with text + CSS background
- Audit and rewrite alt text across content pages
- Publish HTML versions of business-critical PDFs

### Deep fixes (>5 hrs / requires dev)
- Migrate SPA to SSR/SSG (Next.js, Remix, Astro all support this) — biggest impact for React/Vue marketing sites
- Server-side render at minimum the marketing/content surface (keep app shell client-rendered if needed)
- Establish content extraction tests in CI (curl assertions on raw HTML)

## Worked example

[Placeholder — populate from first real audit]

## Common failure patterns

- Webflow + heavy JS interactions: usually OK because Webflow SSRs by default
- Framer sites: variable, often need manual SSR check
- Next.js Pages Router with `getStaticProps`: usually fine
- Next.js App Router with Server Components: fine by default
- Vite + React SPA: almost always fails this pillar
- Wix: usually fine for marketing pages, weaker for app-like sections
- Squarespace: fine
- WordPress: fine, depending on theme

## References
- [Google's "what's in the HTML" guidance](https://developers.google.com/search/docs/crawling-indexing/javascript/javascript-seo-basics)
- [HTML semantic elements reference](https://developer.mozilla.org/en-US/docs/Web/HTML/Element)
