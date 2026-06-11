# Local Connector

Provides a file-based fallback for Jira and Confluence when the Atlassian MCP server is not connected. No authentication required. No external services needed.

## When It Activates

**Auto-detect** — no configuration required. Before any `atlassian-*` alias call, the agent checks whether `mcp__atlassian__*` tools are available (see `AGENTS.md` Connection Mode). If they are not:

- Live mode is skipped
- Every alias resolves to local file operations against `workspace/` instead
- All reads use the built-in Read and Grep tools; all writes use Write and Edit

This is transparent to skills — they call the same aliases regardless of mode.

## No Auth Needed

The local connector requires no OAuth, no API tokens, and no `.mcp.json`. It operates entirely on the local filesystem.

## Layout Summary

```
workspace/                        # git-ignored; created on first write
  .meta/
    counters.json                 # issue key counters per project key
  jira/
    {PROJECT_KEY}/
      issues/{PROJECT_KEY}-{N}.md
      sprints/sprint-{N}.md
  confluence/
    {SPACE_KEY}/{page-slug}.md
```

Full file format specifications — frontmatter schemas, slug derivation, key allocation, and JQL/CQL translation table — are in `standards/local-store.md`.

## Defaults in Local Mode

`DEFAULT_JIRA_PROJECT_KEY` and `DEFAULT_CONFLUENCE_SPACE_ID` (from `AGENTS.md`) name the `workspace/jira/` and `workspace/confluence/` folders respectively. If unset, the agent prompts once and defaults to `PROD` / `TEAM`.

## Validation

To confirm the fallback works:

1. Ensure no `.mcp.json` is present (or the Atlassian server is not running)
2. Run `/ingest` with a local file — e.g. `/ingest ~/notes/meeting.md`
3. Confirm the agent creates:
   - `workspace/jira/{KEY}/issues/{KEY}-1.md` with valid frontmatter and template body
   - `workspace/confluence/{SPACE}/{slug}.md` with valid frontmatter
   - `workspace/.meta/counters.json` updated with the new key counter
4. Run `/groom` — confirm it Greps the issue files, reports quality flags, and edits frontmatter/body on confirmation
5. Run `/sprint-plan` — confirm it creates `workspace/jira/{KEY}/sprints/sprint-1.md` and stamps `sprint:` on committed issues
6. Run `/status` — confirm burndown/velocity computed from issue `story_points` + sprint `state`, with "insufficient data" when fewer than 2 closed sprints exist

## Re-enabling Live Mode

Add `.mcp.json` (copy from `.mcp.json.example`) and restart the Claude Code session. The auto-detect will find `mcp__atlassian__*` tools and switch back to live mode. `workspace/` files are unaffected and remain as a local record.

Once live mode is active, run `/sync` to push the accumulated `workspace/` content up to Jira and Confluence. The sync is idempotent (safe to re-run), always dry-runs first, and requires explicit confirmation before any write. Real Jira keys and Confluence page ids are written back into the local file frontmatter so subsequent re-runs skip already-synced items. See `skills/sync/SKILL.md` for full details.

See `connectors/atlassian/CONNECTOR.md` for Atlassian setup details.
