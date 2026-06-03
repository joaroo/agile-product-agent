# agile-product-agent

A Claude Code plugin for agile product workflows — product discovery, documentation, sprint planning, backlog grooming, kanban management, status reporting, retrospectives, and input ingestion from local docs, meeting notes, and email. Works with Jira and Confluence via the official Atlassian MCP server, or fully offline against local `workspace/` files when no connection is configured.

## Setup

1. (Optional) Copy `.mcp.json.example` to `.mcp.json` to enable live Atlassian access
2. Run `claude` in this directory — it will prompt for Atlassian OAuth on first tool use if `.mcp.json` is present
3. Fill in your Jira project keys and Confluence space IDs in `connectors/atlassian/CONNECTOR.md`
4. (Optional) Set `EMAIL_MCP_TOOL` in `.env` to enable `/ingest` from email

### Local fallback

No `.mcp.json`? No problem. When `mcp__atlassian__*` tools are unavailable, the agent automatically reads and writes local markdown files under `workspace/jira/` and `workspace/confluence/`, mirroring the Jira/Confluence object model. All commands work end-to-end with zero external setup.

- `workspace/` is git-ignored and treated as private product data
- See `connectors/local/CONNECTOR.md` for activation, layout, and validation steps
- See `standards/local-store.md` for file formats and JQL/CQL translation
- Worked offline first? Run `/sync` once Atlassian is connected to push `workspace/` up.

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
| `/retro` | Facilitate a sprint retrospective and write the retro page |
| `/sync` | Push local `workspace/` up to Jira and Confluence (requires live Atlassian connection) |

## Personas

Start a session with `/as [role]` to adapt all outputs to your role. Each persona changes how commands structure and frame their responses.

| Command | Role | Output style |
|---------|------|-------------|
| `/as pm` | Product Manager | Insight-first, strategic framing, stakeholder-ready |
| `/as ux` | UX Researcher | Research artifact templates, user-centric language, evidence and open questions flagged |
| `/as design` | Designer | Design artifact templates, all states + accessibility by default, outcome-based ACs |
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
| `standards/requirements.md` | Requirements specs, traceability matrix, process flows (BA artifacts) |
| `standards/personas.md` | Agent user personas and per-persona output adaptations |
| `standards/local-store.md` | Local fallback file formats, key allocation, JQL/CQL translation |

## Connectors

- `connectors/atlassian/` — Atlassian MCP (Jira + Confluence)
- `connectors/local/` — Local file fallback (auto-active when Atlassian MCP is absent)
- `connectors/email/` — Provider-agnostic email MCP (set `EMAIL_MCP_TOOL`)
