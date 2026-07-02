---
title: "Guest Checkout Synthesis"
space: TEAM
parent: guest-checkout-lifecycle
owner: sofie
status: Active
last_reviewed: 2026-05-08
---

# UX Research & Synthesis: Guest Checkout

**Owner:** sofie  **Last reviewed:** 2026-05-08  **Status:** Active

Synthesis of the checkout funnel data and stakeholder notes. No dedicated user research exists for this initiative yet — findings below are grounded in analytics and support signals; unevidenced claims are marked provisional.

## Carried Context
- **Initiative:** Guest Checkout · **Lifecycle Index:** [Guest Checkout Lifecycle](guest-checkout-lifecycle.md)
- **Stage:** UX · **Persona:** UX Researcher
- **Upstream artifact(s):** [Guest Checkout Discovery Brief](guest-checkout-discovery-brief.md), [Guest Checkout Input Brief](guest-checkout-input-brief.md)
- **Problem / goal:** First-time buyers abandon checkout at the forced account wall (62% drop); enable guest purchase before Black Friday, target checkout completion +5pp.
- **Key decisions so far:** Shadow accounts keyed by email
- **Open questions inherited:** Existing-account email collision (priya); funnel event split (jonas)

---

## Personas

**First-time buyer (provisional — assumption-based, no interviews yet)**
Arrives from a campaign or search with a single item in mind. Has no relationship with the brand and no reason to want an account. Goal: pay and leave in under two minutes.

## Journey Map

| Stage | Actions | Thoughts | Feelings | Pain points | Opportunities |
|-------|---------|----------|----------|-------------|---------------|
| Cart → checkout | Reviews items, taps checkout | "Almost done" | Confident | — | — |
| Account wall | Asked to register before paying | "Why do I need an account to buy socks?" | Irritated | Forced signup before value | REQ-1: email-only path |
| Payment | Enters payment details | "Is this safe?" | Cautious | — | Trust signals near pay button |
| Confirmation | Sees order confirmed | "Done." | Relieved | Prompt fatigue risk | REQ-2: invite after trust is earned |
| Post-purchase | Wants to check order status days later | "Where's my order?" | Anxious if lost | No account = no order page | REQ-3: email + order number lookup |

## Jobs to be Done
- **Functional:** complete a purchase quickly without creating credentials
- **Emotional:** feel safe giving payment details to an unfamiliar shop
- **Social:** — (none identified)

## Findings

### First-time buyers abandon at forced signup
**Evidence:** 62% funnel drop at the account step (analytics, BRD); weekly support complaints. **Severity:** High **Frequency:** 62% of first-time sessions

### Account prompts before value delivery feel coercive (provisional)
**Evidence:** none direct — inferred from drop-off pattern and industry research. Needs validation. **Severity:** Medium **Frequency:** unknown

## Insights
- The account wall taxes the moment of highest purchase intent; the account has value to us, not to the first-time buyer — sequence it after the purchase.

## Open Questions
- Does the collision case (guest email = existing account) confuse or reassure users?

## Research Gaps
- No qualitative data on why buyers abandon → proposed Spike: `Spike: Validate account-wall abandonment reasons` (see PROD-4 note in handover)

---

*Last updated: 2026-05-08*
