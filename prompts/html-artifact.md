# interactive HTML artifact

create a complete interactive HTML artifact from the request.

## usage

```text
/prompts:html-artifact "an interactive pricing calculator"
```

## steps

1. Read `$ARGUMENTS`, referenced files, and project conventions. ask one question only when a missing choice changes the result.

2. Load and apply these skills:
   - `/Users/thor/.codex/skills/executive-communication/SKILL.md` first, to turn the source into an answer-first information hierarchy with material evidence, implications, uncertainty, and an explicit decision or action.
   - `/Users/thor/.agents/skills/apple-design/SKILL.md` for direct manipulation, immediate feedback, interruptible motion, spatial consistency, materials, accessibility, and restraint.
   - `/Users/thor/.agents/skills/better-typography/SKILL.md` for the type scale, measure, hierarchy, wrapping, numeric stability, contrast, and text details. Read only the linked topic references needed for this artifact.
   - `/Users/thor/.agents/skills/animation-vocabulary/SKILL.md` only when the user describes motion vaguely or asks what an effect is called. Use its exact vocabulary; do not treat it as a motion-design guide.

3. Design the information before designing the interface:
   - Identify the audience and the artifact's job: decision, input, awareness, exploration, explanation, or action.
   - Extract only material facts. Separate evidence from interpretation and label uncertainty plainly.
   - Write the main takeaway as one sentence that can stand alone: what matters, why it matters, and what the reader should do or understand.
   - Support it with 2–4 points ordered by decision impact, then place detail, provenance, and secondary context beneath them.
   - Make the ask, next action, or “No action needed” explicit when applicable.
   - Draft a content outline before choosing cards, charts, tabs, animation, or layout.

4. Protect the information hierarchy in the default view:
   - The takeaway, essential evidence, current state, and required action must be visible without hovering, dragging, opening a modal, changing a tab, or discovering a gesture.
   - Interaction may compare, filter, simulate, inspect, or reveal supporting depth. It must not hide facts required to understand the conclusion.
   - Progressive disclosure is for secondary detail, methodology, and provenance—not the answer.
   - Visual prominence follows decision importance, not what is easiest to animate.

5. Use Apple design principles to express that information clearly. Design around one clear purpose and one obvious primary interaction. Keep controls close to what they affect, preserve user agency, maintain wayfinding, and make every state understandable without instructions.

6. Build a single HTML file with embedded CSS and JavaScript unless the user or existing project requires another structure. Avoid dependencies unless they materially improve the requested interaction. Use semantic HTML, keyboard-accessible controls, visible focus states, and responsive layout.

7. Make the artifact genuinely interactive:
   - Feedback begins on pointer-down and stays continuous during direct manipulation.
   - Gesture-driven motion tracks the pointer 1:1, respects the grab offset, and uses pointer capture.
   - Motion starts from the current visible state, remains interruptible, and hands off gesture velocity when relevant.
   - Use transform and opacity for frame-by-frame motion; avoid layout-thrashing animation.
   - Use critically damped motion by default. Add bounce only when momentum from a gesture justifies it.
   - Enter and exit along consistent paths, with origins anchored to their triggers.
   - Provide reduced-motion, reduced-transparency, and increased-contrast alternatives where relevant.

8. Treat typography as part of the information hierarchy:
   - Use a small semantic type scale, tight leading and slightly negative tracking for display text, and comfortable body leading.
   - Keep reading measure around 60–75 characters.
   - Balance headings, wrap descriptions deliberately, and use tabular numbers for changing values.
   - Keep mobile inputs at least 16px and regular text at accessible contrast.

9. Verify the actual artifact in a browser:
   - Exercise every control and state transition.
   - Check keyboard operation, narrow and wide layouts, overflow, and focus visibility.
   - Check reduced-motion behavior.
   - Confirm the main takeaway, evidence, and action remain understandable before using any interaction.
   - Confirm no essential information is available only through hover, animation, gesture, tabs, or modal state.
   - Fix console errors and obvious interaction or rendering defects.

10. Save the artifact in the current workspace unless the request gives a destination. report the path, takeaway, interaction, and browser checks.

## rules

- Build the artifact; do not stop at a mockup, design description, or code snippet.
- Content hierarchy comes before visual hierarchy; visual hierarchy must preserve it.
- Never trade comprehension for visual cleanliness. Add context when removing it would make the conclusion easier to misread.
- Never require interaction to discover the artifact's point.
- Prefer one coherent interaction over a dashboard of shallow features.
- Motion must orient, provide feedback, or preserve continuity; decorative motion does not earn its cost.
- Never lock input while an animation finishes.
- Do not invent metrics, content, or claims that appear factual. Use clearly illustrative data when real data is unavailable.
- Match an existing project's styling system instead of introducing a second one.
- Keep the result portable: one file, no build step, unless the task requires otherwise.
