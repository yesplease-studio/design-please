---
name: design-discover
description: >
  Run a structured interview and asset audit to surface what a user is designing,
  what direction they are drawn to, and what they want to avoid. Produces a
  Discovery Brief that design-author ingests as its primary input. Use this skill
  before design-author on any new engagement, especially when the user can't yet
  describe their direction in specific terms. Trigger when the user says things
  like "help me figure out what I want", "I know I want it to look like X but I
  don't know how to say that", "we need design direction before building",
  "audit our current design", or "I have no design background and I'm about to
  open Claude Design." Skip this skill if the user already has a rich design
  brief and wants to go straight to authoring edits or encoding for Claude Design.
system: design
integrations:
  required: []
  optional:
    - WebFetch (for fetching reference product URLs when screenshots aren't provided)
---

# Skill: design-discover

**Purpose:** Run a structured interview and asset audit that surfaces the design problem, the user's reference anchors and anti-anchors across Look/Feel/Act, and their existing assets. Produces a Discovery Brief that `design-author` ingests as its primary source material.

---

## Why This Skill Exists

Most non-designers know good design when they see it but can't describe what they want in terms a tool or designer can act on. Ask a founder "what should the app look like?" and you'll get "modern and clean" -- which describes most products and distinguishes none.

Discovery solves this by working through four lenses:

1. **The problem** -- who is this for, what stage, what use case, what constraints
2. **Existing assets** -- screenshots, codebases, Figma files, brand guidelines, prior design work (if any exist)
3. **Reference anchors** -- products, cultural references, and specific visual/emotional/behavioral patterns the user is drawn to, across Look, Feel, and Act
4. **Anti-anchors** -- what to avoid, which is often more revealing than the positive anchors, especially given Claude Design's strong default house style

The synthesis step -- turning reference patterns into direction that is distinctly this product's own -- is where the real value lands. This is also where a human facilitator adds the most value, but the skill produces useful output independently. When the user has no opinion on a dimension, the skill offers to choose and explains why; over repeated runs, the user becomes more articulate.

---

## When to Use vs. Skip

**Use design-discover when:**
- Starting a new design engagement (no existing brief)
- The user has anchors in their head ("like Linear", "like Posthog") but can't translate them to specifics
- The user is about to open Claude Design and doesn't have a prompt strategy yet
- Design direction is inconsistent across existing assets
- The user has a design background but wants the brief formalized

**Skip design-discover when:**
- The user already has a specific, complete design brief and wants to edit it (use `design-author`)
- The user has a brief and needs it encoded for Claude Design (use `design-encode`)
- A recent Claude Design or designer output needs auditing against an existing brief (use `design-validate`, V2)
- The engagement is a minor refresh of one dimension (use `design-refresh`, V2)

---

## Workflow

### Phase 1: Scope the Engagement

Before diving into visual direction, establish what is being designed. Ask 2-3 questions via `AskUserQuestion`:

- **What are you designing?** (A product, a specific feature, a marketing site, a pitch deck, a design system, a rebrand)
- **Is there existing design work?** (Screenshots, a live URL, Figma files, a codebase, brand guidelines, prior Claude Design output)
- **What is the handoff target?** (Claude Design, Figma, a human designer, a developer, yourself)

These three answers determine how the rest of the skill runs:

- If no existing design work → skip Phase 3, spend more time on anchors in Phase 4
- If rich existing design work → Phase 3 is the bulk of the session
- If the handoff is Claude Design → emphasize default override and explicit parameter specificity in Phase 5
- If the handoff is a human designer → de-emphasize override; emphasize specificity and anti-anchors

---

### Phase 2: Problem Framing Interview (Dim 1)

Establish the problem before the aesthetic. Borrowed from IDEO's Empathize → Define. Work through four areas using `AskUserQuestion`. Batch 2-4 questions per call.

**Audience:**
- Who is the design for? (Describe them in their own words, not as a persona. "A PM at a Series B SaaS trying to get an infra decision shipped before quarter-end" beats "tech-savvy decision-maker.")
- What device, context, or moment will they encounter this in?
- What do they already know? What are they comparing it against?

**Use case and scope:**
- What specific flow, screen, or artifact is in scope?
- What is explicitly out of scope?
- What does success look like, from their perspective?

**Stage and constraints:**
- What stage is the product at? (Pre-launch, MVP, scaling, mature, rebrand)
- What tech stack is this built on or will it be built on? (Relevant for token format: Tailwind, CSS variables, design tokens JSON, Figma variables)
- Are there accessibility requirements? (WCAG level, contrast, motion preferences)
- Are there brand guidelines, existing design systems, or visual constraints the design must live inside?

**Testable success criteria:**
- How will you know the design worked? (Specific — "first-time users complete onboarding without opening help" beats "better UX")

Capture verbatim when the user uses their own language — these quotes go in the Discovery Brief unchanged.

---

### Phase 3: Existing Assets Audit (if assets exist)

Skip this phase if the user has no existing design work.

Ask the user for what they have. Accept any format:
- Screenshots of the current product
- Live URLs (use `WebFetch` if available)
- Figma file links
- Codebase paths (read directly if local)
- Brand guideline PDFs or docs
- Prior Claude Design output or chat history
- Moodboards, Pinterest boards, or inspiration dumps

**For each asset, extract:**

- **Typography** — named families if identifiable (not "sans-serif"; "Inter" or "Unknown geometric sans")
- **Color strategy** — palette breadth (monochrome, monochrome+accent, full), named semantic roles if present
- **Spacing and density** — compact vs. spacious, grid visible or loose
- **Layout patterns** — centered, left-aligned, data-dense, split, card-based
- **Imagery** — photographic, illustrative, geometric, none
- **Iconography** — outline, filled, rounded, angular, none
- **Motion** (if visible in a live product) — none, utility, expressive
- **Information hierarchy** — how the eye is directed through the composition
- **Inconsistencies across assets** — where direction drifts

Inconsistency is information. If the marketing site uses IBM Plex but the app uses Inter, that is a finding, not a flaw.

**Call out what works and what doesn't** in the user's eyes, not yours:

> "Looking at your current product, I notice the dashboard uses a generous 16px baseline grid with monochrome + a single teal accent, but the marketing site uses a tighter grid and three accent colors. Is the drift intentional? When you look at both, which one feels more like the direction you want?"

---

### Phase 4: Anchor Interview — Look, Feel, Act

The core of discovery. Most users can't produce specific direction from scratch, but they can point to products they admire. The skill's job is to extract what specifically resonates from each anchor, across three axes.

Run the interview three times (once per axis). Use `AskUserQuestion` with 2-4 questions per call. When the user offers a product as an anchor, fetch it via `WebFetch` (or ask for 2-3 screenshots) and analyze before moving on.

---

#### Axis 1: Look (what you see)

> "Name 2-4 products whose visual design you admire. They don't need to be in your industry — any products whose look makes you think 'I wish ours looked like that.'"

For each:
- **What specifically resonates?** Push past "I like how Linear looks" to: "Is it the tight spacing? The monochrome + single accent? The typography? The way data-dense screens don't feel cluttered?"
- **What would you borrow vs. what's specific to their context?** Linear's density might not fit a marketing tool. Stripe's hero layouts might not fit a dev tool.

When the user names a product, fetch or request screenshots. Extract concrete patterns:
- Typography families (name them; approximate if uncertain, and say so)
- Color strategy
- Density level (compact / balanced / spacious)
- Layout patterns
- Imagery posture
- Iconography style

**If the user has no look anchors:** Offer to choose. Propose 2-3 anchors appropriate for the problem (from Phase 2) and the anti-anchors (if any are already surfaced). Name the choice, cite the reasoning ("for a fintech dashboard with power users, a Linear/Vercel/Bloomberg Terminal axis tends to fit"), and name alternatives considered.

---

#### Axis 2: Feel (what it evokes)

> "How should the product feel to the person using it? Not 'premium' or 'friendly' — more specific. What emotional register should it produce?"

If the user struggles (most do), offer references:
- Cultural movements (Swiss modernism, Memphis, Bauhaus, brutalism, solarpunk, cyberpunk, cottagecore)
- Product feels (Posthog's dry confidence, Linear's quiet precision, Notion's warm playfulness, Arc's optimistic weirdness, Claude's thoughtful calm)
- Real-world anchors (a well-designed hardware store, a hotel lobby, a quiet library, a busy newsroom)

For each feel anchor the user picks:
- **What specifically does it evoke?** Confidence, warmth, energy, formality, playfulness, trust, quiet, drama, dryness.
- **What patterns produce that feeling?** Typography weight extremes, color temperature, animation pacing, copy tone.

**Feel is the weakest self-serve dimension.** When the user has no feel anchor, this is where you should most aggressively offer to choose. Non-designers often lack the vocabulary here entirely. Propose 2-3 feel anchors grounded in the problem, and explain what each would imply downstream.

---

#### Axis 3: Act (how it behaves)

> "How should the product behave when the user interacts with it? Think about motion, pacing, density, and how much the UI does for the user."

Prompts when the user doesn't know where to start:
- Does the UI move, or does it stay still? (Utility motion / expressive motion / no motion)
- Is the UI keyboard-first, pointer-first, or touch-first?
- How dense is the information? (Spreadsheet-dense / balanced / minimal)
- Does the UI assume and act, or does it wait and confirm? (Linear's reversible actions / banks' confirm-everything pattern)
- How intelligent is the UI? (Passive form / active suggestions / full agentic)

For each act anchor the user picks, ask which product behaves most like the target behavior, and extract specific interaction patterns.

**Name the NN/g heuristics relevant to the use case:**
- Form-heavy → error prevention, flexibility and efficiency of use
- Dashboard → visibility of system status, recognition rather than recall
- First-use / onboarding → help and documentation, aesthetic and minimalist design
- Destructive actions → error prevention, user control and freedom

Do not require the user to name heuristics. Surface the 3-4 that matter most for the use case and get their reaction.

---

#### Synthesis across axes

After all three axes, produce a working synthesis sentence in the user's hearing:

> "So the direction is: look like Linear (tight, monochrome + one accent, IBM Plex Sans), feel like Posthog (dry, confident, a little understated), act like the Claude desktop app (keyboard-first, fast, opinionated defaults, reversible). Does that sound right? What's off?"

The synthesis sentence is the most load-bearing output of the interview. Iterate on it until the user says "yes, that's it."

---

### Phase 5: Anti-Anchor Interview

Anti-anchors are half the brief. Work through two lenses:

**Visual anti-anchors:**
- What competitors or peers have design you find generic, dated, or off-putting? What specifically bothers you?
- What visual trends in your space do you want to avoid?
- Are there specific typefaces, colors, or layouts that feel wrong?

**Claude Design defaults (if handoff target is Claude Design):**

State the defaults explicitly and get a read on each:

> "Claude Design has a strong default house style: cream / off-white backgrounds, serif display type (Georgia, Fraunces, Playfair), italic accents, and a terracotta/amber palette. For a [fintech dashboard / dev tool / marketing site / whatever the user has], that's going to feel wrong. Can we rule each of those out in writing?"

Produce an explicit default-override list the user confirms. This becomes the highest-leverage line in the Claude Design Configuration.

---

### Phase 6: Dimension Assessment

Score the user's current design-brief maturity across all six dimensions. Use the maturity levels from `systems/design/SYSTEM.md`.

For each dimension:
- **Score (1-5)** with brief justification
- **Evidence** — what was observed or stated that supports the score
- **Key gap** — the single most important thing missing at this dimension

This is a starting-point assessment. Most first-time users score 1-2 across most dimensions. That is expected. The assessment is diagnostic, not evaluative.

---

### Phase 7: Produce the Discovery Brief

Synthesize everything into a Discovery Brief. This is the handoff artifact to `design-author`.

```markdown
# Design Discovery Brief: [Project Name]
Date: YYYY-MM-DD
Engagement: [scope from Phase 1]
Handoff target: [Claude Design / Figma / human designer / dev]

---

## Problem Framing

### Audience
[In the audience's own language, not as a persona]

### Use case and scope
- **In scope:** [flows, screens, artifacts]
- **Out of scope:** [explicitly excluded]
- **Stage:** [pre-launch / MVP / scaling / mature / rebrand]

### Constraints
- Tech stack: [names]
- Accessibility: [requirements]
- Brand / existing system: [what exists and must be respected]
- Performance / other: [named constraints]

### Success criteria
[Testable — what "done" looks like to the audience]

### Verbatim quotes from user
[Direct quotes captured during the interview, unchanged]

---

## Existing Assets Audit
_Omit this section if no existing assets._

### Assets reviewed
- [Asset 1 — type, source]
- [Asset 2 — type, source]

### Patterns observed
- Typography: [findings]
- Color: [findings]
- Spacing/density: [findings]
- Layout: [findings]
- Imagery: [findings]
- Iconography: [findings]
- Motion: [findings if observable]
- Information hierarchy: [findings]

### Inconsistencies
[Where direction drifts across assets — not a flaw, a finding]

### What the user said worked / didn't work
[In their words]

---

## Anchors

### Look anchors
**Anchor 1: [Product / reference]**
- What resonates: [specific quality]
- Extracted patterns: [typography, color, spacing, etc.]
- Source: [URL / screenshot reference]

**Anchor 2: [Product / reference]**
[Same structure]

**Anchor 3: [Product / reference]**
[Same structure]

### Feel anchors
**Anchor 1: [Product / cultural movement / scene]**
- Evokes: [specific register]
- Patterns that produce it: [typography weight, color temperature, pacing, tone]

**Anchor 2: [same]**

### Act anchors
**Anchor 1: [Product / interaction pattern]**
- Behavior: [specific pattern]
- Extracted: [interaction specifics, density, motion posture]

**Anchor 2: [same]**

### Relevant NN/g heuristics
- [Heuristic 1 — why it's load-bearing for this use case]
- [Heuristic 2 — why it's load-bearing]
- [Heuristic 3 — why it's load-bearing]

---

## Anti-Anchors

### Visual anti-anchors
- [Specific pattern to avoid] — [why it doesn't fit]
- [Specific pattern to avoid] — [why it doesn't fit]

### Competitor / peer anti-anchors
- [Product] — [what specifically to avoid]

### Claude Design default overrides
_Include only if handoff is Claude Design._

Explicitly rule out:
- [ ] Cream / off-white backgrounds (~#F4F1EA)
- [ ] Serif display (Georgia, Fraunces, Playfair)
- [ ] Italic accents
- [ ] Terracotta / amber palette
- [ ] [Any additional project-specific defaults to avoid]

### Candidate banned specifics
[Typefaces, colors, patterns to explicitly prohibit]

---

## Synthesis

### Working direction sentence
[The "Look like X, feel like Y, act like Z" sentence the user confirmed in Phase 4]

### Distinctive combination
[What makes this combination of anchors specific to this product, rather than a generic mashup. One paragraph.]

---

## Dimension Assessment

| # | Dimension | Score | Key Finding |
|---|-----------|-------|-------------|
| 1 | Problem Definition | X/5 | [one sentence] |
| 2 | Look Direction | X/5 | [one sentence] |
| 3 | Feel Direction | X/5 | [one sentence] |
| 4 | Act Direction | X/5 | [one sentence] |
| 5 | Design System Scope | X/5 | [one sentence] |
| 6 | Handoff Readiness | X/5 | [one sentence] |

---

## Recommended Next Steps

### Immediate (design-author)
[Which artifacts to create first — design-brief.md at minimum; design-system-spec.md if scope is large enough]

### After authoring
[Whether design-encode is next, or whether gaps need to be filled first]

---

## Open Questions

| Question | Dimension | Notes |
|----------|-----------|-------|
| [Question] | [dimension] | [context or who needs to answer] |
```

---

### Phase 8: Present and Hand Off

Share the Discovery Brief with the user. Walk through:

- **The synthesis sentence** — does the combination still feel right when you read it?
- **The anchors and anti-anchors** — any you want to remove, strengthen, or add?
- **The default overrides** (if Claude Design) — explicit and complete?
- **The dimension scores** — any surprises?
- **The open questions** — can any be answered now?

If the user is ready to proceed: pass the Discovery Brief as the primary input to `design-author`. The brief replaces the need for `design-author` to re-interview from scratch.

If critical open questions remain: document them and note which sections of the Design Brief can be drafted now vs. which need more input.

---

## Quality Standards

A good Discovery Brief:

- **Names specific anchors with extracted patterns, not just product names.** "Like Linear" is not useful. "Linear — tight 8px grid, IBM Plex Sans, monochrome + single teal accent, no hero imagery, data-dense tables without feeling cluttered" is.
- **Names anti-anchors with equal specificity.** "Not corporate" is not a boundary. "Not SaaS-template hero sections, not purple gradients on white, not 'intelligent' AI glyphs with sparkles" is.
- **States default overrides explicitly when Claude Design is the handoff target.** At minimum: cream, serif, italic, terracotta, Inter are ruled out in writing. Additional specifics as the project demands.
- **Captures the user's verbatim language.** Direct quotes from the user about their audience, their problem, or their taste are preserved unchanged. Paraphrase later; never here.
- **Is honest about dimension maturity.** Low scores are diagnostic. Most first-time users score 1-2 across most dimensions — that is expected, and the brief should make it feel like a starting point.
- **Teaches when it chooses.** Where the user had no opinion on a dimension, the brief records the choice, the reasoning, and the alternatives considered — so the user is more articulate the next time.
- **Produces a synthesis sentence the user confirmed out loud.** The "Look like X, feel like Y, act like Z" sentence is the single load-bearing output. If the user didn't confirm it, the brief is not ready.
