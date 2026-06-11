---
description: Turn discovery + UX + design into dev-ready epics/stories with BDD ACs and a DoR check.
argument-hint: [initiative or epic scope]
---

# /handover

Run the **handover** skill.

Initiative or scope: $ARGUMENTS
(If empty, resolve the active initiative from its Lifecycle Index or ask.)

Decomposes upstream artifacts into epics and stories with BDD acceptance criteria, links specs and research, adds technical context and out-of-scope, runs a Definition-of-Ready check, and creates the Jira issues — only after explicit confirmation. Adopts the Engineering Lead lens.

## Usage

```
/handover
/handover [initiative or scope]
```

Examples:
- `/handover checkout` — dev handover package for the checkout initiative
- `/handover` — handover for the current lifecycle initiative

## Skill

See `skills/handover/SKILL.md` for the full workflow. Runs standalone or as the Dev Handover stage of `/lifecycle`. Never creates Jira issues without confirmation.

## Required Config

- `DEFAULT_JIRA_PROJECT_KEY` in AGENTS.md
- `DEFAULT_CONFLUENCE_SPACE_ID` in AGENTS.md
- Atlassian OAuth active — or none; falls back to local `workspace/` files (see `connectors/local/CONNECTOR.md`)
