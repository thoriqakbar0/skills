# de-slop

remove AI artifacts and unrelated cleanup before a PR.

## checklist

- remove scratch notes unless they are maintained documentation.
- remove comments and docstrings that repeat the code.
- replace excessive mocks with behavior checks when practical.
- remove invented or uncited metrics.

## flow

1. list each finding with its file and line.
2. ask which findings to fix: `1 3 4`, `1-5`, `all`, or `none`.
3. apply the selected edits and summarize the result.

Safety: do not delete `README.md`, `CONTRIBUTING.md`, `CHANGELOG.md`, or `docs/**` without explicit confirmation.
