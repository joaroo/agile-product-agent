---
key: PROD-2
type: Story
summary: Allow guests to check out with only an email address
status: Backlog
priority: High
epic: PROD-1
story_points: 5
labels: [user-feedback, blocked]
assignee:
sprint:
created: 2026-05-08
updated: 2026-05-08
---

## Context
REQ-1. Replaces the account wall with an email-only guest path; shadow account created behind the scenes. Design Spec (guest checkout entry flow, all states): workspace/confluence/TEAM/guest-checkout-design-spec.md

## Acceptance Criteria
- [ ] **Happy path**: Given a shopper with items in the cart, when they enter a valid email and choose "Continue as guest", then they reach the payment step without creating credentials
- [ ] **Sad path**: Given an invalid email, when they attempt to continue, then the field shows "Enter a valid email so we can send your receipt." and focus returns to the field
- [ ] **Edge case**: Given a guest completes payment, when the order is created, then a shadow account keyed by the email exists and the loyalty flow for logged-in customers is unchanged (REQ-4)

## Out of Scope
- Email-collision behaviour (blocked on decision — see Notes)
- Social login

## Notes
BLOCKED: guest email matching an existing account — decision owned by priya. Fails DoR until resolved.
