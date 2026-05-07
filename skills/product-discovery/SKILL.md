# product-discovery

Synthesizes Jira and Confluence data to surface product opportunities, gaps, and insights.

Derived from: product-manager + ux-researcher + knowledge-synthesizer (awesome-agnostic-skills biz)

Style reference: `standards/ux.md`

## Trigger Conditions

Invoked by `/discover`. Also triggered when the user asks about product direction, feature gaps, user pain points, or competitive positioning relative to existing Jira/Confluence content.

## Inputs

- Jira project key (from AGENTS.md DEFAULT_JIRA_PROJECT_KEY or user-provided)
- Confluence space ID (from AGENTS.md DEFAULT_CONFLUENCE_SPACE_ID or user-provided)
- Optional: focus area or theme (e.g. "onboarding", "billing", "performance")

## Workflow

1. **Cross-product search** — Use `atlassian-cross-search` with the focus area (or broad query if none) to get a landscape view
2. **Jira signal mining** — Use `atlassian-search-jira` with JQL:
   - High-priority open issues: `project = {KEY} AND priority in (High, Highest) AND status != Done ORDER BY created DESC`
   - Long-standing bugs: `project = {KEY} AND issuetype = Bug AND created <= -30d AND status != Done`
   - User-reported issues: `project = {KEY} AND labels = "user-feedback" ORDER BY votes DESC`
   - If no results on any query, state "not found" — do not invent
3. **Confluence knowledge scan** — Use `atlassian-search-confluence` to find: product specs, user research (research reports, personas, journey maps per `standards/ux.md`), decision docs, retrospective notes
4. **Opportunity synthesis** — Identify:
   - Recurring themes across Jira issues (cluster by label/component)
   - Gaps where Jira signals exist but no Confluence spec or decision doc
   - Stale open issues that may indicate blocked or deprioritized work
   - User-facing pain points vs internal tech debt ratio
5. **Output** — Produce a structured discovery report (see Output Requirements)

## Output Requirements

```markdown
## Product Discovery Report

### Focus Area
[stated focus or "General"]

### Top Opportunities
1. [Theme] — [evidence: N issues, source links]
2. ...

### Gaps
- [Area] — signals in Jira but no spec/decision in Confluence

### Stale Work
- [Issue key + title] — open N days, last updated [date]

### Recommended Next Steps
- [ ] [Action]
```

## Verification Checklist

- All issue counts from live JQL queries — NEVER estimate
- All Confluence references include page title and space
- No invented issue keys or page IDs
- If a query returns 0 results, state that explicitly
- Focus area respected throughout — do not drift into unrelated topics
