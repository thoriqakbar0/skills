# commit

create focused commits with conventional commit messages.

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

3. group related changes. use `feat|fix`, `test`, `docs`, then `refactor|chore` when applicable.
- Keep commits atomic
- Do not mix unrelated concerns
- Never `git add .`

4. stage and commit each group.
```bash
git add path/to/file1 path/to/file2
git commit -m "type(scope): short description"
```

5. report the commits and remaining changes.
```bash
git log --oneline -10
git diff --stat origin/$(git remote show origin | awk '/HEAD branch/ {print $NF}')...HEAD
```

Read if present: `AGENTS.md`, `CLAUDE.md`, `CONTRIBUTING.md`, `.gitmessage`.
