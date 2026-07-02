---
title: "Guest Checkout Discovery Brief"
space: TEAM
parent: guest-checkout-lifecycle
owner: priya
status: Active
last_reviewed: 2026-05-08
---

# Guest Checkout Discovery Brief

**Owner:** priya  **Last reviewed:** 2026-05-08  **Status:** Active

First-time buyers hit a forced account wall at checkout and 62% of them leave. Removing the wall for guests — and earning the account after the purchase — is the highest-leverage conversion fix available before Black Friday.

## Carried Context
- **Initiative:** Guest Checkout · **Lifecycle Index:** [Guest Checkout Lifecycle](guest-checkout-lifecycle.md)
- **Stage:** Discovery · **Persona:** Product Manager
- **Upstream artifact(s):** [Guest Checkout Input Brief](guest-checkout-input-brief.md)
- **Problem / goal:** First-time buyers abandon checkout at the forced account wall (62% drop); enable guest purchase before Black Friday, target checkout completion +5pp.
- **Key decisions so far:** Shadow accounts keyed by email
- **Open questions inherited:** Existing-account email collision (priya); analytics funnel split (jonas)

---

## Problem
First-time buyers must create an account before paying. 62% drop at that step; support hears about it weekly. The friction costs revenue exactly where intent is highest — at checkout.

## Goals
- Checkout completion for first-time buyers +5pp (REQ-1)
- ≥20% of guest purchasers accept the post-purchase account invite (REQ-2)
- Guests self-serve order lookup without contacting support (REQ-3)

## Non-Goals
- Social login
- Saved payment methods for guests
- Any change to the logged-in loyalty flow (REQ-4 is a constraint, not new work)

## User Stories
See Dev Handover — PROD-2, PROD-3, PROD-4 under epic PROD-1.

## Solution Overview
Guest path through the existing checkout keyed on email only; shadow account created behind the scenes; one-tap "save your details" invite on the confirmation page; order lookup by email + order number.

## Open Questions
| Question | Owner | Due |
|----------|-------|-----|
| Existing-account email collision behaviour | priya | before Design |
| Funnel event to split account-wall vs payment drop-off | jonas | before build |

---

*Last updated: 2026-05-08*
