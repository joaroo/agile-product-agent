---
title: "Guest Checkout Design Spec"
space: TEAM
parent: guest-checkout-lifecycle
owner: sofie
status: Active
last_reviewed: 2026-05-08
---

# Design Spec: Guest Checkout

**Designer:** sofie  **Status:** Active  **Last updated:** 2026-05-08

## Carried Context
- **Initiative:** Guest Checkout · **Lifecycle Index:** [Guest Checkout Lifecycle](guest-checkout-lifecycle.md)
- **Stage:** Design · **Persona:** Designer
- **Upstream artifact(s):** [Guest Checkout Synthesis](guest-checkout-synthesis.md), [Guest Checkout Discovery Brief](guest-checkout-discovery-brief.md)
- **Problem / goal:** First-time buyers abandon checkout at the forced account wall (62% drop); enable guest purchase before Black Friday, target checkout completion +5pp.
- **Key decisions so far:** Shadow accounts keyed by email; post-purchase invite must not block confirmation
- **Open questions inherited:** Existing-account email collision (priya) — **blocks the collision state below**

---

## Overview
Three flows: guest checkout entry, post-purchase account invite, guest order lookup. The account wall is replaced by an email field with a secondary "sign in" link for returning customers.

## User Stories Covered
- REQ-1 → guest checkout entry (PROD-2)
- REQ-2 → post-purchase invite (PROD-3)
- REQ-3 → order lookup (PROD-4)

## Flows

### Guest checkout entry
**Entry point:** cart → "Checkout" · **Exit point:** payment step

#### States
| State | Description | Notes |
|-------|-------------|-------|
| Default | Email field + "Continue as guest" primary, "Sign in" secondary | |
| Loading | Button spinner, field locked | |
| Empty | Continue disabled until email present | |
| Error | Invalid email: "Enter a valid email so we can send your receipt." | says what to do next |
| Success | Advances to payment; email pinned in header | |

#### Interactions
- Email collision with an existing account → state pending decision (open question); placeholder behaviour: proceed as guest, no disclosure

### Post-purchase account invite
**Entry point:** order confirmation rendered · **Exit point:** dismissed or account created

#### States
| State | Description | Notes |
|-------|-------------|-------|
| Default | Inline card under confirmation: "Save your details for next time" + one-tap create | never a modal; never blocks confirmation |
| Loading | Card button spinner | |
| Empty | — n/a (card only renders with a completed order) | stated explicitly |
| Error | "We couldn't create your account. Your order is safe — try again from your confirmation email." | reassures first |
| Success | "You're set. We'll remember you next time." card collapses | brief, no follow-up nag |

### Guest order lookup
**Entry point:** "Find my order" in footer + confirmation email link · **Exit point:** order detail view

#### States
| State | Description | Notes |
|-------|-------------|-------|
| Default | Email + order number fields | |
| Loading | Skeleton order card | |
| Empty | No match: "We couldn't find that order. Check the order number in your confirmation email." | no account-existence leak |
| Error | Service failure: retry guidance | |
| Success | Read-only order detail | |

## Edge Cases
- Guest email matches existing account (blocked on decision)
- Order lookup rate-limiting to prevent enumeration

## Content
All strings above are real copy, no placeholders. Terminology: "guest" never "anonymous user"; "save your details" never "register".

## Accessibility
- [x] Keyboard-navigable, logical focus order
- [x] Touch targets ≥44px
- [x] WCAG AA contrast on all states
- [x] Screen-reader labels on email/order fields and invite card
- [x] Error states announced via live region

## Open Questions
| Question | Owner | Due |
|----------|-------|-----|
| Email collision behaviour | priya | before PROD-2 build |

## Out of Scope
- Social login, saved payment methods (v1 exclusions)

---

*Last updated: 2026-05-08*
