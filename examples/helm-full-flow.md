# Example: Helm Treasury — Full Design System Flow

A fictional walkthrough showing the complete `design-please` flow for a US B2B fintech company. Helm Treasury is a 22-person cash-management analytics tool for mid-market treasury teams, Series A, based in New York. The founder, Julia Okafor, is a former corporate treasurer turned founder — strong domain expertise, no design background.

---

## Starting Point

Julia opens `design-please` in Claude Code and says:

> "I've been prompting Claude Design for our dashboard UI for a week and every output comes back with cream backgrounds and serif fonts. We're a fintech tool — our users are treasurers looking at cash positions across 40 bank accounts. This looks like a boutique hotel brochure. I don't know how to describe what I want. I just know this isn't it."

### Mapping the challenge

(Note: `design-recommend` is V2. For the V1 walkthrough, the skill sequence is chosen conversationally.)

**Presenting problem:** Claude Design house-style drift + user can't articulate direction
**Primary dimensions:** Dim 2 (Look Direction), Dim 3 (Feel Direction), Dim 6 (Handoff Readiness — missing default override)
**Secondary:** Dim 1 (Problem Definition — scope of the dashboard isn't specified yet)

> "This is a two-part problem. First, we need to give you direction you can actually point Claude Design at — which means working through Look, Feel, and Act specifically, not just 'fintech dashboard.' Second, we need to encode explicit overrides of Claude Design's defaults, because they're going to keep showing up until you rule them out in writing. Let's run `design-discover` first to pin down direction, then `design-author` to write the brief, then `design-encode` to produce a Claude Design config you can paste at the top of every session."

---

## Phase 1: design-discover

### Phase 1.1: Scope the engagement

Three questions up front:

- **What are you designing?** → "The Helm product dashboard. Specifically the Cash Position screen — that's our home screen, it shows total cash across all accounts, trends, and alerts. Then downstream: Account Detail and Transfer Builder screens."
- **Is there existing design work?** → "Three screens I've been generating in Claude Design. They all look wrong. Also a logo and wordmark our brand agency designed last year — those are fine."
- **What is the handoff target?** → "Claude Design. I don't have a designer. I'm not hiring one yet."

Handoff target confirms we'll need aggressive default overrides.

### Phase 1.2: Problem framing interview (Dim 1)

**Audience.** "Our primary user is a treasurer at a mid-market company — $100M to $1B revenue. She is managing cash across 15 to 40 bank accounts. Her job is making sure the company has enough cash everywhere it needs to be, moving money around to avoid overdrafts and maximize yield, and reporting the position to the CFO daily. She opens our product once or twice a day, spends 5-15 minutes, and wants to see her numbers without clicking around."

Verbatim captured.

**Use case and scope.** "In scope: Cash Position screen, Account Detail screen, Transfer Builder screen. Out of scope: onboarding, admin, reporting exports — we have those on static pages and they can stay ugly for now."

**Stage and constraints.** "Series A, MVP in market. Stack is React + Tailwind. Accessibility: we need WCAG AA because our buyers are enterprise and it comes up in security reviews. No brand guidelines beyond the logo. The logo is navy and a single accent color — #0A3161 navy, #14B8A6 teal."

**Success criteria.** "A treasurer opens the Cash Position screen and in 10 seconds knows: total cash, whether any account is below its minimum, and whether anything unusual happened in the last 24 hours. Fast scan, no squinting."

### Phase 1.3: Existing assets audit

Julia shares the three Claude Design outputs. Extracted patterns:

| Pattern | Finding |
|---------|---------|
| Typography | Playfair Display for headings, Inter for body — classic Claude Design default |
| Color | Cream background (#F4F1EA), terracotta accents, warm neutrals |
| Spacing | Generous, magazine-like — 32px+ between sections |
| Layout | Centered hero pattern at the top of each screen |
| Density | Low. Two numbers per viewport height. |
| Imagery | Italic quote pulled from "treasurer testimonials" (fabricated by Claude Design) |
| Motion | Gentle fade-ins |

**User reaction:** "This looks like a B2B SaaS landing page from 2022. It's wrong in every possible way for what we do. Treasurers don't want to scroll to see their cash position."

### Phase 1.4: Anchor interview

#### Look anchors

Julia offers three products:

- **Linear** — "Clean, dense, no wasted space. I like the monochrome + one accent thing."
- **Bloomberg Terminal** — "I know it looks ugly. But it shows treasurers 10,000 things at once and they love it. Our users come from that world."
- **Vercel dashboard** — "The dark mode. Very technical-looking without being hostile."

Fetched screenshots. Extracted:

| Anchor | Typography | Color | Density | Layout | Patterns to borrow |
|--------|-----------|-------|---------|--------|-------------------|
| Linear | Inter (ruled out — too generic per Anthropic cookbook) or likely "IBM Plex Sans" adjacent | Monochrome + one blue-purple accent | Compact | Left-aligned, sidebar nav, data tables | Density, monochrome+accent, tight info hierarchy |
| Bloomberg Terminal | Monospace-heavy, technical sans | Black + orange/amber on black | Extreme — 30+ numbers visible at once | Multi-pane, keyboard-driven | Information density, keyboard-first behavior, "more on screen" philosophy — but modernized |
| Vercel dashboard | Geist Sans | Black/near-black + single accent (green or violet) | Balanced — not as dense as Linear, denser than most SaaS | Left nav + main + optional right panel | Monochrome confidence, dark-mode as default, subtle accent |

Julia has no opinion on exact typography. Applying the teach-through-choice pattern:

> "I'd propose **IBM Plex Sans** for body at 14px/1.5 and **JetBrains Mono** for numbers and table data. Rationale: IBM Plex reads as technical without being cold — it's widely used in fintech and enterprise (and IBM uses it for their own dashboards, so it has credibility). JetBrains Mono for numbers makes them align in tables and gives the Bloomberg-adjacent technical feel you're reaching for without being as punishing as an actual monospace body font. Alternatives I considered: Geist (too 2024-startup-generic, would make you look like every YC company), Inter (ruled out by Anthropic cookbook — too default), Söhne (great but commercial license adds cost)."

Julia confirms.

**Color:** Julia wants the navy and teal from her logo to show up. Proposal: monochrome base (near-black on near-white OR inverted to dark mode) + teal as the only interactive accent. Navy reserved for logo/brand surfaces only, not UI chrome. Proposal accepted.

**Dark mode:** Proposed as primary, light mode as secondary — Bloomberg and Vercel anchors both lean dark; treasurers often work in low-light conditions; dark mode reduces eye strain on long data-viewing sessions. Accepted.

#### Feel anchors

Julia struggles. "It should feel... serious? Competent? Not cheerful."

Offering references:

- **Posthog** — dry, confident, a little dry humor, product-first
- **Plaid's public API docs** — calm authority, not selling, just informing
- **Bloomberg Terminal (again)** — unapologetically technical, assumes the user is an expert

Julia picks Posthog ("that dryness — no exclamation points, no 'amazing'") and Plaid docs ("calm, authoritative, doesn't oversell"). Rules out anything "cheerful" — "our users are moving millions of dollars. 'You're doing great!' messaging is infantilizing."

**Extracted feel register:** Dry, confident, understated, technical, slightly austere. Treats the user as a peer professional. Never cheerful, never uses exclamation points, never offers encouragement.

#### Act anchors

- **Motion posture:** Utility only. "Data updates should not animate in. I want the number to just change."
- **Interaction anchor:** Linear — keyboard-first, fast, reversible actions, no confirmation dialogs for actions that can be undone.
- **Density:** High. "Treasurers would rather see 30 things on one screen than click through three screens."
- **UI intelligence:** Passive with surfaced alerts. "We can highlight an account that's below minimum, but don't auto-suggest transfers. The user decides."
- **Confirmation pattern:** Confirm transfers (irreversible + moves money), not view state or sort changes.
- **Input modality:** Keyboard-first (power users, desktop primary), pointer parity, no mobile priority in V1.

**Relevant NN/g heuristics proposed:**
- Visibility of system status (cash numbers must be current and say so)
- Recognition rather than recall (show all accounts, don't make users remember codes)
- Error prevention (transfer flow must prevent overdrafts, mis-routed amounts)
- Aesthetic and minimalist design (treasurer density tolerance is high but noise must be removed — show what she needs, nothing else)

### Phase 1.5: Anti-anchor interview

Julia's visual anti-anchors:
- Any fintech that uses "friendly" illustrations of coins and piggy banks
- Pastel palettes, pink and baby blue
- Purple gradients
- Hero-section marketing patterns on product screens
- Anything with mascots

Fintech-specific anti-anchors by name:
- QuickBooks — "too small-business, too cartoonish"
- Mint — "looks fine for consumers, wrong for us"
- "Any budgeting app" — "we are not a budgeting app"

**Claude Design default overrides.** Walked through the defaults; Julia rules each out with vigor:

- ❌ Cream / off-white backgrounds — "god no"
- ❌ Serif display — "no, treasurers don't want to read the Wall Street Journal op-ed page"
- ❌ Italic accents — "these are financial numbers, not poetry"
- ❌ Terracotta / amber — "would look like we're fall-themed. Teal only."
- ❌ Inter, Roboto, system defaults — per cookbook
- ❌ Purple gradients on white — "not a crypto product"

### Phase 1.6: Synthesis sentence

Proposed to Julia:

> "**Look like Linear/Vercel in dark mode with Bloomberg density. Feel like Posthog crossed with Plaid docs — dry, confident, technical, unsentimental. Act like Linear — keyboard-first, fast, reversible, opinionated defaults, density over whitespace.**"

Julia: "Yes. That's exactly it."

### Phase 1.7: Dimension assessment

| # | Dimension | Score | Key Finding |
|---|-----------|-------|-------------|
| 1 | Problem Definition | 4/5 | Audience, scope, stage, success criteria all specific. Constraints clear. |
| 2 | Look Direction | 3/5 | Strong anchors, typography decided, color strategy decided, defaults overridden — but density specifics and layout grid not yet pinned. |
| 3 | Feel Direction | 4/5 | Anchors named with extracted register. Anti-register explicit. |
| 4 | Act Direction | 3/5 | Motion, interaction, density, heuristics named — but specific interaction patterns (sort, filter, keyboard shortcuts) not yet spec'd. |
| 5 | Design System Scope | 1/5 | Not yet chosen. To be done in `design-author`. |
| 6 | Handoff Readiness | 2/5 | Direction is forming but no brief or config exists yet. |

### Phase 1.8: Discovery Brief handoff

Discovery Brief is written to `discovery-brief.md`. Julia reads it and confirms the synthesis sentence and the anchor extractions. One open question: "Does dark mode primary mean we don't build a light mode at all, or is light mode derived?" Flagged for `design-author`.

---

## Phase 2: design-author

### Phase 2.1: Read Discovery Brief

Discovery Brief loaded. Direction is strong on Dims 1-4. Dim 5 (system scope) needs to be chosen. Dim 6 specifics need to be filled in.

### Phase 2.2: Fill gaps

**Design system scope.** Teach-through-choice:

> "For a product with three connected screens in V1 but more coming (you mentioned Reports, Settings, Admin later), I'd propose **tokens + atoms + molecules** — not the full system yet. Tokens give you color, type, spacing, motion truth. Atoms give you buttons, inputs, chips, tags, badges at the level of detail Claude Design needs to not improvise. Molecules give you the cards, toolbars, table rows, and nav items you'll repeat across Cash Position, Account Detail, and Transfer Builder. I'd leave organisms/templates for later — you don't need a documented 'dashboard layout' pattern yet because you only have one dashboard. Alternatives: tokens only (too thin — Claude Design will invent components every time); full system (overbuilt for 22-person company with three screens). Okay?"

Julia confirms tokens + atoms + molecules.

**Dark mode / light mode decision:** Dark primary, light derived from the same tokens with inverted neutrals. Teal accent stays identical. Proposal accepted.

**Layout grid:** Proposed 4px baseline, 8px spacing rhythm, 12-column grid on >1280px viewport, single-column under 1024px. Accepted.

**Keyboard shortcuts:** Deferred to the spec, since this is implementation-level. Recorded as out-of-scope for V1 brief.

### Phase 2.3: Outline and confirm

Presented the Design Brief outline and System Spec outline. Julia approves with one change: "Add 'no fractional cents displayed — always whole dollars with thousands separators' as a constraint. We've had bugs where we rendered $1,234.56789 and a treasurer flagged it."

### Phase 2.4: Design Brief written

`design-brief.md` written. Key sections (abbreviated):

```markdown
# Design Brief: Helm Treasury Dashboard

Date: 2026-04-18
Handoff target: Claude Design
Status: approved

## 1. Problem Definition

### Audience
"A treasurer at a mid-market company — $100M to $1B revenue. Managing cash across 15-40 bank accounts. Opens the product 1-2x per day, spends 5-15 minutes, wants to see her numbers without clicking around." — Julia Okafor, 2026-04-18

### Use case in scope
- Cash Position screen (home)
- Account Detail screen
- Transfer Builder screen

### Out of scope
- Onboarding, admin, reporting exports

### Stage
Series A, MVP in market

### Constraints
- Tech stack: React + Tailwind
- Accessibility: WCAG AA
- Brand: #0A3161 navy (logo surfaces only), #14B8A6 teal (interactive accent)
- Data display: always whole dollars with thousands separators, never fractional cents

### Success criteria
Treasurer opens Cash Position and within 10 seconds knows: total cash, any account below minimum, anything unusual in the last 24 hours.

## 2. Look Direction

### Typography
- Body: IBM Plex Sans, 14px / 1.5
- Headings: IBM Plex Sans, 600 weight, 3x scale jump (body 14 → H2 28 → H1 48)
- Numerical data: JetBrains Mono, 14px / 1.4 (aligns in tables, technical feel)
- Rationale: IBM Plex reads as technical-but-warm and is credibly fintech (IBM uses it for Watsonx dashboards). JetBrains Mono for numbers matches the Bloomberg-adjacent technical anchor without punishing body readability.

### Color strategy
- Palette: Monochrome + single accent
- Primary mode: dark
  - Background: oklch(14% 0.01 240)  /* near-black with faint cool cast */
  - Surface: oklch(18% 0.01 240)
  - Elevated surface: oklch(22% 0.01 240)
  - Text primary: oklch(95% 0.01 240)
  - Text secondary: oklch(65% 0.01 240)
  - Accent (teal): oklch(72% 0.14 180)
  - Semantic: success oklch(70% 0.15 145), warning oklch(80% 0.16 75), error oklch(65% 0.22 25)
- Light mode: derived; invert neutrals; accent unchanged
- Dark mode is primary; light mode is secondary

### Spacing and density
- Base unit: 4px
- Rhythm: 4, 8, 12, 16, 24, 32, 48
- Density: compact — tables at 32px row height, sections separated by 16-24px, no generous marketing-style whitespace
- Rationale: treasurers trade whitespace for density; Bloomberg anchor implies more data per viewport

### Layout
- 12-column grid >1280px; single column <1024px
- Left sidebar nav (fixed), main content area, optional right panel (for transfer builder and alerts)
- Left-aligned — no centered hero patterns

### Imagery
None. This is a data product.

### Iconography
Outline, 1.5px stroke, 20px default size, Lucide or Heroicons. No filled icons.

### Anchors
- **Linear:** tight spacing, monochrome + single accent, left-aligned information hierarchy, no hero patterns
- **Vercel dashboard:** dark-mode default, calm neutrality, subtle accent
- **Bloomberg Terminal:** density philosophy — "show more on screen" — modernized; monospace for numbers

### Anti-anchors
- Any fintech with coin/piggy bank illustrations
- Pastel palettes, pink/baby blue
- Purple gradients
- Hero sections on product screens
- Mascots, emoji in UI
- QuickBooks, Mint aesthetic (cartoonish, small-business-friendly)

### Default overrides (Claude Design)
- ❌ Cream / off-white backgrounds
- ❌ Serif display (Georgia, Fraunces, Playfair)
- ❌ Italic accents
- ❌ Terracotta / amber palette
- ❌ Inter, Roboto, system defaults
- ❌ Purple gradients on white
- ❌ Centered hero compositions
- ❌ Fabricated testimonials or pull-quote imagery

## 3. Feel Direction

### Emotional register
Dry, confident, understated, technical, slightly austere. Treats the user as a peer professional. Never cheerful. No exclamation points. No encouragement messaging. Doesn't oversell.

### Anchors
- **Posthog:** dry confidence, no-hype, product-first
- **Plaid docs:** calm authority, informative not promotional

### Anti-anchors
- "You're doing great!" messaging — infantilizing to a treasurer moving millions
- Emoji in UI copy
- Marketing-voice exclamations

## 4. Act Direction

### Motion posture
Utility only. State changes happen; they do not animate in. Loading uses a subtle progress indicator, not a spinner. No ambient motion.

### Interaction anchors
- **Linear:** keyboard-first, command palette, reversible actions without confirmation, fast

### Density expectations
30+ data points per viewport on Cash Position. Table rows at 32px height. No whitespace for whitespace's sake.

### UI intelligence
Passive with surfaced alerts. Highlight accounts below minimum; do not auto-suggest transfers.

### Confirmation pattern
- Transfers: confirm (irreversible, moves money)
- View/sort/filter: no confirmation (reversible)

### Input modality
Keyboard-first, pointer parity. Mobile is not prioritized in V1.

### NN/g heuristics
- **Visibility of system status:** every number must show its currency, asOf timestamp, and update state
- **Recognition rather than recall:** all 40 accounts visible with search; never require the user to remember account codes
- **Error prevention:** Transfer Builder prevents overdrafts and mis-routed amounts before confirmation
- **Aesthetic and minimalist design:** treasurer density tolerance is high, but noise must be removed

## 5. Design System Scope

**Chosen:** tokens + atoms + molecules

**Rationale:** Three V1 screens with more coming (Reports, Settings, Admin). Tokens give color/type/spacing/motion truth. Atoms give buttons, inputs, chips. Molecules give repeated patterns (table rows, cards, toolbars, nav items). Full-system (organisms/templates) is overbuilt at 22 people / three screens.

**Out of scope at this level:** organism patterns (App Shell, Dashboard Template), full theme switcher UI.

## 6. Handoff Target

**Target:** Claude Design

**Inputs:**
- design-brief.md (this file)
- design-system-spec.md
- 3 Linear screenshots (Issues view, Roadmap, Project detail)
- 2 Vercel dashboard screenshots (deployments, analytics)
- 1 Bloomberg Terminal screenshot (FX monitor) for density reference
- Tailwind config with OKLCH tokens (forthcoming; derived from design-system-spec)

## 7. Teach-Through-Choice Log

| Dimension | Choice | Reasoning | Alternatives considered |
|-----------|--------|-----------|------------------------|
| Typography | IBM Plex Sans + JetBrains Mono | Technical-but-warm; IBM fintech credibility; monospace numbers align in tables; avoids Inter default | Geist (generic), Inter (ruled out), Söhne (license cost) |
| Dark mode primary | Yes, light derived | Anchors (Vercel, Bloomberg) are dark; low-light viewing common for treasurers | Light primary (rejected — conflicts with Bloomberg anchor and audience context) |
| System scope | Tokens + atoms + molecules | Three screens now, more later; avoids improvisation without overbuilding | Tokens only (too thin), full system (overbuilt) |
```

### Phase 2.5: Design System Spec written

`design-system-spec.md` written at the chosen scope. Token section pulls directly from the brief. Atoms section covers Button, Input, Select, Checkbox, Radio, Switch, Chip, Tag, Badge, Tooltip, Icon — each with variants, states, and token bindings. Molecules section covers Table row, Card, Toolbar, Nav item, Modal, Alert banner. Organisms are explicitly out of scope.

Sample excerpt:

```markdown
### Button

**Variants:**
- Primary — teal accent fill, near-black text; for singular primary action per screen
- Secondary — surface background, text-primary foreground; for secondary actions
- Ghost — transparent background, text-primary; for tertiary and toolbar actions
- Danger — error semantic fill, near-black text; for destructive actions only

**Sizes:**
- sm — 28px height, 12px horizontal padding
- md — 32px height, 16px horizontal padding (default)
- lg — 40px height, 20px horizontal padding (rare)

**States:** default, hover (+4% luminance on background), focus (2px teal outline with 2px offset, outside element), active (-4% luminance), disabled (50% opacity, no hover/active), loading (spinner in place of label)

**Accessibility:** 44x44px minimum tap target on touch; keyboard-operable; aria-busy on loading

**Token bindings:** padding, height use spacing scale; color uses semantic tokens; focus ring uses --focus-ring
```

### Phase 2.6: Present and hand off

Julia reviews. "This is the first time I've seen anything that matches what I was trying to tell Claude Design." Approves. Next step: `design-encode`.

---

## Phase 3: design-encode

### Phase 3.1: Read Definition artifacts

Loaded `design-brief.md` and `design-system-spec.md`. No generic sections — brief is ready to encode.

### Phase 3.2: Determine target and format

- Target: Claude Design
- Format: both (paste-ready config + instructions)

### Phase 3.3: Claude Design Configuration written

`claude-design-config.md` produced. Key content (abbreviated):

```xml
<frontend_aesthetics>

<audience_and_context>
Treasurers at $100M-$1B revenue companies managing cash across 15-40 bank accounts. Desktop power users, 1-2 sessions per day of 5-15 minutes. Expert audience; no hand-holding. WCAG AA required.
</audience_and_context>

<defaults_to_avoid>
This is a fintech cash-management dashboard. Do NOT use the default aesthetic. Specifically:
- NOT cream / off-white backgrounds (~#F4F1EA)
- NOT serif display (no Georgia, Fraunces, Playfair)
- NOT italic accents
- NOT terracotta / amber palette
- NOT Inter, Roboto, Open Sans, Lato, system defaults
- NOT purple gradients
- NOT centered hero compositions on product screens
- NOT coin / piggy bank illustrations
- NOT pastel palettes, pink or baby blue
- NOT mascots, emoji in UI, exclamation points in copy
- NOT fabricated testimonials or pull-quote imagery
- NOT QuickBooks / Mint / consumer-fintech visual language
</defaults_to_avoid>

<typography>
- Body: IBM Plex Sans, 14px / 1.5, weight 400 for body / 500 for emphasis / 600 for headings
- Headings: IBM Plex Sans, weight 600, 3x scale jump (H3 20 → H2 28 → H1 48)
- Numerical data: JetBrains Mono, 14px / 1.4 (tables, metrics, amounts)
- No italic anywhere
</typography>

<color>
Palette: monochrome + single teal accent.
Dark mode primary, light mode derived.

Dark mode (primary):
- bg: oklch(14% 0.01 240)
- surface: oklch(18% 0.01 240)
- surface-elevated: oklch(22% 0.01 240)
- text: oklch(95% 0.01 240)
- text-muted: oklch(65% 0.01 240)
- accent: oklch(72% 0.14 180)  /* teal — only interactive color */
- success: oklch(70% 0.15 145)
- warning: oklch(80% 0.16 75)
- error: oklch(65% 0.22 25)

Light mode: invert neutrals to near-white base, keep teal accent identical.

Navy #0A3161 is reserved for logo/brand surfaces only; not UI chrome.
</color>

<spacing_and_density>
Base unit: 4px. Spacing scale: 4, 8, 12, 16, 24, 32, 48.
Density: compact. Table rows 32px height. Section gaps 16-24px. No generous whitespace.
</spacing_and_density>

<layout>
12-column grid on viewports >1280px; single column <1024px.
Left sidebar nav (fixed), main content area, optional right panel.
Left-aligned. Never centered hero patterns.
</layout>

<imagery>
None. This is a data product. Do not add illustrations, stock photography, or decorative imagery.
</imagery>

<iconography>
Outline only, 1.5px stroke, 20px default, Lucide or Heroicons. No filled icons.
</iconography>

<motion>
Utility only. No ambient motion. No animated entrances for data. Numbers change instantly on update. Loading uses subtle progress indicators, not spinners on top of content.
</motion>

<inspirations>
Draw from:
- Linear — tight spacing, monochrome + single accent, left-aligned info hierarchy, no hero patterns
- Vercel dashboard — dark-mode default, calm neutrality, subtle accent
- Bloomberg Terminal — density philosophy (modernized), monospace numbers

Feel: dry, confident, understated, technical, slightly austere. Treats the user as a peer professional. Unsentimental.

Act: keyboard-first, fast, reversible actions without confirmation (except transfers), opinionated defaults.
</inspirations>

<anti_inspirations>
Avoid the language of:
- QuickBooks, Mint (consumer / small-business cartoonish)
- Generic fintech with coin and piggy-bank illustrations
- Marketing-site hero sections
- "Friendly" AI assistant chrome with sparkles
</anti_inspirations>

<interaction_and_behavior>
Input: keyboard-first, pointer parity, mobile not prioritized.
Density: 30+ data points per viewport on Cash Position.
UI intelligence: passive with surfaced alerts. Highlight accounts below minimum; do not auto-suggest transfers.
Confirmation: transfers confirm (moves money); sort/filter/view do not confirm (reversible).
NN/g heuristics load-bearing: visibility of system status, recognition rather than recall, error prevention, aesthetic and minimalist design.
</interaction_and_behavior>

<design_system_scope>
Tokens + atoms + molecules. See design-system-spec.md for full specification. Do not improvise components beyond the stated scope. If a pattern isn't specified, ask before generating.
</design_system_scope>

</frontend_aesthetics>
```

**First-prompt template:**

```
I'm designing the Cash Position screen for Helm Treasury — the home dashboard of a cash management tool for corporate treasurers.

Audience: a treasurer managing cash across 15-40 bank accounts, wants to see total cash, any account below minimum, and anything unusual in the last 24 hours within 10 seconds of opening.

What I need first: a wireframe of the Cash Position screen at 1440px width, dark mode, showing:
- Left sidebar nav (Cash Position highlighted, Accounts, Transfers, Reports below)
- Top section: total cash metric, 24h change, any active alerts
- Main section: accounts table — name, bank, balance, 24h change, status indicator
- Right panel: alert detail when an account is selected

Use the aesthetic direction in the <frontend_aesthetics> block above. Reference the uploaded Linear and Vercel screenshots for visual language. Reference the uploaded Bloomberg screenshot for density expectation — our product should feel dense in that direction, not as dense.

Before you start, state in writing: (a) typefaces (confirm IBM Plex Sans + JetBrains Mono), (b) color tokens in OKLCH (confirm the dark palette), (c) spacing unit (confirm 4px base), (d) motion posture (confirm utility only). I'll confirm or correct, then you can generate.
```

**Drift watchlist:**

| Drift pattern | Corrective prompt |
|---------------|-------------------|
| Cream background creeps in on a card or elevated surface | "All surfaces use the declared dark neutrals. No cream anywhere, including elevated surfaces." |
| Serif appears for a large metric | "All typography is IBM Plex Sans or JetBrains Mono. No serif anywhere." |
| Teal accent shifts toward purple or blue | "Accent is oklch(72% 0.14 180) — teal, not purple or blue." |
| Emoji appears in copy or status indicators | "No emoji. Use icons from the iconography spec (outline, 1.5px stroke) for status." |
| Marketing hero appears at top of screen | "No hero sections. Cash Position opens to the data, not a welcome banner." |
| A mascot / illustration appears | "No illustrations. This is a data product." |
| Sentence case copy adds exclamation points | "No exclamation points. Copy is declarative, not celebratory." |

### Phase 3.4: Design Instructions written

`design-instructions.md` produced. Covers pre-session checklist, prompt patterns for common tasks (new screen, tightening, adding a component), when to steer vs. let Claude run, and when to stop and re-ground.

### Phase 3.5: Hand off

Julia pastes the `<frontend_aesthetics>` block, uploads the anchor screenshots, and runs the first-prompt template.

**Claude Design's planning declaration:**
> Typography: IBM Plex Sans for body and headings, JetBrains Mono for numeric data. ✓
> Color: dark mode, oklch(14% 0.01 240) background, teal accent at oklch(72% 0.14 180). ✓
> Spacing: 4px base, compact density. ✓
> Motion: utility only, no ambient motion, instant number updates. ✓

Julia confirms. Claude Design generates the wireframe. Cash Position screen comes back dark, dense, monochrome with a single teal accent line under alert indicators, no hero, no cream, no serifs.

Julia: "That took six minutes including the planning step. Last week I spent three days fighting the defaults."

---

## Outcome

After one session, Helm Treasury has:

- `discovery-brief.md` — raw inputs, anchors, dimension scores
- `design-brief.md` — canonical direction on all six dimensions
- `design-system-spec.md` — tokens + atoms + molecules at the chosen scope
- `claude-design-config.md` — paste-ready Claude Design configuration
- `design-instructions.md` — how to use Claude Design on this project

**Dimension scores after engagement:**

| # | Dimension | Before | After |
|---|-----------|--------|-------|
| 1 | Problem Definition | 2 | 4 |
| 2 | Look Direction | 1 | 4 |
| 3 | Feel Direction | 1 | 4 |
| 4 | Act Direction | 1 | 4 |
| 5 | Design System Scope | 1 | 3 (atoms + molecules specified; organisms deferred) |
| 6 | Handoff Readiness | 1 | 4 |

The engagement moved Julia from "I know what's wrong but I can't describe what I want" to a brief that produces on-target Claude Design output from the first prompt. Follow-up is expected in 6-9 months to add organism-level patterns as screens multiply.

---

## What this example illustrates

1. **Default overrides do the heaviest lifting.** The explicit "not cream, not serif, not italic, not terracotta" block is what closes the gap Julia was fighting.
2. **Specific anchors beat adjectives.** "Linear + Vercel + Bloomberg" plus extracted patterns is what Claude Design can act on. "Modern and clean" produced a week of wrong output.
3. **Teach-through-choice compounds.** Julia finished the session more articulate about typography, color strategy, and density than she started. Her next run will be faster.
4. **The brief is the unit of work, not the design.** Claude Design produced the wireframe; `design-please` produced the brief that made the wireframe right on the first try.
5. **Scope is a judgment call.** Tokens + atoms + molecules was the right answer; full system would have been overbuilt and tokens-only would have let Claude Design improvise components every session.
