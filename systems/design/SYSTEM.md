# Design System

A system for scoping, defining, and encoding a company's visual and behavioral design direction so that non-designers can produce specific briefs, AI tools like Claude Design generate on-brand output, and handoffs to designers or developers start from shared ground. Humans provide taste, judgment, and approval. AI handles interviewing, synthesis, and artifact generation. When the user lacks a design background, the system teaches by choosing on their behalf and explaining why.

---

## 1. Core Concepts

### 1.1 What the System Does

The Design System is the translation layer between "I know good when I see it" and "here is a brief a tool or a designer can execute against." It takes messy inputs -- existing product screenshots, reference products, codebases, brand adjectives, design system fragments -- and produces structured artifacts that specify Look, Feel, and Act with enough precision to generate from.

It solves three problems:

1. **Scoping.** Converts vague direction ("make it feel more premium", "something like Linear") into a structured brief with named anchors, anti-anchors, and explicit parameters for typography, color, motion, density, and behavior.
2. **Encoding.** Produces a machine-readable configuration so Claude Design (and similar AI tools) generate from the user's direction instead of the tool's default house style (cream background, serif display, terracotta accent — wrong for most products).
3. **Teaching.** When the user has no opinion on a dimension, the system offers to choose, cites the reasoning, and names the alternatives considered. Over repeated use, the user becomes more articulate about design.

### 1.2 The Three Layers

Every design engagement produces artifacts at three layers:

| Layer | Artifacts | Audience | Lifespan | Authored by |
| --- | --- | --- | --- | --- |
| **Discovery** | Discovery Brief | Claude (input for Author), voice owner | Short-lived, per-engagement | `design-discover` skill |
| **Definition** | Design Brief, Design System Spec | Designers, developers, AI tools, future self | Long-lived, updated as the product evolves | `design-author` skill |
| **Encoding** | Claude Design Configuration, Design Instructions | Claude Design (and other AI tools), prompt authors | Derived from Definition layer; refreshed when the brief changes | `design-encode` skill |

The Definition layer is the source of truth. The Encoding layer is a projection of it for a specific tool (Claude Design today; other tools can be added without changing the brief). The Discovery layer is consumed by Author and then kept as provenance — it is not a live artifact.

### 1.3 The Skills

**V1 (shipping):**

| Skill | Role | When used |
| --- | --- | --- |
| `design-discover` | Interview the user; collect references, anti-references, existing assets; score the six dimensions | Before authoring, especially when the user can't yet describe their direction |
| `design-author` | Create and edit the Design Brief and Design System Spec | After discovery; when the direction changes; when the product evolves |
| `design-encode` | Translate the brief into a Claude Design system prompt and usage instructions | After authoring; before the user opens Claude Design for the first time |

**V2 (planned):**

| Skill | Role | When used |
| --- | --- | --- |
| `design-validate` | Audit Claude Design (or designer) output against the brief; flag drift, gaps, inconsistencies | After a round of generation; before shipping |
| `design-refresh` | Propose amendments as the product, audience, or strategic direction evolve | Periodically; after validate identifies systematic gaps |
| `design-recommend` | Map a challenge or situation to the right skill and dimensions | Entry point when the user is unsure where to start |

Skills are invoked independently and compose into workflows. They never overlap in responsibility: `design-discover` interviews and analyzes but never writes Definition-layer artifacts; only `design-author` writes the Design Brief; only `design-encode` generates tool configurations.

---

## 2. The Six Dimensions

The Design System measures design-brief maturity across six dimensions. Each dimension answers a core question about whether a specific aspect of the brief is executable.

| # | Dimension | Core question |
|---|-----------|---------------|
| 1 | Problem Definition | Is the design problem clearly scoped — audience, use case, stage, constraints? |
| 2 | Look Direction | Are visual anchors specified, tool defaults explicitly overridden, and concrete parameters named? |
| 3 | Feel Direction | Are emotional and tonal anchors specified with references and anti-references? |
| 4 | Act Direction | Are interaction patterns, motion, density, and UI intelligence specified? |
| 5 | Design System Scope | Has the atomic level been chosen and documented — tokens, atoms, full system? |
| 6 | Handoff Readiness | Can Claude Design, a designer, or a developer execute from this brief without a follow-up conversation? |

### 2.1 Dimension Descriptions

**Dim 1: Problem Definition**
The foundational dimension. Covers whether the brief states who the design is for, what use case or flow is in scope, what stage the product is at, what constraints apply (tech stack, accessibility, existing brand or design system, performance budget), and what success looks like. Without this, Look/Feel/Act direction is untethered. A brief at level 1 has no scoped problem — just "redesign the app." A brief at level 5 states audience, stage, flows in scope, constraints, and a testable definition of "done."

Borrowed from IDEO's Empathize → Define sequence: the problem statement must name the user and their situation before any solutioning begins.

**Dim 2: Look Direction**
What you see. Covers typography families (concrete, not categorical — "IBM Plex Sans" not "clean sans"), color strategy (monochrome + accent, full palette, semantic colors), spacing and density (compact data-dense vs. spacious marketing), layout patterns, imagery posture (photographic, illustrative, geometric, none), iconography style. Also covers explicit overrides of tool defaults — Claude Design's default cream/serif/terracotta aesthetic must be rejected in writing for any product where it doesn't fit, which is most products.

A brief at level 1 has brand adjectives ("modern", "clean") and no specifics. A brief at level 5 names typefaces, color tokens, density level, layout grid, and explicit anti-anchors ("not Inter, not Material, not purple gradients").

**Dim 3: Feel Direction**
What it evokes. Covers the emotional register the design should produce: warmth, energy, formality, playfulness, trust, confidence, quiet, drama. Named via reference products ("feels like Posthog — dry, confident, not trying too hard"), cultural references ("solarpunk", "Swiss brutalism", "IDE dark mode"), and anti-references ("not cheerful, not corporate, not startup-glossy"). Feel is the dimension most non-designers describe first and least specifically. A brief at level 1 says "premium" or "friendly." A brief at level 5 pins feel to named references with specific patterns extracted from each.

**Dim 4: Act Direction**
How it behaves. Covers interaction responsiveness, motion (none, utility, expressive), keyboard vs. pointer emphasis, density of information, pacing (does the UI wait, or does it assume), opinionated defaults, confirmation patterns, intelligence of the UI (how much it does for the user). Act is the most often ignored axis in non-designer briefs — people describe look and feel but forget that a Linear-clone that behaves like Jira is not a Linear-clone. A brief at level 1 has no behavioral specification. A brief at level 5 names interaction anchors, motion posture, density expectations, and NN/g usability heuristics relevant to the use case.

**Dim 5: Design System Scope**
What kind of system is being specified. Uses Brad Frost's atomic design taxonomy to pick the right scope rather than overbuilding:

- **Tokens only** — colors, typography scale, spacing, radii, shadows. Enough for Claude Design to generate consistent one-offs.
- **Tokens + atoms** — adds buttons, inputs, chips, tags with variants. Enough for UI that looks cohesive across screens.
- **Tokens + atoms + molecules** — adds cards, form groups, toolbars, navigation items. Enough for apps with meaningful reuse.
- **Full system** — organisms, templates, pages, documented patterns. Enough for a team to build new features without redesigning primitives.

A brief at level 1 has no scope chosen — scope emerges accidentally from whatever Claude Design happens to generate. A brief at level 5 declares the scope, specifies it at that level, and does not pretend to specify beyond it.

**Dim 6: Handoff Readiness**
Whether the brief is executable by the chosen handoff target. Covers: is the target specified (Claude Design, Figma, human designer, dev direct)? Are the target-specific inputs prepared (screenshots for Claude Design, token JSON for dev, component inventory for designer)? Is there a prompt or instruction document that tells the target how to consume the brief? For Claude Design specifically, this dimension also measures whether the default house style has been explicitly overridden.

A brief at level 1 is a conversation in Slack. A brief at level 5 produces first-round output from the target that is 80%+ on-brief.

### 2.2 Dimension Dependencies

Dimensions have dependencies that affect sequencing. Working on a downstream dimension before its dependencies are in place produces briefs that look complete but fail when executed.

```
Dim 1 (Problem Definition) ← upstream of everything
    ↓
Dim 2, 3, 4 (Look, Feel, Act) ← can be worked on in parallel
    Each needs Problem Definition to ground it.
    They inform each other but do not block each other.
    ↓
Dim 5 (Design System Scope) ← needs at least Dim 2 direction to be meaningful
    Scope is a judgment call about how much to systematize based on
    use case complexity (Dim 1) and visual coherence needs (Dim 2).
    ↓
Dim 6 (Handoff Readiness) ← depends on all of the above
    Cannot hand off what has not been defined.
```

**Foundation dimension (1):** Always first. A brief without a defined problem is unanchored and everything downstream inherits the confusion.

**Direction dimensions (2, 3, 4):** The three axes of Look/Feel/Act. Work on them in parallel — they inform each other. A user who has strong feel anchors often has implicit look anchors. An interaction-heavy product drives specific look decisions (density, visual hierarchy).

**Operational dimensions (5, 6):** These turn direction into an executable system. Skip them and the brief will need to be re-litigated on every prompt to Claude Design.

### 2.3 Maturity Levels

Each dimension is scored on a 1-5 scale:

| Level | Label | What it looks like |
|-------|-------|--------------------|
| 1 | Not started | No intentional direction. "Make it look good." Every prompt to Claude Design produces something different; every designer asks the same clarifying questions. |
| 2 | Aware | Recognizes design direction matters. Has adjectives ("modern, clean, premium") or a vague anchor ("we like how Stripe looks"). Not actionable. |
| 3 | Developing | Initial anchors named, some specifics on one or two dimensions. Used inconsistently. Claude Design still defaults to house style because overrides aren't explicit. |
| 4 | Established | All three of Look/Feel/Act have specific anchors, anti-anchors, and explicit parameters. Design system scope is chosen. Claude Design produces on-brief first-round output with moderate follow-up. |
| 5 | Leading | Brief is distinctive and executable. Claude Design produces first-round output that ships with minor edits. A human designer could pick up the brief and produce matching work. Overrides of the default house style are explicit and complete. |

**Scoring principles:**
- Be honest. If you don't have evidence at this level, score lower.
- A single session rarely moves a dimension more than 1 point.
- Movement only counts when the brief reflects it, not when it was discussed.
- Low scores are diagnostic, not shameful — they tell you where to focus.

---

## 3. Challenge-to-Dimension Mapping

When a user describes a problem, map it to dimensions before recommending a skill. This table is the recommendation engine for `design-recommend` (V2) and a reference for human facilitators.

| What they say | Primary dimension(s) | Why | Recommended path |
|---------------|---------------------|-----|------------------|
| "Claude Design keeps giving me cream backgrounds and serif fonts, and my product is a fintech dashboard" | 2 (Look), 6 (Handoff) | Default house style hasn't been explicitly overridden in the prompt. | `design-encode` (override block) |
| "I know I want it to look like Linear but I don't know how to say that" | 2 (Look), 3 (Feel) | User has anchors but can't translate them to parameters. | `design-discover` → `design-author` |
| "Our product works but it feels dated" | 2 (Look), 3 (Feel) | Visual and emotional direction drift without maintenance. | `design-discover` → `design-refresh` (V2) |
| "Every screen looks slightly different" | 5 (System Scope), 6 (Handoff) | No shared system. Each generation is a new starting point. | `design-author` (system spec focus) |
| "I have Figma components but Claude Design ignores them" | 5 (System Scope), 6 (Handoff) | System exists but isn't encoded for the tool. | `design-encode` |
| "Users say our dashboard feels hard to scan" | 4 (Act), 2 (Look) | Density and information hierarchy. Could be look (typography, spacing) or act (interaction pacing). | `design-discover` (act + look focus) |
| "I need a pitch deck and I have no design background" | 1 (Problem), 2 (Look), 3 (Feel) | Starting from zero on a specific artifact. | `design-discover` → `design-author` → `design-encode` |
| "We're rebranding and need a whole new system" | All six | Full rebuild of direction. | Full engagement: discover → author → encode |
| "Designer gives us files that don't get built correctly" | 6 (Handoff), 5 (System Scope) | Handoff artifact doesn't carry enough information. | `design-author` (system spec + handoff focus) |
| "Marketing site and product look nothing alike" | 5 (System Scope), 3 (Feel) | Missing shared system, or intentional register shift that isn't documented. | `design-discover` → `design-author` |
| "We prompt Claude Design every time and get different results" | 6 (Handoff), 2-4 | No persistent brief. Every prompt re-litigates direction. | `design-encode` |
| "The designer and the developer disagree on what the spec means" | 6 (Handoff), 5 (System Scope) | Brief is ambiguous at the handoff target. | `design-author` (system spec) |
| "Claude Design's output is fine but forgettable" | 2 (Look), 3 (Feel) | Safe not sharp. Anchors are generic or too many. | `design-discover` (tighten anchors) |
| "We want a design system but don't know where to start" | 5 (System Scope), 1 (Problem) | Scope question. Answer is usually "less than you think." | `design-discover` → `design-author` (scope choice) |
| "Product is shipping and I need design to not be the bottleneck" | 6 (Handoff), 2-4 | Execution readiness. User needs a working encoder, not a full rebrand. | `design-encode` (minimum viable) |

---

## 4. Sequencing Principles

When a user needs work across multiple dimensions, sequence the work to respect dependencies and maximize value.

### 4.1 Default sequence for users starting from scratch

```
1. design-discover (assess all 6 dimensions, collect anchors, score)
2. design-author: Problem Definition (Dim 1) + Look/Feel/Act (Dims 2-4)
3. design-author: Design System Spec at chosen scope (Dim 5)
4. design-encode: Claude Design config and instructions (Dim 6)
```

Most of the value lives in steps 1-2. Steps 3-4 are where the brief becomes self-sustaining across sessions.

### 4.2 Default sequence for users with existing design work

```
1. design-discover (score against existing assets)
2. design-author (fill gaps and tighten weakest dimensions)
3. design-encode (update or create the tool config)
```

V2 adds:

```
1. design-validate (audit recent Claude Design or designer output against the brief)
2. design-refresh (propose amendments for the weakest dimensions)
3. design-author (apply amendments)
4. design-encode (update configurations)
```

### 4.3 Sequencing rules

- **Dim 1 first, always.** No direction without a defined problem.
- **Dims 2, 3, 4 in parallel.** Look, Feel, and Act inform each other. Do not serialize them; let them cross-reference.
- **Dim 5 after Dims 2-4 have at least initial direction.** You cannot specify a system without knowing what it systematizes.
- **Dim 6 last.** Handoff readiness presupposes the other five. Encoding an incomplete brief just writes confusion into a system prompt.
- **When the handoff target is Claude Design, always include an explicit default override.** This is the single highest-leverage line in the config.

---

## 5. The Design System Artifacts

The Definition layer produces two structured artifacts (Design Brief, Design System Spec). The Encoding layer produces two (Claude Design Configuration, Design Instructions). The Discovery layer produces one (Discovery Brief). Each has a template in `templates/` and is created or edited only by its authoring skill.

### 5.1 Discovery Brief (`discovery-brief.md`)

The raw-input document.

**Contains:**
- Problem framing (audience, use case, stage, constraints) captured from the interview
- Reference anchors (products, screenshots, codebases, brand adjectives) with specific patterns extracted
- Anti-references (what to avoid) with specific patterns extracted
- Existing assets inventory (Figma files, screenshots, codebase links, brand guidelines, prior design work)
- Initial dimension scores (1-5 per dimension) with evidence
- Notable tensions or unresolved questions

**Authored by:** `design-discover`. **Feeds into:** `design-author`.

### 5.2 Design Brief (`design-brief.md`)

The canonical artifact. The one document the rest of the system projects from.

**Contains:**
- Problem Definition — audience, use case in scope, stage, constraints, success criteria
- Look Direction — typography (named families), color strategy (named palette), spacing/density, layout, imagery posture, iconography style, explicit anti-anchors, explicit default overrides
- Feel Direction — emotional register, named reference products and cultural references, anti-references, specific patterns to carry from each
- Act Direction — motion posture, interaction anchors, density expectations, UI intelligence level, relevant NN/g heuristics
- Design System Scope — chosen atomic level (tokens / +atoms / +molecules / full) with rationale
- Handoff Target — which target (Claude Design, Figma, human designer, dev direct); what inputs the target will receive

**Dimensions addressed:** Primarily Dims 1-4. Also records the Dim 5 choice.

**Authored by:** `design-author`. **Feeds into:** `design-author` (system spec), `design-encode`.

### 5.3 Design System Spec (`design-system-spec.md`)

The specification Claude Design, a designer, or a developer generates from. Not a parallel output to what Claude Design produces — the input spec that constrains it.

**Contains, scaled to chosen atomic scope:**
- Tokens — color palette (OKLCH values preferred), typography scale, spacing scale, radii, shadows, motion curves
- Atoms (if in scope) — buttons, inputs, chips, tags, badges, icons with variants
- Molecules (if in scope) — cards, form groups, toolbars, nav items, modals with composition rules
- Organisms / templates / pages (if in scope) — documented patterns and when to use them
- Behavior notes — keyboard shortcuts, interaction patterns, state handling (hover, focus, disabled, loading, error)
- What is explicitly out of scope — stated so the tool does not improvise

**Dimensions addressed:** Dim 5. Informs Dim 6.

**Authored by:** `design-author`. **Feeds into:** `design-encode`.

### 5.4 Claude Design Configuration (`claude-design-config.md`)

The machine-readable projection for Claude Design. Structured after Anthropic's `<frontend_aesthetics>` cookbook pattern.

**Contains:**
- System prompt block (paste-ready) — condensed Look/Feel/Act direction, explicit default overrides, named anchors
- Default-override block — the single most important section; names the cream/serif/terracotta defaults and replaces them with the user's direction
- Inputs manifest — what screenshots, codebase links, Figma files, brand assets to upload at the start of a Claude Design session and why
- First-prompt template — a tested starting prompt for the first Claude Design interaction on this project
- Iteration guidance — what to adjust via chat vs. inline comments vs. sliders, based on the defined direction

**Dimensions addressed:** Dim 6.

**Authored by:** `design-encode`. **Projected from:** Design Brief and Design System Spec.

### 5.5 Design Instructions (`design-instructions.md`)

The human-readable companion to the tool config. How to use Claude Design effectively for this specific project.

**Contains:**
- Pre-session checklist — what to have open, uploaded, or linked before opening Claude Design
- Prompt patterns that work for this project — concrete examples grounded in the brief
- When to steer vs. let Claude run — based on the defined direction and density of spec
- Default-drift watchlist — the specific house-style habits that will creep back in without active pushback (serif headings, cream wash, centered layouts)
- Handoff notes — what to do with Claude Design output next (ship, send to Claude Code, export to Figma, hand to dev)

**Dimensions addressed:** Dim 6.

**Authored by:** `design-encode`.

---

## 6. The Four Design Canon Sources

Each source is embedded at a specific layer of the system. They are not competing frameworks — each earns its place at a different moment.

### 6.1 IDEO / design thinking — embedded in `design-discover`

IDEO's Empathize → Define sequence shapes how `design-discover` starts. Before any look, feel, or act question, the skill establishes:

- Who is this design for (audience, in their language, not the company's)
- What situation are they in (context, stage, emotional state)
- What problem are they trying to solve (outcome, not feature)
- Why now (what changed)

This grounds Dim 1 (Problem Definition). A brief written without this grounding is a stylistic exercise rather than a design response.

### 6.2 NN/g usability heuristics — embedded in `design-author`

Nielsen Norman Group's 10 usability heuristics act as quality gates in the Act Direction section of the Design Brief. For any interactive product, the brief must name which heuristics are load-bearing for the use case:

- Form-heavy product → error prevention, flexibility and efficiency of use
- Dashboard → visibility of system status, recognition rather than recall
- First-use product → help and documentation, aesthetic and minimalist design
- Destructive-action product → error prevention, user control and freedom

The skill does not require all ten. It requires the user to name the three or four that matter most, with a sentence on what they imply for the design.

### 6.3 Atomic design (Brad Frost) — embedded in `design-author` at the system spec step

Brad Frost's atomic design taxonomy (atoms → molecules → organisms → templates → pages) is used only to scope the design system. The system picks the atomic level to specify and stops there.

This prevents the most common failure mode in design system work: specifying a full system when the product needs tokens, or specifying tokens when the product needs a full system. Both directions waste time and produce briefs that cannot be executed at the right level.

Default recommendations by project type:

| Project | Default scope |
|---------|--------------|
| Pitch deck, landing page, one-off marketing asset | Tokens only |
| Marketing site with 3-10 pages | Tokens + atoms |
| Web app with 10-50 screens | Tokens + atoms + molecules |
| Product with multiple surfaces, multi-team build | Full system |

### 6.4 Anthropic `<frontend_aesthetics>` cookbook — embedded in `design-encode`

Anthropic's Prompting for Frontend Aesthetics cookbook is the operational spine of `design-encode`. Its three strategies are treated as non-negotiable in every Claude Design configuration produced:

1. **Guide specific design dimensions individually.** The config names typography, color, motion, spacing, density as separate fields — never collapsed into a single adjective.
2. **Reference design inspirations, not abstractions.** The config names products, IDE themes, cultural aesthetics (solarpunk, cyberpunk, brutalism, Swiss modernism, Memphis, Bauhaus). "Modern and clean" is never allowed as a final anchor.
3. **Explicitly call out defaults to avoid.** Every config includes an override block. At minimum: "Not cream. Not serif display. Not terracotta. Not Inter. Not purple gradients on white." Specific to the project's actual direction, of course.

The cookbook's paste-ready `<frontend_aesthetics>` system prompt is the skeleton for the Claude Design Configuration artifact.

---

## 7. Where a Facilitator Adds Value

`design-please` is designed to be self-serve usable. Every skill produces useful output when run independently by a motivated non-designer. But some dimensions benefit significantly from a facilitator — especially one with taste and a library of references.

| Dimension | Self-serve quality | With facilitator |
|-----------|-------------------|------------------|
| 1 (Problem Definition) | Good; the questions are objective | Facilitator catches vague scopes and pushes for specificity earlier |
| 2 (Look Direction) | Adequate; users default to familiar anchors | Facilitator expands the reference library and spots when the chosen anchor doesn't fit the problem |
| 3 (Feel Direction) | Weakest self-serve dimension | Facilitator hears the gap between what the user says and what they mean, and can offer references from outside the user's exposure |
| 4 (Act Direction) | Adequate for simple products; poor for complex ones | Facilitator brings interaction pattern library, spots when "fast" conflicts with "thorough" |
| 5 (Design System Scope) | Good; the atomic taxonomy is self-explanatory | Facilitator keeps the user from overbuilding; the default answer is usually "less than you think" |
| 6 (Handoff Readiness) | Strong; encoding is systematic | Facilitator adds tool-specific tricks and watches for drift patterns |

The highest-value facilitation is on Dim 3 (Feel Direction). Most non-designers can describe look (by pointing to products they've seen) and act (by describing frustration), but they lack the vocabulary for feel. A facilitator who can name cultural references, historical movements, and emotional registers accelerates feel direction by an order of magnitude.

---

## 8. Design Principles

**The brief is the unit of work, not the design.** The output of `design-please` is not a visual artifact. It is a brief that produces visual artifacts when fed to Claude Design, a designer, or a developer. Users who conflate the two end up trying to make `design-please` do Claude Design's job, and it will not.

**Specificity beats abstraction, always.** "Clean and modern" is not a brief. "IBM Plex Sans for body, Clash Display for headlines, monochrome with a single coral accent, 8px spacing grid, compact density, Linear-style information hierarchy" is. Every dimension must land at this level of specificity or it is not done.

**Reference anchors are the fastest path to specificity.** Non-designers cannot produce specific visual language from scratch. But they can point to 3-5 products whose design resonates. The skill's job is to extract what specifically resonates from each anchor and compose those patterns into direction.

**Anti-references are half the brief.** What to avoid is as load-bearing as what to reach for, especially with Claude Design's strong default house style. A brief that names five anchors and no anti-anchors will drift toward the tool's defaults. A brief that names three anchors and three anti-anchors will hold.

**Override the defaults explicitly.** Claude Design has a strong cream-serif-terracotta bias. For most products (dashboards, dev tools, fintech, healthcare, enterprise), the bias is wrong. The override must be in the config, in writing, for every project that isn't actually hospitality/editorial/portfolio.

**Teach when you choose.** When the user signals they don't have an opinion on a dimension, the skill offers to choose, names the choice, cites the reasoning, and names the alternatives considered. This compounds: a user who runs `design-please` three times finishes more articulate than they started.

**Atomic scope is a judgment call, not an aspiration.** The temptation is always to specify the full system. Resist it. Most projects need tokens or tokens+atoms. Full systems are for products with multiple surfaces and multiple teams, not for a landing page.

**Design-please is tool-agnostic; the encoder is not.** The Design Brief and Design System Spec are written without reference to any specific tool. The Claude Design Configuration is a projection of the brief for one tool. When a new tool arrives (Figma AI, Vercel v0, whatever comes next), a new encoder can be written against the same unchanged brief.

**Briefs decay.** Design direction drifts as the product grows, the audience shifts, and the founder's taste evolves. Without refresh cycles, a brief has a half-life of about 9-12 months. The V2 maintenance loop (validate → refresh → re-author → re-encode) is what keeps briefs useful past the first six months.
