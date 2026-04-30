# Agent Discoverability

Productized audit service that scores how visible a business is to AI agents — ChatGPT, Claude, Perplexity, Gemini, and emerging autonomous shoppers — and rebuilds the client's site to be cited and recommended.

## What this repo is

Source of truth for the methodology, deliverable templates, outreach assets, project-scoped subagents, and (eventually) the marketing site. Built by a solo Calgary operator using Claude Code.

## Structure

```
methodology/         The 7-pillar audit framework
deliverables/        Templates clients receive ($1,500 fixed-price audit)
outreach/            CASL-compliant cold outreach assets and target lists
site/                Marketing site (added once first audits close)
.claude/skills/      Project-scoped skills (e.g., run-audit playbook)
.claude/agents/      Project-scoped subagents (email, intake, QA — built phased)
```

## Status

Phase 1: methodology written, manual audit delivery, no automation. See task list for current sprint.

## Conventions

- Skills follow gerund naming (`auditing-...`, `running-...`)
- Subagents use noun-based role names (`email-comms`, `client-support`, `qa-reviewer`)
- Methodology updates land in `methodology/` first; skill files reference it, never duplicate it
- Client-specific files go in a gitignored `clients/` directory
