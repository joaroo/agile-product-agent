# Jira Standards

Reference for all skills that create, update, or query Jira issues.

---

## Issue Types

| Type | When to use |
|------|-------------|
| **Epic** | Large body of work spanning multiple sprints; always tied to a product goal |
| **Story** | User-facing feature or behaviour; written from user perspective |
| **Task** | Internal/technical work with no direct user-facing outcome |
| **Bug** | Unintended behaviour in existing functionality |
| **Spike** | Time-boxed investigation; always has a fixed point estimate and a defined output |
| **Sub-task** | Breakdown of a Story or Task; never standalone |

Never use Story for bugs. Never use Task for user-facing features.

---

## Title Conventions

| Type | Pattern | Example |
|------|---------|---------|
| Story | `As a [user], I can [action]` OR imperative: `[Verb] [object] for [user]` | `Allow users to reset password via email` |
| Task | Imperative: `[Verb] [object]` | `Migrate auth service to Postgres` |
| Bug | `[Component]: [observed behaviour] when [condition]` | `Checkout: order total shows $0 when coupon applied` |
| Spike | `Spike: [question to answer]` | `Spike: Evaluate WebSocket vs SSE for live updates` |
| Epic | Noun phrase describing the outcome | `Guest Checkout Flow` |

- Sentence case for all titles
- No trailing punctuation
- No issue key in title (Jira adds it)
- Max ~80 characters

---

## Required Fields

### Story / Task
- **Summary** (title) — required
- **Description** — required; include context + acceptance criteria
- **Epic link** — required (every story/task belongs to an epic)
- **Story points** — required before sprint commitment
- **Priority** — required (default: Medium)

### Bug
- **Summary** — required
- **Description** — required; include steps to reproduce, expected vs actual, environment
- **Priority** — required; use Highest for production outages
- **Severity label** — `sev1` / `sev2` / `sev3`

### Spike
- **Summary** — required
- **Description** — required; include the question, timebox, and expected deliverable
- **Story points** — fixed to the timebox (e.g. 2 points = 2 days max)

---

## Description Template

### Story / Task
```
## Context
[Why this work exists. Link to spec, decision, or user research.]

## Acceptance Criteria
- [ ] [Criterion in BDD format — see standards/bdd.md]

## Out of Scope
- [What this ticket explicitly does not cover]

## Notes
[Technical hints, links, dependencies]
```

### Bug
```
## Steps to Reproduce
1. ...

## Expected Behaviour
[What should happen]

## Actual Behaviour
[What actually happens]

## Environment
Browser/OS/version:
User account (if relevant):

## Possible Cause
[If known]
```

---

## Priority Definitions

| Priority | Definition |
|----------|-----------|
| **Highest** | Production outage or data loss affecting users now |
| **High** | Significant user impact; needs to be in the next sprint |
| **Medium** | Normal work; prioritised within backlog |
| **Low** | Nice to have; can wait multiple sprints |
| **Lowest** | Backlog parking; reviewed quarterly |

---

## Label Taxonomy

Use labels consistently. Don't invent new labels without team agreement.

| Label | Meaning |
|-------|---------|
| `user-feedback` | Originated from user-reported issue or support ticket |
| `tech-debt` | Internal improvement with no direct user value |
| `blocked` | Cannot proceed; comment must explain blocker |
| `needs-design` | Requires design input before development |
| `needs-spec` | Requires spec or AC before sprint commitment |
| `sev1` / `sev2` / `sev3` | Bug severity (1 = critical, 3 = minor) |
| `quick-win` | Low effort, high value; < 1 point |
| `spike` | Investigation work (use in addition to Spike issue type) |

---

## Definition of Ready

A Story or Task is sprint-ready when:
- [ ] Title follows naming convention
- [ ] Description has Context + Acceptance Criteria sections
- [ ] Epic link set
- [ ] Story points estimated
- [ ] No `needs-design` or `needs-spec` labels
- [ ] No open blocking dependencies

## Definition of Done

- [ ] Acceptance criteria all passing
- [ ] Code reviewed and merged
- [ ] Tests written (unit + integration where applicable)
- [ ] Confluence spec updated if behaviour changed
- [ ] Issue transitioned to Done

---

## Workflow Statuses

```
Backlog → To Do → In Progress → In Review → Done
                                           ↘ Won't Do
```

- **Backlog**: not yet sprint-ready
- **To Do**: sprint-committed, not started
- **In Progress**: actively being worked
- **In Review**: PR open or awaiting QA
- **Done**: all DoD criteria met
- **Won't Do**: explicitly deprioritised; add comment explaining why

---

## JQL Patterns

```jql
# Sprint-ready backlog
project = {KEY} AND status = Backlog AND "Story Points" is not EMPTY AND labels not in (needs-spec, needs-design) ORDER BY priority DESC

# Active sprint
project = {KEY} AND sprint in openSprints()

# Blocked items
project = {KEY} AND labels = blocked AND status != Done

# Stale in-progress (>5 days, no update)
project = {KEY} AND status = "In Progress" AND updated <= -5d

# High-priority bugs
project = {KEY} AND issuetype = Bug AND priority in (High, Highest) AND status != Done ORDER BY priority DESC

# Scope creep (added after sprint start)
project = {KEY} AND sprint in openSprints() AND created >= startOfSprint()
```
