# BDD Standards

Reference for writing acceptance criteria and feature specifications in Gherkin-style BDD format. Used by the backlog-grooming, sprint-planning, and input-ingestion skills when drafting or reviewing issue ACs.

---

## Why BDD for Acceptance Criteria

Acceptance criteria written in Given/When/Then format are:
- Testable by definition — each criterion maps directly to a test case
- Unambiguous — concrete examples replace vague requirements
- Shared language — business, product, and engineering read the same thing
- Executable — can be automated directly with Cucumber, Pytest-BDD, etc.

---

## Gherkin Syntax

```gherkin
Feature: [Feature name — noun phrase]

  Background: (optional — shared preconditions for all scenarios)
    Given [shared state]

  Scenario: [Scenario name — describes the outcome, not the steps]
    Given [initial context — system state before action]
    When  [action taken by user or system]
    Then  [expected outcome — observable, verifiable]
    And   [additional outcome]
    But   [exception or negative case]
```

---

## Scenario Naming

Name scenarios by **outcome**, not procedure.

| Bad | Good |
|-----|------|
| `Test login` | `User with valid credentials accesses dashboard` |
| `Click reset button` | `Password reset email sent to registered address` |
| `Error handling` | `Invalid coupon code shows inline error without clearing cart` |

Pattern: `[Actor] [observable outcome] [condition if relevant]`

---

## Given / When / Then Rules

### Given — context
- Describes the **state of the system** before anything happens
- Not about user actions — use past tense or present state
- Include only what's relevant to this scenario

```gherkin
Given a registered user exists with email "user@example.com"
Given the user's cart contains 2 items totalling £45.00
Given the checkout page is open
```

### When — action
- Single action per `When` — if you need two, split into two scenarios
- Describes what the **user or system does**
- Concrete and specific

```gherkin
When the user enters an invalid coupon code "FAKE99"
When the user submits the checkout form
When the payment provider returns a timeout error
```

### Then — outcome
- **Observable and verifiable** — something visible in the UI, a DB state, an email sent
- Avoid implementation detail ("the API is called" is not an observable outcome)
- Multiple `And` lines are fine; each is a separate assertion

```gherkin
Then an inline error message reads "Coupon code not recognised"
And the cart total remains £45.00
And the coupon input field is cleared
And no order is created
```

---

## Scenario Types

### Happy path
The normal successful flow. Always write this first.

```gherkin
Scenario: Registered user completes checkout with valid payment
  Given a logged-in user with items in their cart
  When the user completes checkout with a valid card
  Then an order confirmation page is shown
  And a confirmation email is sent to the user's email address
  And the order appears in the user's order history
```

### Sad path / error handling
What happens when things go wrong.

```gherkin
Scenario: Payment declined at checkout
  Given a logged-in user with items in their cart
  When the user submits checkout with a card that is declined
  Then a payment failure message is shown
  And the cart is preserved
  And no order is created
```

### Edge case
Boundary conditions and less-common but valid states.

```gherkin
Scenario: User applies coupon that reduces order to £0
  Given a cart with one item costing £5.00
  When the user applies coupon "FREE5" (£5 off)
  Then the order total shows £0.00
  And the payment step is skipped
  And the order is confirmed without card entry
```

### Negative / security
Explicitly prohibited actions.

```gherkin
Scenario: Guest user cannot access order history
  Given an unauthenticated user
  When the user navigates to /orders
  Then they are redirected to the login page
  And a message reads "Sign in to view your orders"
```

---

## Mapping ACs to Jira

In a Jira issue description, write each scenario as a checklist item referencing BDD format:

```markdown
## Acceptance Criteria

- [ ] **Happy path**: Given a logged-in user with cart items, when checkout completes with valid card, then order confirmation shown + email sent
- [ ] **Declined card**: Given checkout submitted with declined card, then failure message shown, cart preserved, no order created
- [ ] **£0 order**: Given coupon reduces total to £0, then payment step skipped and order confirmed
- [ ] **Guest redirect**: Given unauthenticated user navigates to /orders, then redirected to login
```

For complex features, link to a Confluence spec page that contains the full Gherkin feature file.

---

## What Makes a Good AC

| Criterion | Bad example | Good example |
|-----------|-------------|--------------|
| **Testable** | "works correctly" | "shows error message 'Invalid email'" |
| **Specific** | "user is notified" | "confirmation email sent to user's registered address" |
| **Observable** | "system processes payment" | "order confirmation page shown with order number" |
| **Bounded** | "handles all errors" | "shows inline error for declined card; redirects to support for timeout" |
| **Single concern** | AC covers login AND checkout | Split into separate ACs per flow |

---

## Gherkin Anti-patterns

- **Conjunctive When**: `When the user enters email and clicks submit` — split into two steps or one specific action
- **UI implementation in Then**: `Then the submit button is disabled` — instead: `Then the form cannot be submitted`
- **Vague Given**: `Given the system is set up` — be specific about what state matters
- **Testing the test**: `Then the test passes` — meaningless
- **Scenario as procedure**: scenarios that read like test scripts rather than behaviour descriptions

---

## Feature File Placement

If the project generates or tracks feature files:

```
features/
├── checkout/
│   ├── payment.feature
│   ├── coupons.feature
│   └── guest-checkout.feature
├── auth/
│   ├── login.feature
│   └── password-reset.feature
└── README.md   # maps features to Jira epics
```

One feature file per functional area. One scenario per distinct behaviour.
