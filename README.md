# Codex skills

[![skills.sh](https://skills.sh/b/thoriqakbar0/skills)](https://skills.sh/thoriqakbar0/skills)

Reusable skills, prompts, and agent instructions for OpenAI Codex.

Use this repository to add focused workflows to Codex. Each skill includes its instructions and supporting files.

## Requirements

- Git
- `rsync`
- OpenAI Codex for Codex-specific skills and prompts

## Install one skill

use the skills CLI:

```sh
npx skills add thoriqakbar0/skills
```

select one or more skills when the CLI prompts you.

you can also install a skill manually.

Clone the repository:

```sh
git clone https://github.com/thoriqakbar0/skills.git
cd skills
```

Copy the skill directory into your Codex skills directory:

```sh
mkdir -p "$HOME/.codex/skills/technical-documentation"
rsync -a technical-documentation/ "$HOME/.codex/skills/technical-documentation/"
```

Replace `technical-documentation` with the skill name that you want.

Copy the full directory. A skill can use bundled agents, references, assets, or scripts.

Restart Codex after installation. Then invoke the skill by name, such as `$technical-documentation`.

## available skills

### [`coding-standards`](./coding-standards/)

use this skill for TypeScript implementation, design, review, and testing.

the skill applies correctness rules across data boundaries, domain models, errors, modules, asynchronous work, and side effects. it requires precise types, explicit resource ownership, safe telemetry, and tests through real system boundaries.

the skill treats every applicable rule as a proof obligation. it requires current verification or a clear report for each unverified claim.

invoke it with `$coding-standards`.

### [`orchestrate`](./orchestrate/)

use this skill for substantial work that can benefit from multiple agents.

the skill divides work into narrow assignments with separate ownership. it selects a suitable reasoning level for each agent and prevents overlapping work. the main agent combines the results and stays available to the user.

the skill keeps approvals with the user. do not use it for a small task that one agent can complete directly.

invoke it with `$orchestrate`.

### [`self-improve`](./self-improve/)

use this skill to improve Codex behavior from evidence in previous Codex sessions.

the skill reads the local Codex session index and rollout files. it can list sessions, show a transcript, find repeated corrections, and audit installed skills. it separates proposals for skills, project `AGENTS.md`, and global `~/.codex/AGENTS.md` instructions.

the skill only creates proposals by default. it cites the source sessions and does not edit instruction files without explicit approval. it requires the local Codex session system and is not portable to other agent runtimes without changes.

invoke it with `$self-improve`.

### [`technical-documentation`](./technical-documentation/)

use this skill to build or review documentation for humans and agents.

the skill inventories product documentation and governance files before it writes. it supports new evergreen documentation and updates to an existing documentation structure. it also checks `AGENTS.md`, `CONTRIBUTING.md`, aliases, links, commands, navigation, and multilingual parity when these items are in scope.

the skill produces updated documentation, validation notes, coverage details, and remaining gaps. it uses the AGPL-3.0-only license.

invoke it with `$technical-documentation`.

each skill starts with a `SKILL.md` file. read that file before installation to understand its behavior and requirements.

## Install prompts

The [`prompts`](./prompts/) directory contains reusable prompts for GitHub workflows, tests, design documents, skill creation, and autonomous work.

Copy all prompts into your Codex prompts directory:

```sh
mkdir -p "$HOME/.codex/prompts"
rsync -a prompts/ "$HOME/.codex/prompts/"
```

This command replaces local prompt files that have the same names. Review the changes before you copy customized prompts.

## Update an installation

Pull the latest repository changes:

```sh
git pull --ff-only
```

Run the applicable `rsync` command again. The command updates matching files but does not remove obsolete local files.

## Agent instructions

[`AGENTS.md`](./AGENTS.md) contains Thoriq's shared agent policy and response style.

This file is personal configuration. Review and merge its rules with your existing instructions. Do not replace project rules without review.

## Repository structure

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
