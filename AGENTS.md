# AGENTS.md — Subagent Governance

## Connection Mode

Before any `atlassian-*` alias call, check whether `mcp__atlassian__*` tools are available in the current session:

- **Live mode** — `mcp__atlassian__*` tools are present → resolve aliases to the Atlassian MCP tools as listed below. Normal operation.
- **Local fallback mode** — `mcp__atlassian__*` tools are absent → resolve every `atlassian-*` alias to local file operations against `workspace/` per `standards/local-store.md`. No authentication required. The `email-read` alias has no local fallback; if email source is requested, state "email connector not configured" and stop.

Mode is auto-detected; no manual toggle is needed. Skills call the same aliases regardless of mode. `/sync` bridges local → live: it requires live mode and pushes accumulated `workspace/` content up to Jira and Confluence (see `skills/sync/SKILL.md`).

## Tool Aliases

All skills reference connectors by alias only. Never hardcode MCP tool names in skill files.

| Alias | Resolves to (live) | Fallback (local) |
|-------|-------------|-----------------|
| `atlassian-read-jira` | `mcp__atlassian__getJiraIssue` | Read issue frontmatter from `workspace/jira/{KEY}/issues/{KEY}.md` |
| `atlassian-search-jira` | `mcp__atlassian__searchJiraIssuesUsingJql` | Grep issue frontmatter per JQL translation table in `standards/local-store.md` |
| `atlassian-write-jira` | `mcp__atlassian__createJiraIssue` (new) · `mcp__atlassian__editJiraIssue` (update) · `mcp__atlassian__transitionJiraIssue` (status) | Create or update `workspace/jira/{KEY}/issues/{KEY}.md`; allocate key via `.meta/counters.json` |
| `atlassian-read-confluence` | `mcp__atlassian__getConfluencePage` | Read `workspace/confluence/{SPACE}/{slug}.md` |
| `atlassian-search-confluence` | `mcp__atlassian__searchConfluenceUsingCql` | Grep across `workspace/confluence/{SPACE}/` titles and bodies per CQL translation table in `standards/local-store.md` |
| `atlassian-write-confluence` | `mcp__atlassian__createConfluencePage` (new) · `mcp__atlassian__updateConfluencePage` (update) | Write `workspace/confluence/{SPACE}/{slug}.md` |
| `atlassian-cross-search` | `mcp__atlassian__searchAtlassian` (fetch by ARI: `mcp__atlassian__fetchAtlassian`) | Grep across both `workspace/jira/` issue summaries and `workspace/confluence/` page titles and bodies |
| `email-read` | `${EMAIL_MCP_TOOL}` | — (no local fallback; requires EMAIL_MCP_TOOL in .env) |

Tool names above track the hosted Atlassian Rovo MCP server (connector Option A) and may shift between server releases. With the fully local `mcp-atlassian` server (connector Option B), the same aliases resolve to its tool names instead:

| Alias | Resolves to (`mcp-atlassian`, Option B) |
|-------|------------------------------------------|
| `atlassian-read-jira` | `mcp__atlassian__jira_get_issue` |
| `atlassian-search-jira` | `mcp__atlassian__jira_search` |
| `atlassian-write-jira` | `mcp__atlassian__jira_create_issue` (new) · `mcp__atlassian__jira_update_issue` (update) · `mcp__atlassian__jira_transition_issue` (status) |
| `atlassian-read-confluence` | `mcp__atlassian__confluence_get_page` |
| `atlassian-search-confluence` | `mcp__atlassian__confluence_search` |
| `atlassian-write-confluence` | `mcp__atlassian__confluence_create_page` (new) · `mcp__atlassian__confluence_update_page` (update) |
| `atlassian-cross-search` | no single tool — run `jira_search` + `confluence_search` and merge results |

Resolution rule: match aliases against whichever `mcp__atlassian__*` tools the session actually exposes. If a name from either table is absent, resolve by capability (read issue / JQL search / create page / …) rather than failing.

## Permission Tiers

### Reader agents
Access: `atlassian-read-jira`, `atlassian-search-jira`, `atlassian-read-confluence`, `atlassian-search-confluence`, `atlassian-cross-search`
No write access. No email access.

Agents in this tier: `jira-reader`, `confluence-reader`, `backlog-reader`, `capacity-analyzer`, `insights-synthesizer`

Exception: `insights-synthesizer` may write a single Discovery Brief page via `atlassian-write-confluence` — only when the user explicitly confirms saving a `/discover` report as a brief, never as part of the report itself.

### Writer agents
All reader access plus: `atlassian-write-jira`
Cannot read email. `sprint-writer` may additionally write the sprint planning page via `atlassian-write-confluence` on confirmation; `kanban-writer` cannot write to Confluence at all.

Agents in this tier: `sprint-writer`, `kanban-writer`

### Doc-updater agents
All reader access plus: `atlassian-write-confluence`
Cannot write to Jira. Cannot read email.

Agents in this tier: `doc-updater`

### Ingest agents
Reader access plus: `email-read`
Cannot write anything directly — outputs structured action-item lists for human review before any write.

Agents in this tier: `ingest-parser`

### Retro-facilitator agents
All reader access plus: `atlassian-write-confluence` (retro page only).
May create Jira action-item issues via `atlassian-write-jira` only after explicit user confirmation — never automatically.
Cannot write to any Confluence page other than the sprint retrospective page. Cannot read email.

Agents in this tier: `retro-facilitator`

### Sync agents
All reader access plus: `atlassian-write-jira` AND `atlassian-write-confluence` (the only operation requiring both write aliases simultaneously).
Runs in **live mode only** — refuses with a clear message when `mcp__atlassian__*` tools are absent.
Always presents a dry-run summary and requires explicit user confirmation before any write.
Never pulls remote changes down; `workspace/` is the source of truth.

Agents in this tier: `sync-runner`

### Handover agents
All reader access plus: `atlassian-write-jira` AND `atlassian-write-confluence`.
Unlike `sync-runner`, these create new content (a handover page + decomposed Jira issues) rather than pushing existing `workspace/` content, and they work in **both** live and local fallback modes.
Always present the proposed issues and page for review and require explicit user confirmation before any write — never create Jira issues automatically.

Agents in this tier: `handover-runner`

### Orchestrator agents
Coordinate the stage skills (`ingest`, `discover`, `ux`, `design`, `handover`) and the per-initiative Lifecycle Index. The orchestrator owns **no writes of its own** beyond the Lifecycle Index page (via `atlassian-write-confluence`); all stage artifacts are written by the stage skills under their own tiers, and only at a confirmed stage gate. Never auto-advances past a gate.

Agents in this tier: `lifecycle-orchestrator` (see `skills/lifecycle/SKILL.md` and `standards/lifecycle.md`)

## Defaults

Project defaults live in a `.env` file in the working directory (the project where the plugin runs) — **not** in this file — so they stay per-project and survive plugin updates. Users copy `.env.example` (shipped with the plugin) to their project root as `.env` and fill it in.

Before using any default, read `.env` from the working directory. If the file or a given key is missing, prompt the user once; in local fallback mode, default `DEFAULT_JIRA_PROJECT_KEY` / `DEFAULT_CONFLUENCE_SPACE_ID` to `PROD` / `TEAM` (which also name the `workspace/jira/` and `workspace/confluence/` subdirectories).

| Key | Purpose |
|-----|---------|
| `DEFAULT_JIRA_PROJECT_KEY` | Default Jira project; also names the `workspace/jira/` subfolder in local mode |
| `DEFAULT_CONFLUENCE_SPACE_ID` | Default Confluence space; also names the `workspace/confluence/` subfolder in local mode |
| `DEFAULT_JIRA_BOARD_ID` | Numeric board ID for kanban / burndown (live mode only) |
| `EMAIL_MCP_TOOL` | MCP tool name backing the `email-read` alias (no local fallback) |

## Persona Awareness

Skills adapt tone, detail level, and output structure based on the user's role. **`standards/personas.md` is the single canonical source** for the seven personas and their per-command adaptations — skills read it at step 0; do not duplicate its content elsewhere.

The active persona persists at `workspace/.meta/persona` (one line, the full persona name, e.g. `UX Writer`). `/as` writes it; every skill reads it at step 0. If the file is absent, default to Product Manager: insight-first, strategic framing, stakeholder-ready outputs.

### Lifecycle stage ownership

The end-to-end flow (`/lifecycle`) assigns each stage to its owning persona, which becomes the **default lens** for that stage even when no persona is set via `/as`:

| Stage | Owning persona | Stage skill |
|-------|----------------|-------------|
| Ingest | Business Analyst | `ingest` |
| Discovery | Product Manager | `discover` |
| UX | UX Researcher | `ux` |
| Design | Designer | `design` |
| Dev Handover | Engineering Lead | `handover` |

A stage never abandons its discipline standard regardless of the active persona (e.g. `/design` always produces all states + accessibility per `standards/design.md`). See `standards/lifecycle.md`.

## Anti-hallucination Rules

- NEVER invent Jira issue keys, ticket titles, or status values — always search first
- NEVER fabricate Confluence page IDs or space keys — always resolve via search
- Missing data must be stated explicitly ("not found") rather than approximated
- All JQL queries must use valid field names — verify against `atlassian-read-jira` schema before use
- **Local fallback mode**: "search first" means Grep the `workspace/` directory using the JQL/CQL translation table in `standards/local-store.md`; issue keys are allocated via `.meta/counters.json` and never invented; missing data is still stated as "not found"
