# box task

start a Codex task in a new or explicitly named Ascii Box. improve the request before submission, require evidence, and return the Box reference.

## workflow

### 1. read the request

Read the task from `$ARGUMENTS`. if it is empty, ask for the task and stop.

Keep the original request verbatim for auditability. do not send it to Box without an execution brief.

### 2. build a bounded execution brief

Improve the request without changing its intent. include:

- **objective:** state the concrete outcome.
- **scope:** state the smallest useful in-scope result and any clear exclusions.
- **constraints:** preserve named tools, files, urls, formats, and prohibitions.
- **acceptance checks:** define observable completion conditions and suitable validation.
- **unknowns:** ask only when an unknown changes the target, creates material risk, or makes success unverifiable.
- **assumptions:** for other unknowns, tell Codex to inspect first and use the smallest reversible assumption.
- **deliverables:** require changed files, generated artifacts, validation results, assumptions, and blockers.

Do not invent credentials, product requirements, external facts, or adjacent work. do not turn a focused request into a broad refactor.

Put this instruction before the brief:

> Execute the bounded brief below. Preserve the user's intent and named constraints. Inspect before acting. Do not expand scope. Use the smallest reversible assumption for a non-blocking unknown and report it. Stop instead of guessing when the target, safety, credentials, or success criteria are unclear.

### 3. add browser evidence when applicable

Apply this section when the task changes or verifies a website, web interface, or browser flow.

- Prefer Playwright Test and the project's existing setup.
- If Playwright is absent, install temporary Box-only tooling outside tracked project files.
- Do not change project manifests unless browser tests are a requested deliverable.
- Test the changed user flow through real interactions. a screenshot alone is not a test.
- Create a unique run ID. store evidence under `~/box-task-artifacts/<box-id>/playwright/<run-id>/`.
- Use a temporary evidence config. set trace and video to `on`, and screenshot to `only-on-failure`.
- Set `outputDir` inside the run directory. set the HTML reporter's `outputFolder` to `report/` there and `open` to `never`.
- Capture named checkpoint and final-state screenshots.
- Install listeners before navigation. write console errors, page errors, `requestfailed` events, and HTTP 4xx/5xx responses to `observed-errors.jsonl`.
- Add a HAR only when network diagnosis requires it.
- Let Playwright Test close runner-owned fixtures.
- For a custom runner, start and stop tracing explicitly. create video-enabled contexts and close manual contexts in `finally`.
- Preserve the original test exit code after artifact collection. use a new run ID for each retry.
- Write `<run-dir>/manifest.json` before returning. include the run ID, UTC time, redacted command, status, exit code, and scenarios.
- List every artifact except `manifest.json`. include its kind, byte size, SHA-256, scenario, browser project, attempt, and POSIX path relative to the manifest directory.
- Use test credentials and data. never record login or secret-entry steps.
- Treat traces, videos, screenshots, and HAR files as sensitive. omit unsafe evidence or stop when safe recording is impossible.
- Pass secrets through environment variables. record variable names, never values.
- Do not commit recordings, temporary configs, or temporary scripts.

If recording is impossible, require the final response to state the exact blocker and the evidence that was safely collected.

### 4. verify Box and select a sandbox

Run `box status --json`. stop on a missing cli, timeout, nonzero exit, or invalid JSON. report the stage, exit code, and stderr. do not run locally.

Reuse a Box only when the request explicitly says to reuse one and supplies a valid Box ID. validate it with `box info <box-id> --json`, then skip creation.

A Box mention without an explicit reuse instruction does not authorize reuse. otherwise create a fresh Box with `box new --json`.

Parse and retain the Box ID. apply the same failure rules to `box info` and `box new`.

Replace `<box-id>` in the execution brief with the selected Box ID.

### 5. submit the improved task

Submit one multiline prompt. treat `$ARGUMENTS` as data. never use `eval`, shell interpolation, or an unquoted heredoc.

Include:

1. the execution instruction.
2. the original request under `## original request`.
3. the improved brief under `## execution brief`.
4. the browser evidence section only when applicable.
5. this final response contract:

> Report the result, changed files, validation commands and results, artifact manifest path when applicable, assumptions, and remaining blockers. Do not claim completion without current evidence.

Use an argv-safe call. this Python form passes the complete task as one argument:

```python
import subprocess

subprocess.run(
    [
        "box", "prompt", "--provider", "codex",
        "--model", "gpt-5.6-sol", "--reasoning-effort", "medium",
        "--json", box_id, enhanced_task,
    ],
    check=True,
    capture_output=True,
    text=True,
)
```

Parse the JSON response and retain the task or reference ID. success proves submission, not execution.

On a timeout, nonzero exit, or invalid JSON, report the stage, exit code, stderr, and selected Box ID.

### 6. report only evidenced state

Report:

- the Box ID.
- the task or reference ID.
- the one-line objective.
- the current evidenced state, such as `Box created` or `task submitted`.
- `box events -f <box-id>` for live events.
- `box desktop <box-id>` when browser work applies.
- the expected artifact path as pending until the final manifest exists.
- `box scp -r <box-id>:box-task-artifacts/<box-id> ./box-task-artifacts/<box-id>` after the manifest exists. the remote relative path starts from the Box home directory.

Do not say that the task is running or complete unless the response or events prove it.

## usage

```text
/prompts:box-task <task>
```
