# Pillar 3 — `llms.txt` (non-scored signal)

## Status: included for completeness, not scored

As of May 2026, no major AI engine has confirmed using `llms.txt` for retrieval, ranking, or grounding. Google's John Mueller stated directly on Bluesky (December 2024, reaffirmed 2025): "FWIW no AI system actually uses llms.txt." Adoption among top-1M domains sits in the 1–10% range. Crawlers fetch the file but do not weight it.

The check stays in the audit for two reasons: (a) sophisticated buyers ask about it, and (b) it's a 1-hour build that costs nothing if a future engine adopts the standard. **It does not move the audit score.** Treat as a hygiene artifact, not a lever.

## What it is

A markdown file served at the site root (`/llms.txt`) that gives an AI-readable summary of the site: what the business does, key pages, contact info, authoritative URLs. Proposed by Jeremy Howard (late 2024). Optional companion `/llms-full.txt` includes longer-form authoritative content.

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
- [ ] Total file size under 50KB
- [ ] Links resolve to live pages (no 404s)
- [ ] No marketing fluff — just authoritative pointers
- [ ] `/llms-full.txt` (optional) present if the business has substantial documentation worth inlining

### Reporting

Do not assign a 0–10 score. In the audit deliverable, report as one of:
- **Present and well-formed** — file exists, follows the template, links resolve
- **Present but malformed** — file exists, fails 2+ checks above
- **Absent** — no file at `/llms.txt`

Include in the "Quick wins" appendix of the audit if absent or malformed. Do not include in the main score calculation.

## How to fix (if client asks)

### Quick win (under 1 hr)
- Generate a baseline `/llms.txt` from the site's existing nav and footer using the template below
- Curate which pages belong (focus on authoritative, evergreen content — not promotional)

### Substantive (1–3 hrs)
- Add `/llms-full.txt` with inlined service descriptions, FAQs, and policies for sites with rich documentation
- Auto-generate from CMS as part of build pipeline so it stays current

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

## References
- [llms.txt proposal — Jeremy Howard](https://llmstxt.org/)
- [Mueller statement on llms.txt non-use](https://bsky.app/profile/johnmu.com) — Google Search Advocate, December 2024, reaffirmed 2025
- [llms.txt directory — sites that have adopted it](https://directory.llmstxt.cloud/)
