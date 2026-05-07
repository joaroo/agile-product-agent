# backlog-grooming

Queries, prioritizes, and improves the quality of the Jira backlog. Suggests story breakdowns and acceptance criteria.

Derived from: business-analyst + product-manager (awesome-agnostic-skills biz)

Style references: `standards/jira.md`, `standards/bdd.md`

## Trigger Conditions

Invoked by `/groom`. Also triggered when user asks to refine, clean, or prioritize backlog, or wants to improve story quality.

## Inputs

- Jira project key (from AGENTS.md or user-provided)
- Optional: specific epic or label to scope grooming
- Optional: grooming mode — `triage` (quick pass), `deepen` (add ACs), `prune` (remove stale)

## Workflow

1. **Fetch backlog** — Use `atlassian-search-jira`:
   `project = {KEY} AND status in (Backlog, "To Do") ORDER BY priority DESC, rank ASC`
   Fetch up to 50 items. Apply label/epic filter if provided.

2. **Quality assessment** — For each item, flag:
   - Missing acceptance criteria (no "AC:" or "Acceptance Criteria" section in description)
   - Vague titles (fewer than 5 words, or starts with "Fix", "Update" without context)
   - Oversized stories (estimate >8 points or no estimate + complexity signals in title)
   - Stale items (created >90 days ago, never updated, no assignee)
   - Duplicates (search for similar titles using `atlassian-cross-search`)

3. **Prioritization review** — Group items by:
   - High value / low effort (quick wins)
   - High value / high effort (big bets)
   - Low value (candidates for pruning)

4. **Improvement suggestions** — For each flagged item, produce:
   - Refined title (if vague)
   - Draft acceptance criteria (if missing)
   - Breakdown suggestion (if oversized: split into sub-tasks)

5. **Confirm changes** — Present all suggestions to user before any writes

6. **Apply** (on confirmation) — Use `atlassian-write-jira` to update descriptions, add ACs, adjust priority. All AC drafts must follow `standards/bdd.md` Given/When/Then format and `standards/jira.md` description template

## Output Requirements

```markdown
## Backlog Grooming Report

### Quality Issues
| Key | Issue | Suggestion |
|-----|-------|-----------|
| ... | ...   | ...       |

### Prioritization
**Quick wins (do next):** [keys]
**Big bets:** [keys]
**Prune candidates:** [keys] — [reason]

### Proposed AC Drafts
**[Key] [Title]**
Acceptance Criteria:
- [ ] ...
```

## Verification Checklist

- All issue data from live Jira queries — never fabricate estimates or priorities
- Stale threshold (90 days) applied consistently
- Never delete or archive items without explicit user confirmation
- Acceptance criteria drafts must follow BDD Given/When/Then format per `standards/bdd.md` — testable and observable, not aspirational
- Issue titles must follow naming conventions in `standards/jira.md`
- Duplicate detection uses search results, not pattern-matching assumptions
