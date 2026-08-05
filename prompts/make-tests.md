# make tests

add tests that prove the current change works.

## steps

1. Identify the code under test
```bash
git diff --name-only
```

2. propose three to six behavior tests across success, boundary, failure, and regression paths. confirm priorities.

3. Write tests following existing patterns (no excessive mocking).

4. Run the smallest relevant test command (then the full suite if needed).

5. report covered behavior and remaining risk.
