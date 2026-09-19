# Risk research

Use this reference before proposing properties.

## Bounded local scan

Trace the target behavior through its callers and observable outputs. Then make focused passes for applicable risks:

1. Data flow, state ownership, persistence, and serialization boundaries.
2. Claimed safety, progress, ordering, and compatibility guarantees.
3. Boundary values, asymmetry, truncation, duplication, reversal, and aliasing.
4. Concurrency, lifecycle changes, replay, retries, and partial failure.
5. Resource limits, security boundaries, and external dependency behavior.
6. Existing tests, recent fixes, bug clusters, and suspiciously quiet code.
7. User impact and unproven assumptions such as "this cannot happen."
8. A wildcard pass for interactions or strange behavior missed above.

Skip an irrelevant pass and state why. Stop when another pass is unlikely to change the proposed property set.

## Evidence rules

Use specifications, types, public documentation, callers, and existing tests as contract evidence. Function names carry little weight.

Search relevant history, issues, incidents, and related repositories. Name the competing explanation before trusting an external claim.

A claimed guarantee becomes a property candidate. It does not become an established fact.

A reported bug becomes evidence only after primary material confirms the mechanism. Record uncertainty when evidence cannot settle it.

## Bug arguments

For each risk, write the likely mistake and an input where competing implementations differ.

Prefer witnesses with distinct values, uneven lengths, boundaries on both sides, and non-commuting operations. Symmetric inputs hide ordering defects.

Group risks that share a mechanism. Rank them by impact, likelihood, current coverage, reachability, and checker independence.
