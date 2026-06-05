---
name: document-update
description: Create or update Confluence pages (specs, decision records, meeting notes, UX/design docs) using house templates. Use when the user runs /update-docs or asks to write or update a page, spec, ADR, or wiki entry.
---

# document-update

Creates and updates Confluence pages with structured, well-formatted content.

Derived from: technical-writer + documentation-engineer (awesome-agnostic-skills biz)

Style references: `standards/confluence.md`, `standards/ux.md`, `standards/design.md`, `standards/content.md`, `standards/requirements.md`, `standards/local-store.md`

## Trigger Conditions

Invoked by `/update-docs`. Also triggered when user asks to write, update, or create a Confluence page, spec, ADR, meeting notes template, or wiki entry.

## Inputs

- Target: existing page title/ID to update, OR new page title + parent page
- Confluence space ID (from AGENTS.md or user-provided)
- Content source: user prompt, local file path, or paste

## Workflow

0. **Resolve connection mode** — Resolve all `atlassian-*` aliases per `AGENTS.md` Connection Mode; in local fallback mode, translate queries/writes per `standards/local-store.md`. **Check active persona** — if set via `/as`, apply adaptations from `skills/persona-switch/SKILL.md` to template selection and output framing. If unclear who the page is for and no persona is set, ask before drafting.

1. **Resolve target** — If updating: use `atlassian-search-confluence` to find the page by title. If creating: confirm parent page exists via `atlassian-read-confluence`
2. **Read current state** (updates only) — Use `atlassian-read-confluence` to fetch existing content before any modification
3. **Draft content** — Apply `standards/confluence.md`:
   - Title follows naming convention per `standards/confluence.md` (e.g. `Decision: X`, `YYYY-MM-DD Meeting Name`, `Design Spec: X`)
   - For UX artifacts (research reports, personas, journey maps): use templates from `standards/ux.md`
   - For design artifacts (specs, component docs, handoff notes, design reviews): use templates from `standards/design.md`
   - For content artifacts (voice and tone guide, terminology glossary): use templates from `standards/content.md`
   - For BA artifacts (requirements specs, traceability matrices, process flows): use templates from `standards/requirements.md`
   - Owner + Last reviewed + Status header block
   - Short summary paragraph
   - Structured sections per page type (decision record, spec, meeting notes, etc.)
   - Code blocks with language specifier for any technical content
   - Last-updated note at bottom
4. **Confirm before write** — Show draft to user for approval if content is substantial (>200 words) or the page is being overwritten
5. **Write** — Use `atlassian-write-confluence` with the confirmed content
6. **Verify** — Use `atlassian-read-confluence` to confirm the page reflects the new content

## Output Requirements

On completion:
```
Updated: [Page Title]
URL: [Confluence page URL if available]
Space: [Space key]
Action: created | updated
```

## Verification Checklist

- NEVER overwrite a page without reading its current content first
- NEVER create a page without confirming the parent space/page
- Always show draft for user confirmation on destructive updates
- Code blocks must use appropriate language specifiers
- No fabricated page IDs — always resolve via search
