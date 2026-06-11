# Design Standards

Reference for documenting design work — component specs, handoff, design review, and design system governance. Used by update-docs and groom skills when creating design documentation or evaluating design task quality.

---

## Design Artifact Types

| Artifact | Purpose | Confluence location |
|----------|---------|-------------------|
| **Design spec** | Detailed behaviour, states, and annotations for a feature | `Design/Specs/[Feature Name]` |
| **Component doc** | Design system component: variants, usage, anatomy, do/don't | `Design/Components/[Component Name]` |
| **Design decision** | Record of a significant design choice and rationale | `Design/Decisions/[topic]` (or use `Decision:` prefix) |
| **Design review notes** | Feedback from a design review session | `Design/Reviews/[YYYY-MM-DD] [Feature]` |
| **Pattern library entry** | Reusable interaction or layout pattern | `Design/Patterns/[Pattern Name]` |
| **Handoff note** | Specs, assets, and instructions for engineering handoff | `Design/Handoff/[Feature] Handoff` |

---

## Design Spec Template

```
# Design Spec: [Feature Name]

**Designer:** [name]  **Status:** Draft | Ready for review | Approved | Shipped
**Last updated:** [date]  **Jira epic:** [key]
**Design file:** [link to design tool]

## Overview
[One paragraph: what this feature does and who it's for]

## User Stories Covered
- [JIRA-KEY] [story title]

## Flows

### [Flow name]
[Description of what the user is doing in this flow]

**Entry point:** [where the user comes from]
**Exit point:** [where the user goes next]

#### States
| State | Description | Notes |
|-------|-------------|-------|
| Default | ... | |
| Loading | ... | |
| Empty | ... | |
| Error | ... | |
| Success | ... | |

#### Interactions
- **[Trigger]:** [what happens — be specific about animation, transition, or behaviour]

### [Next flow]
...

## Edge Cases
- [Scenario]: [expected behaviour]

## Content
[Copy, labels, error messages, placeholder text — exact strings where decided]

## Accessibility
- [ ] All interactive elements keyboard-navigable
- [ ] Focus order matches visual order
- [ ] Touch targets ≥ 44×44px
- [ ] Colour contrast meets WCAG AA
- [ ] Screen reader labels defined for icon-only actions

## Open Questions
| Question | Owner | Status |
|----------|-------|--------|
| ... | ... | Open |

## Out of Scope
- [What this spec does not cover]
```

---

## Component Documentation Template

```
# Component: [Name]

**Status:** Experimental | Stable | Deprecated
**Last updated:** [date]  **Design file:** [link]

## Anatomy
[Label each part of the component — can be a diagram description if no image]

## Variants
| Variant | When to use |
|---------|-------------|
| [name] | ... |

## States
| State | Trigger | Visual change |
|-------|---------|---------------|
| Default | — | ... |
| Hover | cursor over | ... |
| Active | pressed | ... |
| Focused | keyboard nav | ... |
| Disabled | prop disabled | ... |
| Loading | async action | ... |
| Error | validation fail | ... |

## Usage

### Do
- [Correct usage]

### Don't
- [Incorrect or discouraged usage]

## Spacing & Sizing
[Key measurements, padding, min/max dimensions]

## Accessibility
- Role: [ARIA role]
- Label: [how it should be labelled for screen readers]
- Keyboard: [key interactions]

## Related components
- [Component name] — [relationship]
```

---

## Handoff Notes Template

```
# Handoff: [Feature Name]

**Designer:** [name]  **Date:** [date]  **Sprint:** [sprint name]
**Design file:** [link]  **Spec:** [Confluence spec link]
**Jira stories:** [keys]

## What's in scope
[Brief: what is being handed off]

## Assets
| Asset | Format | Location |
|-------|--------|----------|
| Icons | SVG | [link] |
| Images | [format] | [link] |

## Design tokens used
[List only tokens relevant to this feature — don't enumerate all tokens]
| Token | Value | Usage |
|-------|-------|-------|
| ... | ... | ... |

## Motion / animation
[Easing, duration, trigger for any animated interactions — or "no animation"]

## Responsive behaviour
| Breakpoint | Behaviour |
|-----------|-----------|
| Mobile (<768px) | ... |
| Tablet (768–1024px) | ... |
| Desktop (>1024px) | ... |

## Known constraints
[Any design decisions constrained by tech, time, or data — so engineering understands the why]

## Review checklist
- [ ] Design file is at final state (no WIP layers visible)
- [ ] All states documented in spec
- [ ] Assets exported and linked
- [ ] Accessibility notes complete
- [ ] Open questions resolved or documented
```

---

## Design Review Process

### When to hold a design review
- Before handoff to engineering
- At key decision points (direction change, novel interaction, cross-platform impact)
- After significant user research findings

### Review format
1. Designer presents: problem, constraints, proposed solution
2. Reviewers ask clarifying questions — no solutions yet
3. Structured feedback: what works, what's unclear, what risks exist
4. Designer captures feedback in review notes page
5. Designer decides and documents resolution (not consensus by committee)

### Design Review Notes Template (Confluence)

```
# Design Review: [Feature] — [YYYY-MM-DD]

**Designer:** [name]  **Attendees:** [names]
**Design file:** [link]  **Spec:** [link]

## Feedback

### [Reviewer name]
- [Feedback item — specific, not evaluative]

## Decisions
- [Decision made in response to feedback + rationale]

## Actions
- [ ] [Change to make] — [owner]
```

---

## Jira Issue Types for Design Work

| Work | Issue type | Title pattern |
|------|-----------|---------------|
| New feature design | Story | `Design [feature name] for [user]` |
| Component design | Task | `Design component: [name]` |
| Design system update | Task | `Update [component/token]: [change]` |
| Design review | Task | `Design review: [feature]` |
| Handoff | Task | `Handoff: [feature] to engineering` |
| Design spike / exploration | Spike | `Spike: Explore [design problem]` |
| Accessibility fix | Bug | `[Component]: [accessibility issue]` |

### Design task AC format

Design tasks use outcome-based acceptance criteria, not process steps:

```
## Acceptance Criteria
- [ ] All states (default, loading, empty, error, success) documented in spec
- [ ] Design reviewed and feedback incorporated
- [ ] Handoff notes complete with assets and tokens
- [ ] Accessibility checklist passed
```

Do not write ACs like "designer opens Figma and creates mockup" — describe the deliverable, not the method.

---

## Design System Governance

- New components require a component doc before they can be considered part of the design system
- Components in `Experimental` status can be used but may change; do not depend on their API
- `Deprecated` components must have a replacement documented before deprecation is final
- Token changes require a design decision record and a migration note

## Design ↔ Engineering Handoff Checklist

Before a design is considered handed off:
- [ ] Spec page complete and linked from Jira story
- [ ] All states covered (incl. empty, error, loading)
- [ ] Responsive behaviour documented
- [ ] Motion/animation spec included (or explicitly "none")
- [ ] Assets exported and linked
- [ ] Accessibility requirements noted
- [ ] Open questions resolved or clearly flagged
- [ ] Designer available for questions during implementation sprint
