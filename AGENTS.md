# AGENTS.md — Subagent Governance

## Tool Aliases

All skills reference connectors by alias only. Never hardcode MCP tool names in skill files.

| Alias | Resolves to | Notes |
|-------|-------------|-------|
| `atlassian-read-jira` | `mcp__atlassian__read_jira` | |
| `atlassian-search-jira` | `mcp__atlassian__search_jira` | JQL queries |
| `atlassian-write-jira` | `mcp__atlassian__write_jira` | Writer tier only |
| `atlassian-read-confluence` | `mcp__atlassian__read_confluence` | |
| `atlassian-search-confluence` | `mcp__atlassian__search_confluence` | CQL queries |
| `atlassian-write-confluence` | `mcp__atlassian__write_confluence` | doc-updater only |
| `atlassian-cross-search` | `mcp__atlassian__search_atlassian` | Natural language cross-product search |
| `email-read` | `${EMAIL_MCP_TOOL}` | input-ingestion only; set EMAIL_MCP_TOOL in .env |

## Permission Tiers

### Reader agents
Access: `atlassian-read-jira`, `atlassian-search-jira`, `atlassian-read-confluence`, `atlassian-search-confluence`, `atlassian-cross-search`
No write access. No email access.

Agents in this tier: `jira-reader`, `confluence-reader`, `backlog-reader`, `capacity-analyzer`, `insights-synthesizer`

### Writer agents
All reader access plus: `atlassian-write-jira`
Cannot write to Confluence. Cannot read email.

Agents in this tier: `sprint-writer` (Jira only), `kanban-writer`

### Doc-updater agents
All reader access plus: `atlassian-write-confluence`
Cannot write to Jira. Cannot read email.

Agents in this tier: `doc-updater`

### Ingest agents
Reader access plus: `email-read`
Cannot write anything directly — outputs structured action-item lists for human review before any write.

Agents in this tier: `ingest-parser`

## Defaults

Fill in after Atlassian OAuth setup (see `connectors/atlassian/CONNECTOR.md`):

```
DEFAULT_JIRA_PROJECT_KEY=       # e.g. MYPROJ
DEFAULT_CONFLUENCE_SPACE_ID=    # e.g. ~accountid or space key
DEFAULT_JIRA_BOARD_ID=          # numeric board ID for kanban/burndown
```

## Anti-hallucination Rules

- NEVER invent Jira issue keys, ticket titles, or status values — always search first
- NEVER fabricate Confluence page IDs or space keys — always resolve via search
- Missing data must be stated explicitly ("not found") rather than approximated
- All JQL queries must use valid field names — verify against `atlassian-read-jira` schema before use
