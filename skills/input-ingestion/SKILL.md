# input-ingestion

Parses unstructured inputs — local docs, meeting notes, chat transcripts, or email — and extracts structured Jira issues and Confluence page content for human review before any write.

Derived from: business-analyst (requirements extraction) + technical-writer (awesome-agnostic-skills biz)

Style references: `standards/jira.md`, `standards/bdd.md`, `standards/confluence.md`

## Trigger Conditions

Invoked by `/ingest`. Also triggered when user pastes meeting notes, shares a doc path, or asks to "create tickets from this".

## Inputs

- Source type: `file` (local path) | `paste` (content in conversation) | `email` (via email-read alias)
- Source content or path
- Target: `jira` | `confluence` | `both`
- Jira project key and Confluence space (from AGENTS.md or user-provided)

## Workflow

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

4. **Draft Confluence page** — If decisions or context blocks exist, follow `standards/confluence.md`:
   - Title per naming convention: `YYYY-MM-DD [Meeting Name]` or `Decision: [topic]`
   - Page type template: meeting notes or decision record as appropriate
   - Sections: Context | Decisions | Open Questions | Action Items

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
