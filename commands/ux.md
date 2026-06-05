---
description: Synthesise discovery + research into UX artifacts — personas, journey map, JTBD, findings.
argument-hint: [initiative or focus]
---

# /ux

Run the **ux-synthesis** skill.

Initiative or focus: $ARGUMENTS
(If empty, resolve the active initiative from its Lifecycle Index or ask.)

Produces a UX Research & Synthesis artifact — personas, journey map, Jobs to be Done, findings (as user behaviours, with evidence), and open questions — per `standards/ux.md`. Adopts the UX Researcher lens by default.

## Usage

```
/ux
/ux [initiative or focus]
```

Examples:
- `/ux onboarding` — UX synthesis for the onboarding initiative
- `/ux` — synthesis for the current lifecycle initiative

## Skill

See `skills/ux-synthesis/SKILL.md` for the full workflow. Runs standalone or as the UX stage of `/lifecycle`.

## Required Config

- `DEFAULT_CONFLUENCE_SPACE_ID` in AGENTS.md
- Atlassian OAuth active — or none; falls back to local `workspace/` files (see `connectors/local/CONNECTOR.md`)
