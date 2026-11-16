---
name: codehygiene
description: >-
  MUST USE whenever writing, editing, refactoring, or reviewing code in Python, TypeScript, JavaScript, Rust, Go, C, C++, Zig, Java, C#, or Lua. Enforces a mandatory 10 ms code-readability gate that catches what compilers and formatters miss: AI slop, narrator comments, swallowed errors, deep nesting, `*Factory`/`*Manager` ceremony, cryptic 1-2 char variable golf, magic-number indexing, and copy-pasted blocks. Run before finishing every coding turn—no exceptions.
---

# Code Hygiene Gate

Passing tests, typecheckers, and formatters (`ruff`, `eslint`, `prettier`, `rustfmt`, `gofmt`, `clang-format`) proves code executes—not that it is readable. Treat `codehygiene` as a mandatory build gate on every turn that writes or modifies source code. Never declare a coding task complete until touched files pass cleanly.

## Run It

The runtime (`__init__.py`, `__main__.py`) and `weights.npz` (`13 KiB` `1.58-bit` ternary model) live directly inside `<SKILL_DIR>` (the directory containing this `SKILL.md`) and run offline across macOS, Linux, and Windows:

```bash
python3 "<SKILL_DIR>" <touched-files> --check
```

- Environments where `python` is the active interpreter (virtualenvs, conda, Windows): `python "<SKILL_DIR>" <touched-files> --check`
- Environments without `numpy` preinstalled: `uv run --with numpy "<SKILL_DIR>/__init__.py" <touched-files> --check`
- Installed on `PATH`: `codehygiene <touched-files> --check`
- Structured output for batch fixes: pass `--json` (or pipe unsaved snippets via `stdin`).

Supported extensions (`11` languages): `.py`, `.ts`, `.tsx`, `.js`, `.jsx`, `.mjs`, `.cjs`, `.rs`, `.go`, `.c`, `.h`, `.cc`, `.cpp`, `.cxx`, `.hpp`, `.zig`, `.java`, `.cs`, `.lua`.

## Mandatory Workflow

1. **Implement & verify behavior:** Get tests, types, and syntax checks passing first.
2. **Run the gate:** Execute `codehygiene` with `--check` on **every** created or modified file in a supported language.
3. **Eliminate every squiggle:** Fix all lines flagged at `>= 0.55` probability, and clean up structural smells in any new file scoring `< 0.70`.
4. **Re-run after edits:** Fixing one smell (extracting helpers, renaming symbols) can introduce another if done sloppily. Repeat until `--check` exits `0` and tests remain green.

## How to Fix Each Squiggle

| Squiggle `kind` | Root Cause | Required Fix |
| :--- | :--- | :--- |
| `deep_nesting` | Pyramid control flow (`4+` indent levels) | Invert conditions into early `return` / `continue` guard clauses or extract a flat helper. |
| `noisy_comment` | Comment restates the next line's identifiers, `# Step 1` narration, `// ... rest of implementation` elision placeholders, `3+` lines of commented-out code, `TODO`, or lint-noise | Delete narrator comments, placeholder elisions, and commented-out code; keep comments only for non-obvious *why*, invariants, or constraints. |
| `swallowed_error` | Bare `except Exception: pass`/`return None`, empty `catch {}`, redundant runtime type guards (`typeof`/`Array.isArray`/`isinstance` + silent neutral return) on statically typed internal functions, silent `process.env`/`os.getenv` fallback chains or `localhost`/`dev-secret` defaults, `as any` / `as unknown` / `cast(Any, ...)`, `\|\| {}` / `or []`, or `# noqa` / `@ts-ignore` / `# pyright: ignore` / `// biome-ignore` | Catch specific errors, fail fast on missing config, trust internal static types, and fix root causes instead of casting or suppressing lints. |
| `poor_naming` | Enterprise ceremony (`*Factory`, `*Manager`, `*Helper`, `*Impl`), dead suffixes (`*_old`, `*_bak`, `*_backup`, `*_deprecated`, `*Deprecated`), 1–2 char variable walls (`xx = ...`), or raw magic indices (`buf[47]`) | Delete graveyard copies, use plain domain nouns/verbs (`>= 3` chars), and extract named constants for non-trivial indices or sizes. |
| `dense_line` | Chained `.map().filter().reduce()`, nested ternaries, or lines past `96`–`110` cols | Split into named intermediate steps and wrap long expressions cleanly. |
| `special_casing` | Repeated `if x == "a": return ...` literal ladders or scattered `typeof` / `isinstance` checks | Replace branch ladders with a dispatch table/map or normalize types at the boundary. |
| `duplicate_block` | Copy-pasted imperative blocks `8–31` lines apart | Extract shared logic into a parameterized helper or loop over data. |
| `cramped_layout` | Long walls of statements with zero blank-line separation | Separate distinct setup, transformation, and return phases with single blank lines. |

## No-Excuse Rules

- **Never skip because tests or linters passed.** Syntax linters do not detect AI slop, ceremony, swallowed errors, or variable golf.
- **Never skip for "small" edits, scripts, or test files.** Quick patches are where swallowed exceptions, narrator comments, and 2-letter variable golf appear most often.
- **Never claim the tool is unavailable.** The runtime and `13 KiB` weights live right inside `<SKILL_DIR>` and run offline in `~10 ms`.
- **Never game the score.** Do not shorten clear names into 2-letter abbreviations, cram multiple statements onto one line, strip real error handling, or raise `-t` / `--threshold` above `0.55` to force a `0` exit code.
- **Legacy files with pre-existing debt:** Zero squiggles are permitted in any file created in this task or in any function/region touched in this turn. When untouched lines far outside edit scope fail `--check`, verify via `--json` that zero squiggles remain in touched regions and report the untouched legacy line numbers explicitly.
- **Never eyeball instead of executing.** Run the command in this turn and verify the actual output before responding.
