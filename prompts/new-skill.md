# new skill

create a reusable skill from the conversation or request.

## steps

1. **Detect context**
   - If history exists: auto-capture workflow into skill
   - if no history exists, use the request.
   - Use thread context clues to infer name, description, and triggers

2. Determine host (Codex, Claude, Cursor) from current runtime
   - report the selected host and installation path.

3. Propose the skill name, description, triggers, location, and whether scripts are needed
   - Proceed unless user rejects or corrects

4. Check existing skills for patterns (host-specific)
```bash
ls ~/.codex/skills/
ls ~/.claude/skills/
ls ~/.cursor/skills/
ls agents/skills/
```

5. Create skill structure:
```
{skill-name}/
  SKILL.md
  resources/         # references, samples, templates
  scripts/           # if tools requested
    run.py|ts|sh
```

6. Write SKILL.md:
```markdown
---
name: skill-name
description: one-line
allowed_tools: [list inferred from context]
---

# skill name

## when to use
- trigger conditions

## steps
1. ...
2. ...

## tools created
- scripts/run.py: what it does
```

7. report each created file and the invocation name.

## flags

`--interview`: Ask detailed questions (purpose, triggers, inputs, outputs, edge cases)

## script templates

**Python (uv):**
```python
def main() -> int:
    return 0

if __name__ == "__main__":
    raise SystemExit(main())
```

**TypeScript (bun):**
```typescript
console.log("ok");
```

**Shell:**
```bash
#!/usr/bin/env bash
set -euo pipefail
echo "ok"
```

## rules

- Default to capturing conversation if history exists
- Default host to current runtime and install there
- Ask at most one question, only if ambiguity blocks execution
- Only create scripts if requested
- Match existing skill patterns in repo
