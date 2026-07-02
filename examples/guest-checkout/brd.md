# BRD: Guest checkout (draft, v0.2)

Author: M. Larsen (Head of E-commerce) · 2026-05-04

Guests abandon checkout when forced to create an account. Support sees this weekly; analytics shows 62% of first-time buyers drop at the account wall. We need guest checkout before Black Friday.

Requirements (rough):

- Let shoppers complete a purchase with just an email address — no password, no account.
- After the order confirmation, invite them to save their details as an account (one click, don't nag).
- Order lookup for guests: they must be able to find their order later via email + order number.
- Must not break the existing loyalty-points flow for logged-in customers.

Out of scope for v1: social login, saved payment methods for guests.

---

## Appendix — meeting notes, checkout sync, 2026-05-06

Attendees: M. Larsen, priya (PM), jonas (eng), sofie (design)

- priya: biggest unknown is how many drop-offs are account-wall vs payment friction — analytics can't split them today. Jonas to check if the funnel event exists.
- jonas: guest orders need a customer record anyway — proposal: shadow accounts keyed by email, promoted to real accounts if the user opts in. Nobody objected.
- sofie: post-purchase account prompt must not block the confirmation page. Success copy TBD.
- M. Larsen: target is checkout completion +5pp for first-time buyers. Hard deadline: feature-complete two weeks before Black Friday.
- Open: what happens when a guest email matches an existing account? Parked — needs a decision.
