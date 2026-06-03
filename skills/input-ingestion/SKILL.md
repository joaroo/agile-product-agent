---
name: input-ingestion
description: Parse local docs, meeting notes, transcripts, or email into draft Jira issues and Confluence pages for review before any write. Use when the user runs /ingest, shares a doc or notes, or asks to create tickets from content.
---

# input-ingestion

Parses unstructured inputs — local docs, meeting notes, chat transcripts, or email — and extracts structured Jira issues and Confluence page content for human review before any write.

Derived from: business-analyst (requirements extraction) + technical-writer (awesome-agnostic-skills biz)

Style references: `standards/jira.md`, `standards/bdd.md`, `standards/confluence.md`, `standards/ux.md`, `standards/design.md`, `standards/requirements.md`, `standards/local-store.md`

## Trigger Conditions

Invoked by `/ingest`. Also triggered when user pastes meeting notes, shares a doc path, or asks to "create tickets from this".

## Inputs

- Source type: `file` (local path) | `paste` (content in conversation) | `email` (via email-read alias)
- Source content or path
- Target: `jira` | `confluence` | `both`
- Jira project key and Confluence space (from AGENTS.md or user-provided)

## Workflow

0. **Resolve connection mode** — Resolve all `atlassian-*` aliases per `AGENTS.md` Connection Mode; in local fallback mode, translate queries/writes per `standards/local-store.md`. **Check active persona** — if set via `/as`, apply adaptations from `skills/persona-switch/SKILL.md`. Default persona: Product Manager. BA persona: flag ambiguity before creating tickets and route requirements content to `standards/requirements.md`. UX Researcher: route to `standards/ux.md`; Designer: route to `standards/design.md`.

1. **Ingest source**
   - `file`: Read the file from local filesystem (markdown, txt, pdf summary)
   - `paste`: Use content provided directly in the conversation
   - `email`: Use `email-read` alias to fetch email(s) by label, thread ID, or date range

2. **Extract structure** — Identify:
   - **Action items**: sentences with "will", "should", "to do", assigned names, or checkbox markers
   - **Decisions**: sentences stating conclusions, approvals, or direction changes
   - **Open questions**: unresolved items, "TBD", questions without answers
   - **Context blocks**: background, goals, constraints worth preserving in Confluence

3. **Draft Jira issues** — For each action item, follow `standards/jira.md`:
   - Title: imperative sentence per naming convention, <10 words
   - Description: Context + Acceptance Criteria sections; ACs in BDD Given/When/Then format per `standards/bdd.md`
   - Type: Story (user-facing), Task (internal), or Bug (defect) per issue type definitions
   - Suggested assignee: if name mentioned in context
   - Priority: High if deadline mentioned or marked urgent, Medium otherwise

4. **Draft Confluence page** — If decisions or context blocks exist, follow the relevant standard:
   - General notes/decisions: `standards/confluence.md` (meeting notes or decision record template)
   - UX research content (research findings, personas, journey maps): `standards/ux.md`
   - Design content (specs, handoff notes, design review notes): `standards/design.md`
   - Requirements content (requirements specs, traceability matrices, process flows): `standards/requirements.md`
   - Title per naming convention in the relevant standard

5. **Present for review** — Show all drafted items. Do NOT write to Jira or Confluence until user explicitly confirms each item or says "create all"

6. **Write** (on confirmation) — Use `atlassian-write-jira` and/or `atlassian-write-confluence` for confirmed items only

## Output Requirements

```markdown
## Ingestion Results: [Source]

### Jira Issues to Create (N)
**[Draft title]**
Type: Task | Priority: Medium
Description: [2-3 lines]
Assignee: [name or unassigned]

---

### Confluence Page to Create
**Title:** [title]
**Space:** [space key]
[Preview of page content]

---
Confirm: "create all" | "create issues only" | "skip N" | "edit N"
```

## Verification Checklist

- NEVER write to Jira or Confluence without explicit user confirmation
- Action items must be clearly extractable — do not invent tasks not present in source
- Assignee is a suggestion only — never set without confirmation
- If source is ambiguous or lacks clear action items, report "no actionable items found" rather than fabricating
- Email access requires EMAIL_MCP_TOOL set in .env — if not set, state "email connector not configured"
