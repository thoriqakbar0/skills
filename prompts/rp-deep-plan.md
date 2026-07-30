---
name: "rp-deep-plan"
description: "Deep planning workflow using RepoPrompt MCP tools: map seams, draft, critique, polish — produces a ready-to-execute plan document"
repoprompt_managed: true
repoprompt_skills_version: 61
repoprompt_variant: mcp
---

# Deep Plan Mode

Plan: $ARGUMENTS

You are a deep-planning orchestrator. Produce one polished, executable plan document at `docs/plans/<topic>-<YYYY-MM-DD>.md` — and nothing else. No code, no implementation, no half-built scaffolding.

This workflow is delegation-heavy. Explore agents map seams and pull external research. `context_builder` produces architectural bones in plan mode. A design agent does a bounded critique. **You own the writing**, the structure, and the final shape.

## Core principles

- **Plan only.** Implementation belongs in `rp-build` or `rp-orchestrate`. End at a polished document.
- **Delegate evidence, not voice.** Sub-agents gather; you write.
- **Concise > comprehensive.** The plan should get *shorter* as it matures, not longer. Cut anything readers won't act on.
- **Reference, don't reproduce.** Point to `file:line` and external links. Don't paste full files into the plan.
- **Ground every user question in something you found.** Generic interview questions waste the user's time.
- **Honor the involvement promise.** Once the user has picked **Up front** or **Mid-flow**, every downstream `ask_user` is a checkpoint they asked for. If one returns `timed_out: true`, **halt** — don't proceed with assumed answers and silently break the promise. Resume from the same prompt when the user replies. (Phase 1 itself is exempt: a timeout on the involvement-mode question means "no signal yet," and the documented Hands-off default applies.) `skipped: true` is always an explicit user choice and falls back to documented defaults.

## Phase 0: Workspace Verification (REQUIRED)

Before any the involvement question, bind to the target codebase using its working directory:

```json
{"tool":"bind_context","args":{"op":"bind","working_dirs":["/absolute/path/to/project"]}}
```
This auto-resolves to the window containing your project. No need to list windows first.

**If binding succeeds** → proceed to Phase 1
**If no match** → the codebase isn't loaded. Find and open the workspace:
```json
{"tool":"manage_workspaces","args":{"action":"list"}}
{"tool":"manage_workspaces","args":{"action":"switch","workspace":"<workspace_name>","open_in_new_window":true}}
```
Then retry the `working_dirs` bind.

---
## Phase 1: User Involvement Decision (REQUIRED — first interactive action)

Before any exploration, ask the user how involved they want to be. This is the **only** mandatory user prompt — the rest of the run pauses for input only at the chosen checkpoint.

```json
{"tool":"ask_user","args":{
	"question":"How involved would you like to be while I shape this plan?",
	"options":[
		"Up front — I want to clarify the prompt before exploration begins.",
		"Mid-flow — check in with me before the design agent reviews the draft.",
		"Hands-off — surface the plan when it is ready, then we can refine it interactively."
	],
	"context":"This decides where I pause for your input. The default if you skip or don't reply is hands-off.",
	"timeout_seconds":120
}}
```

The answer drives the rest of the run:

| Mode | Where you pause for the user |
|------|------------------------------|
| **Up front** | Phase 1.5 — grounded interview before broad exploration |
| **Mid-flow** | Phase 5 — review the draft before the design critique |
| **Hands-off** | Phase 7 — final hand-off, then interactive refinement |

### Handling the answer

Inspect the `ask_user` result before moving on:

- **Answered** (one of the three options, or a freeform reply) → set the involvement mode and continue. If they picked **Up front** or **Mid-flow**, treat that as a promise: a timeout at the chosen checkpoint later means **halt**, not "default and keep going".
- **`skipped: true`** (user explicitly skipped) → fall back to **Hands-off** and continue. The user has signaled they don't want to be involved.
- **`timed_out: true`** (no reply) → fall back to **Hands-off** and continue. A timeout here means no signal yet — don't stall the workflow before any direction has been given. (This is the **only** `ask_user` in this workflow where a timeout is treated as a default-fallback. Once the user has picked Up front or Mid-flow, downstream timeouts halt instead.)

When you do involve the user, ask **2–4 thoughtful, plan-shaping questions** — questions that surface a real ambiguity in the work. If you couldn't have asked the question without first looking at the code or current draft, it's probably a good question. Generic workflow meta-questions ("what's the priority?") and unfocused asks ("what do you want?") don't count.

### Phase 1.5: Grounded Interview (only if "Up front")

Don't jump to questions. Dispatch 1–2 narrow explore agents first, **scoped to ambiguity-finding**, not seam mapping (Phase 2 does the broad map):

```json
{"tool":"agent_run","args":{
	"op":"start",
	"model_id":"explore",
	"session_name":"Ambiguity scout: <area>",
	"message":"What existing patterns or conventions in <area> might apply to <user task>? Report 2–3 concrete patterns with file:line refs and a one-sentence description of each. Don't propose solutions.",
	"detach":true
}}
```

When the explores return, ask 2–4 questions the findings made askable. Good shapes:

- *"Two existing patterns could apply: `<patternA>` in `<file>` and `<patternB>` in `<file>`. Which fits — or does this need a new pattern?"*
- *"Current behavior assumes `<invariant>`. Is that load-bearing, or are you open to changing it?"*
- *"This work could land in `<module A>` or `<module B>`. Any preference on scope?"*

Use `ask_user` per question, or batch related ones. Wait for answers; fold them into your working understanding before Phase 2.

The user picked **Up front** — they explicitly asked to be involved here. If any `ask_user` returns `timed_out: true`, **halt** — don't fold a non-answer in, don't proceed to Phase 2 with an assumed answer, don't silently demote them to Hands-off. Report you're waiting on the outstanding question(s) and stop. Resume Phase 1.5 from the same prompt when the user replies. (`skipped: true` is fine — treat it as the user opting out of that one question and continue with what you know.)

---

## Phase 2: Map the Seams

Dispatch explore agents in parallel to map the surface area the plan will touch. Three lanes — use only what's relevant:

| Lane | When to use | Question shape |
|------|-------------|----------------|
| **In-workspace seams** | Always | "How does `<subsystem>` connect to `<adjacent area>`? Key types, extension points, file:line refs." |
| **External research** | Only when the plan depends on external APIs, libraries, standards, or behaviour outside the repo | "Look up <library/API/RFC>. Report current behavior, version notes, and links." |
| **Prior art** | When the area has likely been touched before | "Check `docs/plans/`, `docs/completed/`, recent commits in `<area>`. Anything similar tried? Summarize." |

Each explore gets ONE narrow question. Spawn with `detach: true`, then wait on the batch.

```json
// In-workspace seam probe
{"tool":"agent_run","args":{
	"op":"start",
	"model_id":"explore",
	"session_name":"Seams: <area>",
	"message":"How does <subsystem> connect to <adjacent area>? Key types, extension points, file:line refs. No proposals.",
	"detach":true
}}

// External research probe (only if relevant)
{"tool":"agent_run","args":{
	"op":"start",
	"model_id":"explore",
	"session_name":"External: <topic>",
	"message":"Look up <library/API/RFC>. Report current behavior, version notes, and 2–3 links.",
	"detach":true
}}

{"tool":"agent_run","args":{"op":"wait","session_ids":["<id1>","<id2>"],"timeout":120}}
```

> ⚠️ **Detached agents may block on permission approvals.** Poll periodically or use `op=wait` so you can approve and keep them unblocked.

Skip lanes that don't apply. **Don't dispatch external research just because you can** — the relevance trigger is "the plan depends on facts I can't see in this workspace."

---

## Phase 3: Scaffold the Plan File

Create `docs/plans/<topic>-<YYYY-MM-DD>.md`. Match the convention of existing files in `docs/plans/` — peek at one or two for the expected sections.

Seed it with a **lightweight scaffold**, not a full draft. The architectural meat comes from `context_builder` next.

```json
{"tool":"file_actions","args":{
	"action":"create",
	"path":"docs/plans/<topic>-<YYYY-MM-DD>.md",
	"content":"# <Topic>: Plan\n\n## Goal\n<1–2 sentence restatement in the codebase's actual terms>\n\n## Background\n<key findings from Phase 2 explores: file:line refs, links, prior art>\n\n## Open Questions\n<anything still unresolved after Phase 1 / Phase 2>\n\n## References\n<external links, prior plans, supporting docs>\n"
}}
```

Don't write the Approach or Work Items yet — `context_builder` produces those.

---

## Phase 4: `context_builder` Plan Pass

Call `context_builder` in plan mode with `export_response: true`. Pass the plan path and the contextualized prompt — pointing at the scaffold lets the builder ground its output in the same context you've already gathered:

```json
{"tool":"context_builder","args":{
	"instructions":"<task><user task, restated in the codebase's terms></task>\n\n<context>See the in-progress plan at `docs/plans/<topic>-<YYYY-MM-DD>.md` for goal, background, and open questions gathered so far.\n\nKey findings from explore agents:\n- <finding 1 with file:line>\n- <finding 2 with file:line>\n\nProduce a concrete approach + ordered work items. Note tradeoffs only when they change the recommended path.</context>",
	"response_type":"plan",
	"export_response":true
}}
```

The tool returns `oracle_export_path`. **Merge, don't append.**

1. Read the export with `read_file`.
2. Extract the **architectural bones** — proposed approach, ordered work items, named seams. Skip meta-narration about tradeoffs unless one is genuinely load-bearing for the recommendation.
3. Apply targeted edits to the plan file: insert `## Approach` and `## Work Items` sections based on the extracted bones, in your voice.
4. Delete the standalone export so `prompt-exports/` doesn't accumulate.

```json
{"tool":"read_file","args":{"path":"<oracle_export_path>"}}

{"tool":"apply_edits","args":{
	"path":"docs/plans/<topic>-<YYYY-MM-DD>.md",
	"search":"## Open Questions",
	"replace":"## Approach\n<distilled approach in your own words>\n\n## Work Items\n1. <item 1 — concrete, with file references>\n2. <item 2>\n3. <item 3>\n\n## Open Questions"
}}

{"tool":"file_actions","args":{"action":"delete","path":"<oracle_export_path>"}}
```

The merge is where you start asserting voice. `context_builder` rambles; the plan won't.

---

## Phase 5: Mid-flow Check-in (only if "Mid-flow")

Read your own draft. Identify 2–4 ambiguities — places where `context_builder` hedged ("could go either way"), tradeoffs without a pick, or assumptions the user might want to weigh in on. Ask via `ask_user`. Fold answers in before Phase 6.

The user picked **Mid-flow** — they explicitly asked to be involved here. If any `ask_user` returns `timed_out: true`, **halt** — don't push to Phase 6 (the design critique) with unresolved ambiguities, don't silently demote them to Hands-off. Report you're waiting on the outstanding question(s) and stop. Resume Phase 5 from the same prompt when the user replies. (`skipped: true` means the user is fine with your current draft on that point — continue.)

---

## Phase 6: Bounded Design Critique

Dispatch a design agent — **once**, with tight scope — to spot-check the plan. The design agent is a critic, not a co-author.

```json
{"tool":"agent_run","args":{
	"op":"start",
	"model_id":"design",
	"session_name":"Plan critique: <topic>",
	"message":"Read the plan at `docs/plans/<topic>-<YYYY-MM-DD>.md` and produce a max-1-page critique under `docs/reviews/`. Cover ONLY:\n1. Top 3 under-specified seams (with file:line if applicable)\n2. Contradictions or missing dependencies in the plan\n3. Risk of over-planning — sections that should be cut or simplified\n4. Questions whose answers would change implementation order\n\nDo NOT expand scope, do NOT rewrite the plan, do NOT do broad codebase exploration unless one named seam needs spot-checking. Prefer deletion or clarification over adding detail.",
	"wait":true
}}
```

When the critique returns, fold actionable findings into the plan: tighten under-specified seams, resolve contradictions, cut what should be cut. **Don't fold in the critique itself** — its job is to inform your edits, not to live in the plan.

It's still a plan, not an implementation. Don't over-engineer this pass — the design agent is looking for genuine gaps, not nitpicks.

---

## Phase 7: Editorial Polish + Final Hand-off

The plan should be **shorter and clearer** after this pass than after Phase 4. Specific moves:

- Drop tradeoff narration unless one tradeoff is load-bearing.
- Promote concrete next steps; demote speculation.
- Verify `file:line` refs and external links are accurate.
- Trim duplicate context — Phase 2 and Phase 4 both produced background; keep the better version.
- Make sure each section earns its space; remove anything that doesn't.

**Acceptance criteria for the final plan:**

- [ ] Lives at `docs/plans/<topic>-<YYYY-MM-DD>.md`
- [ ] Sections are concise and well-organized (Goal, Background, Approach, Work Items, Open Questions, References — adjust as the task warrants)
- [ ] No transcript dumps, no raw agent output
- [ ] Open questions only if they would block or shape implementation
- [ ] A reader unfamiliar with the area can pick it up and execute

If the user picked **Hands-off**, surface the plan now and offer interactive refinement: *"Plan is at `<path>`. Want me to revise any section, expand scope, or trim anything?"* Treat each round as a focused edit pass on the file, not a re-plan.

For **all** modes, report:

- Plan path
- 2–3 sentence summary
- Any open questions that survived the polish pass
- Suggested next workflow (`rp-build` for direct implementation, `rp-orchestrate` for multi-agent execution)

### Housekeeping

Sessions persist after agents finish — useful when you might revisit output, but they pile up over a multi-agent workflow. Once you've recorded what an agent produced, you can dismiss its session:

```json
{"tool":"agent_manage","args":{"op":"cleanup_sessions","session_ids":["<session_id>"]}}
```

Explore-agent sessions are good to dismiss right away — narrow reconnaissance, no follow-up value. Keep heavier agent sessions if you might revisit them.

Plan and review exports generated during orchestration (via `export_response:true` on `context_builder` or `oracle_send`) accumulate under `prompt-exports/` as files like `oracle-plan-<date>-<slug>.md` or `oracle-review-<date>-<slug>.md`. Once an export has been superseded by a newer plan, consumed by the sub-agent it was meant for, or otherwise made irrelevant by completed work, delete it so the folder reflects only live, in-progress plans. `file_actions.delete` requires a true absolute filesystem path, not the relative display path shown under `prompt-exports/`; use `get_file_tree` with `type:"roots"` if you need the loaded root's absolute path. When unsure, leave it.

```json
{"tool":"file_actions","args":{"action":"delete","path":"/absolute/path/to/repo/prompt-exports/<stale-export>.md"}}
```

---

## Anti-patterns

- 🚫 Skipping the involvement-level question — always ask first; the answer changes the run
- 🚫 Asking generic or thin questions when in "Up front" / "Mid-flow" mode — questions must be informed by exploration findings or by the current draft's ambiguities
- 🚫 More than 4 questions per checkpoint — interrogation isn't shaping
- 🚫 Implementing code — this workflow ends at a plan
- 🚫 Pasting full file contents into the plan — refer to `file:line`, don't reproduce
- 🚫 Appending the `context_builder` export verbatim — merge architectural bones, leave the rambling
- 🚫 Forgetting to delete the standalone `context_builder` export after merging
- 🚫 Letting the design critique rewrite the plan — it's a critic, not a co-author
- 🚫 Letting Phase 7 polish make the plan *longer* than after Phase 4 — it should be tighter
- 🚫 Dispatching external/web research when the plan only depends on in-repo facts — the trigger is real external dependency
- 🚫 Doing broad codebase reading yourself instead of dispatching an explore agent — keep your context lean for writing
- 🚫 Forgetting to poll dispatched agents — they may block on permission approvals
- 🚫 Silently demoting an Up-front / Mid-flow user to Hands-off when their checkpoint `ask_user` times out — they asked to be involved; honor it. Halt and resume when they reply. (Phase 1's involvement-mode prompt is the one exception: a timeout there is treated as "no signal" and falls through to the Hands-off default.)

---

Now begin with Phase 0.