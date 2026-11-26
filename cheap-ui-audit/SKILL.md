---
name: cheap-ui-audit
description: Audits frontend components, pages, and screenshots for visual signals that make a UI look cheap, generic, or unpolished across layout discipline, micro-consistency, typography, color restraint, and edge-case states, returning prioritized, concrete CSS/Tailwind fixes. Use when asked to audit UI quality, polish a component or screen, fix generic/amateur styling, or review frontend code or screenshots.
---

# Cheap UI Audit

Diagnose concrete visual and structural flaws that make an interface look cheap, noisy, or slapped together—and prescribe exact, high-leverage CSS/Tailwind fixes instead of vague design commentary.

Read `references/cheap-ui-signals.md` before auditing so every finding maps to a concrete signal and drop-in fix.

## When to Use

- Auditing a component, page, PR diff, or screenshot for visual polish
- Identifying why a working UI still looks generic, cluttered, or amateur
- Producing prioritized Tailwind/CSS cleanup passes before shipping a feature
- Checking edge-case UI states (empty, loading, error, long text, narrow viewports)

## Audit Workflow

1. **Inspect the real surface:** Read the component/page code (and inspect screenshots or live DOM if provided). Identify the primary user action, surface hierarchy, and design tokens already in use.
2. **Audit across all five dimensions** using `references/cheap-ui-signals.md`:
   - **Layout discipline:** Eliminate card-in-card nesting, inconsistent spacing rhythms, misaligned baselines, and cramped or floating whitespace.
   - **Micro-consistency:** Align control heights, nested border-radius math (`outer = inner + padding`), hairline borders, icon stroke/size parity, and button hierarchy (one primary action per region).
   - **Typography:** Tighten type scale and weight contrast, enforce `tabular-nums` on numbers/metrics/timestamps, fix line-height/measure, and eliminate noisy all-caps or low-contrast metadata clutter.
   - **Color restraint:** Replace raw palette colors, rainbow pill spam, translucent-bg + opaque-border badges (use opaque bg with no border instead), gratuitous gradients/glows, and muddy shadows with semantic tokens and calm neutral hierarchy (one accent doing real work).
   - **Edge-case states:** Verify long/unbroken strings (`min-w-0`, `truncate`, `break-words`), empty states, zero-CLS loading skeletons, error/retry states, disabled styling, and `focus-visible` keyboard rings.
3. **Prioritize by visual leverage:** Return the highest-impact fixes first (typically top 5–10 findings) rather than burying major structural issues under 20 micro-nits.
4. **Deliver concrete code fixes:** For each finding, include:
   - **Signal & Impact:** What looks cheap and why it hurts scannability or trust.
   - **Location:** Exact component/element or visual region.
   - **Fix:** Exact Tailwind classes or CSS/markup diff to apply (using the repo's semantic tokens when present).
5. **Apply when requested:** If the user asked to fix (not just audit), implement the highest-leverage improvements cleanly without breaking layout contracts or accessibility.

## Red Flags

- Giving vague advice like "improve visual hierarchy" or "make spacing cleaner" without exact classes or values
- Suggesting gratuitous gradients, glassmorphism, glow effects, or extra shadows to make a UI look "premium"
- Putting every section inside a bordered, shadowed card ("card soup")
- Styling badges with a translucent background plus a solid/opaque border (prefer an opaque background with no border)
- Using multiple competing primary CTA buttons in the same view or modal
- Auditing only the happy path while ignoring long names, zero items, loading flickers, or narrow widths

## Gotchas

- Restraint beats decoration: removing redundant borders, dividers, icons, and badges almost always raises perceived quality more than adding visual effects.
- Respect the project's existing design system and semantic tokens (`bg-background`, `text-muted-foreground`, `border-border`)—never introduce raw Tailwind palette colors (`gray-500`, `blue-600`) into a token-driven codebase.
- Never sacrifice contrast, hit-target size, or keyboard `focus-visible` clarity in the name of minimalism.
