# /as

Set the active persona for this session. Subsequent commands will adapt their output tone, structure, and detail level accordingly.

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
| `ux` | UX Designer |
| `dev` | Engineering Lead |
| `scrum` | Scrum Master |
| `ba` | Business Analyst |

## Examples

```
/as pm
/as ux
/as dev
/as scrum
/as ba
/as reset
```

## Skill

See `skills/persona-switch/SKILL.md` for full behaviour.
