# fix CI

find the first failing check, fix its cause, and run the smallest relevant check again.

## steps

1. Identify failures
```bash
gh pr checks {PR} 2>/dev/null || true
gh run list --limit 10 2>/dev/null || true
```

2. open the logs or read the provided output. identify the first causal error.

3. fix the cause.
- Prefer minimal, correct changes
- Do not “fix” by skipping tests

4. run the narrowest local check that covers the failure. then report:
- what failed
- what changed
- how to re-run
