---
title: "Guest Checkout Input Brief"
space: TEAM
parent: guest-checkout-lifecycle
owner: priya
status: Active
last_reviewed: 2026-05-08
---

# Guest Checkout Input Brief

**Owner:** priya  **Last reviewed:** 2026-05-08  **Status:** Active

Candidate requirements extracted from the guest checkout BRD (v0.2) and the 2026-05-06 checkout sync notes. Each requirement is traced to its source; ambiguities are recorded as open questions, not resolved by assumption.

## Carried Context
- **Initiative:** Guest Checkout · **Lifecycle Index:** [Guest Checkout Lifecycle](guest-checkout-lifecycle.md)
- **Stage:** Ingest · **Persona:** Business Analyst
- **Upstream artifact(s):** BRD: Guest checkout v0.2 (source file)
- **Problem / goal:** First-time buyers abandon checkout at the forced account wall (62% drop); enable guest purchase before Black Friday, target checkout completion +5pp.
- **Key decisions so far:** Shadow accounts keyed by email (from sync notes, unopposed)
- **Open questions inherited:** Guest email matches an existing account — behaviour undecided (owner: priya)

---

## Sources

| Source | Type | Date |
|--------|------|------|
| BRD: Guest checkout v0.2 (M. Larsen) | BRD | 2026-05-04 |
| Checkout sync meeting notes | Meeting notes | 2026-05-06 |

## Candidate Requirements

| ID | Requirement | Priority | Source |
|----|-------------|----------|--------|
| REQ-1 | The system shall let a shopper complete a purchase with only an email address — no password or account | Must | BRD §Requirements |
| REQ-2 | The system shall invite a guest to save their details as an account after order confirmation, without blocking the confirmation page | Must | BRD §Requirements + sync notes (sofie) |
| REQ-3 | The system shall let a guest retrieve their order via email + order number | Must | BRD §Requirements |
| REQ-4 | Guest checkout shall not change the loyalty-points flow for logged-in customers | Must | BRD §Requirements |

## Out of Scope
- Social login (BRD, v1 exclusion)
- Saved payment methods for guests (BRD, v1 exclusion)

## Ambiguities & Open Questions

| Question | Owner | Due |
|----------|-------|-----|
| What happens when a guest email matches an existing account? | priya | before Design |
| Can analytics split account-wall drop-off from payment friction? | jonas | Discovery |

---

*Last updated: 2026-05-08*
