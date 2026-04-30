# Project-Scoped Skills

Skills here load only when Claude Code is run inside this project. Reusable workflows that are not generalizable beyond this business belong here.

Generalizable skills (e.g., a generic AEO methodology that any operator could use) belong in the global `~/Projects/claude-skills/` repo, not here.

## Planned skills

| Skill | Purpose | Built when |
|---|---|---|
| `running-an-audit` | End-to-end audit workflow given a client URL: probe LLMs, score pillars, generate PDF | After methodology pillars 01–07 are written |
| `drafting-outreach` | Generate CASL-compliant outreach to a target firm given their site + role | After vertical is chosen |
| `monitoring-citations` | Monthly citation-tracking workflow for retainer clients | After first retainer signed |

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
