# Atlassian Connector

Provides access to Jira and Confluence via the official Atlassian Rovo MCP server.

## MCP Server

Two connection options — both expose the same `mcp__atlassian__*` tools. Pick whichever you prefer; put it in `.mcp.json`.

### Option A — Hosted (remote) Rovo MCP server

No local install. Authenticates via browser OAuth on first tool call.

```json
{
  "mcpServers": {
    "atlassian": {
      "type": "http",
      "url": "https://mcp.atlassian.com/v1/mcp"
    }
  }
}
```

Equivalent CLI: `claude mcp add --transport http atlassian https://mcp.atlassian.com/v1/mcp`

### Option B — Local stdio package

Runs the MCP server locally via `npx`.

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

Copy `.mcp.json.example` → `.mcp.json` to activate (it ships with Option A; swap in the block above for Option B).

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

Project defaults (`DEFAULT_JIRA_PROJECT_KEY`, `DEFAULT_CONFLUENCE_SPACE_ID`, `DEFAULT_JIRA_BOARD_ID`) live in a `.env` file in your project root, not here. Copy `.env.example` to `.env` and fill it in after OAuth setup. See the Defaults table in `AGENTS.md` for what each key controls.

## Validation

After setup, run `/mcp` in Claude Code to confirm the `atlassian` server is connected.
Then run `/discover` — a successful response confirms read access to both Jira and Confluence.

## Endpoint Note

The hosted server now uses `/v1/mcp` (streamable HTTP) — the config above is current. The legacy `/v1/sse` endpoint is being phased out; if you have an older `.mcp.json` pointing at `/v1/sse`, update it to `/v1/mcp`. The local `npx @atlassian/mcp-server` package tracks the endpoint automatically.
