---
title: "Guest Checkout Lifecycle"
space: TEAM
parent:
owner: priya
status: Active
last_reviewed: 2026-05-08
---

# Guest Checkout Lifecycle

**Owner:** priya  **Last reviewed:** 2026-05-08  **Status:** Active

Guest checkout before Black Friday: remove the account wall for first-time buyers, invite account creation after purchase. All five stages complete; stories handed to engineering.

---

## Stage Status

| Stage | Status | Persona | Artifact |
|-------|--------|---------|----------|
| Ingest | Done | Business Analyst | [Guest Checkout Input Brief](guest-checkout-input-brief.md) |
| Discovery | Done | Product Manager | [Guest Checkout Discovery Brief](guest-checkout-discovery-brief.md) |
| UX | Done | UX Researcher | [Guest Checkout Synthesis](guest-checkout-synthesis.md) |
| Design | Done | Designer | [Guest Checkout Design Spec](guest-checkout-design-spec.md) |
| Dev Handover | Done | Engineering Lead | [Guest Checkout Handoff](guest-checkout-handoff.md) |

## Traceability

Source/BRD → Requirement IDs → Discovery → UX → Design → Epics/Stories

| Requirement ID | Source | Discovery theme | UX finding | Design coverage | Jira issue(s) |
|----------------|--------|-----------------|------------|-----------------|---------------|
| REQ-1 | BRD §Requirements (guest purchase, email only) | Account wall drop-off | First-time buyers abandon at forced signup | Guest checkout flow, all states | PROD-2 |
| REQ-2 | BRD §Requirements (post-purchase account prompt) | Convert guests after trust is earned | Prompts before value delivery feel coercive | Post-purchase prompt, non-blocking | PROD-3 |
| REQ-3 | BRD §Requirements (guest order lookup) | Guest self-service | Guests need order access without an account | Order lookup flow | PROD-4 |

---

*Last updated: 2026-05-08*
