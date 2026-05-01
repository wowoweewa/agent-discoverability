# Prompt Panel — Template

Used by the Auditor subagent during Pillar 6 (Citation Probing). 25 queries across 8 categories. Customize per client by filling the variables.

## Variables per client

| Variable | Example |
|---|---|
| `{CATEGORY}` | "real estate agents", "family law firms", "physiotherapy clinics" |
| `{CITY}` | "Calgary", "Calgary SE", "Edmonton" |
| `{COMPETITOR_A}`, `{B}`, `{C}` | 3 direct competitors named explicitly |
| `{CLIENT_NAME}` | client business name |
| `{NICHE}` | sub-segment (e.g., "first-time homebuyers", "small business tax") |
| `{SPECIFIC_SERVICE}` | concrete service offered (e.g., "buyer representation") |
| `{USE_CASE}` | scenario (e.g., "complex contract dispute") |

## The 25 queries

### A. Category + location (5)
1. Best `{CATEGORY}` in `{CITY}`?
2. Top-rated `{CATEGORY}` in `{CITY}`?
3. Who are the most reputable `{CATEGORY}` in `{CITY}`?
4. Recommended `{CATEGORY}` near `{CITY}`?
5. I'm looking for a `{CATEGORY}` in `{CITY}`, who should I consider?

### B. Need + buying intent (4)
6. I need `{SPECIFIC_SERVICE}`; who should I hire in `{CITY}`?
7. What `{CATEGORY}` should I work with for `{USE_CASE}`?
8. Best `{CATEGORY}` for `{NICHE}` in `{CITY}`?
9. Who can help me with `{SPECIFIC_SERVICE}` in `{CITY}`?

### C. Comparison (3)
10. `{COMPETITOR_A}` vs `{COMPETITOR_B}` — which is better for `{USE_CASE}`?
11. `{COMPETITOR_A}` vs `{COMPETITOR_C}` — pros and cons?
12. Alternatives to `{COMPETITOR_A}` in `{CITY}`?

### D. Long-tail informational (4)
13. How do I choose a `{CATEGORY}`?
14. What should I know before hiring `{CATEGORY}`?
15. Common mistakes when choosing `{CATEGORY}`?
16. Questions to ask `{CATEGORY}` before signing?

### E. Brand-direct (3)
17. Tell me about `{CLIENT_NAME}`.
18. What does `{CLIENT_NAME}` do?
19. Reviews of `{CLIENT_NAME}`.

### F. Pricing (2)
20. How much do `{CATEGORY}` cost in `{CITY}`?
21. Typical pricing for `{SPECIFIC_SERVICE}` in `{CITY}`?

### G. Trust / credentials (2)
22. Most trustworthy `{CATEGORY}` in `{CITY}`?
23. `{CATEGORY}` with the best reputation in `{CITY}`?

### H. Niche / segment (2)
24. `{CATEGORY}` specializing in `{NICHE}` in `{CITY}`?
25. `{CATEGORY}` for `{NICHE}` in `{CITY}`?

## Run protocol

For each of the 25 queries on each of 4 LLMs (100 runs total):

1. Open a fresh chat or temporary session
2. Paste the query verbatim — no preamble, no clarification, no follow-up
3. Capture: LLM name + model version + date + full verbatim response
4. Score: Cited (Yes) / Cited (Indirect) / Not Cited (Competitor) / Not Cited (Other) / Not Cited (Refusal)
5. If a competitor is cited, record which one and the snippet

## Output format

Per query, the Auditor produces a row:

```
query_id | category | llm | model_version | date | citation_status | competitors_cited | response_excerpt
```

Aggregated per LLM and overall, the Auditor produces:

- Citation rate = (Yes + 0.5×Indirect) / 25
- Competitor share = sum of competitor citations / 25 per competitor
- Refusal rate = Refusals / 25 (high refusal rate is its own diagnostic — usually means the LLM doesn't have category authority signals)

## Customizing per vertical

When a vertical is locked (task #4), build a vertical-specific overlay at `methodology/prompt-panels/<vertical>/queries.md` that:

- Adds 5–10 vertical-specific queries (e.g., for law: "[practice area] lawyer for [client type]")
- Adjusts the comparison queries to use the named competitors common in that vertical
- Adds regulatory or credential-specific queries where relevant (e.g., for healthcare: insurance, board certification)

The 25-query base panel stays constant across all clients; the overlay is what makes each audit feel custom.

## What NOT to do

- Don't run queries with personalization — always use logged-out, temporary sessions
- Don't run queries from a different geographic IP than the client's target market
- Don't editorialize the queries — the panel is the panel
- Don't reuse cached responses; LLM outputs change weekly
