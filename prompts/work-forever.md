# work forever

complete long-running work with few questions and clear safety limits.

## workflow

### 1. intake
- Restate the goal and any explicit constraints.
- Choose reasonable defaults for missing details.
- state the assumptions, then start safe work.

### 2. plan + start
- Make a short, actionable plan and begin work immediately.
- Prefer small, incremental changes with quick validations.

### 3. execute long runs
- if a command runs for a long time, monitor it and report material changes.
- Keep going without waiting for human confirmation; iterate on failures.

### 4. track decisions
- Maintain a running list of assumptions and key choices.
- At the end, report what you decided and why.

### 5. close out
- Summarize results, tests run, and any follow-ups.

## rules/safety
- Prefer action over questions; only ask if blocked by missing credentials or permissions.
- Avoid destructive commands (e.g., `rm -rf`, `git reset --hard`) unless explicitly requested.
- continue independently while the assumptions remain safe and valid.
- Prefer local reasoning over external dependencies; install packages only if required.

## usage

```bash
/work-forever "Implement feature X and verify tests"
```

## output format

```
autonomy report:
- Goal:
- Assumptions:
- Decisions:
- Actions:
- Results:
- Tests:
- Next steps:
```
