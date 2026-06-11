---
description: Turn UX + discovery into a Design Spec — all states, accessibility, component docs, handoff.
argument-hint: [initiative or focus]
---

# /design

Run the **design** skill.

Initiative or focus: $ARGUMENTS
(If empty, resolve the active initiative from its Lifecycle Index or ask.)

Produces a Design Spec covering every state (default, loading, empty, error, success), an accessibility checklist, component docs, and handoff notes — per `standards/design.md`. Adopts the Designer lens by default; all states and accessibility are always included.

## Usage

```
/design
/design [initiative or focus]
```

Examples:
- `/design checkout` — design spec for the checkout initiative
- `/design` — spec for the current lifecycle initiative

## Skill

See `skills/design/SKILL.md` for the full workflow. Runs standalone or as the Design stage of `/lifecycle`.

## Required Config

- `DEFAULT_CONFLUENCE_SPACE_ID` in AGENTS.md
- Atlassian OAuth active — or none; falls back to local `workspace/` files (see `connectors/local/CONNECTOR.md`)
