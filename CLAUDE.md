# Project: Agent Discoverability

Productized AEO/GEO audit service. Solo operator. $1,500 fixed-price audit + $400/mo monitoring retainer. CASL-compliant outreach. Calgary-based, serves Canadian and US clients.

## When working in this folder

- The methodology in `methodology/` is the source of truth. Never invent audit logic; reference the pillar files.
- Audit deliverables follow `deliverables/audit-template.md` (section structure only — no visual design). Final PDF styling is applied by the `designing-audit-pdf` skill (to be built).
- Client work goes in `clients/<client-slug>/` (gitignored).
- For outreach copy, default to CASL-compliant patterns (sender ID + physical address + unsubscribe + B2B with conspicuously published contact).

## Project subagents (build phased — don't create until needed)

| Subagent | Path | Built when |
|---|---|---|
| `email-comms` | `.claude/agents/email-comms.md` | Sending >5 outreach messages/week |
| `client-support` | `.claude/agents/client-support.md` | Inbound inquiries >5/week |
| `qa-reviewer` | `.claude/agents/qa-reviewer.md` | ≥10 audits delivered |

## What NOT to do

- Don't build automation infrastructure before the manual workflow has run 3+ times.
- Don't conflate methodology with skill plumbing — methodology is markdown content, skills are thin invocation triggers.
- Don't write to `clients/` from autocomplete or speculative work; client folders only get touched during real engagements.
- Don't push commits without explicit user approval.

## Open decisions (see top-level task list)

- Vertical to focus first 10 outreach on (research pending)
- Brand name and positioning angle
- Whether the marketing site lives in this repo or as a separate `wowoweewa/agent-discoverability-site` repo
