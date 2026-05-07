# agile-product-agent

A Claude Code plugin for agile product workflows. Connects Jira and Confluence via the official Atlassian MCP server to enable product discovery, documentation, sprint planning, backlog grooming, kanban management, status reporting, and input ingestion from local docs, meeting notes, and email.

## Setup

1. Copy `.mcp.json.example` to `.mcp.json`
2. Run `claude` in this directory — it will prompt for Atlassian OAuth on first tool use
3. Fill in your Jira project keys and Confluence space IDs in `connectors/atlassian/CONNECTOR.md`
4. (Optional) Set `EMAIL_MCP_TOOL` in `.env` to enable `/ingest` from email

## Commands

| Command | Description |
|---------|-------------|
| `/discover` | Product discovery from Jira + Confluence |
| `/update-docs` | Create or update Confluence pages |
| `/sprint-plan` | Plan next sprint from backlog |
| `/groom` | Groom and prioritize backlog |
| `/status` | Project health + burndown report |
| `/kanban` | Triage and move Kanban cards |
| `/ingest` | Parse docs, meeting notes, or email into Jira issues / Confluence pages |

## Skills

See `skills/` for full workflow definitions. Each skill embeds the relevant competency framework from [awesome-agnostic-skills](https://github.com/joaroo/awesome-agnostic-skills) biz category.

## Connectors

- `connectors/atlassian/` — Atlassian MCP (Jira + Confluence)
- `connectors/email/` — Provider-agnostic email MCP (set `EMAIL_MCP_TOOL`)
