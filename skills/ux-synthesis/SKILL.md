---
name: ux-synthesis
description: Synthesise discovery + raw research/notes into UX research artifacts — personas, journey map, JTBD, findings, open questions — per standards/ux.md. Use when the user runs /ux, asks for UX synthesis, personas, or a journey map, or reaches the UX stage of /lifecycle.
---

# ux-synthesis

Turns a Discovery Brief and ingested research/notes into structured UX research artifacts, surfacing user behaviours, evidence, and open questions rather than design solutions.

Derived from: ux-researcher + research-synthesizer (awesome-agnostic-skills biz)

Style references: `standards/ux.md`, `standards/lifecycle.md`, `standards/confluence.md`, `standards/local-store.md`

## Trigger Conditions

Invoked by `/ux` and by `product-lifecycle` at the UX stage. Also triggered when the user asks for a persona, journey map, JTBD, or research synthesis for an initiative.

## Inputs

- Initiative name and Lifecycle Index link (from `/lifecycle`, or resolved/created when run standalone)
- Upstream: Discovery Brief (problem, goals) + any ingested research, interview notes, usability findings
- Confluence space (from `AGENTS.md` or user-provided)

## Workflow

0. **Resolve connection mode** — Resolve all `atlassian-*` aliases per `AGENTS.md` Connection Mode; in local fallback mode, translate queries/writes per `standards/local-store.md`. **Adopt the UX Researcher lens** by default (user-centric language, findings as behaviours not recommendations, evidence and quotes, open questions flagged). If a different persona is set via `/as`, honour its cross-cutting framing but keep the `ux.md` artifact structure.

1. **Recover context** — Read the Lifecycle Index and the Discovery Brief. Gather upstream research/notes (from the Input Brief, linked appendix pages, or user-provided). If no research exists, state that and proceed with the synthesis labelled as **assumption-based** (flag every unevidenced claim).

2. **Synthesise** — Per `standards/ux.md`, produce as applicable:
   - **Persona(s)** using the Persona Template — only when there is research to ground them; otherwise mark as provisional
   - **Journey map** using the Journey Map Structure (actions / thoughts / feelings / pain points / opportunities)
   - **Jobs to be Done** (functional / social / emotional)
   - **Findings** stated as user behaviours/attitudes with Evidence, Severity, Frequency (Research Report Template)
   - **Insights** — patterns across findings
   - **Open questions** — what the research did not answer

3. **Flag research gaps** — Where a finding lacks evidence, mark it and propose a research Spike per the "Jira Issue Types for UX Work" table in `standards/ux.md` (do not create the Spike here — note it for handover/discovery).

4. **Assemble the artifact** — A UX Research & Synthesis page opening with the **Carried Context** header per `standards/lifecycle.md`, then the synthesised sections above.

5. **Confirm before write** — Present the draft. Write only on confirmation, using `atlassian-write-confluence` to `Research/[Initiative] Synthesis` (per `ux.md` locations). When run via `/lifecycle`, return to the orchestrator gate instead of writing directly.

## Output Requirements

```markdown
# UX Research & Synthesis: [Initiative]

**Owner:** [name]  **Last reviewed:** [date]  **Status:** Draft

## Carried Context
- **Initiative:** ... · **Lifecycle Index:** [link]
- **Stage:** UX · **Persona:** UX Researcher
- **Upstream artifact(s):** [Discovery Brief link]
- **Problem / goal:** [copied from Discovery Brief]
- **Key decisions so far:** ...
- **Open questions inherited:** ...

## Personas
[Persona Template per ux.md — or "provisional / assumption-based"]

## Journey Map
[Journey Map Structure per ux.md — link to visual tool if available]

## Jobs to be Done
- **Functional / Social / Emotional**

## Findings
### [Finding stated as a user behaviour]
**Evidence:** ... **Severity:** ... **Frequency:** N/N

## Insights
...

## Open Questions
...

## Research Gaps
- [Unevidenced claim] → proposed Spike: [title per ux.md]

---
*Last updated: [date]*
```

## Verification Checklist

- Findings are stated as user behaviours/attitudes, NOT as design or product recommendations
- Every finding has evidence; unevidenced claims are explicitly marked provisional/assumption-based
- Open questions are flagged, never papered over with false confidence
- Carried Context header present and populated, with Problem/goal copied from the Discovery Brief
- Uses `ux.md` templates exactly; does not invent a freeform structure
- NEVER write to Confluence without confirmation; no fabricated page IDs
- Personas are only asserted as grounded when research supports them
