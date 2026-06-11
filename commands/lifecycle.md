---
description: Run the end-to-end product flow — ingest → discovery → UX → design → dev handover — gated per stage.
argument-hint: [ingest | discovery | ux | design | handover]
---

# /lifecycle

Run the **lifecycle** skill.

Stage to jump to: $ARGUMENTS
(If empty, run or continue from the current stage recorded in the initiative's Lifecycle Index.)

Walks an initiative through all five product stages — each owned by a persona, each producing one traceable artifact — gating for review between stages and threading context via a Lifecycle Index page.

| Stage | Persona | Artifact |
|-------|---------|----------|
| Ingest | Business Analyst | Input Brief |
| Discovery | Product Manager | Discovery Brief |
| UX | UX Researcher | UX Research & Synthesis |
| Design | Designer | Design Spec |
| Dev Handover | Engineering Lead | Dev Handover Package + Jira issues |

## Usage

```
/lifecycle                 # run/continue from the current stage
/lifecycle ux              # jump to the UX stage (recovers context from the index)
/lifecycle handover        # jump straight to dev handover
```

Examples:
- `/lifecycle` — start a new flow (ingest a BRD/notes) or continue an in-progress one
- `/lifecycle discovery` — resume at discovery for an already-ingested initiative

## Skill

See `skills/lifecycle/SKILL.md` for the full orchestration workflow, and `standards/lifecycle.md` for the stage map, Carried Context header, and Lifecycle Index format.

## Required Config

- `DEFAULT_JIRA_PROJECT_KEY` in AGENTS.md
- `DEFAULT_CONFLUENCE_SPACE_ID` in AGENTS.md
- Atlassian OAuth active — or none; falls back to local `workspace/` files (see `connectors/local/CONNECTOR.md`)
