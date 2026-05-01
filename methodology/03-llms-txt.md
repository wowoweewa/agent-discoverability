# Pillar 3 — `llms.txt`

## What it is

A markdown file served at the site root (`/llms.txt`) that gives an AI-readable summary of the site: what the business does, key pages, contact info, and authoritative URLs. Proposed by Jeremy Howard (late 2024) as an emerging standard analogous to `robots.txt` and `sitemap.xml`. Optional companion file `/llms-full.txt` includes longer-form authoritative content for inlining.

## Why it matters

When an LLM agent visits a site to answer a user query, it benefits from a curated, low-noise summary instead of crawling the whole site. `llms.txt` delivers that summary in a format LLMs parse cleanly. Adoption is still early in 2026 — which means it's both a low-effort wedge for visibility and a way to signal AI-readiness to sophisticated buyers.

## How to audit

### Inputs
- Site URL → check `/llms.txt` and `/llms-full.txt`

### Checks
- [ ] `/llms.txt` exists and returns 200
- [ ] Markdown formatted, not HTML
- [ ] Begins with `# [Business Name]`
- [ ] One-paragraph blockquote summary directly under the H1
- [ ] Sectioned by H2 with logical groupings (e.g., "## Services", "## Documentation", "## Pricing")
- [ ] Each entry under a section is a markdown link (`[label](url)`) optionally followed by a short description
- [ ] Total file size reasonable (under 50KB)
- [ ] Links resolve to live pages (no 404s)
- [ ] No marketing fluff or sales copy — just authoritative pointers
- [ ] `/llms-full.txt` (optional) present if the business has substantial documentation worth inlining
- [ ] Linked from `robots.txt` or sitemap (helps discovery)

### Scoring rubric
- **0–2 (Critical)**: No `llms.txt` present
- **3–5 (Poor)**: File exists but malformed, contains marketing copy, or has broken links
- **6–7 (Adequate)**: Valid `llms.txt` with summary and key sections; missing some logical groupings
- **8–9 (Strong)**: Complete, well-organized `llms.txt` covering services, contact, key resources
- **10 (Excellent)**: Both `llms.txt` and `llms-full.txt` present and well-maintained; linked from `robots.txt`; updated within last 90 days

## How to fix

### Quick wins (under 1 hr)
- Generate a baseline `/llms.txt` from the site's existing nav and footer
- Reference: see template below

### Substantive fixes (1–5 hrs)
- Curate which pages belong (focus on authoritative, evergreen content — not promotional)
- Add `/llms-full.txt` with inlined service descriptions, FAQs, and policies for sites with rich documentation
- Link `/llms.txt` from `robots.txt` (`Sitemap: /llms.txt` is non-standard but increasingly recognized)

### Deep fixes (>5 hrs / requires dev)
- Auto-generate `llms.txt` from CMS content as part of build pipeline so it stays current
- Variant `llms.txt` per language for multilingual sites

## Template

```markdown
# [Business Name]

> [One paragraph: what the business does, who it serves, where it operates. 2–3 sentences max.]

## About
- [About the company](/about): brief description
- [Team](/team): brief description

## Services
- [Service 1](/services/service-1): what it solves
- [Service 2](/services/service-2): what it solves

## Resources
- [Documentation](/docs): brief description
- [Case studies](/case-studies): brief description
- [Blog](/blog): brief description

## Contact
- [Contact](/contact): how to reach the team
- Email: hello@example.com
- Location: City, Province/State, Country
```

## Worked example

[Placeholder — populate from first real audit]

## References
- [llms.txt proposal — Jeremy Howard](https://llmstxt.org/)
- [llms.txt directory — sites that have adopted it](https://directory.llmstxt.cloud/)
