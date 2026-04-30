# Project-Scoped Subagents

Subagents in this folder are loaded only when Claude Code is run inside this project. They are personas/roles specific to running an Agent Discoverability audit business.

## Build order (phased)

Don't create these until the trigger condition is met. Building agents for traffic that doesn't exist is the #1 stall pattern for solo "automated businesses."

| Subagent | Trigger to build | Purpose |
|---|---|---|
| `email-comms` | Sending >5 outreach messages/week | Drafts CASL-compliant outreach + follow-up. Operator approves before send. |
| `client-support` | >5 inbound inquiries/week | Answers FAQ, qualifies leads, books discovery calls. |
| `qa-reviewer` | ≥10 audits delivered | Reviews each audit PDF for methodology adherence and tone before client send. |

## Subagent file format

Each subagent lives at `.claude/agents/<name>.md` with frontmatter:

```yaml
---
name: <name>
description: <one-line — when to invoke this agent>
tools: <subset of tools the agent should have>
---

<system prompt for the subagent>
```

## What NOT to do

- Don't build a subagent until its trigger condition is met.
- Don't give a subagent send-on-its-own authority. Operator approves all client-facing output until volume justifies otherwise (months out).
- Don't duplicate methodology into the subagent prompt. Reference `methodology/` files instead.
