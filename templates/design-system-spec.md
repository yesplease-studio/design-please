# Design System Spec: {{PROJECT_NAME}}

Date: {{YYYY-MM-DD}}
Scope: {{tokens only | tokens + atoms | tokens + atoms + molecules | full system}}
Source brief: design-brief.md

---

## Tokens

### Color
- **Neutrals:** {{OKLCH values, e.g., --neutral-50: oklch(98% 0 0)}}
- **Primary / accent:** {{OKLCH values}}
- **Semantic:** success, warning, error, info — {{OKLCH}}
- **Surface / background:** {{layered if needed — bg, surface, surface-elevated}}

### Typography
- **Families:** {{named}}
- **Scale:** {{full scale with sizes, line-heights, letter-spacing}}
- **Weights:** {{used weights only}}

### Spacing
- **Scale:** {{e.g., 0, 4, 8, 12, 16, 24, 32, 48, 64}}
- **Baseline grid:** {{yes/no; size}}

### Radii
{{All used radii — sm, md, lg, full}}

### Shadows
{{All used elevations — subtle, default, elevated, modal}}

### Motion
- **Easing curves:** {{named — e.g., --ease-out-expo, --ease-in-out-quart}}
- **Durations:** {{named buckets — fast 120ms, normal 200ms, slow 400ms}}
- **Rules:** {{when motion happens vs. doesn't}}

---

## Atoms
_Include this section only if scope ≥ tokens + atoms._

### Button
- **Variants:** primary, secondary, ghost, danger
- **Sizes:** sm, md, lg
- **States:** default, hover, focus, active, disabled, loading
- **Token bindings:** {{which tokens each variant uses}}
- **Accessibility:** {{keyboard, ARIA}}

### Input
{{Variants, sizes, states, token bindings, accessibility}}

### Textarea
{{...}}

### Select
{{...}}

### Checkbox
{{...}}

### Radio
{{...}}

### Switch
{{...}}

### Chip
{{...}}

### Tag
{{...}}

### Badge
{{...}}

### Tooltip
{{...}}

### Icon
{{Style, source library, sizes}}

---

## Molecules
_Include this section only if scope ≥ tokens + atoms + molecules._

### Card
- **Composition:** {{which atoms it contains, in what arrangement}}
- **Variants:** {{if any}}
- **States:** {{if any}}

### Form group
{{...}}

### Toolbar
{{...}}

### Navigation item
{{...}}

### Modal
{{...}}

### Drawer
{{...}}

### Menu
{{...}}

### List item
{{...}}

### Table row
{{...}}

---

## Organisms / Templates / Pages
_Include this section only if scope is full system._

### {{Organism name — e.g., App shell, Dashboard layout, Data table}}
{{Composition rules, variants, state handling, when to use}}

---

## Behavior notes

### Keyboard shortcuts
{{If applicable}}

### Interaction patterns
{{Hover, focus, drag, select behaviors}}

### State handling
{{Loading, error, empty, disabled — consistent patterns across components}}

### Accessibility
{{Contrast ratios, focus rings, keyboard nav, screen reader considerations}}

---

## Out of scope

{{Explicit list of what this spec does not cover. The handoff target should not improvise beyond this.}}
