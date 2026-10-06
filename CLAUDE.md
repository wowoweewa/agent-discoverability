# Project: Agent Discoverability

A free seven-pillar method, with Claude skills and one subagent, for auditing how visible a business website is to AI assistants (ChatGPT, Claude, Perplexity, Gemini). It is a lead magnet into consulting, never a priced product. No prices go in any tracked file.

## Read order at resume

1. PROGRESS.md, top entry only: state, next step, watch-outs.
2. TASKS.md, first unchecked item: the next action and its owner tag.
3. PLAN.md when the task needs the why or a closed decision.
4. RESEARCH.md when the task needs the evidence behind a decision.

## Facts

- Stack: Markdown method files, three project skills and one subagent under `.claude/`. No app, no site, no dependencies.
- Run: in a Claude Code session in this folder, say "run an audit on https://<site>". The `running-an-audit` skill dispatches the `auditor` subagent and writes a draft to `clients/<slug>/`. "render the audit" runs `designing-audit-pdf`.
- Test: no tests; there is no code.
- Deploy: not deployed.
- Repo: PUBLIC GitHub `wowoweewa/agent-discoverability`, default branch `main`.
- Local only (gitignored; never commit them): PROGRESS.md, PLAN.md, RESEARCH.md, `research/`, `outreach/`, `clients/`. They hold business detail. The retired price text is in `research/pricing-retired-2026-10-05.md`.
- Privacy scan: `python3 ~/Projects/skills-public/scripts/privacy_scan.py`, run from this folder. One known harmless hit: the example address at `methodology/03-llms-txt.md` line 71.

## When working in this folder

- The methodology in `methodology/` is the source of truth. Never invent audit logic; reference the pillar files.
- Audit deliverables follow `deliverables/audit-template.md` (section structure only, no visual design). Final PDF styling is applied by the `designing-audit-pdf` skill.
- Client work goes in `clients/<client-slug>/` (gitignored).
- For outreach copy, default to CASL-compliant patterns (sender ID + physical address + unsubscribe + B2B with conspicuously published contact).
- The repo is public. Write nothing personal in a tracked file: no email address, no client name, no price.
- Run the privacy scan and /security-review before every push; the repo is public.

## Project subagents

Five role-based subagents are planned and one is built, `auditor`. The roster and the trigger that allows each build are in `.claude/agents/README.md`. Don't build a subagent before its trigger is met. Don't add an agent without a recurring, business-critical need.

## What NOT to do

- Don't build automation infrastructure before the manual workflow has run 3+ times.
- Don't conflate methodology with skill plumbing. Methodology is markdown content; skills are thin invocation triggers.
- Don't write to `clients/` from autocomplete or speculative work; client folders only get touched during real engagements.

## Open decisions

Open decisions and tasks live in TASKS.md; never list them here.
