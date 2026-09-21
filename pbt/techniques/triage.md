# Failures: triage and reporting

Keep a running ledger of every failing property. Each entry ends with one of two outcomes:

- a finding, reduced, root-caused in the implementation, and reported in the test result;
- a written note that the property, generator, contract, or environment was wrong, with the fix or remaining ambiguity.

A finding is not done until a standalone reproducer has been built or executed under the project's normal checks and has failed. A reproducer written from memory of the API is a guess. Verify every name and signature against the source.

Decide implementation defect versus test defect from evidence, never convenience. Never silently delete, weaken, or narrow a failing property.

When the property states an exact contract, such as an equality, ordering or hashing law, protocol rule, roundtrip, or documented endpoint, do not use tolerance to hide a disagreement. A floating-point limitation may explain the mechanism. It does not change the verdict.

Before loosening an oracle as approximate or underspecified, check the documentation, sibling implementations, downstream consumers, and changelog for the disputed behavior. A tolerance or generator exclusion added after a failure is a weakening. Record the evidence and the resulting finding or test correction.

Write the final report from a fresh full-suite run. Include every failure from that run. Cite the ledger note for any earlier failure that no longer appears.
