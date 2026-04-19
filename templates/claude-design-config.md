# Claude Design Configuration: {{PROJECT_NAME}}

Date: {{YYYY-MM-DD}}
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
{{From design-brief Problem Definition. One paragraph — audience, stage, use case.}}
</audience_and_context>

<defaults_to_avoid>
This project is {{kind of product}}. Do NOT use the default aesthetic. Specifically:
- NOT cream / off-white backgrounds (~#F4F1EA)
- NOT serif display (no Georgia, Fraunces, Playfair)
- NOT italic accents
- NOT terracotta / amber palette
- NOT Inter, Roboto, Open Sans, Lato, system default fonts
- NOT purple gradients on white
- {{Project-specific additions — any patterns ruled out in the brief}}
</defaults_to_avoid>

<typography>
- Body: {{Named family}} at {{size}} / {{line-height}}, weight {{400 / 500 / etc.}}
- Headings: {{Named family}} with {{scale jump — e.g., 3x from body 14 to H2 28 to H1 48}}
- Monospace (if used): {{Named family}}
- Pairing rationale: {{one sentence}}
- Weights used: {{list}}
</typography>

<color>
Palette strategy: {{monochrome | monochrome + accent | duotone | full}}.
Dark mode {{primary | secondary | not used}}.

{{Mode 1 — typically primary}}:
- bg: {{oklch(...)}}
- surface: {{oklch(...)}}
- surface-elevated: {{oklch(...)}}
- text: {{oklch(...)}}
- text-muted: {{oklch(...)}}
- accent: {{oklch(...)}}  /* single interactive accent */
- success: {{oklch(...)}}
- warning: {{oklch(...)}}
- error: {{oklch(...)}}

{{Mode 2, if applicable}}:
{{Describe how it's derived — e.g., "Light mode: invert neutrals to near-white base, keep accent identical."}}
</color>

<spacing_and_density>
Base unit: {{4px | 8px}}. Spacing scale: {{listed}}.
Density: {{compact | balanced | spacious}}. {{Specific guidance — e.g., "Table rows 32px height. Section gaps 16-24px. No generous whitespace."}}
</spacing_and_density>

<layout>
{{Grid spec — columns, breakpoints}}.
{{Primary pattern — e.g., "Left sidebar nav (fixed), main content area, optional right panel."}}
{{Alignment — e.g., "Left-aligned. Never centered hero patterns."}}
</layout>

<imagery>
{{Photographic / illustrative / geometric / none — specifics. If none: state it explicitly. "None. This is a data product. Do not add illustrations, stock photography, or decorative imagery."}}
</imagery>

<iconography>
{{Style, weight, source library — e.g., "Outline only, 1.5px stroke, 20px default, Lucide or Heroicons. No filled icons."}}
</iconography>

<motion>
- Posture: {{none | utility | expressive}}
- {{When motion happens — e.g., "State changes happen instantly; no animated entrances for data."}}
- Easing: {{named curves if expressive}}
</motion>

<inspirations>
Draw from these anchors:
- {{Anchor 1}} — specifically: {{extracted patterns}}
- {{Anchor 2}} — specifically: {{extracted patterns}}
- {{Anchor 3}} — specifically: {{extracted patterns}}

Feel: {{one sentence of emotional register}}
Act: {{one sentence of behavioral posture}}
</inspirations>

<anti_inspirations>
Avoid the patterns of:
- {{Anti-anchor 1}} — specifically: {{patterns to avoid}}
- {{Anti-anchor 2}} — specifically: {{patterns to avoid}}
</anti_inspirations>

<interaction_and_behavior>
- Input modality: {{keyboard-first | pointer-first | touch-first}}
- Density expectations: {{specifics}}
- UI intelligence: {{passive | suggestive | active | agentic}}
- Confirmation pattern: {{when confirmations happen, when actions are reversible}}
- NN/g heuristics to honor: {{named list}}
</interaction_and_behavior>

<design_system_scope>
This project uses a {{tokens only | tokens + atoms | tokens + atoms + molecules | full system}} design system. See design-system-spec.md for the full specification. Do not improvise components beyond the stated scope. If a pattern isn't specified, ask before generating.
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
| {{Anchor 1}} screenshots (2-3) | Visual language for Look | {{how to produce or where to find}} |
| {{Anchor 2}} screenshots (2-3) | Visual language for Look | {{source}} |
| {{Anti-anchor}} screenshots (1-2) | Boundary examples | {{source}} |
| {{Codebase path or Figma file, if applicable}} | Token fidelity | {{path / link}} |
| Brand assets (logo, wordmark) | If applicable | {{path}} |

Input hierarchy for Claude Design (most to least fidelity):
1. Codebase with named tokens (Tailwind config, CSS variables)
2. Figma files with named styles and variables
3. Screenshots with explicit descriptions

Use the highest-fidelity input available for this project.

---

## 3. First-prompt template

Start the Claude Design session with this prompt (paste after the §1 block):

```
I'm designing {{specific scope — e.g., "the Cash Position screen of Helm Treasury, a fintech dashboard"}}.

Audience: {{from brief}}
Stage: {{from brief}}
What I need first: {{specific first artifact — e.g., "a wireframe of the Cash Position screen at 1440px, dark mode, showing left sidebar nav, total-cash metric at top, accounts table, and right-panel alert detail"}}

Use the aesthetic direction in the <frontend_aesthetics> block above. Reference the uploaded screenshots of {{anchor products}} for visual language. Do not use the default Claude Design aesthetic.

Before you start, state in writing: (a) typefaces (confirm from the block), (b) color tokens in OKLCH (confirm the palette), (c) spacing unit (confirm base), (d) motion posture. I'll confirm or correct, then you can generate.
```

The "state in writing before generating" instruction is critical — Claude Design's planning phase is the first steering opportunity, and forcing declaration catches default drift before tokens are spent on generation.

---

## 4. Default-drift watchlist

Claude Design's defaults are strong. Watch for these specifically, and correct immediately:

| Drift pattern | What it looks like | Corrective prompt |
|---------------|-------------------|-------------------|
| Cream / warm wash | Background drifts to ~#F4F1EA on a card or surface | "Reset the background to {{declared neutral OKLCH}}. Do not warm it." |
| Serif creep | Headings or display text become Fraunces/Playfair/Georgia | "Replace display serif with {{declared display face}}. No serifs on this project." |
| Italic accents | Italic pull-quotes, italic labels, italic marketing text | "Remove italics. This project does not use italic accents." |
| Terracotta / amber bias | Accent colors warm toward orange/red-brown | "Reset accent to {{declared accent OKLCH}}. No warm amber tones." |
| Inter / Roboto default | Body text falls back to Inter | "Use {{declared body face}}. Not Inter, not Roboto." |
| Centered hero layouts | Marketing-site centered compositions on app screens | "Left-aligned layout. Not centered hero patterns." |
| {{Project-specific drift pattern}} | {{how it appears}} | {{corrective}} |

---

## 5. Iteration guidance

**Use chat for:** global changes (typography, palette, density, overall posture). These should propagate across all generated artifacts.

**Use inline comments for:** element-level refinement. Pin the comment to the element; keep the scope tight.

**Use the tweaks panel (sliders / pickers) for:** tuning within an established direction (radius size, accent hue, density) — not for direction shifts.

**Use direct text editing for:** copy corrections. Do not use it to fight aesthetic drift — go back to chat and re-steer.

**Use undo/redo when:** a whole edit pass drifted. Undo rolls back a pass, not keystrokes.

**Draw on designs when:** words aren't working. Sketch the intent on the generated artifact.

---

## 6. Export and handoff

Based on the handoff target from the Design Brief:

- **→ Claude Code:** Use the first-class handoff bundle.
- **→ Canva:** Direct send; fully editable after export.
- **→ PDF / PPTX / standalone HTML:** Native export. Check for default-drift before sending.
- **→ Shareable URL:** For review cycles.
- **→ Figma import:** Claude Design reads Figma files as source; export-back is unconfirmed at launch.
