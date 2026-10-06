# Project Subagents — Minimal Roster

Five role-based subagents. Names match how a real business is staffed so the operator never has to translate function names. All client-facing output goes through the operator before send until volume + track record justify trust delegation.

## The roster

Built so far: Auditor only (`.claude/agents/auditor.md`). The other four are planned; see Build order.

| Agent | What they own | Project skills | Existing global skills used |
|---|---|---|---|
| **CEO** | Strategy + routing. Single front door. Reads incoming requests and dispatches to the right agent. Final approver on anything client-facing before send. | — (uses judgment over project context) | — |
| **Marketer** | Outreach drafts (cold email + LinkedIn), prospect research, content, positioning, social | `drafting-outreach` | `market-research-expert`, `gws-gmail` |
| **Auditor** | The technical team. Given a client URL, runs the 7-pillar methodology, probes LLMs, scores each pillar, drafts the audit markdown using `deliverables/audit-template.md` | `running-an-audit` | `claude-api` |
| **QA** | Reviews every audit before it leaves. Checks methodology adherence, evidence quality, tone, factual accuracy | `reviewing-audit-quality` | — |
| **Customer Support** | Inbound FAQ, lead qualification, booking calls, client comms during and after audit | `handling-inbound` | `gws-gmail`, `gws-calendar` |

## What's intentionally NOT here

- **Bookkeeper** — manual Google Sheet for first 6 months, far better than encoding categories before you know what matters
- **Compliance logger** — operator logs CASL evidence manually until volume justifies a system
- **Designer** — design is delivered via the `designing-audit-pdf` skill, not a separate agent
- **Onboarding/intake/sales** — folded into Customer Support and CEO

If a need is recurring and not absolutely required to run the business, it doesn't get its own agent.

## Subagent file format

Each subagent lives at `.claude/agents/<name>.md` with frontmatter:

```yaml
---
name: <name>
description: <one-line — when to invoke this agent>
tools: <subset of tools the agent should have, or omit for default>
---

<system prompt for the subagent>
```

## Build order

| # | Agent | Trigger to build |
|---|---|---|
| 1 | **Auditor** | Methodology pillars 01–07 written. Spine of the business. |
| 2 | **Marketer** | Auditor can deliver. Outreach to fill the pipeline. |
| 3 | **CEO** | Marketer + Auditor stable. CEO becomes the front door. |
| 4 | **QA** | 5+ audits manually delivered. Operator knows what "good" looks like. |
| 5 | **Customer Support** | Inbound inquiries exceed ~5/week. |

## What NOT to do

- Don't build subagents before their trigger condition is met
- Don't give any subagent send-on-its-own authority. CEO drafts/approves; operator hits send
- Don't duplicate methodology into subagent prompts — reference `methodology/` files
- Don't add new agents without a recurring, business-critical need
