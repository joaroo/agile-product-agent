# Product Lifecycle Standard

Canonical reference for the end-to-end product flow: **Ingest → Discovery → UX → Design → Dev Handover**. Used by the `product-lifecycle` orchestrator and the per-stage skills (`input-ingestion`, `product-discovery`, `ux-synthesis`, `design-spec`, `dev-handover`).

Each stage is owned by a persona, produces **one canonical artifact** in that persona's format (per the discipline standard), and **gates for human review** before the next stage begins. Every artifact opens with a **Carried Context** header so the next persona inherits the thread, and a per-initiative **Lifecycle Index** page threads the whole chain.

---

## Stages

| # | Stage | Persona lens | Standard | Canonical artifact | Confluence location |
|---|-------|-------------|----------|--------------------|---------------------|
| 1 | **Ingest** | Business Analyst | `requirements.md`, `confluence.md` | Input Brief — sources + candidate requirements with traceability IDs | `Discovery/[Initiative] Input Brief` |
| 2 | **Discovery** | Product Manager | `confluence.md` (Spec / PRD) | Discovery Brief — problem, goals, non-goals, opportunities, success metrics | `Discovery/[Initiative] Discovery Brief` |
| 3 | **UX** | UX Researcher | `ux.md` | UX Research & Synthesis — personas, journey map, JTBD, findings, open questions | `Research/[Initiative] Synthesis` |
| 4 | **Design** | Designer | `design.md` | Design Spec — all states + accessibility, component docs, handoff notes | `Design/Specs/[Initiative]` |
| 5 | **Dev Handover** | Engineering Lead | `jira.md`, `bdd.md` | Dev Handover Package — epics/stories with BDD ACs, linked specs, tech context, DoR check | `Design/Handoff/[Initiative] Handoff` + Jira issues |

Delivery (`/sprint-plan`, `/kanban`, `/status`, `/retro`) picks up **after** handover. The lifecycle ends by pointing the user to those commands; it does not run them.

Stage order is fixed, but a stage may be **skipped** (recorded as `Skipped` in the index with a one-line reason) when not applicable — e.g. a pure tech-debt initiative may skip UX and Design.

---

## Carried Context header

Every stage artifact (and the Dev Handover page) **must open** with this block, immediately after the page title/owner header. It is the mechanism that carries context forward without duplicating full upstream documents.

```markdown
## Carried Context
- **Initiative:** [name] · **Lifecycle Index:** [link to index page]
- **Stage:** [this stage] · **Persona:** [owning persona]
- **Upstream artifact(s):** [links to prior stage docs — most recent first]
- **Problem / goal:** [1–2 lines inherited from the Discovery Brief]
- **Key decisions so far:** [bullets — the decisions a downstream reader must respect]
- **Open questions inherited:** [bullets — unresolved items passed down, with owner if known]
```

Rules:
- **Problem / goal** is copied (not paraphrased away) from the Discovery Brief once it exists; before Discovery, it summarises the Input Brief.
- **Open questions inherited** must carry forward every unresolved question from upstream that this stage does not close. Closed questions move to a "Resolved" note in the body.
- Links use the same form the connector uses elsewhere (Confluence page link in live mode; relative `workspace/confluence/{SPACE}/{slug}.md` path in local mode).

---

## Lifecycle Index page

One per initiative. The single source of truth for where the initiative is in the flow and how artifacts trace to each other. Created by the `product-lifecycle` orchestrator at the start of a run and updated after every stage.

**Title:** `[Initiative] Lifecycle` · **Location:** `Discovery/[Initiative] Lifecycle` · **Naming:** follows `confluence.md` (sentence case, no trailing punctuation).

```markdown
# [Initiative] Lifecycle

**Owner:** [name]  **Last reviewed:** [YYYY-MM-DD]  **Status:** Active

One-paragraph summary of the initiative and its current stage.

---

## Stage Status

| Stage | Status | Persona | Artifact |
|-------|--------|---------|----------|
| Ingest | Done / In progress / Not started / Skipped | Business Analyst | [link or —] |
| Discovery | ... | Product Manager | [link or —] |
| UX | ... | UX Researcher | [link or —] |
| Design | ... | Designer | [link or —] |
| Dev Handover | ... | Engineering Lead | [link or —] |

## Traceability

Source/BRD → Requirement IDs → Discovery → UX → Design → Epics/Stories

| Requirement ID | Source | Discovery theme | UX finding | Design coverage | Jira issue(s) |
|----------------|--------|-----------------|------------|-----------------|---------------|
| REQ-1 | [BRD §x] | ... | ... | ... | [KEY-n] |

---

*Last updated: [YYYY-MM-DD]*
```

- **Status** values reuse the Confluence `Status` macro vocabulary from `confluence.md`: `Not started` / `In progress` / `Done` (plus `Skipped`).
- **Requirement IDs** (`REQ-N`) are allocated at the Ingest stage and reused as the spine of the traceability table. They are independent of Jira keys and never renumbered.
- Storage is a normal Confluence page (or local `workspace/confluence/{SPACE}/` file per `standards/local-store.md`) — no new persistence mechanism.

---

## Gating rules

- The orchestrator **never writes** any artifact (Confluence page, `workspace/` file, or Jira issue) without explicit user confirmation, consistent with `/ingest` and `/sync`.
- After producing each stage artifact the orchestrator presents it and offers: `proceed` (advance to next stage), `edit` (revise this artifact), `stop` (pause; index records current state for later resume).
- A stage may be re-entered directly (`/lifecycle ux`, or the standalone `/ux`); the stage reads the Lifecycle Index to recover context rather than restarting the flow.

---

## Persona ownership

Stage skills adopt their owning persona as the **default lens** even when no persona is set via `/as`. An explicitly-set persona still takes precedence for cross-cutting framing, but a stage never abandons its discipline standard (e.g. running `/design` always produces all states + accessibility per `design.md`, regardless of active persona). See `standards/personas.md` and `skills/persona-switch/SKILL.md`.
