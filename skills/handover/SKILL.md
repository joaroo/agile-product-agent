---
name: handover
description: Turn discovery + UX + design artifacts into a dev-ready handover — epics/stories with BDD acceptance criteria, linked specs, technical context, and a Definition-of-Ready check — then create the Jira issues on confirmation. Use when the user runs /handover or reaches the Dev Handover stage of /lifecycle.
---

# handover

Packages everything upstream into engineering-ready work: a decomposition into epics and stories with BDD acceptance criteria, linked specs and research, explicit technical context and out-of-scope, and a Definition-of-Ready check. Produces a handover page and the Jira issues — only after explicit confirmation.

Derived from: engineering-lead + business-analyst (awesome-agnostic-skills biz)

Style references: `standards/jira.md`, `standards/bdd.md`, `standards/lifecycle.md`, `standards/design.md`, `standards/confluence.md`, `standards/local-store.md`

## Trigger Conditions

Invoked by `/handover` and by `lifecycle` at the Dev Handover stage. Also triggered when the user asks to "make this dev-ready", "break this into stories with ACs", or "hand this to engineering".

## Inputs

- Initiative name and Lifecycle Index link (from `/lifecycle`, or resolved/created when run standalone)
- Upstream: Discovery Brief, UX Synthesis, Design Spec (whichever exist)
- Jira project key and Confluence space (from `AGENTS.md` or user-provided)

## Workflow

0. **Resolve connection mode** — Resolve all `atlassian-*` aliases per `AGENTS.md` Connection Mode; in local fallback mode, translate queries/writes per `standards/local-store.md` (issue keys allocated via `.meta/counters.json`, never invented). **Adopt the Engineering Lead lens**: BDD ACs, edge cases and error states explicit, precise scope, honest readiness — regardless of active persona.

1. **Recover context** — Read the Lifecycle Index and all upstream artifacts. Map the Requirement IDs (REQ-N) and Design Spec flows to the work that must be built.

2. **Decompose** — Per `standards/jira.md`:
   - One **Epic** (noun phrase) for the initiative if not already present
   - **Stories** for user-facing behaviour; **Tasks** for internal/technical work; **Bugs** only for defects; **Spikes** for any open research/feasibility questions inherited from UX/design
   - Each issue follows the title convention and the Story/Task **Description Template** (Context + Acceptance Criteria + Out of Scope + Notes)
   - `## Context` links the Discovery Brief, UX Synthesis, and Design Spec; design stories link the spec and reference all states + accessibility per `design.md`

3. **Write BDD acceptance criteria** — Per `standards/bdd.md`, each story gets Given/When/Then ACs covering happy path, sad path, and relevant edge cases; ACs are observable and testable. Map them as the Jira AC checklist format from `bdd.md`.

4. **Add technical context** — Constraints, dependencies, linked docs in `## Notes`; explicit `## Out of Scope`. Flag dependencies between issues.

5. **Run the Definition-of-Ready check** — For each story/task, verify the DoR checklist in `standards/jira.md` (title convention, Context + ACs, epic link, story points, no `needs-design`/`needs-spec`, no open blockers). List any item that fails DoR — do not silently mark it ready. Apply `needs-design`/`needs-spec` labels where upstream artifacts are missing.

6. **Assemble the handover artifact** — A Dev Handover page opening with the **Carried Context** header per `standards/lifecycle.md`, summarising the epic/story breakdown, the DoR status per item, and the full traceability (REQ-N → issue). Reuse the Design Handoff Checklist results from `design` where present.

7. **Confirm, then write** — Present the proposed Jira issues (titles, types, ACs summary, DoR status) and the handover page. Write **only** on explicit confirmation, consistent with `/ingest` and `/sync`:
   - `atlassian-write-jira` for each confirmed issue (epic first, then stories/tasks/bugs/spikes, then sub-tasks — topological order per `standards/local-store.md`)
   - `atlassian-write-confluence` for the handover page
   - Update the Lifecycle Index traceability table with the created issue keys and set the Dev Handover stage to `Done`
   When run via `/lifecycle`, return to the orchestrator gate for the confirmation.

8. **Point to delivery** — On completion, note: "Run `/sprint-plan` to schedule these, or `/groom` to refine any item that failed DoR."

## Output Requirements

```markdown
# Dev Handover: [Initiative]

**Owner:** [name]  **Last reviewed:** [date]  **Status:** Active

## Carried Context
- **Initiative:** ... · **Lifecycle Index:** [link]
- **Stage:** Dev Handover · **Persona:** Engineering Lead
- **Upstream artifact(s):** [Design Spec, UX Synthesis, Discovery Brief links]
- **Problem / goal:** [copied from Discovery Brief]
- **Key decisions so far:** ...
- **Open questions inherited:** ...

## Epic
[Epic title] — [link/key once created]

## Stories & Tasks
### [KEY?] [Story title]
Type: Story | Points: [est] | Priority: [P]
Context: [links to spec/research]
Acceptance Criteria (BDD):
- [ ] **Happy path**: Given ..., when ..., then ...
- [ ] **Sad path**: ...
- [ ] **Edge case**: ...
Out of Scope: ...
DoR: ✓ ready | ✗ [failing item]

## Definition-of-Ready Summary
| Issue | DoR | Failing items |
|-------|-----|---------------|

## Traceability
| REQ-N | Story/Task | Spec coverage |

---
Confirm: "create all" | "create N" | "edit N" | "skip N"
```

## Verification Checklist

- Every story/task has BDD Given/When/Then ACs covering happy + sad + edge cases; ACs are observable and testable
- Issue types correct: Story = user-facing, Task = internal, Bug = defect, Spike = investigation (per `standards/jira.md`)
- Each issue links its upstream Context (Discovery/UX/Design); design stories reference all states + accessibility
- DoR run per item; failures are listed, not hidden; `needs-design`/`needs-spec` applied where upstream is missing
- NEVER create Jira issues or write the handover page without explicit user confirmation
- In local fallback mode: keys allocated via `.meta/counters.json`, never invented; issues written in topological order
- Carried Context header present and populated; Lifecycle Index updated with created keys on completion
- Do not fabricate estimates — if points are unknown, state "unestimated" and flag against DoR
