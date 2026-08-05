# box task

start a Codex task in a new Ascii Box and return the Box reference.

## steps

1. Read the task from `$ARGUMENTS`. if it is empty, ask for the task and stop.
2. Verify Box Code is ready with `box status --json`. If authentication or the API is unavailable, report the exact failure and stop; do not run the task locally.
3. Create a Box with `box new --json` and read the Box ID from the JSON response. Do not select an existing Box unless the task explicitly names one.
4. Spawn Codex with `box prompt --provider codex --model gpt-5.6-sol --reasoning-effort medium --json <box-id> "$ARGUMENTS"`. Preserve the task verbatim; do not broaden or rewrite it.
5. Report the Box ID, task reference, and this status command: `box events <box-id>`.

## usage

`/box-task <task>`
