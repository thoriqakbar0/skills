# thoriq's codex skills and prompts

[![skills.sh](https://skills.sh/b/thoriqakbar0/skills)](https://skills.sh/thoriqakbar0/skills)

four reusable skills and 13 ready-to-run prompts for Codex.

use a skill for repeatable behavior across tasks. use a prompt for one focused workflow.

## choose the right format

| format | use it when | invoke it with |
| --- | --- | --- |
| skill | you need repeatable behavior across tasks | `$skill-name` |
| prompt | you need one focused workflow | `/prompts:prompt-name` |

## install the skills

install from the [`skills.sh` package page](https://skills.sh/thoriqakbar0/skills):

```sh
npx skills add thoriqakbar0/skills
```

select the skills and agents that you need. restart the agent after installation.

install one skill globally for Codex without prompts:

```sh
npx skills add thoriqakbar0/skills --skill coding-standards --agent codex --global --yes
```

invoke an installed skill by name:

```text
$coding-standards review this TypeScript change and fix the verified defects
```

## choose a skill

| skill | use it for |
| --- | --- |
| [`coding-standards`](#coding-standards) | TypeScript design, implementation, review, and tests |
| [`orchestrate`](#orchestrate) | substantial tasks with independent work for multiple agents |
| [`self-improve`](#self-improve) | improvements supported by evidence from previous Codex sessions |
| [`technical-documentation`](#technical-documentation) | documentation that people and agents can use without hidden context |

## what you get

- **one workflow per skill:** install only the behavior that you need.
- **required evidence:** Codex must run checks that match the risk.
- **clear boundaries:** each skill states its limits and approval points.
- **plain-text instructions:** inspect every instruction, script, and reference.
- **standard format:** each skill uses `SKILL.md`.

## coding-standards

[`coding-standards`](./coding-standards/) applies strict engineering rules to TypeScript changes.

use it for implementation, architecture, code review, refactoring, or test design. it checks the complete path from unknown input to observable behavior.

it guides Codex to:

- parse external data at system boundaries.
- make invalid states difficult to represent.
- model expected failures as typed values.
- keep domain logic separate from adapters and runtime code.
- give every side effect, promise, and resource a clear owner.
- make repeated write operations safe.
- prevent secrets from entering logs and errors.
- test behavior through real system boundaries.
- preserve strict TypeScript checks.

Codex must provide current evidence for each applicable rule. it must report any result that it cannot verify.

invoke it with `$coding-standards`.

## orchestrate

[`orchestrate`](./orchestrate/) divides substantial work across agents while you keep control.

use it when a task has independent research, implementation, or review work. skip it when one agent can complete the task directly.

it guides Codex to:

- divide work into narrow assignments.
- prevent agents from editing the same scope.
- select a suitable reasoning level for each assignment.
- keep the main agent available to you.
- combine agent results into one verified outcome.
- keep approvals and important decisions with you.

invoke it with `$orchestrate`.

## self-improve

[`self-improve`](./self-improve/) turns repeated corrections into evidence-backed instruction improvements.

use it when the same correction or workflow problem appears across multiple sessions.

it can:

- list local Codex sessions.
- render a session as a readable transcript.
- find repeated corrections across sessions.
- audit installed skills for missing guidance.
- separate proposals for skills, project instructions, and global instructions.
- cite the sessions that support each proposal.

the skill proposes changes first. it does not edit instruction files without your explicit approval.

this skill requires the local Codex session database and rollout files. it does not work unchanged in other agent systems.

invoke it with `$self-improve`.

## technical-documentation

[`technical-documentation`](./technical-documentation/) builds documentation that readers can follow and maintain.

use it for readmes, product documentation, contributor guides, agent instructions, or a full documentation review.

it guides Codex to:

- inventory documentation before writing.
- define the audience and reader outcome.
- preserve the current structure in an existing project.
- prefer durable wording for evergreen documentation.
- check commands, links, navigation, and referenced files.
- align `AGENTS.md`, `CONTRIBUTING.md`, and compatible instruction files.
- check multilingual parity when multiple languages exist.
- report validation results and remaining gaps.

this skill uses the `AGPL-3.0-only` license. see its [`SKILL.md`](./technical-documentation/SKILL.md) for source metadata.

invoke it with `$technical-documentation`.

## reusable prompts

the [`prompts` collection](./prompts/) provides ready-to-run workflows for GitHub work, tests, planning, refactoring, and long tasks.

use a prompt when you need a repeatable command without installing a complete skill.

### GitHub and delivery prompts

| prompt | use it to | what it does |
| --- | --- | --- |
| [`gh-address-pr-comments`](./prompts/gh-address-pr-comments.md) | resolve selected PR review comments | applies focused changes and reports unresolved comments |
| [`gh-commit`](./prompts/gh-commit.md) | divide local changes into focused commits | avoids unrelated files and never stages the complete worktree |
| [`gh-fix-ci`](./prompts/gh-fix-ci.md) | find and fix the first causal CI error | runs the smallest local check that covers the failure |
| [`gh-review-pr`](./prompts/gh-review-pr.md) | review a PR for correctness, tests, and material risk | separates required fixes, suggestions, and questions |
| [`gh-ship`](./prompts/gh-ship.md) | commit selected files, push a branch, and create a PR | stops on `main` or `master` and avoids `git add .` |

### Building and testing prompts

| prompt | use it to | what it does |
| --- | --- | --- |
| [`box-task`](./prompts/box-task.md) | send an improved execution brief to Codex in a new or named Ascii Box | preserves intent, adds acceptance checks, and records Playwright evidence for browser work |
| [`de-slop`](./prompts/de-slop.md) | find AI artifacts and unrelated cleanup before a PR | shows findings first and changes only the selected items |
| [`design-doc`](./prompts/design-doc.md) | turn an implementation idea into a reviewable design | produces goals, design, alternatives, open questions, and an implementation plan |
| [`html-artifact`](./prompts/html-artifact.md) | build a self-contained interactive HTML artifact | saves one browser-tested file with accessible controls and clear information hierarchy |
| [`make-tests`](./prompts/make-tests.md) | add behavior tests for the current change | covers success, boundary, failure, and regression paths |
| [`work-forever`](./prompts/work-forever.md) | continue long work with few questions | states assumptions, monitors long commands, and keeps destructive actions blocked |

`box-task` keeps the original request, then adds scope, assumptions, acceptance checks, and an evidence contract. Browser tasks record Playwright traces, videos, screenshots, logs, and a downloadable artifact manifest.

### Authoring prompts

| prompt | use it to | what it does |
| --- | --- | --- |
| [`new-cmd`](./prompts/new-cmd.md) | turn a conversation or request into a reusable command | matches the active agent runtime and reports the exact invocation |
| [`new-skill`](./prompts/new-skill.md) | turn a conversation or request into a reusable skill | creates only the files and scripts that the workflow needs |

copy all prompts into Codex:

```sh
mkdir -p "$HOME/.codex/prompts"
rsync -a prompts/ "$HOME/.codex/prompts/"
```

this command replaces prompt files with matching names. review local changes before you copy customized prompts.

invoke a copied prompt by name:

```text
/prompts:gh-review-pr 123
```

## manual installation

use manual installation when you do not want the skills CLI.

```sh
git clone https://github.com/thoriqakbar0/skills.git
cd skills
mkdir -p "$HOME/.codex/skills/technical-documentation"
rsync -a technical-documentation/ "$HOME/.codex/skills/technical-documentation/"
```

replace `technical-documentation` with the skill name that you want. copy the complete directory because a skill can use bundled resources.

## update a manual installation

pull the current repository version:

```sh
git pull --ff-only
```

run the applicable `rsync` command again. this updates matching files but does not remove obsolete files.

## shared agent instructions

[`AGENTS.md`](./AGENTS.md) contains Thoriq's shared agent policy and response style.

treat this file as personal configuration. review and merge its rules with your project instructions.

## repository map

```text
.
├── AGENTS.md                 shared agent instructions
├── README.md                 repository guide
├── coding-standards/         TypeScript engineering skill
├── orchestrate/              multi-agent coordination skill
├── prompts/                  reusable Codex prompts
├── self-improve/             session review skill and script
└── technical-documentation/  documentation skill and references
```

review each skill before use. a skill can direct an agent to run tools with your permissions.
