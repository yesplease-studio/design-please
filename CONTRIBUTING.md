# Contributing to design-please

Thanks for your interest in contributing. `design-please` is open-source and the methodology improves fastest when practitioners share what they've learned from using it on real projects.

## Ways to contribute

- **Report field experience.** Ran `design-please` on a project and something was awkward, misleading, or blocking? Open an issue with the context, what was expected, and what happened.
- **Propose a skill or system improvement.** A new phase, a better question, a tighter quality standard, a missing canon source — open a PR with the change and a short rationale.
- **Add an example walkthrough.** Fictional is fine; real is better (with permission). Walkthroughs that cover different project types (marketing site, pitch deck, mobile, design system refresh) are especially valuable.
- **Extend the encoder.** V1 targets Claude Design. If you want to add an encoder for another AI design tool (Vercel v0, Lovable, Figma AI), the brief and system spec stay the same — only the encoder changes. PRs welcome.
- **Fix docs.** Typos, unclear phrasing, outdated references to Claude Design's capabilities — all appreciated.

## How to propose changes

1. Open an issue first if the change is substantial. Small fixes can go straight to PR.
2. Keep changes scoped. One skill or one artifact per PR is easier to review than a broad overhaul.
3. If your change affects the methodology (systems/design/SYSTEM.md), include the reasoning in the PR description. Methodology changes are deliberately slow to merge.
4. If your change affects a SKILL.md, note whether the change is additive (new phase or quality standard), modificative (rewording for clarity), or structural (reshaping the workflow). Structural changes get more scrutiny.

## Commit message convention

Match the rest of the please-family repos:

- `<skill-name>: <what changed>` — e.g., `design-encode: tighten default-override block`
- `design: <what changed>` — methodology changes in `systems/design/`
- `config: <what changed>` — profile template changes
- `docs: <what changed>` — README, SETUP, CONTRIBUTING
- `examples: <what changed>` — walkthroughs

Keep messages concrete. "Improve design-author" is not specific enough; "design-author: require typography family to be named, not categorical" is.

## What good contributions look like

- **Specific.** "Typography section is thin" is not actionable; "design-brief template's Typography section should require weights used, not just families" is.
- **Grounded in use.** "I ran this on a pitch deck and noticed the skill assumed interactive product" is worth more than speculation.
- **Honest about trade-offs.** If your change adds specificity at the cost of brevity, say so. If it helps one project type and is neutral for others, name both.
- **Match the tone.** Direct, practical, specific. No marketing voice. No jargon the reader won't recognize.

## What not to contribute

- Design opinions without methodology backing. `design-please` does not teach one aesthetic — it teaches scoping and encoding.
- Anthropic cookbook updates without citation. When the cookbook changes, PRs should link to the new version.
- Full new skills without discussion. The skill count is deliberately small. Open an issue first.

## Code of conduct

Be direct. Be specific. Assume good faith. Don't be precious about design as a topic — the field's worst instinct is treating it as mystical, and `design-please` exists to make it operational.

## License

All contributions are licensed under Apache 2.0 (same as the repo).
