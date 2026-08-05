# Codex skills

Reusable skills, prompts, and agent instructions for OpenAI Codex.

Use this repository to add focused workflows to Codex. Each skill includes its instructions and supporting files.

## Requirements

- Git
- `rsync`
- OpenAI Codex for Codex-specific skills and prompts

## Install one skill

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

## Available skills

| Skill | Use it to |
| --- | --- |
| [`coding-standards`](./coding-standards/) | Apply strict TypeScript design and test standards. |
| [`orchestrate`](./orchestrate/) | Coordinate agents for large tasks that support parallel work. |
| [`self-improve`](./self-improve/) | Review Codex sessions and propose evidence-based instruction improvements. |
| [`technical-documentation`](./technical-documentation/) | Build and review documentation, contributor guidance, and agent instructions. |

Each skill starts with a `SKILL.md` file. Read that file before installation to understand its behavior and requirements.

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
