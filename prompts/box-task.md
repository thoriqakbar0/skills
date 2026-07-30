# Box Task

Spawn a Codex task in a new Ascii Box and return its Box reference.

## Steps

1. Read the task from `$ARGUMENTS`. If it is empty, ask for the task and stop.
2. Verify Box Code is ready with `box status --json`. If authentication or the API is unavailable, report the exact failure and stop; do not run the task locally.
3. Create a Box with `box new --json` and read the Box ID from the JSON response. Do not select an existing Box unless the user explicitly names one.
4. Spawn Codex with `box prompt --provider codex --model gpt-5.6-sol --reasoning-effort medium --json <box-id> "$ARGUMENTS"`. Preserve the user's task verbatim; do not broaden or rewrite it.
5. Report the Box ID, the spawned task or prompt reference returned by Box Code, and the command to follow its work: `box events <box-id>`.

## Usage

`/box-task <task>`
