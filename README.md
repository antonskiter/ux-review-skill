# UX Review Skills

Two paired [Claude Code](https://docs.claude.com/en/docs/claude-code) skills that turn raw
research and design artifacts into actionable UX feedback:

- **`ux-review-panel`** — runs any UX artifact (PNG, folder of screens, Figma, live product,
  prototype) through a panel of user **personas** and discipline **experts**, then produces a
  research-style report plus a prioritized, stable-ID list of action-items for designers.
- **`persona-builder`** — turns any materials (interview transcripts, support tickets,
  surveys, analytics, a brief) into grounded, distinct personas in exactly the format the
  panel consumes.

They chain: **persona-builder → `personas/` → ux-review-panel.**

## Install

Personal Claude Code skills live in `~/.claude/skills/`. To install both, paste this into a
terminal:

```bash
git clone https://github.com/antonskiter/ux-review-skill.git /tmp/ux-review-skill && \
mkdir -p ~/.claude/skills && \
cp -R /tmp/ux-review-skill/ux-review-panel /tmp/ux-review-skill/persona-builder ~/.claude/skills/ && \
rm -rf /tmp/ux-review-skill && \
echo "Installed: ux-review-panel, persona-builder"
```

Then restart Claude Code (or start a new session) so it picks up the skills. Verify with
`/skills` — both should appear in the list.

### Update to the latest version

Re-run the install command; `cp -R` overwrites the existing folders with the new version.

### Uninstall

```bash
rm -rf ~/.claude/skills/ux-review-panel ~/.claude/skills/persona-builder
```

### Install for one project only

To scope the skills to a single repo instead of globally, copy them into that repo's
`.claude/skills/` instead of `~/.claude/skills/`:

```bash
mkdir -p .claude/skills && \
cp -R /tmp/ux-review-skill/ux-review-panel /tmp/ux-review-skill/persona-builder .claude/skills/
```

## The `personas/` folder

`personas/` in this repo is **not a skill** — it's a project-side scaffold. Personas are
grounded in *your* users, so they live in the project being reviewed, not in the skill.
Copy it into your project root and fill it in (or let `persona-builder` generate the files):

```bash
cp -R /tmp/ux-review-skill/personas /path/to/your/project/
```

`personas/README.md` holds the shared rules; `_TEMPLATE.md` is the starting point for one
persona. `ux-review-panel` discovers personas here automatically.

## Usage

Skills trigger from natural language — you don't call them by name. Examples:

- *"Build personas from these interview transcripts in `research/`"* → **persona-builder**
- *"Run a UX review on the screens in `designs/checkout/`"* → **ux-review-panel**
- *"Here's a Figma link — have some users walk the onboarding and tell me what breaks"* →
  **ux-review-panel**

### A typical end-to-end flow

1. Drop research materials in your project and ask Claude to **build personas**. It proposes
   a candidate set, you confirm, and it writes files into `personas/`.
2. Ask Claude to **review** a set of screens. It captures them, builds a flow graph, walks
   each persona through naively (one screen at a time), runs the experts, and writes a
   report + prioritized action-items under `ux-review/<date>-<target>-<version>/`.

## What's in each skill

```
ux-review-panel/        # the review panel
├── SKILL.md            # entry point: phases 0-6, regression rules
├── experts/            # reviewer roles (usability, visual, journey, a11y)
├── reference/          # heuristics, severity/effort, flow graph, report templates, …
└── prompts/            # persona-walkthrough + expert-review instructions

persona-builder/        # the persona generator
├── SKILL.md            # entry point: phases 0-5
├── reference/          # material intake, synthesis, grounding, output format
└── prompts/            # signal-extraction instruction

personas/               # project-side scaffold (copy into your project)
├── README.md           # shared rules for all personas
└── _TEMPLATE.md        # one-persona template
```

## Design principles

- **Input-agnostic.** Both skills probe the lightest available path first, then ask rather
  than standing up heavy tooling or installing anything.
- **Grounded and honest.** Personas trace traits to evidence and carry `grounded`/`confidence`
  flags; review findings note run limitations and what couldn't be reached.
- **Stable IDs.** Action-items are keyed by a hash of their normalized essence, so they
  survive rewording and let you diff reviews across versions.
- **Untrusted content.** Artifact and material text is treated as data to analyze, never as
  instructions to follow.
- **Speaks your language.** If the input is non-English, reports and personas come back in
  the user's language (real quotes stay verbatim).
