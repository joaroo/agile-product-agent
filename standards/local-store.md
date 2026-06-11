# Local Store Standard

Canonical specification for the local-file fallback. When `mcp__atlassian__*` tools are unavailable, all `atlassian-*` aliases resolve against `workspace/` using this spec. See `AGENTS.md` Connection Mode for the detection logic.

---

## Directory Layout

```
workspace/
  .meta/
    counters.json            # { "MYPROJ": 42 } — last issue number per project key
  jira/
    {PROJECT_KEY}/
      issues/
        {PROJECT_KEY}-{N}.md # one file per issue, e.g. MYPROJ-12.md
      sprints/
        sprint-{N}.md        # sprint definition: goal, state, committed keys
  confluence/
    {SPACE_KEY}/
      {page-slug}.md         # one file per page; parent + hierarchy in frontmatter
```

`workspace/` is git-ignored. It is the user's private product data — never commit it.

---

## Issue File Format

Frontmatter mirrors the Jira fields used by JQL. Body follows the description template in `standards/jira.md`.

```markdown
---
key: MYPROJ-12
type: Story            # Epic | Story | Task | Bug | Spike | Sub-task
summary: Allow users to reset password via email
status: Backlog        # Backlog | To Do | In Progress | In Review | Done | Won't Do
priority: Medium       # Highest | High | Medium | Low | Lowest
epic: MYPROJ-3
story_points: 5
labels: [user-feedback]
assignee:
sprint:                # sprint number when committed
created: 2026-06-03
updated: 2026-06-03
---

## Context
...

## Acceptance Criteria
- [ ] ...

## Out of Scope
...

## Notes
...
```

Field names are exact matches to the Jira field names in `standards/jira.md` (`status`, `priority`, `labels`, `story_points`, `epic`). Do not rename them.

---

## Confluence Page Format

Frontmatter captures hierarchy and metadata. Body follows the relevant template in `standards/confluence.md`.

```markdown
---
title: "Decision: Auth Strategy"
space: TEAM
parent:                # parent page slug, empty = top level
owner:
status: Active         # Draft | Active | Deprecated
last_reviewed: 2026-06-03
---

# Decision: Auth Strategy

**Owner:** [name]  **Last reviewed:** [date]  **Status:** Active

[Summary paragraph]

---

[Body sections per template in standards/confluence.md]

---

*Last updated: [date]*
```

Field names are exact matches to the Confluence fields in `standards/confluence.md` (`owner`, `status`, `parent`). The `space` field must match the `{SPACE_KEY}` directory name.

The page slug (filename without `.md`) is derived from the page title: lowercase, spaces replaced with hyphens, special characters dropped. Example: `Decision: Auth Strategy` → `decision-auth-strategy.md`.

---

## Sprint File Format

```markdown
---
sprint: 7
state: open            # open | closed
goal: One-sentence user-value goal
start: 2026-06-01
end: 2026-06-14
committed: [MYPROJ-12, MYPROJ-15]
---
```

Sprint files live at `workspace/jira/{PROJECT_KEY}/sprints/sprint-{N}.md`. The `sprint` field in an issue frontmatter references the sprint number in this file.

---

## Key Allocation

New issue key = `{PROJECT_KEY}-{counter+1}`; increment the counter in `.meta/counters.json`.

```json
{ "MYPROJ": 42, "TEAM": 7 }
```

- If `counters.json` is missing: scan `workspace/jira/{KEY}/issues/` filenames for the maximum `{N}` and use that as the starting counter. If the directory is also missing, start at 0.
- Never reuse a key — only increment, never decrement.
- Never invent keys from thin air; always allocate via this counter.

---

## JQL / CQL → Local Filter Translation

Use Grep and Read against `workspace/` to satisfy queries. The translation table below maps JQL/CQL clauses to file operations.

### JQL clauses

| JQL clause | Local file operation |
|------------|---------------------|
| `project = {KEY}` | Files under `workspace/jira/{KEY}/issues/` |
| `status = X` | Grep frontmatter `status: X` in issue files |
| `status in (X, Y)` | Grep frontmatter `status:` matching any of X, Y |
| `status != X` | Grep frontmatter `status:` not matching X |
| `priority in (High, Highest)` | Grep frontmatter `priority: High` or `priority: Highest` |
| `labels = X` | Grep frontmatter `labels:` list containing X |
| `labels not in (X, Y)` | Grep frontmatter `labels:` not containing X or Y |
| `issuetype = X` | Grep frontmatter `type: X` |
| `assignee = X` | Grep frontmatter `assignee: X` |
| `assignee is EMPTY` | Grep frontmatter `assignee:` with empty value |
| `"Story Points" is not EMPTY` | Grep frontmatter `story_points:` with non-empty value |
| `epic = X` | Grep frontmatter `epic: X` |
| `sprint in openSprints()` | Issues whose `sprint:` value matches a sprint file with `state: open` |
| `sprint in closedSprints()` | Issues whose `sprint:` value matches a sprint file with `state: closed` |
| `created <= -30d` | Compare frontmatter `created:` date against today minus 30 days |
| `created >= -7d` | Compare frontmatter `created:` date against today minus 7 days |
| `updated <= -5d` | Compare frontmatter `updated:` date against today minus 5 days |
| `ORDER BY priority DESC` | Sort results by priority enum: Highest → High → Medium → Low → Lowest |
| `ORDER BY rank ASC` | Sort by key number ascending (MYPROJ-1 before MYPROJ-2) |
| `ORDER BY created DESC` | Sort by `created:` date descending |

### CQL clauses

| CQL clause | Local file operation |
|------------|---------------------|
| `text ~ "query"` | Grep across `workspace/confluence/{SPACE}/` titles and body text |
| `space = {SPACE}` | Files under `workspace/confluence/{SPACE}/` |
| `title = "X"` | Grep frontmatter `title: "X"` or match filename slug |
| `parent = "X"` | Grep frontmatter `parent: X` |
| `status = X` | Grep frontmatter `status: X` |

### Cross-search

Natural language cross-product search → Grep across both `workspace/jira/` (issue summaries) and `workspace/confluence/` (page titles and bodies).

---

## Velocity and Burndown (status)

In local fallback mode, velocity and burndown for the `status` skill are computed as follows:

- **Velocity (per sprint)**: sum of `story_points` of issues with `status: Done` whose `sprint:` field matches a sprint with `state: closed`.
- **Burndown (current sprint)**: sum `story_points` of all issues in the open sprint; subtract points of issues with `status: Done`.
- **Velocity trend**: requires ≥2 closed sprint data points. If fewer exist, state "insufficient data".

---

## Sync-up (local → Atlassian)

Once the Atlassian MCP is connected, run `/sync` to push `workspace/` content up to real Jira and Confluence. The sync is **one-way** (local is source of truth) and **idempotent** (re-runs update, never duplicate). See `skills/sync/SKILL.md` for the full workflow.

### Sync-back frontmatter fields

After a successful push the real remote identity is written back into the local file alongside the stable local key. The local `key:` field is **never modified**.

**Issues** (added to issue frontmatter):
```markdown
jira_key: MYPROJ-101    # real Jira issue key returned by the write alias
synced_at: 2026-06-03T12:00:00Z
```

**Pages** (added to page frontmatter):
```markdown
confluence_id: 123456   # real Confluence page id returned by the write alias
synced_at: 2026-06-03T12:00:00Z
```

**Sprints** (added to sprint frontmatter):
```markdown
sprint_id: 45           # real Jira sprint id returned by the write alias
synced_at: 2026-06-03T12:00:00Z
```

The presence of `jira_key` / `confluence_id` / `sprint_id` is the idempotency signal: items without these fields are **create** candidates; items with them and `updated:` > `synced_at:` are **update** candidates; all others are skipped.

### Ledger — `workspace/.meta/sync-map.json`

A fast local→remote index maintained alongside frontmatter. **Frontmatter is authoritative**; the ledger is rebuilt from frontmatter if missing.

```json
{
  "jira": { "PROD-1": "MYPROJ-101", "PROD-2": "MYPROJ-102" },
  "confluence": { "team/decision-auth-strategy": "123456" },
  "sprints": { "PROD/1": 45, "PROD/2": 46 },
  "last_sync": "2026-06-03T12:00:00Z"
}
```

Keys in `jira` are local issue keys (`PROD-N`); values are real Jira keys. Keys in `confluence` are `{SPACE}/{slug}`; values are real Confluence page ids. Keys in `sprints` are `{PROJECT_KEY}/{sprint_number}`; values are real Jira sprint ids.

### Sync rules

| Rule | Detail |
|------|--------|
| One-way | Local `workspace/` is source of truth; remote edits are not pulled down |
| Idempotent | Create if no real id recorded; update if `updated:` > `synced_at:`; skip otherwise |
| Topological order | Jira: Epics → Stories/Tasks/Bugs/Spikes → Sub-tasks; sprints created before issue assignment; Confluence: parents before children |
| ID remapping | Before each write, replace local keys in link fields with real remote ids via the ledger: issue `epic:`, sprint `committed:` entries, page `parent:` |
| Best-effort | Status transitions and sprint creation attempted if the API supports them; unsupported operations listed in the reconciliation report rather than failing the run |
| Dry-run gate | Always present a dry-run summary (counts + sample key map) and require explicit confirmation before any write |
