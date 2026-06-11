# UX Standards

Reference for structuring and documenting UX research artifacts. Used by discover and ingest skills when surfacing research signals or parsing research docs into Jira/Confluence.

---

## Research Artifact Types

| Artifact | Purpose | Confluence location |
|----------|---------|-------------------|
| **Research plan** | Define questions, methods, participants before a study | `Research/[Study Name] Plan` |
| **Research report** | Synthesised findings, insights, recommendations after a study | `Research/[Study Name] Report` |
| **Persona** | Archetype of a user segment based on research | `Product/Personas/[Name]` |
| **Journey map** | End-to-end experience of a persona through a flow | `Product/Journey Maps/[Flow] — [Persona]` |
| **Usability test report** | Findings from testing a specific design or prototype | `Research/Usability/[Feature] [YYYY-MM-DD]` |
| **Research synthesis** | Patterns across multiple studies | `Research/Synthesis/[Theme]` |
| **Jobs to be Done** | Functional, social, emotional jobs a user is hiring the product to do | `Product/JTBD` |

---

## Research Plan Template

```
# Research Plan: [Study Name]

**Owner:** [name]  **Date:** [YYYY-MM-DD]  **Status:** Draft | Active | Complete

## Research Questions
1. [Primary question — what decision will this research inform?]
2. [Secondary questions]

## Method
[Moderated interviews | Unmoderated usability test | Survey | Diary study | Analytics review]

**Why this method:** [one sentence rationale]

## Participants
- **Target:** [user segment]
- **Number:** [N]
- **Recruitment criteria:** [screener conditions]

## Session Design
[Interview guide / task list / survey outline — or link]

## Timeline
| Phase | Date |
|-------|------|
| Recruitment | |
| Sessions | |
| Analysis | |
| Report | |

## Success Criteria
[What insights would make this study conclusive?]
```

---

## Research Report Template

```
# Research Report: [Study Name]

**Owner:** [name]  **Date:** [YYYY-MM-DD]  **Participants:** [N]
**Related Jira:** [epic or issue keys]

## Summary
[3–5 bullet point executive summary — key findings only]

## Methodology
[Brief: method, participant count, session format, dates]

## Findings

### [Finding 1 — stated as a user behaviour or attitude, not a recommendation]
**Evidence:** [direct quotes or observed behaviours]
**Severity:** Critical | Major | Minor | Positive
**Frequency:** [N/N participants]

### [Finding 2]
...

## Insights
[Synthesised patterns across findings — "users tend to X because Y"]

## Recommendations
- [ ] [Specific, actionable recommendation] → [suggested Jira issue or epic]

## Open Questions
[What this study did not answer]

## Appendix
[Session recordings, transcripts, raw notes — linked, not embedded]
```

---

## Persona Template

```
# Persona: [Name]

**Segment:** [user type]  **Last updated:** [date]  **Based on:** [N interviews / study links]

## Quote
"[Representative quote that captures their core attitude]"

## About
[2–3 sentences: who they are, context, relationship to the product]

## Goals
- [What they're trying to achieve — functional and emotional]

## Frustrations
- [Pain points relevant to the problem space]

## Behaviours
- [Observable patterns relevant to product decisions]

## Jobs to be Done
- **Functional:** [what they need to get done]
- **Social:** [how they want to be perceived]
- **Emotional:** [how they want to feel]

## What This Means for Us
- [Design / product implication]
```

---

## Journey Map Structure

```
# Journey Map: [Flow Name] — [Persona Name]

**Stages:** [list of stages across the top]
**Scope:** [entry point → exit point]
**Last updated:** [date]  **Based on:** [research source]

Per stage:
- **Actions:** what the user does
- **Thoughts:** what they're thinking
- **Feelings:** emotional state (use a scale: frustrated → neutral → delighted)
- **Pain points:** specific friction
- **Opportunities:** design or product interventions
```

Journey maps should be maintained in a visual tool and linked from Confluence — do not reproduce the full map as a table unless a text summary is needed for accessibility.

---

## Usability Test Report Template

```
# Usability Test: [Feature] — [YYYY-MM-DD]

**Prototype/build:** [version or link]  **Participants:** [N]  **Method:** [moderated/unmoderated]

## Task Completion Rates
| Task | Success | Partial | Fail |
|------|---------|---------|------|
| ... | N% | N% | N% |

## Issues Found
| ID | Description | Severity | Frequency |
|----|-------------|----------|-----------|
| U1 | ... | Critical | N/N |

### Severity Scale
- **Critical** — prevents task completion; must fix before launch
- **Major** — causes significant struggle; fix before launch if possible
- **Minor** — friction but workaround exists; prioritise post-launch
- **Positive** — working well; reinforce in design

## Recommendations
- [ ] [Issue ID] [Action] → [Jira issue key or proposed ticket]

## Quotes
[Selected participant quotes with context]
```

---

## Linking UX to Jira

- Each research recommendation should map to a Jira issue (Story, Task, or Bug)
- Include the Confluence research report link in the Jira issue description under `## Context`
- Tag research-driven issues with label `user-feedback`
- Personas and journey maps should be linked from the relevant Epic description

## Jira Issue Types for UX Work

| Work | Issue type | Title pattern |
|------|-----------|---------------|
| User interview series | Spike | `Spike: [N] user interviews on [topic]` |
| Usability test | Spike | `Spike: Usability test for [feature]` |
| Persona creation | Task | `Create persona: [segment name]` |
| Journey mapping | Task | `Map journey: [flow] for [persona]` |
| Survey | Spike | `Spike: Survey [segment] on [topic]` |
| Research synthesis | Task | `Synthesise research: [theme]` |

UX spikes must have: defined research question, participant criteria, timebox, and expected deliverable in the description.

---

## What Goes in Confluence vs Jira

| Content | Where |
|---------|-------|
| Full research report | Confluence |
| Persona | Confluence |
| Journey map | Confluence (visual tool linked) |
| Research recommendation | Jira issue (linked back to Confluence) |
| Usability issue | Jira bug or story (linked to test report) |
| Research plan | Confluence |
| Session notes / transcripts | Confluence (appendix or child page) |
