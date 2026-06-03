---
name: sprint-planning
description: Plan the next sprint from the Jira backlog: compute velocity, match capacity, and propose scope plus a user-value sprint goal. Use when the user runs /sprint-plan or mentions sprint kickoff or planning the next iteration.
---

# sprint-planning

Reads the Jira backlog, analyzes capacity and priorities, proposes a sprint scope, and writes the sprint goal to Jira/Confluence.

Derived from: scrum-master + project-manager (awesome-agnostic-skills biz)

Style references: `standards/jira.md`, `standards/confluence.md`, `standards/local-store.md`

## Trigger Conditions

Invoked by `/sprint-plan`. Also triggered when user mentions upcoming sprint, sprint kickoff, or asks to plan next iteration.

## Inputs

- Jira project key (from AGENTS.md or user-provided)
- Sprint duration (default: 2 weeks)
- Team capacity in story points or days (user-provided or estimated from last sprint velocity)
- Optional: themes or focus areas to prioritize

## Workflow

0. **Resolve connection mode** — Resolve all `atlassian-*` aliases per `AGENTS.md` Connection Mode; in local fallback mode, translate queries/writes per `standards/local-store.md`. **Check active persona** — if set via `/as`, apply adaptations from `skills/persona-switch/SKILL.md`. Default: Product Manager (sprint goal and user value emphasis).

1. **Velocity baseline** — Use `atlassian-search-jira` to fetch last 3 completed sprints:
   `project = {KEY} AND sprint in closedSprints() ORDER BY sprint DESC`
   Calculate average velocity. If no closed sprints exist, ask user for estimate.

2. **Backlog read** — Use `atlassian-search-jira` to fetch unassigned backlog items:
   `project = {KEY} AND status = Backlog ORDER BY priority DESC, rank ASC`
   Fetch top 30 items. If focus area provided, add label/component filter.

3. **Capacity match** — Select items totaling 70–80% of velocity target (leave buffer for unplanned work). Prioritize:
   - High/Highest priority first
   - Items with defined acceptance criteria over vague ones
   - Smaller items that unblock others

4. **Sprint goal draft** — Synthesize the selected items into a 1–2 sentence sprint goal that describes the user value delivered, not just the task list

5. **Confirm scope** — Present proposed sprint items + goal to user for approval before any write

6. **Write** (on approval):
   - Use `atlassian-write-jira` to move approved items to the new sprint
   - Optionally use `atlassian-write-confluence` to create a sprint planning page in the team space

## Output Requirements

```markdown
## Sprint Plan: Sprint [N]

**Goal:** [1-2 sentence user-value statement]

**Capacity:** [N] points | **Velocity target:** [N] points

### Committed Items
| Key | Title | Points | Priority |
|-----|-------|--------|---------|
| ... | ...   | ...    | ...     |

**Total:** [N] points ([N]% of capacity)

### Excluded (next sprint candidates)
- [Key] [Title] — [reason: too large / lower priority / blocked]
```

## Verification Checklist

- Velocity calculation from real closed sprint data — NEVER assume
- Story point totals must not exceed 85% of stated capacity
- Sprint goal must reference user value, not internal tasks
- Never move items to sprint without explicit user confirmation
- If acceptance criteria missing on >50% of items, flag before proceeding — ACs must follow `standards/bdd.md` to count as present
- Sprint planning Confluence page follows meeting notes template in `standards/confluence.md`
