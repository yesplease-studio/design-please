# Design Brief: {{PROJECT_NAME}}

Date: {{YYYY-MM-DD}}
Handoff target: {{Claude Design | Figma | human designer | dev direct}}
Status: {{draft | in review | approved}}

---

## 1. Problem Definition

### Audience
{{In the audience's own language, not as a persona. Pull verbatim quotes from the Discovery Brief where relevant.}}

### Use case in scope
- {{Flow, screen, or artifact 1}}
- {{Flow, screen, or artifact 2}}

### Use case out of scope
- {{Explicitly excluded}}

### Stage
{{Pre-launch | MVP | scaling | mature | rebrand}}

### Constraints
- **Tech stack:** {{named}}
- **Accessibility:** {{WCAG level, contrast, motion preferences}}
- **Brand / existing system:** {{what must be respected}}
- **Performance:** {{budgets, mobile considerations}}
- **Other:** {{anything else — e.g., data display rules, regulatory}}

### Success criteria
{{Testable — what "done" looks like to the audience.}}

---

## 2. Look Direction

### Typography
- **Body:** {{Named family}} at {{size}} / {{line-height}}
- **Headings:** {{Named family}} with {{scale jump — e.g., 3x from 14 to 28 to 48}}
- **Monospace (if used):** {{Named family}} at {{size}}
- **Weights used:** {{list}}
- **Pairing rationale:** {{one sentence}}

### Color strategy
- **Palette type:** {{monochrome | monochrome + accent | duotone | full}}
- **Primary / neutrals:** {{named colors with OKLCH values preferred}}
- **Accent(s):** {{OKLCH}}
- **Semantic colors:** {{success, warning, error, info — OKLCH}}
- **Dark mode:** {{yes | no | derived | primary}}

### Spacing and density
- **Base unit:** {{e.g., 4px or 8px}}
- **Spacing scale:** {{listed — 4, 8, 12, 16, 24, 32, 48}}
- **Density level:** {{compact | balanced | spacious}}
- **Rationale:** {{one sentence tied to use case}}

### Layout
- **Primary pattern:** {{centered | left-aligned | split | data-dense | card-based}}
- **Grid:** {{columns and breakpoints}}

### Imagery
{{Photographic | illustrative | geometric | none — with specifics. If none: state it.}}

### Iconography
{{Style, weight, source library if any.}}

### Anchors
- **{{Anchor 1}}:** {{specific patterns carried from it}}
- **{{Anchor 2}}:** {{specific patterns carried from it}}

### Anti-anchors
- **{{Anti-anchor 1}}:** {{specific patterns to avoid}}
- **{{Anti-anchor 2}}:** {{specific patterns to avoid}}

### Default overrides (Claude Design)
_Include only if handoff is Claude Design._

- NOT cream / off-white backgrounds (~#F4F1EA)
- NOT serif display (Georgia, Fraunces, Playfair)
- NOT italic accents
- NOT terracotta / amber palette
- NOT Inter, Roboto, system defaults
- NOT purple gradients on white
- {{Project-specific additions}}

---

## 3. Feel Direction

### Emotional register
{{Specific, not "premium" or "friendly". "Dry confidence with understated warmth. Quiet. Competent without trying to prove it."}}

### Anchors
- **{{Product / cultural reference 1}}:** {{what it evokes and what patterns produce it}}
- **{{Product / cultural reference 2}}:** {{same}}

### Anti-anchors
- {{What register to avoid and why.}}

---

## 4. Act Direction

### Motion posture
{{None | utility | expressive — specifics on when motion occurs.}}

### Interaction anchors
- **{{Product behavior 1}}:** {{extracted pattern}}
- **{{Product behavior 2}}:** {{extracted pattern}}

### Density expectations
{{How much information per view; how the UI handles overflow.}}

### UI intelligence level
{{Passive | suggestive | active | agentic — with specifics.}}

### Confirmation and reversibility
{{Which actions confirm, which are reversible, which are both.}}

### Input modality
{{Keyboard-first | pointer-first | touch-first | parity — state priorities.}}

### NN/g heuristics that are load-bearing
- **{{Heuristic 1}}:** {{why it matters for this use case}}
- **{{Heuristic 2}}:** {{why it matters}}
- **{{Heuristic 3}}:** {{why it matters}}

---

## 5. Design System Scope

**Chosen atomic level:** {{tokens only | tokens + atoms | tokens + atoms + molecules | full system}}

**Rationale:** {{why this level, not more or less — one paragraph}}

**What is explicitly out of scope at this level:** {{so the tool does not improvise}}

---

## 6. Handoff Target

**Target:** {{Claude Design | Figma | human designer | dev direct}}

**Inputs the target will receive:**
- {{Input 1 — e.g., this brief}}
- {{Input 2 — e.g., 3 screenshots of anchor products}}
- {{Input 3 — e.g., codebase path for token fidelity}}

**First-use instruction:**
{{One sentence describing how the target should be invoked on this brief.}}

---

## 7. Teach-Through-Choice Log

For each dimension where the user had no opinion and the skill chose:

| Dimension | Choice | Reasoning | Alternatives considered |
|-----------|--------|-----------|------------------------|
| {{Dim}} | {{what was chosen}} | {{why}} | {{what else; why not}} |

---

## 8. Decisions Log

| Date | Decision | Reasoning |
|------|----------|-----------|
| {{YYYY-MM-DD}} | {{what was decided}} | {{why}} |
