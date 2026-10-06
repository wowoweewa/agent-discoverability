# Agent Discoverability

A free seven-pillar method for auditing how visible a business website is to AI assistants (ChatGPT, Claude, Perplexity, Gemini), with the Claude Code skills and subagent that run it.

## What this repo is

Source of truth for the method, the audit report template, and the project-scoped skills and subagent that run an audit. Built by a solo operator using Claude Code. The audit is free and no prices are published here.

## Structure

```
methodology/         The 7-pillar audit framework
deliverables/        The audit report template (section structure only)
.claude/skills/      Project-scoped skills: running-an-audit, probing-citations, designing-audit-pdf
.claude/agents/      Project-scoped subagents: auditor is built, four more are planned
```

## Status

The seven pillars are written. Three skills and the `auditor` subagent are built and ran one full test audit on August 7, 2026. The Pillar 6 citation panel was generated for that audit but has never been run against the live assistants. Open work is in TASKS.md.

## Conventions

- Skills follow gerund naming (`running-an-audit`, `probing-citations`)
- Subagents use role names (`auditor`, `marketer`, `ceo`, `qa`, `customer-support`)
- Methodology updates land in `methodology/` first; skill files reference it, never duplicate it
- Client-specific files go in a gitignored `clients/` directory
