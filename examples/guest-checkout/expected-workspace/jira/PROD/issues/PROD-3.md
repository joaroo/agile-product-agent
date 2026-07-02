---
key: PROD-3
type: Story
summary: Invite guests to save their details after purchase
status: Backlog
priority: High
epic: PROD-1
story_points: 3
labels: [user-feedback]
assignee:
sprint:
created: 2026-05-08
updated: 2026-05-08
---

## Context
REQ-2. One-tap account creation offered on the confirmation page, never blocking it. Design Spec (post-purchase invite flow, all states): workspace/confluence/TEAM/guest-checkout-design-spec.md

## Acceptance Criteria
- [ ] **Happy path**: Given a guest sees their order confirmation, when they tap "Save your details for next time", then an account is created from the shadow record and the card shows "You're set. We'll remember you next time."
- [ ] **Sad path**: Given account creation fails, when the error state renders, then the copy reassures the order is safe and points to the confirmation email — the confirmation page itself is unaffected
- [ ] **Edge case**: Given a guest dismisses the invite, when they complete another guest purchase later, then the invite appears again but never more than once per order

## Out of Scope
- Any modal or interstitial treatment of the invite

## Notes
Success metric: ≥20% invite acceptance (Discovery Brief).
