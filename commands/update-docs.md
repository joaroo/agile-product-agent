---
description: Create or update a Confluence page.
argument-hint: [page title or topic]
---

# /update-docs

Run the **update-docs** skill.

Arguments: $ARGUMENTS
(If empty, ask which page to create or update.)

Creates or updates a Confluence page with structured, well-formatted content.

## Usage

```
/update-docs [page title or ID] [content or instructions]
/update-docs new [title] in [space] [content]
```

Examples:
- `/update-docs "API Reference" add section for authentication`
- `/update-docs new "Sprint 42 Retrospective" in TEAM [paste content]`
- `/update-docs "Architecture Overview" update the database section`

## Skill

See `skills/update-docs/SKILL.md` for full workflow.

## Required Config

- `DEFAULT_CONFLUENCE_SPACE_ID` in AGENTS.md
- Atlassian OAuth active with Confluence write permission — or none; falls back to local `workspace/` files (see `connectors/local/CONNECTOR.md`)
