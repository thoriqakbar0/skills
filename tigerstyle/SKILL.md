---
name: tigerstyle
description: Apply TigerBeetle's TigerStyle when explicitly requested or when a repository adopts it. Use for design, implementation, refactoring, and review of bounded, predictable systems code.
---

# TigerStyle

Prioritize safety, then performance, then developer experience. Use the simplest design that satisfies all three. Fix known correctness failures and unbounded work within scope before adding features.

## Start with the workload and invariants

- Trace the normal entry point and identify external inputs, state ownership, and expected failures.
- Name what must always hold and what work, memory, queue depth, batch size, or retries must be bounded. Identify where each limit is enforced and what happens at capacity. Give intentionally persistent loops bounded work per iteration.
- Prefer explicit control flow and small scopes. Avoid recursion in strict TigerStyle code. Keep branching and mutation with the state owner. Extract calculation into small, pure helpers when that makes the path easier to read.
- Handle malformed input, overload, and I/O failures as ordinary errors. Assert programmer invariants using the project's assertion mechanism. Check meaningful preconditions and postconditions, pair checks at independent boundaries where practical, and use compile-time checks for constant or layout relationships. Keep assertion failures specific.
- Distinguish indexes, counts, and byte sizes. Make units, overflow behavior, and division rounding explicit. Compute values close to where they are used and avoid duplicate mutable state.

## Design for predictable cost

- For performance-sensitive work, estimate bandwidth and latency across network, disk, memory, and CPU. Multiply per-operation cost by frequency to locate the bottleneck before optimizing.
- Batch expensive operations with explicit limits. Let external events enqueue work while the system controls when and how much it processes. Keep control decisions out of hot per-item loops when a batch can share them.
- Verify performance claims with the repo's benchmark or profiling path. Name the workload and the observed bottleneck.

## Match the repository

In TigerBeetle or a project that explicitly adopts strict TigerStyle, follow its local rules. TigerBeetle uses startup allocation with no dynamic allocation afterward, explicit-width integers for domain values, an average of at least two meaningful assertions per function, a 70-line function limit, a 100-column line limit, four-space indentation, snake_case for functions, variables, and files, and no dependencies beyond the Zig toolchain. Run `zig fmt` and the repository's checks. Put qualifiers and units after the domain noun, such as `latency_ms_max`. Use named options for ambiguous arguments and specify library options when defaults affect behavior.

Elsewhere, apply boundedness, explicit error handling, clear invariants, and predictable cost using the project's types, runtime, formatting, and dependency policy. Treat Zig-specific rules and numeric thresholds as TigerBeetle conventions unless the project adopts them.

## Verify the change

Exercise the normal path and the relevant boundary cases, such as empty input, exact capacity, over-capacity, and an expected operating failure. Include state or output checks when failures can occur after partial work. For a review, cite the affected code, explain the failure or cost, and recommend the smallest fix.

[TigerBeetle's TigerStyle guide](https://github.com/tigerbeetle/tigerbeetle/blob/main/docs/TIGER_STYLE.md)
