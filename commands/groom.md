# /groom

Run the **backlog-grooming** skill.

Reviews backlog quality, suggests priorities, flags stale items, and drafts acceptance criteria.

## Usage

```
/groom
/groom [mode] [scope]
```

Modes: `triage` (quick pass) | `deepen` (add ACs) | `prune` (remove stale)

Examples:
- `/groom` — full grooming pass on default project
- `/groom triage` — quick priority + quality check
- `/groom deepen epic:checkout` — add acceptance criteria to checkout epic items
- `/groom prune` — identify and propose removal of stale backlog items

## Skill

See `skills/backlog-grooming/SKILL.md` for full workflow.

## Required Config

- `DEFAULT_JIRA_PROJECT_KEY` in AGENTS.md
- Atlassian OAuth active with Jira write permission
