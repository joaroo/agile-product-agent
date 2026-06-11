---
name: sync
description: Push the local workspace/ fallback up to Jira and Confluence once the Atlassian MCP is connected. Use when the user runs /sync or asks to push, upload, or sync local issues and pages to Jira/Confluence.
argument-hint: [jira | confluence | project key]
---

# sync

Reconciles the local `workspace/` fallback with live Jira and Confluence: classifies each item as create or update, computes topological order, dry-runs the plan for confirmation, pushes on approval, writes real IDs back into local frontmatter, and emits a reconciliation report.

Style references: `standards/local-store.md`, `standards/jira.md`, `standards/confluence.md`

## Trigger Conditions

Invoked by `/sync`. Also triggered when the user says "push local work up", "upload workspace to Jira", "sync my local issues", or "send workspace to Atlassian".

## Inputs

- Optional scope: `jira` | `confluence` | a specific project key or space key (default: everything in `workspace/`)
- Jira project key and Confluence space (from `.env` or user-provided)

## Workflow

0. **Resolve connection mode + persona** — Resolve all `atlassian-*` aliases per `AGENTS.md` Connection Mode. Check active persona via `/as`; default persona: Product Manager.

   **MODE GATE** — `/sync` requires live mode (`mcp__atlassian__*` tools must be present) and a non-empty `workspace/`. In local fallback mode, refuse immediately: "Atlassian not connected — nothing to sync to." and stop. Do not proceed.

1. **Scan & classify** — Enumerate all items in `workspace/` within the requested scope:
   - Issues: `workspace/jira/{KEY}/issues/*.md`
   - Sprints: `workspace/jira/{KEY}/sprints/*.md`
   - Pages: `workspace/confluence/{SPACE}/*.md`

   For each item:
   - **create** — no `jira_key` / `confluence_id` / `sprint_id` in frontmatter (never been synced)
   - **update** — has a recorded real ID, AND `updated:` > `synced_at:` (locally modified since last sync)
   - **skip** — has a recorded real ID, AND `updated:` ≤ `synced_at:` (no local changes)

2. **Resolve targets** — Map local project key (e.g. `PROD`) to the real Jira project key and local space key (e.g. `TEAM`) to the real Confluence space key using `.env` defaults. If the mapping is ambiguous or unset, ask the user once before proceeding.

3. **Topological order** — Sort items to ensure parent references resolve before children:
   - **Jira**: Epics → Stories / Tasks / Bugs / Spikes → Sub-tasks; sprints are created before issues are assigned to them
   - **Confluence**: walk the `parent:` tree; pages with no `parent:` (top-level) are pushed first, then children in breadth-first order

4. **Dry-run summary** — Present the full plan before any write:

   ```markdown
   ## Sync Dry Run

   **Jira project:** MYPROJ  |  **Confluence space:** TEAM
   **Issues:** N create, M update, K skip
   **Sprints:** N create, M update
   **Pages:** N create, M update

   ### Sample key remapping
   | Local key    | Remote key (new) |
   |-------------|-----------------|
   | PROD-1       | MYPROJ-101       |
   | PROD-2       | MYPROJ-102       |
   | team/decision-auth-strategy | (page id TBD) |

   Confirm to push, or type "cancel" to abort.
   ```

   **Do NOT write anything until the user confirms.**

5. **Push (on confirmation)** — For each item in topological order:

   a. **Remap link fields** before the write call:
      - Issue `epic:` — replace local key with real Jira key via `workspace/.meta/sync-map.json` (jira map)
      - Issue `sprint:` — replace local sprint number with real sprint id (sprints map)
      - Page `parent:` — replace local slug with real Confluence page id (confluence map)

   b. **Call the write alias**:
      - Issues + sprints → `atlassian-write-jira`
      - Pages → `atlassian-write-confluence`

   c. **Capture and write back** — on success, record the returned real id into the local file frontmatter:
      - Issues: add/update `jira_key: MYPROJ-101` and `synced_at: <ISO timestamp>`
      - Sprints: add/update `sprint_id: 45` and `synced_at: <ISO timestamp>`
      - Pages: add/update `confluence_id: 123456` and `synced_at: <ISO timestamp>`

   d. **Update the ledger** — add the local→remote mapping to `workspace/.meta/sync-map.json` under the appropriate key (`jira`, `confluence`, `sprints`) and update `last_sync`.

   e. **Best-effort operations** — attempt to set status via Jira transitions; attempt sprint creation/assignment if the API supports it. Record any unsupported operations for the reconciliation report rather than aborting.

6. **Reconciliation report** — Emit after the push completes:

   ```markdown
   ## Sync Complete

   **Jira project:** MYPROJ  |  **Confluence space:** TEAM
   **Issues created:** N  |  **Issues updated:** M
   **Sprints created:** N
   **Pages created:** N  |  **Pages updated:** M

   ### Key map
   | Local key | Remote key/id |
   |-----------|--------------|
   | PROD-1    | MYPROJ-101   |
   | PROD-2    | MYPROJ-102   |
   | team/decision-auth-strategy | 123456 |

   ### Needs manual follow-up
   | Item | Reason |
   |------|--------|
   | PROD-3 | Status "In Review" has no matching Jira transition |
   | Sprint 2 | Sprint API not available on this Jira plan |
   ```

   If no items required manual follow-up, state "All items synced successfully."

## Output Requirements

Two markdown blocks:
1. **Dry-run preview** — counts, target project/space, sample key-remap table, explicit confirmation prompt
2. **Reconciliation report** — created/updated counts, full local↔remote key map, and a "needs manual follow-up" table (empty = success)

## Verification Checklist

- Never fabricate real Jira keys or Confluence IDs — only use values returned by the write aliases
- Dry-run summary presented and explicit user confirmation received before ANY write
- Idempotent: items with `updated:` ≤ `synced_at:` are skipped; re-running never creates duplicates
- Real ids written back to local file frontmatter and ledger after each successful push
- Unsupported operations (status transitions, sprint API) reported, not treated as failures
- Refused cleanly with "Atlassian not connected — nothing to sync to." when in local fallback mode
- Local `key:` field (e.g. `PROD-1`) is never modified; only `jira_key:` / `confluence_id:` / `sprint_id:` and `synced_at:` are added
- Ledger `workspace/.meta/sync-map.json` rebuilt from frontmatter if missing (frontmatter is authoritative)

## Usage

```
/sync
/sync jira
/sync confluence
/sync MYPROJ
```

Examples:
- `/sync` — push everything (issues, sprints, pages)
- `/sync jira` — push Jira issues and sprints only
- `/sync confluence` — push Confluence pages only
- `/sync MYPROJ` — push issues and sprints for a specific project key

## Required Config

- `DEFAULT_JIRA_PROJECT_KEY` and `DEFAULT_CONFLUENCE_SPACE_ID` in `.env` (used to map local keys to real project/space keys)
- **A live Atlassian connection is required** — this command does nothing useful in local-only mode. Add `.mcp.json` (copy from `.mcp.json.example`) and restart the session to enable live mode. See `connectors/atlassian/CONNECTOR.md` and `connectors/local/CONNECTOR.md`.
