# /sprint-plan

Run the **sprint-planning** skill.

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
- Atlassian OAuth active with Jira write permission
