# Content Design Standard

Reference for UX writing and content design — voice and tone, microcopy, terminology, and content review. Used when the **UX Writer** persona is active, and by `design` (the spec's Content section) and `update-docs` (voice and tone guides) regardless of persona.

Content design is an **output lens, not a lifecycle stage**: it threads through UX (voice derived from research), Design (microcopy in the spec), and Dev Handover (final strings in story ACs). There is no `/content` command — use `/as content` to make `/design`, `/ux`, `/update-docs`, and `/ingest` lead with content quality.

---

## Voice vs Tone

- **Voice** is constant — the product's personality. Define it once.
- **Tone** varies by context — the same voice sounds different in an error than in a success message.

### Voice principles

| Principle | We are | We are not |
|-----------|--------|-----------|
| [e.g. Clear] | plain, direct, jargon-free | clever at the cost of clarity |
| [e.g. Human] | warm, on the user's side | robotic or corporate |
| [e.g. Confident] | concise, decisive | hedging or apologetic |

### Tone by context

| Context | Tone | Example |
|---------|------|---------|
| Success / confirmation | brief, affirming | "Saved." |
| Error (user-fixable) | calm, helpful, no blame | "That code didn't work — check it and try again." |
| Error (system fault) | apologetic, reassuring | "Something went wrong on our end. We're on it." |
| Empty state | encouraging, points to the next action | "No projects yet. Create your first one." |
| Destructive confirm | clear about consequences | "Delete this for everyone? This can't be undone." |
| Onboarding | welcoming, low-pressure | — |

---

## Microcopy Principles

- **Clear over clever** — comprehension beats personality every time
- **Brief** — cut every word that doesn't carry meaning
- **Useful** — tell the user what to do next, not just what happened
- **Consistent** — one term per concept (see Terminology Glossary)
- **Front-loaded** — most important word first; users scan, they don't read
- **No dead ends** — every error and empty state offers a way forward

---

## Content Patterns by UI Element

| Element | Rule | Good | Avoid |
|---------|------|------|-------|
| **Button / CTA** | Verb + object; match the user's goal | `Create account` | `Submit`, `OK`, `Click here` |
| **Form label** | Noun; sentence case; no colon | `Email address` | `ENTER YOUR EMAIL:` |
| **Field help** | What's needed and why, before the error | `We'll only use this to send receipts` | (silence, then a surprise error) |
| **Error message** | What happened + how to fix; no blame, no codes | `Password needs at least 8 characters` | `Error: invalid input` |
| **Empty state** | Explain + point to first action | `No invoices yet. Send your first one.` | `No data` |
| **Loading** | Set expectation if >1s | `Generating your report…` | spinner with no words |
| **Success** | Confirm + (optional) next step | `Invite sent` | a paragraph |
| **Notification** | Lead with the change, then the actor | `Maria commented on your draft` | `You have a new notification` |

### Error message formula

`[What happened, plainly] + [how to fix it] — never the user's fault, never a raw code.`

> Not: *"Error 422: validation failed."*
> Yes: *"That email is already registered. Try signing in instead."*

---

## Terminology Glossary

One term per concept across UI, docs, and ACs. Maintain as a Confluence page (`Content/Terminology`).

| Term (use this) | Don't use | Definition / context |
|-----------------|-----------|----------------------|
| Workspace | account, org, team | The top-level container a user logs into |
| Member | user, seat | A person with access to a workspace |

---

## Voice & Tone Guide Template (Confluence)

```
# Voice & Tone Guide

**Owner:** [name]  **Last reviewed:** [date]  **Status:** Active

## Our Voice
[2–3 sentences: the product's personality]

## Voice Principles
[Table: principle | we are | we are not]

## Tone by Context
[Table: context | tone | example]

## Terminology
[Link to Content/Terminology, or inline the key terms]

## Examples in the Wild
[Before/after rewrites that show the voice applied]
```

---

## Content Review Checklist

- [ ] Every state (default, loading, empty, error, success) has real copy — no lorem ipsum or "TBD"
- [ ] Terminology matches the glossary — one term per concept
- [ ] Errors say what to do next; none blame the user or expose raw codes
- [ ] CTAs are verb + object and match the user's goal
- [ ] Tone fits each context (error vs success vs destructive)
- [ ] Plain language: short sentences, common words, no unexplained jargon
- [ ] Inclusive: no idioms that won't localize, no gendered or ableist defaults
- [ ] Accessible: icon-only actions have screen-reader labels; link text is meaningful out of context

---

## Where Content Lives

| Content | Where |
|---------|-------|
| Exact strings for a feature | Design Spec `Content` section (`standards/design.md`) |
| Voice & tone guide | Confluence (`Content/Voice & Tone Guide`) |
| Terminology glossary | Confluence (`Content/Terminology`) |
| Final shipped strings | Referenced in the story's BDD ACs (`standards/bdd.md`) |
| Content change for existing copy | Jira Task: `Update copy: [screen/element]` |

Final copy is a **deliverable, not a placeholder**: a story is not Definition-of-Ready (`standards/jira.md`) if its user-facing strings are still "TBD".
