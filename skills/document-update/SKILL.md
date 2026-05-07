# document-update

Creates and updates Confluence pages with structured, well-formatted content.

Derived from: technical-writer + documentation-engineer (awesome-agnostic-skills biz)

Style reference: `standards/confluence.md`

## Trigger Conditions

Invoked by `/update-docs`. Also triggered when user asks to write, update, or create a Confluence page, spec, ADR, meeting notes template, or wiki entry.

## Inputs

- Target: existing page title/ID to update, OR new page title + parent page
- Confluence space ID (from AGENTS.md or user-provided)
- Content source: user prompt, local file path, or paste

## Workflow

1. **Resolve target** — If updating: use `atlassian-search-confluence` to find the page by title. If creating: confirm parent page exists via `atlassian-read-confluence`
2. **Read current state** (updates only) — Use `atlassian-read-confluence` to fetch existing content before any modification
3. **Draft content** — Apply `standards/confluence.md`:
   - Title follows naming convention (e.g. `Decision: X`, `YYYY-MM-DD Meeting Name`)
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
