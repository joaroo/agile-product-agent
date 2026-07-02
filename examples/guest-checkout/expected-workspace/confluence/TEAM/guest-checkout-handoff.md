---
title: "Guest Checkout Handoff"
space: TEAM
parent: guest-checkout-lifecycle
owner: jonas
status: Active
last_reviewed: 2026-05-08
---

# Dev Handover: Guest Checkout

**Owner:** jonas  **Last reviewed:** 2026-05-08  **Status:** Active

## Carried Context
- **Initiative:** Guest Checkout · **Lifecycle Index:** [Guest Checkout Lifecycle](guest-checkout-lifecycle.md)
- **Stage:** Dev Handover · **Persona:** Engineering Lead
- **Upstream artifact(s):** [Guest Checkout Design Spec](guest-checkout-design-spec.md), [Guest Checkout Synthesis](guest-checkout-synthesis.md), [Guest Checkout Discovery Brief](guest-checkout-discovery-brief.md)
- **Problem / goal:** First-time buyers abandon checkout at the forced account wall (62% drop); enable guest purchase before Black Friday, target checkout completion +5pp.
- **Key decisions so far:** Shadow accounts keyed by email; invite never blocks confirmation
- **Open questions inherited:** Email collision behaviour (priya) — blocks part of PROD-2

---

## Epic
Guest Checkout Flow — PROD-1

## Stories & Tasks
See issue files: PROD-2 (guest checkout entry), PROD-3 (post-purchase invite), PROD-4 (guest order lookup). Full descriptions and BDD ACs live on the issues; this page summarises DoR and traceability only.

## Definition-of-Ready Summary

| Issue | DoR | Failing items |
|-------|-----|---------------|
| PROD-2 | ✗ | Open blocking dependency: email-collision decision (priya) |
| PROD-3 | ✓ | — |
| PROD-4 | ✓ | — |

## Traceability

| REQ-N | Story/Task | Spec coverage |
|-------|-----------|---------------|
| REQ-1 | PROD-2 | Design Spec — guest checkout entry, all states |
| REQ-2 | PROD-3 | Design Spec — post-purchase invite, all states |
| REQ-3 | PROD-4 | Design Spec — order lookup, all states |
| REQ-4 | PROD-2 (constraint in Out of Scope + ACs) | No loyalty-flow change |

---

*Last updated: 2026-05-08*
