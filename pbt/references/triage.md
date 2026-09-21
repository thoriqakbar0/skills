# Counterexample triage

Use this reference when a property fails.

Use [techniques/triage.md](../techniques/triage.md) for the failure ledger and the evidence standard for findings.

## Capture the failure

Record the framework version, command, seed, replay token, minimized input, reached state, and environment.

Reproduce the minimized failure outside shrinking when practical.

## Classify before changing the test

- Implementation defect. An in-domain input violates a supported contract.
- Property defect. The assertion demands behavior the contract does not promise.
- Generator defect. The input violates a precondition or misses the intended state.
- Contract ambiguity. Available evidence supports competing behaviors.
- Environment defect. Setup, dependency, or runtime behavior invalidates the run.

Investigate shared mutable state when identical seeded runs depend on execution order.

## Respond

For implementation defects, preserve the failure and fix production only with implementation authority.

For property or generator defects, correct the model without excluding valid difficult inputs.

For ambiguous contracts, stop with the smallest counterexample and the competing interpretations.

For hangs, aborts, or exhaustion, keep a minimal skipped reproducer when an active test would block the suite.

After a confirmed bug, save the shrunk case. Improve the generator or property to search the surrounding bug family.
