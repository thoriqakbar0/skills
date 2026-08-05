# codex skills

[![skills.sh](https://skills.sh/b/thoriqakbar0/skills)](https://skills.sh/thoriqakbar0/skills)

make Codex more consistent at four jobs: building software, coordinating agents, learning from sessions, and writing documentation.

install only what you need. each skill gives Codex a focused workflow, clear limits, and required checks.

## start in 30 seconds

install from [`skills.sh`](https://skills.sh/thoriqakbar0/skills):

```sh
npx skills add thoriqakbar0/skills
```

select the skills and agents that you want. restart your agent after installation.

install one skill globally for Codex without prompts:

```sh
npx skills add thoriqakbar0/skills --skill coding-standards --agent codex --global --yes
```

invoke an installed skill by name:

```text
$coding-standards review this TypeScript change and fix the verified defects
```

## choose a skill

| skill | choose it when you need |
| --- | --- |
| [`coding-standards`](#coding-standards) | TypeScript code with explicit boundaries, failures, ownership, and verification |
| [`orchestrate`](#orchestrate) | multiple agents working on separate parts of one substantial task |
| [`self-improve`](#self-improve) | evidence-backed improvements from your previous Codex sessions |
| [`technical-documentation`](#technical-documentation) | documentation that people and agents can follow without hidden context |

## what makes these skills useful

- **focused:** you install only the workflows that you need.
- **evidence-based:** Codex must run checks that match the risk.
- **bounded:** each skill states its limits and approval points.
- **inspectable:** you can read every instruction, script, and reference.
- **open:** each skill uses the standard `SKILL.md` format.

## coding-standards

[`coding-standards`](./coding-standards/) makes TypeScript changes easier to trust and maintain.

use it for implementation, architecture, code review, refactoring, or test design. it checks the complete path from unknown input to observable behavior.

it guides Codex to:

- parse external data at system boundaries.
- make invalid states difficult to represent.
- model expected failures as typed values.
- keep domain logic separate from adapters and runtime code.
- give every side effect, promise, and resource a clear owner.
- make retried mutations safe.
- prevent secrets from entering logs and errors.
- test behavior through real system boundaries.
- preserve strict TypeScript checks.

Codex must provide current evidence for each applicable rule. it must report any result that it cannot verify.

invoke it with `$coding-standards`.

## orchestrate

[`orchestrate`](./orchestrate/) helps one Codex task coordinate multiple agents while you keep control.

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

[`self-improve`](./self-improve/) finds durable improvements in previous Codex sessions.

use it when repeated corrections or workflow problems should become better skills or instructions.

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

[`technical-documentation`](./technical-documentation/) builds documentation that is accurate, actionable, and maintainable.

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

the [`prompts`](./prompts/) directory contains prompts for GitHub work, tests, design documents, refactoring, and autonomous tasks.

copy all prompts into Codex:

```sh
mkdir -p "$HOME/.codex/prompts"
rsync -a prompts/ "$HOME/.codex/prompts/"
```

this command replaces prompt files with matching names. review local changes before you copy customized prompts.

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

review every skill before use. installed skills can direct an agent to run tools with your permissions.
