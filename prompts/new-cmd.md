# new command

create a reusable command from the conversation or request.

## steps

1. **Detect context**
   - If history exists: auto-capture workflow into command
   - if no history exists, use the request.
   - Use thread context clues to infer name, description, and usage

2. Determine host (Codex, Claude, Cursor) from current runtime
   - report the selected host and installation path.

3. Check existing commands for style (host-specific)
```bash
ls ~/.codex/prompts/
ls ~/.claude/commands/
ls ~/.cursor/commands/
```

4. Propose the command name, description, usage, location, and key steps first
   - Proceed unless user rejects or corrects

5. Write command using concise format:
```markdown
# name

One-line description.

## steps
1. Step with `bash command`
2. Step with decisions

## usage
/{name} [args]
```

6. report the created file and exact invocation.

## flags

`--interview`: Ask detailed questions about purpose, triggers, inputs, outputs

## rules

- Default to capturing conversation if history exists
- Default host to current runtime and install there
- Ask at most one question, only if ambiguity blocks execution
- Infer everything else from context
