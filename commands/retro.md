---
description: Facilitate a sprint retrospective and write the retro page.
argument-hint: [sprint]
---

# /retro

Run the **retrospective** skill.

Sprint: $ARGUMENTS
(If empty, run the retro for the last closed sprint.)

Pulls sprint metrics, structures what went well / what to improve / action items, and writes the retrospective page on confirmation.

## Usage

```
/retro
/retro [sprint name or number]
```

Examples:
- `/retro` — retrospective for the most recently closed sprint
- `/retro Sprint 41` — retrospective for a specific named sprint

## Skill

See `skills/retrospective/SKILL.md` for full workflow.

## Required Config

- `DEFAULT_JIRA_PROJECT_KEY` in AGENTS.md
- `DEFAULT_CONFLUENCE_SPACE_ID` in AGENTS.md (for the retro page)
- Atlassian OAuth active with Confluence write permission — or none; falls back to local `workspace/` files (see `connectors/local/CONNECTOR.md`)
