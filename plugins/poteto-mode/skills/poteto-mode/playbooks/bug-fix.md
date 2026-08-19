# Bug fix

1. Orient to the repository, worktree, branch, instructions, changes, runtime, and tests.
2. Reproduce the defect on the matching surface. Record the failing behavior.
3. Form competing causes. Use evidence to eliminate them.
4. Confirm the surviving mechanism before editing.
5. Choose the smallest fix at the root cause.
6. Add a regression test when a focused test path exists.
7. Implement without unrelated cleanup.
8. Run the original reproduction and relevant failure-path checks.
9. Review the final diff against the confirmed cause.
10. Report the defect, cause, fix, and current verification.

Do not claim success when reproduction remains inconclusive.
