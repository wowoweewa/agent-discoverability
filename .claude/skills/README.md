# Project-Scoped Skills

Skills here load only when Claude Code is run inside this project. Reusable workflows that are not generalizable beyond this business belong here.

Generalizable skills (e.g., a generic AEO methodology that any operator could use) belong in the global `~/claude-skills` repo, not here.

## Skills

| Skill | Purpose | Status |
|---|---|---|
| `running-an-audit` | End-to-end audit workflow given a client URL: dispatches the auditor, runs a QA pass on the draft, saves it to the client folder | Built |
| `probing-citations` | Pillar 6 citation panel: 25 queries against four assistants, scored, with a delta against the prior cycle | Built |
| `designing-audit-pdf` | Renders the audit markdown as a styled HTML page for print to PDF | Built |
| `drafting-outreach` | Generate CASL-compliant outreach to a target firm given their site + role | Planned, after a vertical is chosen |
| `monitoring-citations` | Monthly citation-tracking workflow | Planned; `probing-citations` already covers the monthly re-run |

## Skill file format

Each skill lives at `.claude/skills/<gerund-name>/SKILL.md` with frontmatter:

```yaml
---
name: <gerund-name>
description: <one-line trigger — when to use this skill>
---

<skill instructions>
```

## What NOT to do

- Don't put methodology *content* in a skill — skills reference `methodology/` files, they don't duplicate them.
- Don't create skills for things that happen once or twice. A skill earns its existence through repeat use.
