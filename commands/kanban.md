---
description: Triage and move Kanban cards; check WIP and flow.
argument-hint: [triage | move | wip-check | flow-review]
---

# /kanban

Run the **kanban-workflow** skill.

Action: $ARGUMENTS
(If empty, ask which action to run.)

Triages incoming work, moves cards, checks WIP limits, and reviews flow health.

## Usage

```
/kanban [action]
```

Actions: `triage` | `move [key] to [status]` | `wip-check` | `flow-review`

Examples:
- `/kanban triage` — triage new issues from the last 7 days
- `/kanban move PROJ-42 to "In Review"` — transition a card
- `/kanban wip-check` — flag WIP limit violations and aging in-progress items
- `/kanban flow-review` — full flow health analysis

## Skill

See `skills/kanban-workflow/SKILL.md` for full workflow.

## Required Config

- `DEFAULT_JIRA_PROJECT_KEY` in AGENTS.md
- Atlassian OAuth active with Jira write permission (for moves) — or none; falls back to local `workspace/` files (see `connectors/local/CONNECTOR.md`)
