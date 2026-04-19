# design-please

An open-source, AI-native framework for scoping, defining, and encoding a company's visual and behavioral design direction.

Takes reference products, brand adjectives, existing screenshots, and codebases as raw material. Produces operational design artifacts: a discovery brief, a design brief, a design system spec, and a Claude Design configuration. Built to be used with [Claude Code](https://claude.ai/code) and [Claude Design](https://www.anthropic.com/news/claude-design-anthropic-labs).

Part of the [please family](https://github.com/yesplease-studio) of open-source frameworks from [Yes Please Studio](https://yesplease.studio).

## How it works

Most non-designers know good design when they see it, but can't describe what they want in terms a tool or designer can act on. The result: AI design tools like Claude Design default to their house style (cream backgrounds, serif display, terracotta accents) regardless of what the user actually needs — which is wrong for most products.

`design-please` produces **operational briefs** — structured documents that specify Look, Feel, and Act with enough precision that Claude Design, a designer, or a developer can execute from them.

The system works through three phases:

1. **Discover.** Interview for reference anchors and anti-anchors across Look/Feel/Act. Audit existing assets if any exist. Score maturity across six dimensions. When the user has no opinion, the system offers to choose and explains why.

2. **Author.** Synthesize discovery into a Design Brief (the canonical artifact) and a Design System Spec at the chosen atomic scope (tokens only, +atoms, +molecules, or full system).

3. **Encode.** Translate the brief into a paste-ready Claude Design configuration — including the explicit default-override block that keeps house-style drift from creeping back in.

## Who this is for

- **Founders and PMs** who are about to open Claude Design and don't know how to prompt it
- **Non-designers shipping product** who know what "wrong" looks like but can't articulate "right"
- **Consultants and facilitators** who need a structured framework for design direction work
- **Teams using AI design tools** who keep getting off-brief output and don't know why

## Quickstart

1. Open this repo in [Claude Code](https://claude.ai/code)
2. Claude will detect `CLAUDE.md.template` and load the system
3. If this is your first time, say **"help me set up"** and Claude will walk you through creating a project profile
4. Once your profile exists, try: **"I'm about to open Claude Design, get me ready"** or **"I know I want it to look like Linear but I don't know how to say that"**

Or skip the profile and go straight to a skill: **"run design-discover on this product"** with anchors and context.

## The six dimensions

`design-please` measures design-brief maturity across six dimensions. Each answers a core question.

| # | Dimension | Core question |
|---|-----------|---------------|
| 1 | Problem Definition | Is the design problem clearly scoped — audience, use case, stage, constraints? |
| 2 | Look Direction | Are visual anchors specified, tool defaults overridden, and concrete parameters named? |
| 3 | Feel Direction | Are emotional and tonal anchors specified with references and anti-references? |
| 4 | Act Direction | Are interaction patterns, motion, density, and UI intelligence specified? |
| 5 | Design System Scope | Has the atomic level been chosen — tokens, atoms, molecules, or full? |
| 6 | Handoff Readiness | Can Claude Design, a designer, or a developer execute from this brief? |

Each dimension is scored 1-5 (Not started → Leading). Most users start at 1-2 across most dimensions. That's normal. The framework helps you move intentionally.

## Skills

| Skill | What it does | When to use |
|-------|-------------|-------------|
| `design-discover` | Scope + asset audit + Look/Feel/Act interview + dimension scoring | Starting fresh, or you have anchors in your head but can't translate them |
| `design-author` | Creates and edits the Design Brief and Design System Spec | Writing the canonical brief; updating direction; applying refreshes |
| `design-encode` | Translates the brief into a Claude Design configuration and instructions | About to open Claude Design; fighting house-style drift |

**V2 roadmap:** `design-validate` (audit output against brief), `design-refresh` (propose amendments), `design-recommend` (map a challenge to the right skill).

## Design artifacts

Skills produce structured artifacts that live in `design/`:

| Artifact | What it contains | Dimension |
|----------|-----------------|-----------|
| `discovery-brief.md` | Problem framing, anchors, anti-anchors, asset audit, dimension scores | All six |
| `design-brief.md` | Canonical Look/Feel/Act direction, anti-anchors, default overrides, handoff target | Dims 1-4, 6 |
| `design-system-spec.md` | Tokens, atoms, molecules (at chosen scope) with concrete values | Dim 5 |
| `claude-design-config.md` | Paste-ready `<frontend_aesthetics>` block, inputs manifest, first-prompt template, drift watchlist | Dim 6 |
| `design-instructions.md` | Human-readable companion: pre-session checklist, prompt patterns, when to steer | Dim 6 |

A minimal engagement produces `design-brief.md` and `claude-design-config.md`. A full engagement produces all five.

## Directory structure

```
design-please/
├── CLAUDE.md.template     # How Claude operates in this system
├── SETUP.md               # Onboarding flow
├── README.md              # This file
├── config/
│   └── _template.md       # Project design profile template
├── systems/
│   └── design/
│       └── SYSTEM.md      # Core methodology (dimensions, maturity, sequencing, canon)
├── skills/                # Executable skills
│   ├── design-discover/
│   ├── design-author/
│   └── design-encode/
├── templates/             # Output artifact templates
│   ├── discovery-brief.md
│   ├── design-brief.md
│   ├── design-system-spec.md
│   ├── claude-design-config.md
│   └── design-instructions.md
├── workflows/             # Composed skill sequences
│   ├── new-engagement.yaml
│   └── design-foundations.yaml
├── tracking/              # Effectiveness tracking
└── examples/              # Walkthroughs
```

## The please family

`design-please` is one of five open-source frameworks:

| Framework | What it does | Core question |
|-----------|-------------|---------------|
| [strategy-please](https://github.com/yesplease-studio/strategy-please) | Strategic workshop recommendation engine | "Where should we focus?" |
| [prd-please](https://github.com/yesplease-studio/prd-please) | Product requirements authoring and validation | "How do we build it reliably?" |
| [sales-please](https://github.com/yesplease-studio/sales-please) | Deal qualification and pipeline management | "Is this deal worth pursuing?" |
| [voice-please](https://github.com/yesplease-studio/voice-please) | Voice system definition and encoding | "What should we sound like?" |
| **design-please** | Design direction and Claude Design encoding | "What should it look like, feel like, and act like?" |

Each framework is standalone. `voice-please` and `design-please` are siblings — voice operationalizes how the company sounds; design operationalizes how it looks, feels, and behaves.

## The four canon sources

`design-please` does not invent design methodology. It embeds four established sources at specific points in the system:

- **IDEO / design thinking** — shapes `design-discover`. Problem definition comes before aesthetic direction.
- **NN/g usability heuristics** — quality gates in `design-author` for interactive products.
- **Brad Frost's atomic design** — scopes the design system spec (tokens → atoms → molecules → full).
- **Anthropic's `<frontend_aesthetics>` cookbook** — the operational spine of `design-encode`, including the mandatory default-override block.

Full source integration: `systems/design/SYSTEM.md` §6.

## Self-serve vs. facilitated

Every skill in `design-please` produces useful output when run independently. You don't need a designer to use this.

That said, some dimensions benefit significantly from an experienced facilitator — especially Feel Direction (Dim 3). The difference between "it should feel premium" and a genuinely distinctive emotional register is usually a person with taste pushing past the first answer and bringing references from outside the user's exposure.

The framework is honest about this: `systems/design/SYSTEM.md` §7 documents where facilitation adds value for each dimension.

## License

Apache 2.0
