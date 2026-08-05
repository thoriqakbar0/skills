# review PR

review a GitHub PR for correctness, test coverage, and material risk.

## steps

1. Fetch basics + checks
```bash
gh pr view {PR} --json title,author,baseRefName,headRefName,additions,deletions,changedFiles -q .
gh pr checks {PR}
```

2. Check out and diff
```bash
gh pr checkout {PR}
BASE=$(gh pr view {PR} --json baseRefName -q .baseRefName)
git diff --stat "$BASE"...HEAD
git diff "$BASE"...HEAD
```

3. Review
- Summarize what changed and why
- Call out risky areas and missing tests
- Note style/maintainability issues only when they matter
- if the PR is large, state the reviewed scope and ask what to prioritize next.

4. Output
- Summary (purpose + risk)
- Must-fix items
- Should-fix items
- questions and follow-up work
