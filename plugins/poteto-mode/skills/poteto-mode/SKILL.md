---
name: poteto-mode
description: Route non-trivial Codex work through a focused, evidence-driven playbook. Use when the user invokes poteto-mode or requests rigorous investigation, implementation, refactoring, verification, session pickup, or safe pause.
---

# Poteto mode

Choose one playbook before acting:

- Read-only explanation or recommendation: [investigation](playbooks/investigation.md).
- Defect diagnosis and correction: [bug fix](playbooks/bug-fix.md).
- New or changed behavior: [feature](playbooks/feature.md).
- Behavior-preserving structural change: [refactoring](playbooks/refactoring.md).
- Resume prior work: [session pickup](playbooks/session-pickup.md).
- Stop work cleanly: [pause safely](playbooks/pause-safely.md).

Read the selected playbook completely. Follow its ordered steps. Do not combine playbooks unless one explicitly hands work to another.

## Rules

1. Name the objective, observable result, constraints, and current evidence.
2. Inspect repository instructions, branch, worktree, changes, runtime, and relevant tests before code work.
3. Parse unknown input at boundaries. Name the data shape before adding stateful logic.
4. Prefer the smallest complete change. Do not add speculative abstractions or unrelated cleanup.
5. Reproduce defects before fixing them when practical.
6. Keep external actions within the user's explicit authority.
7. Treat commit, push, pull request, merge, deploy, release, and external messages as separate permissions.
8. Verify through the real behavior or artifact. Compilation alone is not proof.
9. Review delegated work and the final diff yourself.
10. Apply the `unslop` skill to every reply and written artifact.

Use Codex plans for multi-step work. Use collaboration agents only for independent, bounded work or when the user requests delegation.

## Attribution

Adapted from [pstack Poteto Mode](https://github.com/cursor/plugins/tree/main/pstack/skills/poteto-mode) by Lauren Tan.
