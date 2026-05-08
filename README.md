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
| `/as [role]` | Set active persona to adapt output tone and structure |
| `/discover` | Product discovery from Jira + Confluence |
| `/update-docs` | Create or update Confluence pages |
| `/sprint-plan` | Plan next sprint from backlog |
| `/groom` | Groom and prioritize backlog |
| `/status` | Project health + burndown report |
| `/kanban` | Triage and move Kanban cards |
| `/ingest` | Parse docs, meeting notes, or email into Jira issues / Confluence pages |

## Personas

Start a session with `/as [role]` to adapt all outputs to your role. Each persona changes how commands structure and frame their responses.

| Command | Role | Output style |
|---------|------|-------------|
| `/as pm` | Product Manager | Insight-first, strategic framing, stakeholder-ready |
| `/as ux` | UX Designer | UX/design templates, user-centric language, open questions flagged |
| `/as dev` | Engineering Lead | BDD ACs, edge cases, precise scope, honest status |
| `/as scrum` | Scrum Master | Metrics-first, ceremony-ready, WIP and flow signals |
| `/as ba` | Business Analyst | Requirements traceability, structured templates, ambiguity flagged |
| `/as reset` | — | Return to default (Product Manager) |

See `standards/personas.md` for full detail on how each persona affects each command.

## Skills

See `skills/` for full workflow definitions. Each skill embeds the relevant competency framework from [awesome-agnostic-skills](https://github.com/joaroo/awesome-agnostic-skills) biz category.

## Standards

| File | Covers |
|------|--------|
| `standards/confluence.md` | Page structure, naming, templates, style |
| `standards/jira.md` | Issue types, titles, fields, labels, JQL, DoR/DoD |
| `standards/bdd.md` | Given/When/Then, scenario naming, AC format |
| `standards/ux.md` | Research artifacts, personas, journey maps, usability testing |
| `standards/design.md` | Component specs, handoff, design review, design system governance |
| `standards/personas.md` | Agent user personas and per-persona output adaptations |

## Connectors

- `connectors/atlassian/` — Atlassian MCP (Jira + Confluence)
- `connectors/email/` — Provider-agnostic email MCP (set `EMAIL_MCP_TOOL`)
