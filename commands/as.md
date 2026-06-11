---
description: Set the active output persona for the session.
argument-hint: [pm | ux | design | dev | scrum | ba | reset]
---

# /as

Set the active persona for this session. Subsequent commands will adapt their output tone, structure, and detail level accordingly.

Requested persona: $ARGUMENTS
(If empty, show the current persona.)

## Usage

```
/as [role]
/as                   # show current persona
/as reset             # clear persona, revert to default
```

## Roles

| Shorthand | Full role |
|-----------|-----------|
| `pm` | Product Manager |
| `ux` | UX Researcher |
| `design` | Designer |
| `dev` | Engineering Lead |
| `scrum` | Scrum Master |
| `ba` | Business Analyst |

## Examples

```
/as pm
/as ux
/as design
/as dev
/as scrum
/as ba
/as reset
```

## Skill

See `skills/as/SKILL.md` for full behaviour.
