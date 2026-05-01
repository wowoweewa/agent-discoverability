# Pillar 1 — Structured Data

## What it is

Schema.org markup (preferably JSON-LD in `<head>` or `<body>`) that declares the entities on a page: who the business is, what services/products it sells, what FAQs it answers, what articles it publishes. Structured data is the most reliable signal LLMs use to confidently extract facts about a business.

## Why it matters

Without structured data, an LLM has to infer entities from unstructured prose and often gets the wrong answer or hedges. With proper schema, the LLM answers with citations because it has high confidence in the underlying facts. FAQPage schema in particular shows the highest single-pillar ROI — it converts your existing Q&A content into format LLMs can quote verbatim.

## How to audit

### Inputs
- Site URL
- 5–10 representative inner pages (homepage, services, FAQ, product/case study, contact, blog post)

### Checks
- [ ] JSON-LD present on homepage and all key pages
- [ ] `Organization` or `LocalBusiness` schema with `name`, `url`, `logo`, `address`, `telephone`, `sameAs` (social profiles), `areaServed`
- [ ] `Service` or `Product` schema for primary offerings, with `name`, `description`, and where applicable `offers.price`
- [ ] `FAQPage` schema on any page with Q&A content
- [ ] `Article` or `BlogPosting` schema on content pages (with `author`, `datePublished`, `dateModified`)
- [ ] `BreadcrumbList` schema on inner pages
- [ ] All schema validates in Google Rich Results Test and Schema.org Validator with zero errors
- [ ] `sameAs` URLs point to verified social/professional profiles (LinkedIn, Crunchbase, Wikipedia where relevant) — strengthens entity recognition
- [ ] No conflicting structured data (e.g., two different `Organization` blocks)

### Scoring rubric
- **0–2 (Critical)**: No structured data, or only generic Open Graph tags
- **3–5 (Poor)**: Some schema present but with validation errors, missing core types, or incomplete fields
- **6–7 (Adequate)**: Organization + primary Service/Product schema valid; missing FAQPage and Article schema
- **8–9 (Strong)**: All recommended types present, valid, and complete; `sameAs` populated
- **10 (Excellent)**: All of the above plus `Speakable`, `HowTo`, or domain-specific schema appropriate to the vertical

## How to fix

### Quick wins (under 1 hr)
- Add `Organization` JSON-LD to root layout
- Add `FAQPage` schema to existing FAQ content (just wrap what's already there)
- Add `sameAs` array linking to LinkedIn, social profiles, Crunchbase

### Substantive fixes (1–5 hrs)
- Add `Article` schema across all blog posts (template-level change)
- Add `Service` or `Product` schema with prices where commercially appropriate
- Fix all validation errors flagged in Rich Results Test

### Deep fixes (>5 hrs / requires dev)
- CMS plugin or custom integration auto-generating schema from content models
- Knowledge Graph optimization: claim Wikidata entry, claim Google Knowledge Panel
- Domain-specific schema (e.g., `MedicalBusiness`, `LegalService`, `FinancialProduct`)

## Worked example

[Placeholder — populate from first real audit]

## References
- [Schema.org reference](https://schema.org/)
- [Google Rich Results Test](https://search.google.com/test/rich-results)
- [Schema Markup Validator](https://validator.schema.org/)
- [JSON-LD playground](https://json-ld.org/playground/)
