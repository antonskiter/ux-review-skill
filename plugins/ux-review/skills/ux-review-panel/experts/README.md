# Experts

Each file is a **self-contained reviewer role**. Experts ship with this skill and update
with it (personas live in the project). An expert assesses screens as artifacts against
known heuristics — it does not role-play a user.

## How a role is structured

Frontmatter:

- `role` — short identifier (e.g. `ux-usability`)
- `focus` — one-line description of the lens
- `enabled_by_default` — `true` loads it on every run; `false` is opt-in (orchestrator offers it)

Body, four sections:

- **Optics** — what this role looks at, the mindset it adopts
- **Rules** — the heuristics it applies, linking into
  [../reference/heuristics.md](../reference/heuristics.md). Point at the shared reference;
  don't restate the catalog.
- **What to capture** — the shape of findings it produces
- **What it does NOT do** — explicit boundaries that defer neighboring concerns to the
  roles that own them.

## The default panel

- [ux-usability.md](ux-usability.md) — heuristics, task flow, error prevention/recovery
- [ui-visual.md](ui-visual.md) — hierarchy, consistency, design tokens
- [cx-journey.md](cx-journey.md) — end-to-end path, emotion, friction across screens
- [accessibility.md](accessibility.md) — a11y (`enabled_by_default: false`, opt-in)

## Adding your own expert

Copy any existing file, keep the four-section shape, give it a `focus` no other role owns,
and add a **What it does NOT do** deferring neighboring concerns. Point rules at
[../reference/heuristics.md](../reference/heuristics.md); add new heuristics there with a
stable key rather than inlining, so synthesis dedups by key. Set `enabled_by_default: true`
only if it earns its cost on a typical run.
