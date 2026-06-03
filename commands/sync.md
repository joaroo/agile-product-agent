---
description: Push the local workspace/ fallback up to Jira and Confluence.
argument-hint: [jira | confluence | project key]
---

# /sync

Run the **local-sync** skill.

Scope: $ARGUMENTS
(If empty, sync everything in `workspace/` — issues, sprints, and pages.)

Scans `workspace/` for unsynced or locally-modified items, presents a dry-run summary, and on confirmation pushes them to Jira and Confluence in topological order. Writes real Jira keys and Confluence IDs back into local file frontmatter. Emits a reconciliation report with a key map and any items that need manual follow-up.

## Usage

```
/sync
/sync jira
/sync confluence
/sync MYPROJ
```

Examples:
- `/sync` — push everything (issues, sprints, pages)
- `/sync jira` — push Jira issues and sprints only
- `/sync confluence` — push Confluence pages only
- `/sync MYPROJ` — push issues and sprints for a specific project key

## Skill

See `skills/local-sync/SKILL.md` for full workflow.

## Required Config

- `DEFAULT_JIRA_PROJECT_KEY` and `DEFAULT_CONFLUENCE_SPACE_ID` in AGENTS.md (used to map local keys to real project/space keys)
- **A live Atlassian connection is required** — this command does nothing useful in local-only mode. Add `.mcp.json` (copy from `.mcp.json.example`) and restart the session to enable live mode. See `connectors/atlassian/CONNECTOR.md` and `connectors/local/CONNECTOR.md`.
