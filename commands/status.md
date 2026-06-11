---
description: Project health, burndown, and velocity report.
argument-hint: [sprint | date range]
---

# /status

Run the **status** skill.

Reporting period: $ARGUMENTS
(If empty, report on the current sprint.)

Generates a project health report with burndown data, velocity trends, and risk signals from Jira.

## Usage

```
/status
/status [sprint name or number]
/status last [N] sprints
```

Examples:
- `/status` — current sprint health report
- `/status last 3 sprints` — velocity trend across last 3 sprints
- `/status Sprint 41` — report for a specific named sprint

## Skill

See `skills/status/SKILL.md` for full workflow.

## Required Config

- `DEFAULT_JIRA_PROJECT_KEY` in AGENTS.md
- `DEFAULT_JIRA_BOARD_ID` in AGENTS.md (for burndown data)
- Atlassian OAuth active — or none; falls back to local `workspace/` files (see `connectors/local/CONNECTOR.md`)
