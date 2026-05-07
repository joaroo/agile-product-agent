# Email Connector

Provider-agnostic email access for the `/ingest` command. Set `EMAIL_MCP_TOOL` in `.env` to point to whichever email MCP server you have configured.

## Configuration

```
# .env
EMAIL_MCP_TOOL=mcp__gmail__search_emails   # Gmail example
# EMAIL_MCP_TOOL=mcp__outlook__search      # Outlook example
```

The `email-read` alias in AGENTS.md resolves to this value at runtime.

## Usage in Skills

The `input-ingestion` skill uses the `email-read` alias only. It:
1. Fetches emails by label/thread/date using whatever tool `EMAIL_MCP_TOOL` points to
2. Extracts action items and decisions
3. Presents structured output for human review before any Jira/Confluence writes

## Supported Providers

Any email MCP server that supports search/read operations. Common options:
- Gmail: `mcp__claude_ai_Gmail__*` (Claude AI built-in MCP)
- Outlook/M365: configure via your MCP registry
- Custom IMAP: configure a local MCP server

## Optional

This connector is only required if using `/ingest` with email sources. Local doc and meeting note ingestion works without it.
