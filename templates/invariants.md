# Invariants

Statements true regardless of which feature is being built. Features propose; invariants veto.
Write the boring ones — they are cheap now and expensive to discover late.

## Identity & ownership
- [ ] Every <X> has exactly one <Y>.
- [ ]

## Data & money
- [ ] Money is stored as integer minor units. No floats, ever.
- [ ] All timestamps are UTC; conversion happens at the edge only.
- [ ]

## Access
- [ ] No request may read or write data outside the caller's <tenant/org/account>.
- [ ]

## Lifecycle
- [ ] Deletion is <soft / hard>. State which, and what happens to dependents.
- [ ]

## Failure
- [ ] <What must remain true when an external service is down.>
- [ ]

---
*Adding one: it belongs here only if violating it would be a bug in every feature, not just this one.*
