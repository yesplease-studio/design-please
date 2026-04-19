# Design Instructions: {{PROJECT_NAME}}

For: {{project owner}}
Tool: Claude Design
Source: claude-design-config.md, design-brief.md

---

## Pre-session checklist

Before opening Claude Design, have these ready:
- [ ] claude-design-config.md open (or the §1 block copied)
- [ ] Anchor product screenshots (2-3 per Look anchor, 1-2 per anti-anchor)
- [ ] {{Codebase / Figma / brand assets}} accessible
- [ ] A clear first artifact in mind (not "design the product" — "design the {{specific screen or asset}}")

---

## How to start the session

1. Paste the §1 system prompt block from claude-design-config.md
2. Upload the inputs from the §2 inputs manifest
3. Paste the §3 first-prompt template, filled in for the specific first artifact
4. Wait for Claude Design to declare typography, color, spacing, and motion in writing
5. Confirm or correct before letting it generate

---

## Prompt patterns that work for this project

### For a new screen or artifact
> "Generate {{artifact}} at {{dimensions / format}}. Honor the <frontend_aesthetics> block and the uploaded anchors. Show the planning step (typography, color, spacing, motion) before building."

### For tightening a generated artifact
> "The generated {{artifact}} drifted toward {{specific drift pattern — e.g., 'cream background']. Reset {{specific token}} to {{value from spec}}. Do not {{specific drift}}."

### For adding a component to the design system
> "Add {{component}} to the system. It should compose from {{existing atoms/molecules}}. Variants: {{list}}. States: {{list}}. Token bindings: {{list}}."

### For a pitch deck or one-pager
> "{{Number}}-slide {{deck type}} for {{audience}}. Narrative sequence: {{list}}. Honor the <frontend_aesthetics> block. Typography and color as declared. Not cream / serif / italic / terracotta."

---

## When to let Claude run vs. steer

**Let Claude run when:** direction is declared and you're generating a first pass. Watching the planning phase is steering enough.

**Steer explicitly when:**
- The planning declaration doesn't match the spec (catch early)
- Default drift appears (see §4 of the config's watchlist)
- A new dimension emerges that wasn't in the brief — loop back to `design-author` to extend the brief rather than ad-hoc'ing in chat

**Stop and re-ground when:**
- Three consecutive prompts have failed to move the output toward your intent
- The output is off in a way that's hard to describe — sketch on the design, or regenerate from the first-prompt template

---

## Default-drift watchlist

{{Pulled from §4 of claude-design-config.md — same patterns, restated for easy reference during a session.}}

---

## After the session

- Export the artifact(s) per the handoff target in the Design Brief
- Note any recurring drift patterns — these feed a config update
- If the output was materially off-brief, log it so the brief or config can be tightened next time
