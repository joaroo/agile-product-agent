---
description: Surface product opportunities and gaps from Jira + Confluence.
argument-hint: [focus area]
---

# /discover

Run the **discover** skill.

Focus area: $ARGUMENTS
(If empty, run broad discovery across the default project.)

Queries Jira and Confluence to surface product opportunities, gaps, stale work, and user-facing pain points.

## Usage

```
/discover
/discover [focus area]
```

Examples:
- `/discover` — broad discovery across the default project
- `/discover onboarding` — discovery scoped to onboarding theme
- `/discover billing sprint-ready` — discovery filtered to billing + items ready for sprint

## Skill

See `skills/discover/SKILL.md` for full workflow.

## Required Config

- `DEFAULT_JIRA_PROJECT_KEY` in AGENTS.md
- `DEFAULT_CONFLUENCE_SPACE_ID` in AGENTS.md
- Atlassian OAuth active — or none; falls back to local `workspace/` files (see `connectors/local/CONNECTOR.md`)
