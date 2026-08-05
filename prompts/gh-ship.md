# ship

commit the selected changes, push the branch, and create a PR.

## steps

1. check the branch.
```bash
git branch --show-current
```
- If on `main`/`master`: stop and create a branch: `git checkout -b feat/short-desc`

2. inspect the changes.
```bash
git status --porcelain
git diff --stat
```
- If no changes, stop

3. stage and commit the selected files.
```bash
git add path/to/file1 path/to/file2
git commit -m "type(scope): short description"
```
- Never `git add .`
- One commit unless changes are clearly separate concerns

4. push the branch.
```bash
git push -u origin $(git branch --show-current)
```

5. create a PR when none exists.
```bash
gh pr view --json number 2>/dev/null
```
- If no PR: `gh pr create --title "type(scope): desc" --body "## Summary\n- changes"`
- If PR exists: report URL

Read if present: `AGENTS.md`, `CLAUDE.md`, `CONTRIBUTING.md`.
