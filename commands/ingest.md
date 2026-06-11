---
description: Parse docs, notes, or email into Jira issues / Confluence pages.
argument-hint: [file path | email label]
---

# /ingest

Run the **ingest** skill.

Source: $ARGUMENTS
(If empty, parse content pasted directly into the conversation.)

Parses local docs, meeting notes, chat transcripts, or email and extracts Jira issues and Confluence content for review before any write.

## Usage

```
/ingest [source]
/ingest [file path]
/ingest email [label or thread ID]
```

Examples:
- `/ingest ~/notes/meeting-2026-05-07.md` — parse a local meeting notes file
- `/ingest email label:standup` — fetch and parse recent standup emails
- `/ingest` then paste content — parse content pasted directly into conversation

## Skill

See `skills/ingest/SKILL.md` for full workflow.

## Required Config

- `DEFAULT_JIRA_PROJECT_KEY` in AGENTS.md
- `DEFAULT_CONFLUENCE_SPACE_ID` in AGENTS.md (for page creation)
- Atlassian OAuth active with write permission — or none; falls back to local `workspace/` files (see `connectors/local/CONNECTOR.md`)
- `EMAIL_MCP_TOOL` in `.env` (only required for email source; email has no local fallback)
