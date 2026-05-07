# Atlassian Connector

Provides access to Jira and Confluence via the official Atlassian Rovo MCP server.

## MCP Server

```json
{
  "mcpServers": {
    "atlassian": {
      "command": "npx",
      "args": ["-y", "@atlassian/mcp-server"]
    }
  }
}
```

Copy `.mcp.json.example` → `.mcp.json` to activate.

## Authentication

**Primary: OAuth 2.1 (recommended)**
No env vars needed. On first Atlassian tool call, Claude will prompt for browser-based OAuth authorization. Permissions follow your existing Atlassian access controls.

**Secondary: API token (headless/CI)**
Required for Jira Service Management and Bitbucket access:
```
ATLASSIAN_API_TOKEN=   # from https://id.atlassian.com/manage-profile/security/api-tokens
ATLASSIAN_EMAIL=       # your Atlassian account email
ATLASSIAN_BASE_URL=    # e.g. https://your-org.atlassian.net
```

## Exposed Tools

| Tool | Description |
|------|-------------|
| `mcp__atlassian__read_jira` | Retrieve issues, projects, metadata |
| `mcp__atlassian__search_jira` | JQL-based search |
| `mcp__atlassian__write_jira` | Create issues, add comments, transitions |
| `mcp__atlassian__read_confluence` | Access pages, spaces, comments |
| `mcp__atlassian__search_confluence` | CQL-based search |
| `mcp__atlassian__write_confluence` | Create and update pages/comments |
| `mcp__atlassian__search_atlassian` | Natural language cross-product search |

## Project Defaults

Fill in after OAuth setup — these are referenced in AGENTS.md and used as fallbacks in skills:

```
DEFAULT_JIRA_PROJECT_KEY=       # e.g. MYPROJ
DEFAULT_CONFLUENCE_SPACE_ID=    # space key, e.g. TEAM
DEFAULT_JIRA_BOARD_ID=          # numeric board ID (found in board URL)
```

## Validation

After setup, run `/mcp` in Claude Code to confirm the `atlassian` server is connected.
Then run `/discover` — a successful response confirms read access to both Jira and Confluence.

## Endpoint Note

The `/v1/sse` endpoint sunsets June 30, 2026. The `npx @atlassian/mcp-server` package handles this automatically.
