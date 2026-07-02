---
name: as
description: Set, show, or clear the active output persona (PM, UX Researcher, UX Writer, Designer, Engineering Lead, Scrum Master, BA) for the session so other skills adapt tone, structure, and detail. Use when the user runs /as or asks to switch role/persona or change how outputs are framed.
argument-hint: [pm | ux | content | design | dev | scrum | ba | reset]
allowed-tools: Read, Write, Glob
---

# as

Sets, displays, or clears the active persona. The persona is persisted to `workspace/.meta/persona`, so it survives context compaction and new sessions. All subsequent skill invocations read it at step 0 and adapt their output.

> **Plugin file paths:** Bundled files (`AGENTS.md`, `standards/…`, `connectors/…`, `skills/…`) resolve from the plugin root at `${CLAUDE_SKILL_DIR}/../..`, never the working directory. Only `workspace/…` and `.env` live in the user's project.

## Trigger Conditions

Invoked by `/as`. Also responds to natural language like "switch to PM mode" or "act as a scrum master".

## Inputs

- Role shorthand or name: `pm`, `ux`, `content`, `design`, `dev`, `scrum`, `ba` (and common synonyms below)
- Or: no argument (show current), `reset` (clear)

## Accepted Aliases

| Input | Resolves to |
|-------|-------------|
| `pm`, `product`, `product manager` | Product Manager |
| `ux`, `research`, `researcher`, `user research`, `ux research` | UX Researcher |
| `content`, `uxw`, `ux writer`, `ux-writer`, `content designer`, `content design`, `writer`, `copy`, `copywriter` | UX Writer |
| `design`, `designer`, `ui`, `product designer`, `visual designer` | Designer |
| `dev`, `eng`, `engineer`, `engineering lead`, `tech lead` | Engineering Lead |
| `scrum`, `sm`, `scrum master`, `delivery` | Scrum Master |
| `ba`, `analyst`, `business analyst` | Business Analyst |

## Workflow

1. **Parse input**
   - No argument → read `workspace/.meta/persona` and display its content (or "No persona set — defaulting to Product Manager" if the file is absent)
   - `reset` → delete or empty `workspace/.meta/persona`, confirm "Persona cleared. Defaulting to Product Manager."
   - Known alias → resolve to full persona name
   - Unknown input → list valid options, do not guess

2. **Persist** — write the full persona name as a single line to `workspace/.meta/persona` (create the `.meta/` directory if needed). This file — not conversation memory — is the source of truth other skills read.

3. **Confirm activation** — respond with:
   ```
   Persona set: [Full Role Name]
   [One sentence summary of how this changes outputs]
   Type /as reset to return to default.
   ```

## How Each Skill Adapts

The per-command adaptations for every persona live in `standards/personas.md` (**Per-Command Adaptations** section) — that file is canonical; do not restate its content here. Skills read `workspace/.meta/persona` at step 0 and apply the matching block from that standard.

## Default (no persona set)

Product Manager: insight-first, strategic framing, stakeholder-ready. Recommendations lead, evidence follows. Outputs are accessible to all roles and suitable for sharing upward.

## Usage

```
/as [role]
/as                   # show current persona
/as reset             # clear persona, revert to default
```
