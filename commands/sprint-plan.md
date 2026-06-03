---
description: Plan the next sprint from the backlog.
argument-hint: [focus area]
---

# /sprint-plan

Run the **sprint-planning** skill.

Arguments: $ARGUMENTS
(If empty, auto-detect velocity and plan the next sprint.)

Reads the Jira backlog, analyzes velocity, proposes sprint scope and goal, and writes to Jira/Confluence on confirmation.

## Usage

```
/sprint-plan
/sprint-plan [capacity] [theme]
```

Examples:
- `/sprint-plan` — auto-detect velocity, plan next sprint
- `/sprint-plan 40 points` — plan with explicit capacity
- `/sprint-plan 2 weeks focus: checkout flow` — plan with theme filter

## Skill

See `skills/sprint-planning/SKILL.md` for full workflow.

## Required Config

- `DEFAULT_JIRA_PROJECT_KEY` in AGENTS.md
- Atlassian OAuth active with Jira write permission — or none; falls back to local `workspace/` files (see `connectors/local/CONNECTOR.md`)
