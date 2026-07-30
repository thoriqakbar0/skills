# Codex Skills

A focused collection of reusable Codex skills, prompt commands, and shared agent instructions maintained by Thoriq.

## Quick start

You need Git and `rsync`.

Clone the repository:

```sh
git clone https://github.com/thoriqakbar0/skills.git
cd skills
```

Install a skill without replacing files that already exist:

```sh
mkdir -p "$HOME/.codex/skills/technical-documentation"
rsync -a --ignore-existing technical-documentation/ "$HOME/.codex/skills/technical-documentation/"
```

Replace `technical-documentation` with the skill you want to install. Keep the entire directory together because a skill may depend on its bundled references, assets, or agent definitions.

## Available skills

| Skill | Purpose |
| --- | --- |
| [`coding-standards`](./coding-standards/) | Apply correct-by-construction TypeScript engineering standards. |
| [`orchestrate`](./orchestrate/) | Coordinate focused agents on substantial, parallelizable work. |
| [`technical-documentation`](./technical-documentation/) | Build and review technical documentation, contributor guidance, and agent instruction files. |

Each skill is defined by a `SKILL.md` file in its directory.

## Prompt commands

The [`prompts`](./prompts/) directory contains reusable command-style prompts for GitHub workflows, testing, skill and command creation, interactive artifacts, autonomous work, and RepoPrompt workflows.

Install prompts without replacing existing local commands:

```sh
mkdir -p "$HOME/.codex/prompts"
rsync -a --ignore-existing prompts/ "$HOME/.codex/prompts/"
```

## Agent instructions

[`AGENTS.md`](./AGENTS.md) contains Thoriq's shared agent policy and response style. It is personalized and may conflict with project-specific instructions, so review and merge it instead of blindly replacing an existing file.
