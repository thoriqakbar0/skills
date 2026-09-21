# Test both directions

For every contract, state two kinds of property:

- **Accept**: what valid input must produce, such as a roundtrip, invariant, or agreement with a model or reference implementation.
- **Reject**: what invalid or hostile input must not do, such as being accepted, crashing, hanging, or corrupting state silently.

Parsers and validators often fail on inputs that are almost valid. Try the wrong family, out-of-range fields, leading zeros, trailing garbage, and misplaced separators when the contract has those boundaries.

Test documented claims exactly as written. An off-by-one result against the documentation is a bug even when the behavior looks reasonable.
