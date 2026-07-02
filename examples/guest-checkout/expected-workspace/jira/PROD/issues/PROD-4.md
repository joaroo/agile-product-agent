---
key: PROD-4
type: Story
summary: Let guests look up an order by email and order number
status: Backlog
priority: Medium
epic: PROD-1
story_points: 3
labels: [user-feedback]
assignee:
sprint:
created: 2026-05-08
updated: 2026-05-08
---

## Context
REQ-3. Guests have no account and therefore no order page; provide lookup via email + order number from the footer and the confirmation email. Design Spec (order lookup flow, all states): workspace/confluence/TEAM/guest-checkout-design-spec.md

## Acceptance Criteria
- [ ] **Happy path**: Given a guest with a confirmation email, when they submit the matching email and order number, then a read-only order detail view renders
- [ ] **Sad path**: Given a non-matching pair, when they submit, then the message "We couldn't find that order. Check the order number in your confirmation email." renders without revealing whether the email has an account
- [ ] **Edge case**: Given repeated failed lookups from one client, when the rate limit is hit, then further attempts are throttled to prevent order enumeration

## Out of Scope
- Order modification or cancellation from the lookup view

## Notes
Rate-limiting threshold to be set by engineering; must not leak account existence (see Design Spec edge cases).
