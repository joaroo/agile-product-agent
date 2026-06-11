---
name: kanban
description: Triage incoming work, move cards through kanban stages, check WIP limits, and review flow in Jira. Use when the user runs /kanban or asks to triage, move cards, check WIP, or improve flow.
argument-hint: [triage | move | wip-check | flow-review]
---

# kanban

Triages incoming work, moves cards through kanban stages, and maintains flow hygiene in Jira.

Derived from: scrum-master (kanban mode) + business-analyst (awesome-agnostic-skills biz)

Style references: `standards/jira.md`, `standards/local-store.md`

## Trigger Conditions

Invoked by `/kanban`. Also triggered when user asks to triage new issues, move cards, check WIP limits, or manage flow.

## Inputs

- Jira project key (from `.env` or user-provided)
- Action: `triage` | `move` | `wip-check` | `flow-review`
- For `move`: issue key + target status (user-provided)

## Workflow

0. **Resolve connection mode** — Resolve all `atlassian-*` aliases per `AGENTS.md` Connection Mode; in local fallback mode, translate queries/writes per `standards/local-store.md`. **Check active persona** — if set via `/as`, apply adaptations from `skills/as/SKILL.md`. Default persona: Product Manager; surface WIP violations and blockers at the top regardless of persona.

### triage
1. Fetch new/unprocessed issues: `project = {KEY} AND status = "To Do" AND created >= -7d ORDER BY created DESC`
2. For each: assess priority, suggest assignee (based on component ownership if available), flag if missing description
3. Present triage decisions to user for confirmation before updating

### move
1. Verify issue exists via `atlassian-read-jira`
2. Check valid transitions: fetch available status transitions for the issue
3. Execute transition via `atlassian-write-jira` only after user confirmation (unless user said "just move it")
4. Report new status

### wip-check
1. Fetch all in-progress items: `project = {KEY} AND status = "In Progress"`
2. Group by assignee
3. Flag any assignee with >2 items in progress (WIP limit violation)
4. Identify items in progress >5 days (potential blockers)

### flow-review
1. Run triage + wip-check in sequence
2. Identify bottlenecks: statuses with high item count relative to adjacent stages
3. Suggest flow improvements: item aging, priority mismatches, stale transitions

## Output Requirements

```markdown
## Kanban: [Project Key] — [Action]

### [Action results]
[Structured per action type — triage table, move confirmation, WIP table, or flow analysis]

### Recommended Actions
- [ ] [Specific action with issue key]
```

## Verification Checklist

- NEVER move a card without confirming the target status is a valid transition
- Triage suggestions are recommendations — user confirms before any write
- WIP limit threshold (2 per person) is a default — adjust if user states a different limit
- Aging thresholds (7 days new, 5 days in-progress) stated explicitly in output

## Usage

```
/kanban [action]
```

Actions: `triage` | `move [key] to [status]` | `wip-check` | `flow-review`

Examples:
- `/kanban triage` — triage new issues from the last 7 days
- `/kanban move PROJ-42 to "In Review"` — transition a card
- `/kanban wip-check` — flag WIP limit violations and aging in-progress items
- `/kanban flow-review` — full flow health analysis

## Required Config

- `DEFAULT_JIRA_PROJECT_KEY` in `.env`
- Atlassian OAuth active with Jira write permission (for moves) — or none; falls back to local `workspace/` files (see `connectors/local/CONNECTOR.md`)
