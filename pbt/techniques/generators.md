# Generators

Default to the full documented domain of every parameter the API exposes, including construction and configuration controls such as capacities, degrees, precisions, radii, and feature toggles.

The full domain includes hostile inputs when the contract covers them: empty input, control characters and NUL, extreme sizes and nesting, and invalid shapes alongside valid ones. Use invalid shapes only for properties that define rejection behavior.

A generator for a structured subset is often the sharpest tool. Construct members of the subset directly rather than filtering or clamping the full domain, and keep a full-domain property alongside it.

Bound resource use where values are materialized, including collection sizes, recursion depth, and operation sequences. Do not distort the drawn domain just to avoid a resource limit.
