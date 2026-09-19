# Property and generator design

Use this reference after the test argument is approved.

## Choose a strong property

Prefer properties in this order when the contract supports them:

1. Stateful agreement with an independent model.
2. Differential agreement with a trusted implementation.
3. Roundtrip or inverse behavior.
4. Safety, progress, or structural invariants.
5. Metamorphic relations such as idempotence, monotonicity, or permutation preservation.
6. Bounds and cheap postconditions.
7. No crash.

No-crash properties fit unsafe boundaries and parsers. Pair them with acceptance or rejection checks when the contract distinguishes valid states.

Ask which plausible defect would fail the property. Replace the property when no concrete defect answers that question.

## Build the input space

Generate valid states by construction. Derive dependent values from earlier draws.

Exercise empty, singleton, duplicate, uneven, extreme, Unicode, malformed, and maximum supported shapes when they are in-domain.

Generate operation sequences from the current model state. Offer only valid actions unless invalid transitions belong to the contract.

Track required semantic states. Confirm the run reached them.

Bound collection sizes, operation sequences, retries, and case counts. Use deterministic seeds or framework replay data.

## Keep the checker independent

Do not call production helpers that implement the behavior under test.

Derive expected results from the contract, a smaller model, a trusted implementation, or an algebraic relation.

Use separate data paths when encoding and decoding could share the same defect.

## Use the Antithesis tool family

- Hypothesis owns Python properties and state machines.
- Hegel owns supported compiled and TypeScript properties.
- Bombadil owns browser actions, safety rules, and bounded progress properties.
- Antithesis owns faulted whole-system timelines and internal assertions.

Combine tools only when each explores a distinct state space.
