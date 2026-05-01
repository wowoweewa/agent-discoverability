# Pillar 5 — Agent Readiness

## What it is

Whether autonomous AI shopping/booking agents (OpenAI Operator, Anthropic Computer Use, Manus, emerging agentic shoppers) can complete user-initiated tasks on the site: requesting a quote, booking a service, buying a product, submitting an inquiry — without hitting human-only barriers.

## Why it matters

In 2025–2026 agentic commerce moved from experiment to early adoption. McKinsey projects $1T US B2C agentic commerce by 2030; Stripe shipped agent payments. Sites that block agents miss this entire emerging channel. Sites that *intentionally* expose agent-friendly flows can become the default recommendation in agentic shopping contexts.

## How to audit

### Inputs
- Site URL + the conversion flow(s) that matter (book a call, request quote, buy product)
- Manual walkthrough of each flow with browser DevTools

### Checks
- [ ] Pricing visible without form submission (or clearly stated as "contact for pricing" with reason)
- [ ] Primary booking/quote/buy flow doesn't require account creation
- [ ] Forms accept structured input (no CAPTCHAs on simple lookups; no honeypot-only protection on critical flows)
- [ ] Forms have proper `name` and `id` attributes for fields (not just `placeholder`-only)
- [ ] Field labels are actual `<label>` elements, not visual-only placeholders
- [ ] Submit buttons are `<button>` or `<input type="submit">`, not custom `<div onClick>`
- [ ] Error states are surfaced in HTML/aria, not just visual
- [ ] No mandatory phone verification on first contact
- [ ] Email-based opt-in confirmation flow doesn't require human-only steps (image CAPTCHA, SMS code) for low-stakes inquiries
- [ ] Product/service catalog has stable URLs (not session-bound query strings)
- [ ] Where applicable: structured product feed (Google Merchant feed, JSON catalog) accessible via API or downloadable
- [ ] Schema.org `Offer` with `price` on product/service pages
- [ ] Robots and crawler rules allow access to conversion pages (some sites block these accidentally)

### Scoring rubric
- **0–2 (Critical)**: Conversion gated behind login + CAPTCHA + phone verify; pricing hidden; no structured catalog
- **3–5 (Poor)**: Multiple human-only steps in conversion flow; visual-only forms
- **6–7 (Adequate)**: Forms semantically marked up; pricing visible; CAPTCHA only on submit (not lookup)
- **8–9 (Strong)**: All of the above plus structured product feed; no unnecessary friction
- **10 (Excellent)**: All of the above plus published agent-friendly API or `agent.txt`-style declaration of supported actions

## How to fix

### Quick wins (under 1 hr)
- Add proper `<label>` elements to key forms
- Ensure submit buttons are real `<button>` elements
- Make pricing visible where currently hidden behind "request quote" with no reason

### Substantive fixes (1–5 hrs)
- Remove CAPTCHAs from low-risk lookup flows (keep on submit if needed for fraud)
- Add `Offer` schema with prices to product/service pages
- Test the full conversion flow with keyboard-only navigation (proxy for agent navigation)

### Deep fixes (>5 hrs / requires dev)
- Build a public product/service feed (JSON or Google Merchant format)
- Add an Operator/Computer Use compatibility test as part of QA
- Consider publishing an `agent.txt` or structured action manifest declaring supported agent flows

## Worked example

[Placeholder — populate from first real audit]

## Vertical-specific notes

- **Law firms**: agent-readiness usually means a frictionless "request consultation" form with case-type dropdown — most have this; the audit issue is usually CAPTCHA + phone-required
- **Real estate**: structured listing feeds (already standard via MLS/IDX) are good; the gap is contact form gates
- **Trades/services**: quote-request forms; ensure pricing-range visible to qualify before requesting human time
- **E-commerce**: Schema `Offer` with prices is non-negotiable; structured feed is the differentiator
- **Healthcare**: regulated; `bookable` schema and direct online booking are the agent-readiness signals

## References
- [Schema.org Offer](https://schema.org/Offer)
- [Stripe agentic commerce announcement](https://stripe.com/newsroom)
- [OpenAI Operator](https://openai.com/index/introducing-operator/)
- [Anthropic Computer Use](https://www.anthropic.com/news/3-5-models-and-computer-use)
