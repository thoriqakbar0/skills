# Refactoring

1. State the observable behavior that must remain unchanged.
2. Pin that behavior with a characterization test, snapshot, replay, or equivalence check.
3. Name the target structure and the reader-load reduction it should produce.
4. Remove dead code and redundant wrappers before adding a new abstraction.
5. Move in small steps while keeping the behavior pin green.
6. Migrate every caller and delete obsolete internal APIs in the same change.
7. Verify behavior on the real artifact.
8. Revert changes that add indirection without removing complexity.
9. Report the structural change, behavior pin, and equivalence evidence.

Route any intended behavior change to the feature or bug-fix playbook.
