# Flow graph

Always build a graph: **nodes = screens/states, edges = transitions.** The common case is a
single linear chain — don't manufacture branches to look thorough. Each persona gets its own
route through this one graph.

## Build only edges you can justify

Add a branch **only** with one of:

- an **observed transition** (clicked in a live product/prototype),
- a **Figma prototype link** (an interaction wired in the file),
- an **explicit state** in the screen set (a designed error/empty/success screen something
  must lead to/from),
- a **user-confirmed** branch.

If a transition is unclear, **ask** — a wrong branch sends a persona down a path that
doesn't exist and poisons the findings.

## Deriving edges by input type

- **Live product / prototype** — observed at click time. Record the trigger element on the
  edge; highest confidence.
- **Figma** — read prototype links and frame naming. A wired `→` is an edge; naming (`01
  Login`, `02 …`) suggests order but is weaker than wired links. Default: **sort frames by
  canvas position** (X, then Y) — layer order does not match the canvas. Position-order is an
  *inferred* spine; wired links override it.
- **Static images** — order by file numbering plus best-guess of what follows. Mark
  *inferred*. When numbering is ambiguous, ask.

General rule: **source position (layer order, DOM order, file listing) is not flow
position.** Order by spatial layout or observed navigation; mark anything unobserved as
inferred.

## Mine the object model for transitions

"Pixels, not source" governs screen *images* — not transitions. For edges, object/design
data is often the strongest evidence short of observing a click:

- **Figma** — read interaction/reaction objects, prototype destination references, "on
  click → navigate to {nodeId}" wiring. A button whose object names its target frame is a
  real edge (confidence `figma-prototype`), stronger than any position guess. Query
  design-context/metadata tools for *interactions*, not the picture.
- **Live / DOM** — link `href`s, route definitions, handler targets, form `action`s name
  destinations; turns inferred edges into near-observed ones.
- **Coded prototype** — routing config and navigation calls reveal the edge map directly.

Order screens by layout as a cheap default, but when transitions are the question, go into
the objects. Promote object-backed edges above `inferred`; mark `inferred` only the gaps the
objects don't cover.

## Representing the graph

A small list of nodes and edges is enough:

```
nodes:
  S01-login        (state: default)
  S02-otp          (state: default)
  S02e-otp-error   (state: error)
  S03-dashboard    (state: default)
edges:
  S01 → S02     trigger: "Continue" tap            confidence: observed
  S02 → S03     trigger: correct code              confidence: observed
  S02 → S02e    trigger: wrong code                confidence: figma-prototype
  S02e → S02    trigger: "Try again"               confidence: inferred
```

Tag each edge `observed | figma-prototype | inferred | user-confirmed`. The CX/journey
expert walks edges in order; personas get a route filtered to their scenario.

## Routes per persona

One graph, many paths. For each persona's scenario, trace the subset of nodes/edges a user
pursuing that goal would traverse (e.g. "guest checkout" skips account-creation). Hand the
persona only its route — the ordered list of screens for stepwise reveal. If a scenario
needs an edge the graph lacks, that's a gap: surface it rather than fabricating the edge.
