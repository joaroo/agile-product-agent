---
name: retro
description: Facilitate a sprint retrospective — pull sprint metrics, structure what went well / what to improve / action items, and write the retro page. Use when the user runs /retro or asks to run, prepare, or write up a sprint retrospective.
argument-hint: [sprint]
---

# retro

Facilitates a sprint retrospective: synthesizes sprint outcomes and team input into a structured, ceremony-ready retrospective and writes the retro page to Confluence.

Derived from: scrum-master + retrospective-facilitator (awesome-agnostic-skills biz)

Style references: `standards/confluence.md`, `standards/jira.md`, `standards/local-store.md`

> **Plugin file paths:** Any reference below to `AGENTS.md`, a `standards/…`, `connectors/…`, or another `skills/…` file is bundled with this plugin. Read it relative to the plugin root at `${CLAUDE_SKILL_DIR}/../..` (e.g. `${CLAUDE_SKILL_DIR}/../../standards/jira.md`), **not** the current working directory. Only `workspace/…` and `.env` live in the user's project (the working directory).

## Trigger Conditions

Invoked by `/retro`. Also triggered when the user asks to run a retrospective, prepare a retro, or write up sprint learnings.

## Inputs

- Jira project key (from `.env` or user-provided)
- Target sprint (default: last closed sprint; or user-named sprint)
- Confluence space ID for the retro page (from `.env` or user-provided)
- Optional: team-provided notes, observations, or votes to fold in

## Workflow

0. **Resolve connection mode** — Resolve all `atlassian-*` aliases per `AGENTS.md` Connection Mode; in local fallback mode, translate queries/writes per `standards/local-store.md`. **Check active persona** — if set via `/as`, apply adaptations from `skills/as/SKILL.md`. Default persona: Product Manager (themes and outcomes first); Scrum Master gets the most ceremony-ready format.

1. **Identify sprint** — Use `atlassian-search-jira` to resolve the target sprint:
   `project = {KEY} AND sprint in closedSprints() ORDER BY sprint DESC` (take the most recent unless the user named one). If no closed sprint exists, state "no closed sprint found" and stop.

2. **Gather sprint data** — From the sprint's issues compute:
   - Committed vs completed story points (and item counts)
   - Carryover (committed but not Done)
   - Blocked items (label `blocked` or status Blocked)
   - Scope creep (items added after sprint start)
   - Notable bugs or reopened issues
   All figures come from live queries — never estimate.

3. **Fold in team input** — If the user provided notes, observations, or votes, incorporate them. Otherwise derive discussion prompts from the data signals (e.g. high carryover → "what slowed delivery?").

4. **Synthesize** — Structure findings into:
   - **What went well** — grounded in data and team input
   - **What to improve** — patterns, not blame
   - **Action items** — each concrete, with an owner and a due date

5. **Present for review** — Show the drafted retro (metrics + three sections) to the user. Do NOT write anything until the user confirms.

6. **Write** (on confirmation):
   - Use `atlassian-write-confluence` to create the retro page using the sprint planning / retrospective template in `standards/confluence.md`. Title per naming convention: `Sprint [N] Retrospective`.
   - **Action items → Jira:** offer to create each action item as a Jira issue via `atlassian-write-jira`, but only after explicit user confirmation (same human-in-the-loop rule as `/ingest`). Never create issues automatically.

## Output Requirements

```markdown
## Sprint [N] Retrospective

**Sprint:** [Name] | **Dates:** [start]–[end]
**Committed:** [N] pts | **Completed:** [N] pts ([N]%) | **Carryover:** [N] pts

### What Went Well
- [Point — tied to a metric or quote]

### What To Improve
- [Pattern — not blame]

### Action Items
| Action | Owner | Due |
|--------|-------|-----|
| ... | ... | ... |
```

## Verification Checklist

- All sprint metrics from live JQL / local store — NEVER fabricate completion rates or velocity
- Every action item has a named owner and a due date — flag any that don't
- Retro page follows the retrospective template in `standards/confluence.md`
- Never create Jira action-item issues without explicit user confirmation
- If the sprint has no closed data, state "insufficient data" rather than inventing a narrative
- "What to improve" framed as patterns and process, never individual blame

## Usage

```
/retro
/retro [sprint name or number]
```

Examples:
- `/retro` — retrospective for the most recently closed sprint
- `/retro Sprint 41` — retrospective for a specific named sprint

## Required Config

- `DEFAULT_JIRA_PROJECT_KEY` in `.env`
- `DEFAULT_CONFLUENCE_SPACE_ID` in `.env` (for the retro page)
- Atlassian OAuth active with Confluence write permission — or none; falls back to local `workspace/` files (see `connectors/local/CONNECTOR.md`)
