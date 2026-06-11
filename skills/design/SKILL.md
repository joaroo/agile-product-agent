---
name: design
description: Turn UX synthesis + discovery into a Design Spec with all states (default/loading/empty/error/success), accessibility checklist, component docs, and handoff notes per standards/design.md. Use when the user runs /design, asks for a design spec or handoff, or reaches the Design stage of /lifecycle.
argument-hint: [initiative or focus]
---

# design

Translates a Discovery Brief and UX synthesis into a concrete Design Spec ready for engineering handoff — every state covered, accessibility built in, deliverables described as outcomes.

Derived from: product-designer + design-system-architect (awesome-agnostic-skills biz)

Style references: `standards/design.md`, `standards/content.md`, `standards/lifecycle.md`, `standards/confluence.md`, `standards/local-store.md`

> **Plugin file paths:** Any reference below to `AGENTS.md`, a `standards/…`, `connectors/…`, or another `skills/…` file is bundled with this plugin. Read it relative to the plugin root at `${CLAUDE_SKILL_DIR}/../..` (e.g. `${CLAUDE_SKILL_DIR}/../../standards/jira.md`), **not** the current working directory. Only `workspace/…` and `.env` live in the user's project (the working directory).

## Trigger Conditions

Invoked by `/design` and by `lifecycle` at the Design stage. Also triggered when the user asks for a design spec, component doc, or handoff note for an initiative.

## Inputs

- Initiative name and Lifecycle Index link (from `/lifecycle`, or resolved/created when run standalone)
- Upstream: Discovery Brief (problem, goals, user stories) + UX Research & Synthesis (personas, journeys, JTBD)
- Confluence space (from `.env` or user-provided)

## Workflow

0. **Resolve connection mode** — Resolve all `atlassian-*` aliases per `AGENTS.md` Connection Mode; in local fallback mode, translate queries/writes per `standards/local-store.md`. **Adopt the Designer lens** by default: all states (default, loading, empty, error, success) and an accessibility checklist are mandatory in every spec, regardless of active persona. Outcome-based ACs, not process steps.

1. **Recover context** — Read the Lifecycle Index, Discovery Brief, and UX Synthesis. Map each user story / journey stage to the flows the spec must cover.

2. **Draft the Design Spec** — Per the Design Spec Template in `standards/design.md`:
   - **Overview**, **User Stories Covered** (link Jira keys if any exist yet — otherwise reference Discovery user stories)
   - **Flows** with entry/exit points and the **States** table — every flow lists Default, Loading, Empty, Error, Success (no state omitted)
   - **Interactions**, **Edge Cases**, **Content** (exact copy/labels/error strings where decided — apply `standards/content.md`: real copy for every state, errors that say what to do next, terminology consistent with the glossary)
   - **Accessibility** checklist (keyboard nav, focus order, touch targets ≥44px, WCAG AA contrast, screen-reader labels)
   - **Open Questions** and **Out of Scope**

3. **Add component docs / handoff notes as needed** — For new design-system components, add a Component doc per the Component Documentation Template. Prepare Handoff Notes per the Handoff Notes Template (assets, tokens, motion, responsive behaviour, known constraints) and run the **Design ↔ Engineering Handoff Checklist** in `standards/design.md`.

4. **Assemble the artifact** — A Design Spec page opening with the **Carried Context** header per `standards/lifecycle.md`, then the spec sections above. Carry forward inherited open questions; mark UX questions this spec resolves as Resolved.

5. **Confirm before write** — Present the draft. Write only on confirmation, using `atlassian-write-confluence` to `Design/Specs/[Initiative]` (and `Design/Handoff/[Initiative] Handoff` if handoff notes are separated) per `design.md` locations. When run via `/lifecycle`, return to the orchestrator gate instead of writing directly.

## Output Requirements

```markdown
# Design Spec: [Initiative]

**Designer:** [name]  **Status:** Draft  **Last updated:** [date]

## Carried Context
- **Initiative:** ... · **Lifecycle Index:** [link]
- **Stage:** Design · **Persona:** Designer
- **Upstream artifact(s):** [UX Synthesis link, Discovery Brief link]
- **Problem / goal:** [copied from Discovery Brief]
- **Key decisions so far:** ...
- **Open questions inherited:** ...

## Overview
...

## User Stories Covered
- [story]

## Flows
### [Flow]
**Entry / Exit point:** ...
#### States
| State | Description | Notes |
| Default / Loading / Empty / Error / Success | ... | |
#### Interactions
- ...

## Edge Cases
## Content
## Accessibility
- [ ] keyboard-navigable / focus order / ≥44px / WCAG AA / SR labels

## Open Questions
## Out of Scope

---
*Last updated: [date]*
```

## Verification Checklist

- Every flow's States table includes Default, Loading, Empty, Error, AND Success — none omitted
- Accessibility checklist present in the spec (and per component)
- ACs/deliverables are outcome-based ("all states documented"), never process steps ("open Figma")
- Carried Context header present and populated; Problem/goal copied from the Discovery Brief
- Uses `design.md` templates exactly; design spec is not confused with UX research artifacts
- Handoff Checklist run before marking the spec ready for handover
- NEVER write to Confluence without confirmation; no fabricated page IDs

## Usage

```
/design
/design [initiative or focus]
```

Examples:
- `/design checkout` — design spec for the checkout initiative
- `/design` — spec for the current lifecycle initiative

## Required Config

- `DEFAULT_CONFLUENCE_SPACE_ID` in `.env`
- Atlassian OAuth active — or none; falls back to local `workspace/` files (see `connectors/local/CONNECTOR.md`)
