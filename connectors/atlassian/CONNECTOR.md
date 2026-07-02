# Atlassian Connector

Provides access to Jira and Confluence via MCP — either the official Atlassian Rovo MCP server (hosted or proxied) or a fully local community server.

## MCP Server

Two connection options. Both register under the server name `atlassian`, so mode detection (`mcp__atlassian__*` present) and the alias table in `AGENTS.md` work identically. Pick one; put it in `.mcp.json`.

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

### Option B — Fully local server, API-token auth (`mcp-atlassian`)

Option A talks to Atlassian's hosted Rovo endpoint. For a server that runs **entirely on your machine** — authenticating with the same API tokens the Atlassian CLI (`acli`) uses, no browser OAuth, and working against Server/Data Center as well as Cloud — use the community [`mcp-atlassian`](https://github.com/sooperset/mcp-atlassian) server:

```json
{
  "mcpServers": {
    "atlassian": {
      "command": "uvx",
      "args": ["mcp-atlassian"],
      "env": {
        "JIRA_URL": "https://your-org.atlassian.net",
        "JIRA_USERNAME": "you@example.com",
        "JIRA_API_TOKEN": "<api token>",
        "CONFLUENCE_URL": "https://your-org.atlassian.net/wiki",
        "CONFLUENCE_USERNAME": "you@example.com",
        "CONFLUENCE_API_TOKEN": "<api token>"
      }
    }
  }
}
```

(Also available as a Docker image, `ghcr.io/sooperset/mcp-atlassian`, if you prefer `command: "docker"`.) Create tokens at https://id.atlassian.com/manage-profile/security/api-tokens.

Notes for this option:
- Keep the server name `atlassian` — the plugin detects live mode by the `mcp__atlassian__*` prefix, not by specific tool names.
- Its tool names differ from Rovo's (`jira_search`, `jira_get_issue`, `jira_create_issue`, `jira_update_issue`, `jira_transition_issue`, `confluence_search`, `confluence_get_page`, `confluence_create_page`, `confluence_update_page`). The alias table in `AGENTS.md` resolves by capability, so these map cleanly.
- There is no cross-product search tool — the `atlassian-cross-search` alias resolves to running the Jira and Confluence searches separately and merging results.
- This is a community project, not Atlassian-maintained: your API tokens go in `.mcp.json` env config (git-ignored here), and you should review/pin the version you run.

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

The tools the server actually exposes (names may shift between server releases — resolve by capability if a name is absent):

| Tool | Description |
|------|-------------|
| `mcp__atlassian__getJiraIssue` | Retrieve an issue by ID or key |
| `mcp__atlassian__searchJiraIssuesUsingJql` | JQL-based search |
| `mcp__atlassian__createJiraIssue` | Create a new issue |
| `mcp__atlassian__editJiraIssue` | Update fields on an existing issue |
| `mcp__atlassian__transitionJiraIssue` | Move an issue between statuses |
| `mcp__atlassian__getConfluencePage` | Get a page or live doc by ID |
| `mcp__atlassian__searchConfluenceUsingCql` | CQL-based search |
| `mcp__atlassian__createConfluencePage` | Create a new page or live doc |
| `mcp__atlassian__updateConfluencePage` | Update an existing page or live doc |
| `mcp__atlassian__searchAtlassian` | Natural language cross-product search |
| `mcp__atlassian__fetchAtlassian` | Retrieve content by Atlassian Resource Identifier |

Skills never call these directly — they use the `atlassian-*` aliases mapped in `AGENTS.md`.

## Project Defaults

Project defaults (`DEFAULT_JIRA_PROJECT_KEY`, `DEFAULT_CONFLUENCE_SPACE_ID`, `DEFAULT_JIRA_BOARD_ID`) live in a `.env` file in your project root, not here. Copy `.env.example` to `.env` and fill it in after OAuth setup. See the Defaults table in `AGENTS.md` for what each key controls.

## Validation

After setup, run `/mcp` in Claude Code to confirm the `atlassian` server is connected.
Then run `/discover` — a successful response confirms read access to both Jira and Confluence.

## Endpoint Note

The hosted server (Option A) now uses `/v1/mcp` (streamable HTTP) — the config above is current. The legacy `/v1/sse` endpoint is being phased out; if you have an older `.mcp.json` pointing at `/v1/sse`, update it to `/v1/mcp`.
