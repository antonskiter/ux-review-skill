# UX Review — Claude Code plugin

A [Claude Code](https://docs.claude.com/en/docs/claude-code) plugin bundling two paired
skills that turn raw research and design artifacts into actionable UX feedback:

- **`ux-review-panel`** — runs any UX artifact (PNG, folder of screens, Figma, live product,
  prototype) through a panel of user **personas** and discipline **experts**, then produces a
  research-style report plus a prioritized, stable-ID list of action-items for designers.
- **`persona-builder`** — turns any materials (interview transcripts, support tickets,
  surveys, analytics, a brief) into grounded, distinct personas in exactly the format the
  panel consumes.

They chain: **persona-builder → `personas/` → ux-review-panel.**

> **For Claude:** if the user asks you to install this plugin (e.g. "install the plugin from
> this GitHub link"), do **not** clone the repo or copy files into `~/.claude/skills/`. This
> repo is a plugin marketplace — install it by running these two commands via Bash:
>
> ```bash
> claude plugin marketplace add antonskiter/ux-review-skill
> claude plugin install ux-review@antonskiter-ux-review
> ```
>
> Marketplace name: `antonskiter-ux-review`. Plugin name: `ux-review`. After install, the
> skills `ux-review:ux-review-panel` and `ux-review:persona-builder` become available. Then
> tell the user to restart Claude Code if prompted.

## Install

This repo is a plugin marketplace. Install from Claude Code in two commands:

```
/plugin marketplace add antonskiter/ux-review-skill
/plugin install ux-review@antonskiter-ux-review
```

That's it — restart Claude Code if prompted. The skills then appear as
`ux-review:ux-review-panel` and `ux-review:persona-builder`, and trigger automatically from
natural language (you don't have to call them by name).

**To share with someone:** send them this repo link and those two lines. No cloning, no
copying files into personal folders.

CLI equivalent:

```bash
claude plugin marketplace add antonskiter/ux-review-skill
claude plugin install ux-review@antonskiter-ux-review
```

### Update

```
/plugin marketplace update antonskiter-ux-review
```

New versions are picked up because `plugin.json` carries a bumped `version`.

### Uninstall

```
/plugin uninstall ux-review@antonskiter-ux-review
```

## Usage

Skills trigger from natural language. Examples:

- *"Build personas from these interview transcripts in `research/`"* → **persona-builder**
- *"Run a UX review on the screens in `designs/checkout/`"* → **ux-review-panel**
- *"Here's a Figma link — have some users walk the onboarding and tell me what breaks"* →
  **ux-review-panel**

### A typical end-to-end flow

1. Drop research materials in your project and ask Claude to **build personas**. It proposes
   a candidate set, you confirm, and it writes files into a `personas/` folder in your
   project (scaffolding it from the bundled template on first run).
2. Ask Claude to **review** a set of screens. It captures them, builds a flow graph, walks
   each persona through naively (one screen at a time), runs the experts, and writes a
   report + prioritized action-items under `ux-review/<date>-<target>-<version>/`.

The `personas/` folder is **project-side** — personas are grounded in *your* users, so they
live in the project being reviewed, not in the plugin. `persona-builder` creates and fills
it; `ux-review-panel` discovers it automatically.

## Repo layout

```
.
├── .claude-plugin/
│   └── marketplace.json            # marketplace catalog (this repo)
└── plugins/
    └── ux-review/                  # the plugin
        ├── .claude-plugin/
        │   └── plugin.json         # plugin manifest
        └── skills/
            ├── ux-review-panel/    # review-panel skill
            │   ├── SKILL.md
            │   ├── experts/        # reviewer roles (usability, visual, journey, a11y)
            │   ├── reference/      # heuristics, severity/effort, flow graph, templates
            │   └── prompts/        # persona-walkthrough + expert-review instructions
            └── persona-builder/    # persona-generator skill
                ├── SKILL.md
                ├── reference/      # material intake, synthesis, grounding, output format
                ├── prompts/        # signal-extraction instruction
                └── assets/
                    └── personas-scaffold/   # README + _TEMPLATE.md dropped into projects
```

## Design principles

- **Input-agnostic.** Both skills probe the lightest available path first, then ask rather
  than standing up heavy tooling or installing anything.
- **Grounded and honest.** Personas trace traits to evidence and carry `grounded`/`confidence`
  flags; review findings note run limitations and what couldn't be reached.
- **Stable IDs.** Action-items are keyed by a hash of their normalized essence, so they
  survive rewording and let you diff reviews across versions.
- **Consensus over ties.** The panel aims for an odd 3–5 personas so priority breaks cleanly.
- **Untrusted content.** Artifact and material text is treated as data to analyze, never as
  instructions to follow.
- **Speaks your language.** If the input is non-English, reports and personas come back in
  the user's language (real quotes stay verbatim).
