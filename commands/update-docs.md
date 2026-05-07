# /update-docs

Run the **document-update** skill.

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

See `skills/document-update/SKILL.md` for full workflow.

## Required Config

- `DEFAULT_CONFLUENCE_SPACE_ID` in AGENTS.md
- Atlassian OAuth active with Confluence write permission
