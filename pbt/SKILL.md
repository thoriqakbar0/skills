---
name: pbt
description: Design, implement, clean, migrate, refactor, or debug property-based tests. Use for PBT, generated tests, structured fuzzing, shrinking, counterexamples, weak generators, or property-suite cleanup. Excludes binary fuzzers, benchmarks, static analysis, and exact-output tests.
---

# Property-based testing

Make every property earn trust against a plausible defect.

## Route the task

Use the project's test runner and package manager.

- Use Hypothesis for Python.
- Use Hegel for TypeScript, Rust, Go, C++, Java, and OCaml.
- Use Bombadil for interactive browser state.
- Use Antithesis for whole-system timing, concurrency, and fault behavior.

Load installed tool-specific skills when available. Otherwise inspect current package documentation and types.

After choosing a tool, read [references/api-references.md](references/api-references.md) for its exact API sources.

For a Hegel-supported language, migrate related PBTs to Hegel when practical. Preserve a test when migration loses a guarantee.

Treat missing features, build failures, and unreliable execution as concrete blockers. Record the exact blocker.

## Techniques

Load the focused guide that matches the current stage:

- [techniques/surfaces.md](techniques/surfaces.md) before choosing the API area to test.
- [techniques/directions.md](techniques/directions.md) before writing the property oracle.
- [techniques/generators.md](techniques/generators.md) before designing input strategies.
- [techniques/running.md](techniques/running.md) before setting case counts or time limits.
- [techniques/scale.md](techniques/scale.md) when size or complexity can hide defects.
- [techniques/triage.md](techniques/triage.md) when a property fails.

## Research before coding

Read [references/research.md](references/research.md). Find risky behavior before proposing a property.

Inspect the implementation, callers, tests, documentation, history, issues, incidents, specifications, and related repositories when relevant.

For each risky behavior, name likely mistakes and plausible alternative interpretations. Find an asymmetric or boundary input that separates them.

Treat external bug reports as leads. Validate a defect using code, logs, a reproduction, or its confirmed fix.

Use the installed `antithesis-research` skill for whole-system research. Use the bounded local scan for ordinary code.

## Stop at the test argument

Before editing tests or production code, present a test argument for approval. Include:

- the risky behavior and its evidence;
- each plausible defect;
- one witness that exposes each defect;
- the proposed property;
- the structured generator and states it must reach;
- the independent checker;
- the selected tool and run bound;
- related tests to retain, strengthen, migrate, or remove;
- any small refactor that would expose a stronger property.

An independent checker may be a trusted implementation, model, invariant, or relation. It must not repeat production logic.

Stop and ask when the contract has competing interpretations. Show the smallest discriminating example.

## Implement the approved argument

Read [references/design.md](references/design.md) before writing new properties.

Extend a sound existing test structure. Otherwise create a dedicated PBT module for generators, checkers, replay cases, and triage helpers.

Inspect related properties, generators, and helpers in the affected area. Read [references/cleanup-and-refactoring.md](references/cleanup-and-refactoring.md) before changing them.

Keep focused regression examples when they document a known defect or exact contract.

Generate interesting valid states directly. Include invalid inputs only when rejection behavior is part of the property.

## Prove the test works

Every claimed defect needs proof. Use a confirmed regression or a disposable mutation that introduces the defect.

Confirm the property fails on the defect and passes on correct code. Revert every disposable mutation completely.

Confirm guarded branches and required states were reached. A large case count cannot repair unreachable behavior.

Read [references/triage.md](references/triage.md) for failures. Preserve minimized failures and replay data.

Read [references/evaluation.md](references/evaluation.md) before judging a suite or the skill itself.

## Finish

Report commands, bounds, reached states, killed defects, minimized failures, migrations, refactors, removals, and remaining gaps.

Ordinary tasks keep evidence in the conversation. Long campaigns and Antithesis work keep durable property and triage records in the repository.
