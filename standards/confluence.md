# Confluence Standards

Reference for all skills that create or update Confluence content.

---

## Space & Hierarchy

```
Space (team or product area)
└── Section (top-level: e.g. "Product", "Engineering", "Processes")
    └── Topic (e.g. "Architecture", "Decisions", "Sprint Planning")
        └── Page (specific document)
            └── Child page (sub-topic or dated instance)
```

- Never create orphan pages — always set a parent
- Dated pages (meeting notes, sprint pages) go under their topic as children
- Avoid nesting deeper than 4 levels

## Naming Conventions

| Page type | Format | Example |
|-----------|--------|---------|
| Decision record | `Decision: [topic]` | `Decision: Auth Strategy` |
| Meeting notes | `[YYYY-MM-DD] [meeting name]` | `2026-05-07 Sprint Planning` |
| Spec / PRD | `[Feature Name] Spec` | `Checkout Flow Spec` |
| Retrospective | `Sprint [N] Retrospective` | `Sprint 42 Retrospective` |
| How-to / runbook | `How to [action]` | `How to Deploy to Production` |
| Architecture doc | `[Component] Architecture` | `API Gateway Architecture` |
| Reference | `[Topic] Reference` | `Error Codes Reference` |

Use sentence case. No abbreviations in titles unless universally understood (API, DB, UI are fine).

---

## Page Structure

### All pages

```
# [Title]

**Owner:** [name]  **Last reviewed:** [date]  **Status:** Draft | Active | Deprecated

[One-paragraph summary — what this page is, who it's for, what decision or information it captures]

---

[Body sections]

---

*Last updated: [date]*
```

### Decision record

```
## Context
[Why this decision needed to be made. What forces, constraints, or requirements drove it.]

## Decision
[What was decided, stated plainly. One sentence if possible.]

## Alternatives Considered
- **[Option A]** — [why rejected]
- **[Option B]** — [why rejected]

## Consequences
[What becomes easier, harder, or different as a result. Include known trade-offs.]

## Status
Proposed | Accepted | Superseded by [link]
```

### Spec / PRD

```
## Problem
[User problem or opportunity. One paragraph.]

## Goals
- [Measurable goal]

## Non-Goals
- [Explicitly out of scope]

## User Stories
[Link to or list key Jira issues]

## Solution Overview
[High-level approach. Diagrams welcome.]

## Open Questions
| Question | Owner | Due |
|----------|-------|-----|
| ...      | ...   | ... |
```

### Meeting notes

```
## Attendees
[Names]

## Agenda
1. ...

## Notes
[Per agenda item]

## Decisions
- [Decision made]

## Action Items
- [ ] [Action] — [owner] — [due date]
```

### Sprint planning / retrospective

```
## Sprint Goal
[One sentence user-value statement]

## Committed Items
[Table or linked Jira filter]

## Retrospective (for retro pages)
**What went well:** ...
**What to improve:** ...
**Action items:** ...
```

---

## Style Rules

- **Lead with the point** — summary first, detail below
- **Short paragraphs** — 3–5 sentences max per paragraph
- **Use tables** for comparisons, options, action items — not prose lists
- **Use numbered lists** for sequential steps, bullet lists for unordered items
- **Bold** key terms on first use; avoid bolding for emphasis mid-sentence
- **Avoid passive voice** — "We decided X" not "It was decided that X"
- **No jargon without definition** — link or define on first use
- **Code** in code blocks with language specifier; inline `code` for short references
- **Dates** in ISO format: `YYYY-MM-DD`

## What NOT to put in Confluence

- Real-time status (use Jira)
- API credentials or secrets (use a secrets manager)
- Meeting chat logs verbatim — summarize instead
- Duplicate content — link to the source of truth, don't copy-paste

---

## Macros to use

| Use case | Macro |
|----------|-------|
| Status badge | `Status` macro (Not started / In progress / Done) |
| Link to Jira issue | Jira issue link (renders inline) |
| Code | Code Block with language |
| Warning / note | Info / Warning / Note panel |
| Table of contents | TOC macro on long pages (>4 H2 sections) |

Avoid: page properties report, excerpt, and include macros unless the team already uses them.
