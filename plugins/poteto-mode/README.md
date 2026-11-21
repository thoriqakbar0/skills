# Poteto Mode for Codex

This marketplace plugin packages Cursor pstack 0.15.46 through Michael Denyer’s
[pstack-claude Codex port](https://github.com/michael-denyer/pstack-claude/tree/97d2ba2790822939ee43db0d7856219ea96c450d)
(0.9.146). The marketplace version tracks Cursor pstack. The two source
commits are recorded in `UPSTREAM_COMMIT` and `CODEX_PORT_COMMIT`.

```sh
codex plugin marketplace add thoriqakbar0/skills
codex plugin add poteto-mode@thoriq-skills
```

Start a new Codex task and invoke `$poteto-mode`. The plugin includes its
Codex SessionStart hook and model-routing configuration. See the
[port’s configuration reference](https://github.com/michael-denyer/pstack-claude/blob/97d2ba2790822939ee43db0d7856219ea96c450d/docs/reference.md)
for setup and runtime details.

Run `./scripts/check-upstream.sh` to inspect changes in Cursor pstack since
the pinned revision. See [LICENSE](./LICENSE), [NOTICE.md](./NOTICE.md), and
[NOTICE-skills.md](./NOTICE-skills.md) for attribution.
