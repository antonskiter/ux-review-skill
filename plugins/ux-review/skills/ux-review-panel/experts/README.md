# Experts

Each file here is a **self-contained reviewer role**. Experts ship with this skill and
update with it (unlike personas, which live in the project). An expert assesses the
screens as artifacts against known heuristics — it does not role-play a user.

## How a role is structured

Frontmatter:

- `role` — short identifier (e.g. `ux-usability`)
- `focus` — one-line description of the lens
- `enabled_by_default` — `true` loads it on every run; `false` is opt-in (the
  orchestrator offers it)

Body, four sections:

- **Optics** — what this role looks at, the mindset it adopts
- **Rules** — the heuristics it applies, with links into
  [../reference/heuristics.md](../reference/heuristics.md). Roles don't restate the full
  heuristic catalog; they point at the shared reference so wording stays consistent.
- **What to capture** — the shape of findings it should produce
- **What it does NOT do** — explicit boundaries, so roles don't overlap and report the
  same issue four times. Dedup happens in synthesis, but clean role boundaries keep the
  raw findings distinct in the first place.

## The default panel

- [ux-usability.md](ux-usability.md) — heuristics, task flow, error prevention/recovery
- [ui-visual.md](ui-visual.md) — hierarchy, consistency, design tokens
- [cx-journey.md](cx-journey.md) — end-to-end path, emotion, friction across screens
- [accessibility.md](accessibility.md) — a11y (`enabled_by_default: false`, opt-in)

## Adding your own expert

Copy any existing file, keep the four-section shape, and give it a `focus` that doesn't
already belong to another role — then add a **What it does NOT do** that defers the
neighboring concerns to the roles that own them. Point its rules at
[../reference/heuristics.md](../reference/heuristics.md); add new heuristics there (with a
stable key) rather than inlining them, so synthesis can dedup by rule key. Set
`enabled_by_default` honestly: only `true` if it earns its cost on a typical run.
