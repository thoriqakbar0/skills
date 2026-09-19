# Cleanup and refactoring

Use this reference before changing existing PBT or production structure.

## Inventory the affected area

Classify related tests as:

- a meaningful property to retain;
- a weak property to strengthen;
- an example that exposes a broader property;
- a regression example worth retaining;
- a duplicate with a proven replacement;
- a test needing a small observable boundary;
- a framework migration candidate.

Trace generators, helpers, fixtures, callers, dynamic loading, and CI commands before removal.

## Clean existing PBT

Repair vacuous generators, tautologies, shared-oracle logic, unreachable guards, weak properties, and unbounded sequences.

Migrate supported suites to Hegel when practical. Port the property, input domain, replay behavior, and failure semantics together.

Retain the existing test when Hegel lacks a required feature, fails to build, executes unreliably, or weakens a guarantee.

Remove a duplicate only after mapping its guarantee to a stronger replacement. Prove the replacement catches the same defect.

## Refactor for testability

Make small behavior-preserving changes when they expose a stronger property:

- extract a pure calculation from I/O;
- inject time, randomness, configuration, or another dependency;
- return observable state instead of hiding mutation;
- separate structured data from rendering;
- isolate a state transition from transport code.

Proceed when the change preserves public behavior and stays within the approved argument.

Pause before a public API change, broad restructuring, data migration, or new framework.

Fix confirmed production defects only when the user authorized implementation. Keep the minimized failing case.

Migrate callers before deleting replaced code. Check references, then remove obsolete helpers and compatibility paths.
