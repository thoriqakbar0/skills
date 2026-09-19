# Evaluation

Use this reference to review a property set or measure this skill.

## Review the property portfolio

Check the set separately from individual properties.

- Risk coverage. Every high-risk area has a property or a recorded reason.
- Balance. Easy code does not receive most testing effort.
- Reachability. Generators can construct every required state.
- Checkability. Each property has an observable, independent checker.
- Duplication. Overlapping properties have distinct value.
- Cost. Bounds fit local, CI, and exploratory budgets.
- Tool fit. Each property uses the tool that explores its state space.

Classify findings as gaps, judgment calls, or direct refinements. Fill gaps before declaring the set complete.

## Prove life

For every claimed defect, use a confirmed bug or a disposable mutation.

The property must fail for the intended reason. Revert the mutation and confirm the property passes.

For guarded properties, prove the guard and assertion both executed. For stateful tests, prove required transitions occurred.

## Evaluate the skill

Use repeated comparisons because agent runs vary.

First isolate test quality. Give agents finished programs with planted bugs and score which defects their tests expose.

Then measure complete work. Give agents implementation tasks and score hidden correctness with and without this skill.

Track defects found, weak tests accepted, false positives, token cost, elapsed time, and human interventions.

Use several bug families. Include ordering, boundaries, duplication, truncation, invalid-heavy generation, shared checkers, and state sequences.

Do not treat one successful run as evidence that the skill works.
