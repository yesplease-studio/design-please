---
name: design-author
description: >
  Create and edit Definition-layer design system artifacts: design-brief.md and
  design-system-spec.md. This is the only skill authorized to write or modify
  these artifacts. Use this skill whenever you need to produce a new brief from
  a Discovery Brief, revise an existing brief, apply amendments proposed by
  design-refresh, or change the design system spec. Trigger when the user says
  things like "write the brief", "define the design direction", "build the design
  system", "update the brief", "add a new component to the spec", or "incorporate
  these changes." Always use this skill when design brief or system spec artifacts
  are the output. When the user lacks an opinion on a specific dimension, offer
  to choose, cite reasoning, and name alternatives considered — teach as you write.
system: design
integrations:
  required: []
  optional: []
---

# Skill: design-author

**Purpose:** Create and edit all Definition-layer design system artifacts. This is the only skill authorized to write or modify `design-brief.md` and `design-system-spec.md`.

---

## When to Use

- Starting a new design engagement after `design-discover` has produced a Discovery Brief (create mode).
- The design direction needs updating as the product evolves (edit mode).
- `design-refresh` has proposed amendments that need to be applied (edit mode).
- The user requests changes to the Design Brief or Design System Spec (edit mode).

---

## Two Modes

### Create Mode

Takes a Discovery Brief (or direct user input if no discovery was run) and produces the Design Brief and Design System Spec. Works conversationally in phases to ensure quality and alignment before writing.

### Edit Mode

Takes existing artifacts and applies targeted changes. Preserves unchanged sections. Never weakens specificity without explicit human instruction (don't replace "IBM Plex Sans" with "clean sans-serif", don't replace "Linear-style information hierarchy" with "clean hierarchy").

---

## The Teach-Through-Choice Pattern

When the user signals they don't have an opinion on a dimension (or when the Discovery Brief marks one as "user had no opinion"), the skill should:

1. Offer to make the choice: *"You didn't have a strong view on typography pairing. I can propose one. Want me to?"*
2. If accepted, name the choice: *"IBM Plex Sans for body at 16px / 1.5 line-height, Clash Display for headlines at a 3x jump (body 16 → H2 32 → H1 48)."*
3. Cite the reasoning, anchored to the design canon or an established pattern: *"IBM Plex Sans matches the 'technical but warm' register from your feel direction — it reads as serious without being cold. Clash Display is a distinctive display face that won't be mistaken for Inter/Roboto. The 3x size jump is from Anthropic's cookbook guidance on high-contrast typography pairing."*
4. Name 2-3 alternatives considered and why they were not chosen: *"I considered Geist (felt too 2024-startup-generic for your direction), Fraunces (serif — ruled out by your Claude Design override), and JetBrains Sans (considered; could work if you want more technical, less warm)."*

This pattern applies at every point where the user might defer. Record the choice, reasoning, and alternatives in the Design Brief so a future reader understands why each dimension was set as it was.

---

## Create Mode Workflow

### Phase 1: Gather and Assess Input

**If a Discovery Brief exists:** Read it as the primary source. It contains problem framing, existing assets audit, anchors, anti-anchors, default overrides, synthesis sentence, and dimension assessment. Use it as the foundation — do not re-interview what discovery already covered.

**If no Discovery Brief exists:** The user skipped `design-discover` (or ran it in a previous session). Collect available input:
- Product or project context (what is being designed)
- Reference anchors (products, cultural references)
- Anti-anchors
- Handoff target (Claude Design, Figma, human designer, dev)
- Existing assets (screenshots, URLs, codebase, Figma files)
- Brand or platform constraints

If critical gaps exist (no anchors at all, no problem framing), suggest running `design-discover` first rather than guessing.

**Assess what you have.** Before proceeding, determine:
- Is the problem defined clearly enough to scope the brief (Dim 1)?
- Are there specific anchors on all three axes of Look, Feel, Act (Dims 2-4)?
- Is a design system scope implied by the use case, or does it need to be chosen (Dim 5)?
- Is the handoff target named and are its input requirements met (Dim 6)?

---

### Phase 2: Fill Critical Gaps (if needed)

For each gap that blocks authoring, decide:

- **Ask the user.** Use `AskUserQuestion`; batch 2-4 questions per call.
- **Propose and confirm.** Where you can reasonably propose based on the problem framing and existing anchors, propose — and explain the reasoning, following the teach-through-choice pattern.

**Typical gaps and how to close them:**

| Gap | Close it by |
|-----|-------------|
| No anti-anchors | Ask; or infer from the anchor list and confirm |
| No default overrides (Claude Design handoff) | Walk through the defaults (cream, serif, italic, terracotta, Inter) and get explicit rule-outs |
| No design system scope chosen | Propose based on project type (see §6.3 of SYSTEM.md); confirm |
| No NN/g heuristics named | Propose 3-4 based on use case; confirm |
| No motion direction | Propose based on feel anchors; confirm |
| No density decision | Propose based on use case and look anchors; confirm |

---

### Phase 3: Outline and Confirm Before Writing

Before writing full artifacts, present an outline. This prevents wasted work and surfaces direction disagreements early.

**Design Brief outline:**
- Problem Definition (audience, scope, stage, constraints, success criteria — one-line each)
- Look Direction (typography, color, spacing/density, layout, imagery, iconography, anti-anchors — one-line each)
- Feel Direction (register, anchors, anti-anchors — one-line each)
- Act Direction (motion, interaction, density, intelligence, heuristics — one-line each)
- Design System Scope (proposed atomic level with rationale)
- Handoff Target (named with input requirements)

**Design System Spec outline:**
- Chosen atomic scope
- Token list (colors, type, spacing, radii, shadows, motion — count only)
- Atom list (if in scope — button, input, chip, etc. with variant counts)
- Molecule list (if in scope)
- Organism list (if in scope)
- Behavior notes

Present both outlines together. Ask: *"Before I write this out in full — is this the direction you expect? Anything missing, wrong, or too much?"*

Iterate on the outline until the user confirms. Only then write.

---

### Phase 4: Write the Design Brief

Produce `design-brief.md` from the approved outline. Use the template in `templates/design-brief.md` as the scaffold.

**Structure:**

```markdown
# Design Brief: [Project Name]

Date: YYYY-MM-DD
Handoff target: [Claude Design / Figma / human designer / dev]
Status: [draft / in review / approved]

---

## 1. Problem Definition

### Audience
[In the audience's own language, not as a persona. Pull verbatim quotes from the Discovery Brief.]

### Use case in scope
- [Flow, screen, or artifact 1]
- [Flow, screen, or artifact 2]

### Use case out of scope
- [Explicitly excluded]

### Stage
[Pre-launch / MVP / scaling / mature / rebrand]

### Constraints
- **Tech stack:** [named]
- **Accessibility:** [WCAG level, contrast, motion preferences]
- **Brand / existing system:** [what must be respected]
- **Performance:** [budgets, mobile considerations]
- **Other:** [anything else]

### Success criteria
[Testable — what "done" looks like to the audience]

---

## 2. Look Direction

### Typography
- **Body:** [Named family] at [size] / [line-height]
- **Headings:** [Named family] with [scale jump] (e.g., 3x)
- **Monospace (if used):** [Named family]
- **Pairing rationale:** [one sentence]

### Color strategy
- **Palette type:** [monochrome / monochrome + accent / duotone / full]
- **Primary palette:** [named colors with OKLCH values preferred]
- **Semantic colors:** [success, warning, error, info]
- **Dark mode:** [yes / no / derived]

### Spacing and density
- **Base unit:** [e.g., 4px or 8px]
- **Density level:** [compact / balanced / spacious]
- **Rationale:** [one sentence tied to use case]

### Layout
- **Primary pattern:** [centered / left-aligned / split / data-dense / card-based]
- **Grid:** [if applicable]

### Imagery
[Photographic / illustrative / geometric / none — specifics]

### Iconography
[Style, weight, source library if any]

### Anchors
- **[Anchor 1]:** [specific patterns carried from it]
- **[Anchor 2]:** [specific patterns carried from it]

### Anti-anchors
- **[Anti-anchor 1]:** [specific patterns to avoid]
- **[Anti-anchor 2]:** [specific patterns to avoid]

### Default overrides (Claude Design)
- Not cream / off-white backgrounds
- Not serif display type
- Not italic accents
- Not terracotta / amber
- Not Inter / Roboto / system defaults
- [Any project-specific additions]

---

## 3. Feel Direction

### Emotional register
[Specific, not "premium" or "friendly". "Dry confidence with understated warmth. Quiet. Competent without trying to prove it."]

### Anchors
- **[Product / cultural reference 1]:** [what it evokes and what patterns produce it]
- **[Product / cultural reference 2]:** [same]

### Anti-anchors
- [What register to avoid and why]

---

## 4. Act Direction

### Motion posture
[None / utility / expressive — specifics on when motion occurs]

### Interaction anchors
- **[Product behavior 1]:** [extracted pattern]
- **[Product behavior 2]:** [extracted pattern]

### Density expectations
[How much information per view, how the UI handles overflow]

### UI intelligence level
[Passive / suggestive / active / agentic — with specifics]

### Confirmation and reversibility
[Which actions confirm, which are reversible, which are both]

### Input modality
[Keyboard-first / pointer-first / touch-first / parity]

### NN/g heuristics that are load-bearing
- **[Heuristic 1]:** [why it matters for this use case]
- **[Heuristic 2]:** [why it matters]
- **[Heuristic 3]:** [why it matters]

---

## 5. Design System Scope

**Chosen atomic level:** [tokens only / tokens + atoms / tokens + atoms + molecules / full system]

**Rationale:** [why this level, not more or less — one paragraph]

**What is explicitly out of scope at this level:** [so the tool does not improvise]

---

## 6. Handoff Target

**Target:** [Claude Design / Figma / human designer / dev direct]

**Inputs the target will receive:**
- [Input 1 — e.g., this brief]
- [Input 2 — e.g., 3 screenshots of anchor products]
- [Input 3 — e.g., codebase path for token fidelity]

**First-use instruction:**
[One sentence describing how the target should be invoked on this brief — Claude Design config if applicable, or a pointer to design-encode output]

---

## 7. Teach-Through-Choice Log

For each dimension where the user had no opinion and the skill chose:

| Dimension | Choice | Reasoning | Alternatives considered |
|-----------|--------|-----------|------------------------|
| [Dim] | [what was chosen] | [why] | [what else; why not] |

---

## 8. Decisions log

| Date | Decision | Reasoning |
|------|----------|-----------|
| YYYY-MM-DD | [what was decided] | [why] |
```

Every dimension must land at a specific level or the brief is not done. If any dimension is still generic after writing, flag it and loop back.

---

### Phase 5: Write the Design System Spec

Produce `design-system-spec.md` at the scope chosen in the Design Brief. The spec is not a parallel output to Claude Design — it is the input spec Claude Design generates *from*.

Structure scales with scope. Only write the sections relevant to the chosen level.

```markdown
# Design System Spec: [Project Name]

Date: YYYY-MM-DD
Scope: [tokens only / tokens + atoms / tokens + atoms + molecules / full system]
Source brief: design-brief.md

---

## Tokens

### Color
- **Neutrals:** [OKLCH values, e.g., `--neutral-50: oklch(98% 0 0)`]
- **Primary / accent:** [OKLCH values]
- **Semantic:** success, warning, error, info
- **Surface / background:** [layered if needed]

### Typography
- **Families:** [named]
- **Scale:** [full scale with sizes, line-heights, letter-spacing]
- **Weights:** [used weights only]

### Spacing
- **Scale:** [e.g., 0, 4, 8, 12, 16, 24, 32, 48, 64]
- **Baseline grid:** [yes/no; size]

### Radii
[All used radii]

### Shadows
[All used elevations]

### Motion
- **Easing curves:** [named]
- **Durations:** [named buckets — fast, normal, slow]
- **Rules:** [when motion happens vs. doesn't]

---

## Atoms
_Include only if scope ≥ tokens + atoms._

### Button
- **Variants:** primary, secondary, ghost, danger
- **Sizes:** sm, md, lg
- **States:** default, hover, focus, active, disabled, loading
- **Token bindings:** [which tokens each variant uses]
- **Accessibility:** [keyboard, ARIA]

[Repeat structure for: Input, Textarea, Select, Checkbox, Radio, Switch, Chip, Tag, Badge, Tooltip, Icon]

---

## Molecules
_Include only if scope ≥ tokens + atoms + molecules._

### Card
- **Composition:** [which atoms it contains, in what arrangement]
- **Variants:** [if any]
- **States:** [if any]

[Repeat for: Form group, Toolbar, Navigation item, Modal, Drawer, Menu, List item, Table row, etc.]

---

## Organisms / Templates / Pages
_Include only if scope is full system._

### [Organism name — e.g., App shell, Dashboard layout, Data table]
[Composition rules, variants, state handling, when to use]

---

## Behavior notes

### Keyboard shortcuts
[If applicable]

### Interaction patterns
[Hover, focus, drag, select behaviors]

### State handling
[Loading, error, empty, disabled — consistent patterns]

### Accessibility
[Contrast, focus rings, keyboard nav, screen reader considerations]

---

## Out of scope

[Explicit list of what this spec does not cover. The handoff target should not improvise beyond this.]
```

---

### Phase 6: Present and Hand Off

Share both artifacts. Walk through:

- **The Design Brief** — is every dimension specific enough? Any generic language left?
- **The teach-through-choice log** — does the user agree with the choices made on their behalf?
- **The Design System Spec** — is the scope right? Anything to add or remove?
- **The decisions log** — anything to add?

If the handoff target is Claude Design: recommend `design-encode` next to produce the tool configuration.

If the handoff target is Figma or a human designer: the brief and spec may be enough; note that `design-encode` is optional and tool-specific.

If gaps or disagreements surfaced: loop back to Phase 4 for the brief or Phase 5 for the spec.

---

## Edit Mode Workflow

### Phase 1: Understand the Change

Read the existing artifact. Identify:
- What section is being changed
- What the current state says
- What the requested change is
- Whether the change is additive (new section), modificative (rewriting existing), or subtractive (removing)

### Phase 2: Check for Specificity Regression

Never replace a specific statement with a generic one without explicit instruction. If a user asks for "simpler language" in the brief, confirm the intent — simplifying is fine; de-specifying is not. A brief that says "IBM Plex Sans at 16px / 1.5" should not become "clean readable body font" without the user understanding what is being lost.

### Phase 3: Apply and Present the Diff

Apply the edit. Show the diff. Call out:
- What changed
- What upstream or downstream sections may need to be updated as a consequence
- Whether the change warrants regenerating `design-system-spec.md` or `claude-design-config.md`

---

## Quality Standards

A good Design Brief:

- **Lands every dimension at a specific level.** "Clean, modern, premium" is never the final word on any dimension. Every Look, Feel, Act parameter is named concretely — typefaces, colors with values, anchors with extracted patterns, heuristics with rationale.
- **Records the teach-through-choice log.** Every choice made on the user's behalf is named with its reasoning and alternatives considered. This is how the brief teaches.
- **States default overrides explicitly when Claude Design is the handoff.** The override section is not optional.
- **Scopes the design system honestly.** Most projects need tokens or tokens + atoms. If the spec claims full-system scope, the rationale must justify why.
- **Preserves verbatim user language.** Direct quotes from the user about their audience or their taste are kept unchanged in the Audience and Feel sections.
- **Is testable.** A reader who was not in the session should be able to open the brief, read it, and know what to make. If a section produces "it depends on what they mean," it is not done.

A good Design System Spec:

- **Is scoped to the chosen atomic level.** Does not reach higher; does not pretend to cover less. What is in scope is specified; what is out of scope is stated.
- **Uses concrete token values, not placeholders.** OKLCH colors, px or rem sizes, named curves. Not "a primary color" or "a base spacing unit."
- **Binds atoms and molecules to tokens, not to hex values.** Components reference the token system, so a token change cascades correctly.
- **States what is out of scope explicitly.** The handoff target should not improvise beyond the stated scope.
