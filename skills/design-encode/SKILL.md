---
name: design-encode
description: >
  Translate the Design Brief and Design System Spec into operational artifacts for
  Claude Design (and other AI design tools). Produces claude-design-config.md
  (paste-ready system prompt + explicit default overrides + inputs manifest +
  first-prompt template) and design-instructions.md (how-to-use companion). This
  is the only skill authorized to write to the Encoding layer. Trigger when the
  user says things like "encode for Claude Design", "generate the Claude Design
  config", "give me the system prompt", "I'm about to open Claude Design — get me
  ready", or after design-author has produced or updated the brief. Skip this
  skill if no Definition-layer artifacts exist — run design-discover and
  design-author first.
system: design
integrations:
  required: []
  optional: []
---

# Skill: design-encode

**Purpose:** Translate the Definition-layer artifacts into machine-readable configurations that Claude Design (and similar AI design tools) can consume directly. This is the only skill that writes to the Encoding layer (`claude-design-config.md`, `design-instructions.md`).

---

## Why This Skill Exists

A Design Brief that lives in a markdown document is useful for humans but does not steer Claude Design. Claude Design has a strong default house style (cream backgrounds, serif display, italic accents, terracotta palette) drawn from Anthropic's cookbook. For most products — dashboards, dev tools, fintech, healthcare, enterprise apps — this default is actively wrong. Without an explicit override block, every prompt starts from that default.

`design-encode` closes this gap. It takes the structured artifacts from `design-author` and produces:

- A paste-ready `<frontend_aesthetics>`-style system prompt block
- An explicit, itemized default-override block (the single highest-leverage part of the config)
- An inputs manifest — which screenshots, codebase paths, Figma files, brand assets to upload and why
- A first-prompt template — a tested starter prompt that grounds the first Claude Design interaction
- A human-readable instructions document — how to use Claude Design on this project day-to-day

It follows Anthropic's three cookbook strategies for frontend aesthetics:

1. **Guide specific design dimensions individually** (typography, color, motion, spacing — never collapsed into one adjective)
2. **Reference design inspirations, not abstractions** (named products and cultural movements, not "modern and clean")
3. **Explicitly call out defaults to avoid** (cream, serif, italic, terracotta, Inter, purple gradients)

---

## When to Use

- After `design-author` has produced or updated the Design Brief
- When the user is about to open Claude Design for the first time on this project
- When the user has been prompting Claude Design and keeps getting house-style output ("cream backgrounds again")
- When the brief has been refreshed and the tool config needs updating
- When adding a new handoff target (e.g., Vercel v0, Figma AI) that needs its own encoded config

## Prerequisites

At minimum, these artifacts must exist in the project:
- `design-brief.md` (problem, Look/Feel/Act direction, anti-anchors, default overrides)

Strongly recommended:
- `design-system-spec.md` (tokens, atoms, etc. — makes the config far more precise)

If the prerequisites don't exist, tell the user and suggest running `design-discover` and `design-author` first.

---

## Workflow

### Step 1: Read the Definition Artifacts

Load from the project:

1. `design-brief.md` — problem framing, Look/Feel/Act direction with anchors and anti-anchors, handoff target, default overrides
2. `design-system-spec.md` — tokens and components at the chosen atomic scope

Assess completeness. Call out any section that is generic ("clean sans-serif" rather than a named family) and recommend a loop back to `design-author` before encoding — encoding an incomplete brief just writes confusion into a system prompt.

---

### Step 2: Determine Target Tool and Output Format

Ask the user via `AskUserQuestion`:

- **Which tool is the target?** (Claude Design is the V1 default. Other tools — Vercel v0, Lovable, Figma AI — can be added but use the Claude Design block as scaffold.)
- **Which output format do you prefer?** The user chooses one of:
  - **Instructions file only** (`design-instructions.md`) — for users who want context and will craft their own prompts
  - **Paste-ready block only** (`claude-design-config.md` with the system prompt block as the primary content) — for users who want to copy-paste and go
  - **Both** (default recommendation) — instructions for the user + paste-ready block at the top of the config file

Default to **both** if the user is unsure. They cover the widest range of use cases.

---

### Step 3: Generate the Claude Design Configuration

Produce `claude-design-config.md` using the template from `templates/claude-design-config.md`.

**Translation principles:**

- **Specificity beats abstraction.** Pull exact values from `design-system-spec.md`. The config should name typefaces, OKLCH color values, spacing units, density level, motion curves — never categories.
- **Override defaults first.** The override block is the single most load-bearing part of the config. Put it early (before positive direction) and make it itemized.
- **Use Anthropic's `<frontend_aesthetics>` structure.** The cookbook provides a paste-ready skeleton — honor it. Fill each field from the brief.
- **Reference anchors by name with screenshot direction.** Name the anchor product. Tell the user which screenshots to upload. Anchor-by-name is not a first-class Claude Design feature, so screenshots do the heavy lifting.

**Structure:**

```markdown
# Claude Design Configuration: [Project Name]

Date: YYYY-MM-DD
Source: design-brief.md, design-system-spec.md
Target tool: Claude Design

---

## How to use this file

Paste the block in §1 at the start of every Claude Design session for this project. Upload the files listed in §2 to the session. Use the first-prompt in §3 as your opening. Iterate from there using the guidance in §5.

---

## 1. System prompt block (paste-ready)

```xml
<frontend_aesthetics>

<audience_and_context>
[From design-brief Problem Definition. One paragraph — audience, stage, use case.]
</audience_and_context>

<defaults_to_avoid>
This project is [kind of product]. Do NOT use the default aesthetic. Specifically:
- NOT cream / off-white backgrounds (~#F4F1EA)
- NOT serif display type (no Georgia, Fraunces, Playfair)
- NOT italic accents
- NOT terracotta / amber palette
- NOT Inter, Roboto, Open Sans, Lato, or system default fonts
- NOT purple gradients on white
- [Project-specific additions from design-brief default-override section]
</defaults_to_avoid>

<typography>
- Body: [Named family] at [size] / [line-height]
- Headings: [Named family] with [scale jump]
- Monospace (if used): [Named family]
- Pairing rationale: [one sentence]
- Weights used: [list]
</typography>

<color>
- Palette strategy: [monochrome / monochrome + accent / duotone / full]
- Primary / neutrals / accent: [OKLCH values from design-system-spec]
- Semantic: success, warning, error, info — [OKLCH]
- Dark mode: [yes / no / derived]
</color>

<spacing_and_density>
- Base unit: [e.g., 8px]
- Density: [compact / balanced / spacious]
- Grid: [if applicable]
</spacing_and_density>

<layout>
- Primary pattern: [named — centered / left / split / data-dense / card]
- Grid: [specifics]
</layout>

<imagery>
[Photographic / illustrative / geometric / none — specifics. If none: state it.]
</imagery>

<iconography>
[Style, weight, source library]
</iconography>

<motion>
- Posture: [none / utility / expressive]
- Easing: [named curves]
- When motion happens: [e.g., only on user-initiated state changes; no ambient motion]
</motion>

<inspirations>
The product should draw from these anchors:
- [Anchor 1] — specifically: [extracted patterns]
- [Anchor 2] — specifically: [extracted patterns]
- [Cultural reference] — specifically: [extracted patterns]

Feel: [one sentence of emotional register — "dry confident warmth, quiet competence"]
Act: [one sentence of behavioral posture — "keyboard-first, fast, opinionated defaults, reversible"]
</inspirations>

<anti_inspirations>
Avoid the patterns of these products / styles:
- [Anti-anchor 1] — specifically: [patterns to avoid]
- [Anti-anchor 2] — specifically: [patterns to avoid]
</anti_inspirations>

<interaction_and_behavior>
- Input modality: [keyboard-first / pointer-first / touch-first]
- Density expectations: [specifics]
- UI intelligence: [passive / suggestive / active / agentic]
- Confirmation pattern: [when confirmations happen, when actions are reversible]
- NN/g heuristics to honor: [named list]
</interaction_and_behavior>

<design_system_scope>
This project uses a [tokens only / tokens + atoms / tokens + atoms + molecules / full system] design system. See design-system-spec.md for the full specification. Do not improvise components beyond the stated scope.
</design_system_scope>

</frontend_aesthetics>
```

---

## 2. Inputs manifest

Upload these at the start of the session:

| Input | Purpose | Source |
|-------|---------|--------|
| This config file | Aesthetic direction | Paste the §1 block or attach the file |
| design-system-spec.md | Token and component truth | Attach |
| [Anchor 1] screenshots (2-3) | Visual language for Look | [how to produce or where to find] |
| [Anchor 2] screenshots (2-3) | Visual language for Look | [how to produce or where to find] |
| [Anti-anchor] screenshots (1-2) | Boundary examples for "do not do this" | [how to produce] |
| [Codebase path or Figma file, if applicable] | Token fidelity | [path / link] |
| Brand assets (logo, wordmark) | If applicable | [path] |

Input hierarchy for Claude Design (most to least fidelity):
1. Codebase with named tokens (Tailwind config, CSS variables)
2. Figma files with named styles and variables
3. Screenshots with explicit descriptions

Use the highest-fidelity input available for this project.

---

## 3. First-prompt template

Start the Claude Design session with this prompt (paste after the §1 block):

```
I'm designing [scope — e.g., "a dashboard for the Query Performance page of a PostgreSQL observability tool"].

Audience: [from brief]
Stage: [from brief]
What I need first: [specific first artifact — e.g., "a wireframe of the Query Detail screen at 1440px, showing the query plan, execution metrics, and historical runs"]

Use the aesthetic direction in the <frontend_aesthetics> block above. Reference the uploaded screenshots of [anchor products] for visual language. Do not use the default Claude Design aesthetic.

Before you start, state in writing: (a) the typography you'll use (families, sizes), (b) the color palette (OKLCH values), (c) the spacing unit, (d) the motion posture. I'll confirm or correct, then you can generate.
```

The "state in writing before generating" instruction is critical — Claude Design's planning phase is the first steering opportunity, and forcing declaration catches default drift before tokens are spent on generation.

---

## 4. Default-drift watchlist

Claude Design's defaults are strong. Watch for these specifically, and correct immediately:

| Drift pattern | What it looks like | Corrective prompt |
|---------------|-------------------|-------------------|
| Cream / warm wash | Background drifts from your declared neutral to ~#F4F1EA | "Reset the background to [your declared neutral OKLCH value]. Do not warm it." |
| Serif creep | Headings or display text become Fraunces/Playfair/Georgia | "Replace display serif with [your declared display face]. No serifs on this project." |
| Italic accents | Italic pulled quotes, italic labels, italic marketing text | "Remove italics. This project does not use italic accents." |
| Terracotta / amber bias | Accent colors warm up toward orange/red-brown | "Reset accent to [your declared accent OKLCH]. No warm amber tones." |
| Inter / Roboto default | Body text falls back to Inter | "Use [your declared body face]. Not Inter, not Roboto." |
| Centered hero layouts | Marketing-site centered compositions on app screens | "Left-aligned layout. Not centered hero patterns." |
| [Project-specific drift pattern] | [how it appears] | [corrective] |

---

## 5. Iteration guidance

**Use chat for:** global changes (typography, palette, density, overall posture). These should propagate across all generated artifacts.

**Use inline comments for:** element-level refinement. Pin the comment to the element; keep the scope tight.

**Use the tweaks panel (sliders / pickers) for:** tuning within an established direction (radius size, accent hue, density slider) — not for direction shifts.

**Use direct text editing for:** copy corrections. Do not use it to fight aesthetic drift — go back to chat and re-steer.

**Use undo/redo when:** a whole edit pass drifted. Undo rolls back a pass, not keystrokes — useful after a round of tweaks that went wrong.

**Draw on designs when:** words aren't working. Sketch the intent on the generated artifact and Claude Design will interpret the marks.

---

## 6. Export and handoff

Based on the handoff target from the Design Brief:

- **→ Claude Code:** Use the first-class handoff bundle. The design bundle passes directly into a Claude Code session with the component structure intact.
- **→ Canva:** Direct send; fully editable after export. Good for marketing or deck exports.
- **→ PDF / PPTX / standalone HTML:** Native export. Check the export for default-drift before sending.
- **→ Shareable URL:** For review cycles.
- **→ Figma import:** Claude Design reads Figma files as source; export-back is unconfirmed and should not be relied on.
```

---

### Step 4: Generate the Design Instructions

Produce `design-instructions.md` using the template from `templates/design-instructions.md`. This is the human-readable companion — it assumes the user is about to sit down with Claude Design and needs practical guidance.

**Structure:**

```markdown
# Design Instructions: [Project Name]

For: [project owner]
Tool: Claude Design
Source: claude-design-config.md, design-brief.md

---

## Pre-session checklist

Before opening Claude Design, have these ready:
- [ ] claude-design-config.md open (or the §1 block copied)
- [ ] Anchor product screenshots (2-3 per Look anchor, 1-2 per anti-anchor)
- [ ] [Codebase / Figma / brand assets] accessible
- [ ] A clear first artifact in mind (not "design the product" — "design the Query Detail screen")

---

## How to start the session

1. Paste the §1 system prompt block from claude-design-config.md
2. Upload the inputs from the §2 inputs manifest
3. Paste the §3 first-prompt template, filled in for the specific first artifact
4. Wait for Claude Design to declare typography, color, spacing, and motion in writing
5. Confirm or correct before letting it generate

---

## Prompt patterns that work for this project

Use these as starting points, adjusted for the specific task:

### For a new screen or artifact
> "Generate [artifact] at [dimensions / format]. Honor the <frontend_aesthetics> block and the uploaded anchors. Show the planning step (typography, color, spacing, motion) before building."

### For tightening a generated artifact
> "The generated [artifact] drifted toward [specific drift pattern — e.g., 'cream background']. Reset [specific token] to [value from spec]. Do not [specific drift]."

### For adding a component to the design system
> "Add [component] to the system. It should compose from [existing atoms/molecules]. Variants: [list]. States: [list]. Token bindings: [list]."

### For a pitch deck or one-pager
> "Ten-slide [deck type] for [audience]. Narrative sequence: [list]. Honor the <frontend_aesthetics> block. Typography and color as declared. Not cream / serif / italic / terracotta."

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

[Pulled from §4 of claude-design-config.md — same patterns, restated for easy reference during a session.]

---

## After the session

- Export the artifact(s) per the handoff target in the Design Brief
- Note any recurring drift patterns — these feed `design-refresh` (V2) or a config update
- If the output was materially off-brief, log it as an `improvements/` note (see studio conventions) so the brief or config can be tightened next time
```

---

### Step 5: Present and Hand Off

Share both files. Walk through:

- **The system prompt block** — does it feel complete when read aloud? Any section still generic?
- **The defaults-to-avoid list** — explicit, specific, complete?
- **The inputs manifest** — can the user produce each input? Any missing?
- **The first-prompt template** — does it match the actual first artifact the user needs?
- **The drift watchlist** — is it specific to this project, not generic?

If the user is about to open Claude Design: recommend starting with the first-prompt template and logging anything the watchlist missed back into an `improvements/` note.

If the brief evolves after the first Claude Design session: run `design-encode` again to refresh the config. Do not edit the config by hand — it is a projection, not a source.

---

## Quality Standards

A good Claude Design Configuration:

- **Leads with the default-override block.** The `<defaults_to_avoid>` section appears before positive direction, is itemized, and is specific. Generic "override the defaults" does not count.
- **Uses the `<frontend_aesthetics>` structure.** Fields are separated (typography, color, spacing, motion, etc.), not collapsed into a single paragraph.
- **Names concrete values.** Typefaces by name, colors in OKLCH, spacing in px, motion in named curves. No categories ("a modern sans", "a warm palette").
- **Cites anchors by name with screenshot direction.** Anchor-by-name alone is not a first-class Claude Design feature; the config tells the user which screenshots to upload.
- **Includes a first-prompt template tested against the brief.** The user should be able to paste it and have Claude Design's first planning declaration match the spec.
- **Has a drift watchlist specific to this project.** The watchlist calls out the exact default patterns most likely to appear, with corrective prompts ready.

A good Design Instructions file:

- **Assumes the user is about to sit down with Claude Design.** Pre-session checklist, first-prompt template, and drift watchlist are immediately actionable.
- **Separates "let Claude run" from "steer" from "stop and re-ground."** Gives the user a clear mental model of when to intervene.
- **Points back to the brief for direction changes.** Does not encourage ad-hoc brief evolution in chat.
