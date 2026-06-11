---
name: lifecycle
description: Orchestrate the end-to-end product flow — Ingest → Discovery → UX → Design → Dev Handover — as a gated, persona-owned pipeline that produces one traceable artifact per stage and a Lifecycle Index. Use when the user runs /lifecycle, asks to take an idea/BRD/notes from input to dev-ready, or to run the full product flow.
argument-hint: [ingest | discovery | ux | design | handover]
---

# lifecycle

Drives an initiative through the five product stages, adopting the owning persona at each stage, delegating to the stage skill, threading context via a Lifecycle Index, and gating for human review between stages.

Derived from: product-manager + project-coordinator (awesome-agnostic-skills biz)

Style references: `standards/lifecycle.md`, `standards/confluence.md`, `standards/local-store.md`. Stage standards: `standards/requirements.md`, `standards/ux.md`, `standards/design.md`, `standards/jira.md`, `standards/bdd.md`.

## Trigger Conditions

Invoked by `/lifecycle`. Also triggered when the user asks to "take this from notes/BRD to tickets", "run the whole product flow", or "go from discovery through to dev handover".

## Inputs

- Optional stage to jump to: `ingest | discovery | ux | design | handover` (empty = run/continue from the current stage recorded in the Lifecycle Index)
- Initiative name (from argument, inferred from source, or prompted)
- Source material for the Ingest stage: file path(s), pasted content, or email (per `ingest`)
- Jira project key and Confluence space (from `.env` or user-provided)

## Stage → skill delegation

| Stage | Persona | Skill | Standard |
|-------|---------|-------|----------|
| Ingest | Business Analyst | `ingest` | `requirements.md` |
| Discovery | Product Manager | `discover` → write Discovery Brief via `update-docs` | `confluence.md` (Spec/PRD) |
| UX | UX Researcher | `ux` | `ux.md` |
| Design | Designer | `design` | `design.md` |
| Dev Handover | Engineering Lead | `handover` | `jira.md`, `bdd.md` |

## Workflow

0. **Resolve connection mode** — Resolve all `atlassian-*` aliases per `AGENTS.md` Connection Mode; in local fallback mode, translate queries/writes per `standards/local-store.md`. **Check active persona** — if set via `/as`, it informs cross-cutting framing, but each stage still adopts its owning persona as the lens and never abandons its discipline standard (see `standards/lifecycle.md` Persona ownership).

1. **Identify the initiative** — Resolve the initiative name from the argument, the ingested source, or by asking. Use `atlassian-search-confluence` to check for an existing `[Initiative] Lifecycle` index page.

2. **Locate or create the Lifecycle Index** — If found, read it to recover current stage status and the traceability table. If not found, draft a new Lifecycle Index page per the format in `standards/lifecycle.md` (all stages `Not started`). Do NOT write it until the user confirms starting the flow.

3. **Determine the current stage** — If a stage argument was given, target that stage (recovering context from the index). Otherwise resume at the first stage whose status is `Not started` or `In progress`.

4. **Run the stage** — For the target stage:
   a. Adopt the stage's owning persona as the lens.
   b. Delegate to the stage skill (see table). Pass the initiative name, the Lifecycle Index link, and links to all completed upstream artifacts so the skill can populate its **Carried Context** header.
   c. Ensure the produced artifact opens with a populated Carried Context block per `standards/lifecycle.md` (Problem/goal copied from the Discovery Brief once it exists; inherited open questions carried forward).
   d. For the Discovery stage: run `discover` to synthesise, then render the result as a Discovery Brief using the Spec/PRD template in `standards/confluence.md` (Problem, Goals, Non-Goals, User Stories, Solution Overview, Open Questions).

5. **Gate** — Present the drafted artifact. Offer exactly:
   - `proceed` — write the artifact (on confirmation), update the Lifecycle Index stage row + traceability, advance to the next stage.
   - `edit` — revise this artifact per feedback; re-present; do not advance.
   - `stop` — write nothing further; update the index to reflect what is confirmed; report where to resume.
   Never auto-advance past a gate. Never write Confluence pages, `workspace/` files, or Jira issues without explicit confirmation.

6. **Update the Lifecycle Index** — After each confirmed stage, set the stage row to `Done` (or `Skipped` with a one-line reason if the user skips), add the artifact link, and extend the traceability table (Requirement IDs → stage coverage → Jira keys once handover runs).

7. **Complete** — When Dev Handover is done (or the user stops), summarise the chain and point to delivery: "Run `/sprint-plan` to schedule the handed-over stories, or `/kanban` to triage them."

## Output Requirements

At each gate:
```markdown
## Lifecycle: [Initiative] — Stage [N/5]: [Stage name]

[The drafted stage artifact, Carried Context header first]

---
Stage status: Ingest [✓/–] · Discovery [✓/–] · UX [✓/–] · Design [✓/–] · Handover [✓/–]
Confirm: proceed | edit | stop
```

On completion:
```markdown
## Lifecycle complete: [Initiative]
- Lifecycle Index: [link]
- Artifacts: Input Brief [link] · Discovery Brief [link] · UX Synthesis [link] · Design Spec [link] · Handover [link]
- Jira issues created: [keys]
Next: /sprint-plan or /kanban
```

## Verification Checklist

- NEVER write any artifact (page, workspace file, or Jira issue) without explicit user confirmation at the stage gate
- NEVER skip a gate or auto-advance more than one stage per confirmation
- Every stage artifact must open with a populated Carried Context header — verify before presenting
- The Lifecycle Index must be updated after every confirmed stage; stage status and artifact links must match reality
- Requirement IDs (REQ-N) allocated at Ingest are reused as the traceability spine — never renumber
- A skipped stage is recorded as `Skipped` with a reason, not silently omitted
- In local fallback mode, all reads/writes go through `workspace/` per `standards/local-store.md`; no invented keys or page IDs
- Delegate stage work to the stage skills — do not re-implement extraction, synthesis, spec, or decomposition logic here

## Usage

```
/lifecycle                 # run/continue from the current stage
/lifecycle ux              # jump to the UX stage (recovers context from the index)
/lifecycle handover        # jump straight to dev handover
```

Examples:
- `/lifecycle` — start a new flow (ingest a BRD/notes) or continue an in-progress one
- `/lifecycle discovery` — resume at discovery for an already-ingested initiative

## Required Config

- `DEFAULT_JIRA_PROJECT_KEY` in `.env`
- `DEFAULT_CONFLUENCE_SPACE_ID` in `.env`
- Atlassian OAuth active — or none; falls back to local `workspace/` files (see `connectors/local/CONNECTOR.md`)
