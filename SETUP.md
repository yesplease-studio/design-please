# Setup Guide

This guide walks you through creating a project profile so `design-please` can give you tailored design direction work. Claude follows this flow during your first conversation.

The whole process takes 5-10 minutes.

---

## How onboarding works

### Step 1: Check for existing context

Ask the user:

> "Do you have any existing design assets you can share? A live product URL, screenshots, a Figma file, brand guidelines, a codebase, or prior Claude Design output?"

If they provide something:
- Read and extract what you can: what the product is, what the current design situation looks like, who it's for, what exists already
- Note what's covered and what's still missing
- Move to Step 2 to fill gaps

If they provide a URL, use `WebFetch` to retrieve the page. If they provide a codebase path (Tailwind config, CSS variables), read it for token fidelity. If they provide screenshots, analyze them for typography, color, spacing, density, layout patterns.

If they don't have anything, move straight to Step 2.

### Step 2: Fill gaps conversationally

Based on what's missing from Step 1, ask about:

**What the product is and who it's for**
- What does the product do, in plain language?
- Who is the design for? (Describe them in their own words — role, context, moment of use)
- What stage is the product at? (Pre-launch, MVP, scaling, mature, rebrand)

**Current design situation**
- Who carries design today? (A designer, a founder, a contractor, "nobody")
- Is there an existing design system? If so, at what scope — tokens, atoms, full?
- What is the build stack? (Tailwind, CSS variables, Figma variables, design tokens JSON, native)

**Handoff target**
- Which tool or target will consume the brief? (Claude Design, Figma, a human designer, dev direct)
- If Claude Design: are you already using it, or about to start?

**Aspirations and anti-aspirations**
- Are there products whose design you admire? (Named; any industry is fine)
- Are there products or patterns you want to avoid?
- Adjectives the team uses to describe desired direction?

**Constraints**
- Accessibility requirements (WCAG level, contrast, motion preferences)
- Brand elements that must be respected (logo, wordmark, trademark colors)
- Performance budgets (mobile weight, rendering targets)
- Timeline (when a first artifact is needed)

Don't ask all of these as a list. Have a conversation. Skip questions where the answer is already clear from Step 1. Follow up on interesting answers — especially on reference anchors, which are where most of the signal lives.

### Step 3: Write the profile

Once you have enough context, generate `config/profile.md` using the template from `config/_template.md`.

Before saving:
1. Show the user the complete profile
2. Ask if anything needs adjusting
3. Save only after confirmation

A useful profile beats a complete profile. Mark missing fields as "Unknown" or "Not discussed" rather than guessing.

### Step 4: Suggest next steps

After the profile is saved, suggest the natural next step based on what you've learned:

- **If they have anchors but no brief:** "Want to run `design-discover`? I'll work through Look / Feel / Act with you and produce a Discovery Brief that `design-author` can pick up."
- **If they have a brief but Claude Design is giving them house-style output:** "Want to jump straight to `design-encode`? I'll produce the paste-ready Claude Design configuration, including the default-override block that'll stop the cream/serif drift."
- **If they have nothing and are about to open Claude Design:** "Run `design-discover` first. Ten minutes now saves a week of fighting Claude Design's defaults later."

---

## Notes for Claude

- The goal is a useful profile, not a complete one. Missing fields are fine.
- If the user provides a URL, analyze it for design patterns (typography, color, spacing, density, layout, motion if a live product) — but don't treat the current site as the target direction. Many products were designed once and don't reflect the user's current taste.
- If the user provides screenshots, extract concrete patterns: named typefaces if recognizable (approximate if not, and say so), color strategy, density level, layout pattern.
- Keep the conversation natural. This is a getting-to-know-the-project session, not a form.
- If the user clearly has direction and just needs the encoder output, skip onboarding and go straight to `design-encode` — but confirm that a Design Brief exists first.
- The single most common onboarding pattern: "I've been prompting Claude Design for a week and keep getting cream backgrounds." When you hear this, acknowledge it's the default house style, and frame the profile as the first step toward overriding it.
