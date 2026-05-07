# status-report

Generates project health summaries with burndown data, velocity trends, and risk signals from Jira.

Derived from: project-manager (awesome-agnostic-skills biz)

## Trigger Conditions

Invoked by `/status`. Also triggered when user asks for project health, sprint progress, velocity, or burndown.

## Inputs

- Jira project key (from AGENTS.md or user-provided)
- Board ID (from AGENTS.md DEFAULT_JIRA_BOARD_ID or user-provided)
- Reporting period: current sprint (default), last N sprints, or date range

## Workflow

1. **Active sprint** — Use `atlassian-search-jira`:
   `project = {KEY} AND sprint in openSprints()`
   Fetch all items. Group by status: To Do / In Progress / Done.

2. **Burndown proxy** — Calculate:
   - Total story points in sprint
   - Points completed (status = Done)
   - Points remaining (status != Done)
   - Days elapsed vs sprint duration (ask user for sprint start date if not in Jira)
   - Expected completion rate vs actual

3. **Velocity history** — Use `atlassian-search-jira` for last 3 closed sprints. Compute:
   - Average velocity
   - Trend (improving / stable / declining)
   - Highest and lowest sprint

4. **Risk signals** — Flag:
   - Items in progress >3 days with no update
   - Blocked items (label = "blocked" or status = "Blocked")
   - Items added after sprint start (scope creep)
   - High-priority items still in "To Do" past sprint midpoint

5. **Assemble report** — Produce structured output (see Output Requirements)

## Output Requirements

```markdown
## Project Status: [Project Key] — [Date]

### Active Sprint
**Sprint:** [Name] | **Day** [N] of [N]

| Status | Count | Points |
|--------|-------|--------|
| Done | N | N |
| In Progress | N | N |
| To Do | N | N |

**Burndown:** [N]% complete — [on track | behind | ahead]

### Velocity
| Sprint | Points Completed |
|--------|-----------------|
| ... | ... |
**Average:** [N] | **Trend:** improving / stable / declining

### Risk Signals
- [Flag]: [Issue key + title]

### Summary
[2-3 sentences: current health, main risk, recommended action]
```

## Verification Checklist

- All counts and point totals from live JQL — never estimate
- Burndown must state clearly if sprint start date is assumed vs confirmed
- Risk signals must reference actual issue keys
- Velocity trend requires ≥2 data points — if fewer, state "insufficient data"
