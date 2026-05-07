# Agent User Personas

Archetypes of people who use this agent. Skills should adapt tone, detail level, and output structure based on who is asking. When the user's role is unclear, ask before producing a long-form output.

---

## 1. Product Manager — "the owner"

**Goal:** Make good product decisions quickly, keep the team aligned, ship things that matter.

**Typical requests:**
- `/discover` — what should we be working on?
- `/sprint-plan` — what goes in next sprint?
- `/update-docs` — write or update a spec, decision record, or roadmap entry
- `/ingest` — turn meeting notes or stakeholder feedback into tickets

**What they value:**
- Strategic framing — connect work to outcomes and user needs
- Concise summaries — don't bury the insight in raw data
- Actionable recommendations — "do X" not "consider whether X might be relevant"
- Decision records — they want rationale captured, not just conclusions

**Output style:**
- Lead with the "so what" — insight before evidence
- Use the spec and decision record templates from `standards/confluence.md`
- Link recommendations to user problems or business goals
- Flag trade-offs explicitly

**Frustrations to avoid:**
- Long preambles before the actual answer
- Outputs that require heavy editing before sharing with stakeholders
- Recommendations without rationale

---

## 2. UX Designer — "the advocate"

**Goal:** Understand users deeply, translate research into clear design direction, get designs implemented faithfully.

**Typical requests:**
- `/update-docs` — create a research report, persona, journey map, or design spec
- `/ingest` — parse usability test notes or interview transcripts into findings
- `/discover` — what does existing research say about this area?
- `/groom` — improve UX-related stories before sprint

**What they value:**
- Correct artifact structure — research reports, specs, and handoff docs in the right format
- User-centric language — outcomes for users, not features for the business
- Fidelity to templates — they'll share these docs with engineering and stakeholders
- Accessibility considerations included by default

**Output style:**
- Use templates from `standards/ux.md` and `standards/design.md` exactly
- Lead with user impact, not design decisions
- Include evidence and quotes where relevant
- Flag open questions clearly — designers don't want false confidence

**Frustrations to avoid:**
- Skipping states (empty, error, loading) in design specs
- Vague ACs like "design looks good" — use outcome-based format per `standards/design.md`
- Confusing UX research artifacts with product specs

---

## 3. Engineering Lead — "the builder"

**Goal:** Understand what to build, why, and how it fits the system — then execute without ambiguity.

**Typical requests:**
- `/groom` — are these tickets ready? do they have proper ACs?
- `/sprint-plan` — what's the sprint scope and is it realistic?
- `/status` — how are we tracking?
- `/update-docs` — write or update a technical decision record or architecture note

**What they value:**
- Precision — exact acceptance criteria, clear scope, explicit out-of-scope
- BDD-formatted ACs — testable, observable, no ambiguity
- Technical context in issue descriptions — constraints, dependencies, linked docs
- Honest status reports — don't soften bad news

**Output style:**
- Use BDD Given/When/Then format for all ACs per `standards/bdd.md`
- Include edge cases and error states in specs
- Reference design handoff docs and Confluence specs in issue descriptions
- Status reports should flag risks plainly, not euphemistically

**Frustrations to avoid:**
- Aspirational ACs ("user has a great experience")
- Stories missing context or epic links
- Sprint plans that ignore capacity or include ungroomed tickets

---

## 4. Scrum Master / Delivery Manager — "the facilitator"

**Goal:** Keep the team moving, remove blockers, run good ceremonies, make delivery predictable.

**Typical requests:**
- `/status` — sprint health, burndown, velocity
- `/kanban` — triage new issues, check WIP limits, review flow
- `/sprint-plan` — facilitate the planning process
- `/groom` — prepare backlog for upcoming sprint

**What they value:**
- Data-driven summaries — velocity, burndown, WIP — not opinions
- Ceremony-ready outputs — sprint plans and retros they can run directly
- Risk signals — what's going to cause problems before it does
- Flow hygiene — WIP violations, blocked items, stale cards

**Output style:**
- Tables and metrics first, narrative second
- Sprint plan output should be ready to present to the team
- Status reports include trend (improving/stable/declining), not just snapshots
- Flag blockers and scope creep immediately, don't bury them

**Frustrations to avoid:**
- Status summaries without numbers
- Sprint plans that don't account for velocity or capacity
- Retrospective outputs that are too vague to action

---

## 5. Business Analyst — "the translator"

**Goal:** Turn messy stakeholder input into clear, well-structured requirements the team can build from.

**Typical requests:**
- `/ingest` — parse meeting notes, emails, or workshops into tickets and docs
- `/groom` — improve story quality, add acceptance criteria
- `/update-docs` — write requirements docs, process flows, or decision records
- `/discover` — what requirements already exist around this area?

**What they value:**
- Requirements traceability — every ticket linked to a user need or business goal
- Complete stories — context, ACs, out-of-scope, all present
- Structured templates — they're responsible for quality, not just quantity
- BDD ACs they can hand to both QA and development

**Output style:**
- Use Jira description template (Context + ACs + Out of Scope) from `standards/jira.md`
- ACs in Given/When/Then per `standards/bdd.md`
- Confluence pages follow the relevant template — don't freeform
- When ingesting input, flag ambiguity rather than assuming

**Frustrations to avoid:**
- Tickets without context or epic links
- ACs that can't be tested
- Confluence pages that don't follow the agreed structure

---

## How Skills Should Use These Personas

- If the user identifies their role at the start of a session, adapt accordingly throughout
- When output format is ambiguous, default to the most structured option (engineering lead standard)
- For `/status` and `/discover`, lead with executive summary suitable for a PM or stakeholder; include detail below
- For `/groom` and `/sprint-plan`, default to engineering lead precision on ACs and scope
- For `/update-docs` and `/ingest`, ask "who is this for?" if the page type is unclear — the answer determines which template to use
