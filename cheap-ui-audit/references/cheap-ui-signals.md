# Cheap UI Signals & Concrete Tailwind Fixes

Use this catalog during every `cheap-ui-audit` pass to diagnose high-leverage polish defects and prescribe exact replacements.

## 1. Layout Discipline

| Cheap Signal | Why It Looks Unpolished | Concrete Fix |
| :--- | :--- | :--- |
| **Card soup (boxes inside boxes)** | Nested bordered + shadowed cards add visual noise and shrink usable width. | Reserve borders/elevation for one clear container level; separate inner groups with spacing (`space-y-6`) or a subtle hairline divider (`divide-y divide-border/60`). |
| **Random spacing jumps** | Mixing `p-3`, `p-5`, `gap-2.5`, `mt-7` breaks visual rhythm and makes alignment feel accidental. | Lock to a consistent 4px/8px cadence (`gap-2` tight, `gap-4` group, `gap-6` section, `p-4`/`p-6` surface). |
| **Equal gaps everywhere** | When label-to-input gap equals field-to-field gap, grouping collapses. | Keep intra-group spacing tighter than inter-group spacing (`gap-1.5` label→field, `space-y-4` field→field, `space-y-8` section→section). |
| **Broken optical alignment** | Icons, badges, and text inside flex rows sit off-center or shift vertically. | Use `inline-flex items-center gap-2` with fixed icon boxes (`size-4 shrink-0`) and align text baselines (`items-baseline`) for mixed font sizes. |
| **Unbounded full-bleed forms/prose** | 1200px-wide inputs and paragraphs feel stretched and hard to scan. | Constrain reading/form containers (`max-w-2xl` forms/settings, `max-w-prose` body copy). |

## 2. Micro-Consistency

| Cheap Signal | Why It Looks Unpolished | Concrete Fix |
| :--- | :--- | :--- |
| **Mismatched nested radii** | A `rounded-xl` card containing `rounded-xl` inner blocks creates pinched corners. | Follow concentric radius math: `outer = inner + padding` (e.g., `rounded-xl p-1.5` outer → `rounded-lg` inner, or `rounded-lg p-1` → `rounded-md`). |
| **Competing primary buttons** | Two or three solid accent buttons in one toolbar/modal make the primary action unclear. | Allow **one** primary CTA (`bg-primary text-primary-foreground`) per section/modal; demote others to secondary/outline or ghost variants. |
| **Mismatched sibling control heights** | An input (`h-10`), select (`h-9`), and button (`py-2.5`) side by side look ragged. | Lock toolbar/form controls to one shared height token (`h-9` default, `h-8` compact). |
| **Inconsistent icon weights & sizes** | Mixing 14px/18px/22px icons or 1.5px and 2.5px strokes looks pieced together. | Standardize icon size per context (`size-4` inside buttons/inputs/menus, `size-5` standalone) and keep one icon family and stroke weight. |
| **Harsh/heavy borders & muddy shadows** | Thick `border-2`, high-contrast grey borders, or `shadow-xl` on flat cards scream template UI. | Use crisp 1px hairlines (`border border-border/70`) and subtle elevation only on floating layers (popovers, dialogs, dropdowns). |

## 3. Typography

| Cheap Signal | Why It Looks Unpolished | Concrete Fix |
| :--- | :--- | :--- |
| **Jumping numerals in tables/counters** | Proportional digits jitter horizontally when timers, prices, or counts update. | Apply `tabular-nums` to all metrics, prices, dates, tables, and live counters. |
| **Flat hierarchy (everything looks the same)** | Labels, values, titles, and helper text share nearly identical size and color. | Pair clear contrast: title (`text-sm font-medium text-foreground` or `text-base font-semibold tracking-tight`) + metadata (`text-xs text-muted-foreground`). |
| **ALL-CAPS tracking spam** | Turning every heading, button, and table header into `uppercase tracking-widest` makes the page shout. | Default to sentence case; reserve subtle uppercase (`text-[11px] font-medium uppercase tracking-wider text-muted-foreground`) sparingly for tiny section kickers. |
| **Loose display headings / tight body** | Large headings without tightened tracking look airy; cramped paragraphs are tiring to read. | Add `tracking-tight leading-tight` on headings (`text-xl+`) and `leading-relaxed` on multi-line body copy. |
| **Orphaned wrapping & awkward line breaks** | Short headings or modal descriptions wrap onto one stray word. | Apply `text-balance` on headings and `text-pretty` on short paragraphs/descriptions. |

## 4. Color Restraint

| Cheap Signal | Why It Looks Unpolished | Concrete Fix |
| :--- | :--- | :--- |
| **Translucent-bg + opaque-border badges** | Badges with a translucent tinted fill and a solid colored border (`bg-emerald-500/10 border border-emerald-500`, `bg-destructive/10 border border-destructive`) look dated and noisy. | Use a crisp **opaque background with no border** (`border-0`, e.g., `bg-muted text-foreground`, `bg-secondary text-secondary-foreground`, or solid semantic `bg-destructive text-destructive-foreground`). |
| **Rainbow status/tag explosion** | Five bright saturated pill colors on one row compete with the actual content. | Keep neutral tags muted (`bg-muted text-muted-foreground border-0`); reserve solid semantic badges strictly for status that requires action. |
| **Raw palette colors bypassing theme** | Hardcoded `bg-gray-100`, `text-slate-500`, `#fff`, or `bg-blue-600` break dark mode and drift across screens. | Replace with semantic theme tokens (`bg-background`, `bg-card`, `bg-muted`, `text-foreground`, `text-muted-foreground`, `border-border`, `bg-primary`). |
| **Gratuitous gradients & neon glows** | Decorative gradient text, glowing borders, or glass blur on utility screens reduce legibility. | Default to crisp flat surfaces with subtle contrast steps (`background` → `card` → `muted`) and let content carry the screen. |
| **Grey-on-grey low contrast** | Faint placeholder/label text fails WCAG AA and feels washed out. | Keep readable body/label contrast (`>= 4.5:1`), reserving low-emphasis tones for truly secondary metadata. |

## 5. Edge-Case States

| Cheap Signal | Why It Looks Unpolished | Concrete Fix |
| :--- | :--- | :--- |
| **Flex child overflow breakage** | Long titles, emails, or URLs blow out flex/grid columns and push action buttons off-screen. | Add `min-w-0` on the shrinking flex parent and `truncate` (or `break-words` / `line-clamp-2`) on the text node; put `shrink-0` on trailing buttons/badges. |
| **Blank or lifeless empty states** | An empty table or list shows a blank white void or bare "No data". | Render a compact centered empty state explaining what belongs there plus one clear next action (or clear-filter button). |
| **Layout-shifting loading spinners** | Replacing a whole section with a centered spinner causes jarring height jumps on load. | Use dimension-matched skeletons (`animate-pulse bg-muted rounded-md`) or preserve container height/aspect-ratio (`aspect-video`, fixed row height). |
| **Silent or raw-JSON error states** | Failed fetches either show nothing or dump raw stack/JSON strings. | Show an inline actionable error state (`text-destructive`) with a concise human message and a Retry action. |
| **Missing keyboard & disabled states** | Removing focus outlines (`outline-none` alone) or styling disabled buttons identically to active ones feels sloppy. | Pair `focus-visible:outline-none focus-visible:ring-2 focus-visible:ring-ring focus-visible:ring-offset-2` and clear disabled states (`disabled:pointer-events-none disabled:opacity-50`). |
