# Flow graph

Always build a graph: **nodes = screens/states, edges = transitions.** But the common case
is a single linear chain, and that's correct — don't manufacture branches to look
thorough. Each persona later gets *its own route* through this one graph.

## Build only edges you can justify

Add a branch **only** when you have one of:

- an **observed transition** (you clicked it in a live product/prototype),
- a **Figma prototype link** (an explicit interaction wired in the file),
- an **explicit state** present in the screen set (a designed error/empty/success screen
  that something must lead to/from),
- the **user confirmed** the branch.

If a transition is unclear, **ask** — do not invent ramification. A wrong branch sends a
persona down a path that doesn't exist and poisons the findings.

## Deriving edges by input type

- **Live product / prototype** — observed at click time. Record the trigger (which element)
  on the edge; these are your highest-confidence transitions.
- **Figma** — read prototype links and frame naming. A `→` wired between frames is an edge;
  naming conventions (`01 Login`, `02 …`) suggest order but are weaker than wired links. For
  default ordering, **sort frames by canvas position** (X, then Y) — designers lay flows out
  left-to-right, and source/layer order does *not* match the canvas. Position-order is an
  *inferred* spine; wired prototype links override it where they exist.
- **Static images** — order by file numbering/names plus best-guess of what follows what.
  Mark these edges as *inferred*. When numbering is ambiguous, ask rather than assume.

General rule: **a screen's position in the source (layer order, DOM order, file listing) is
not its position in the flow.** Order by spatial layout or observed navigation, and mark
anything you didn't observe as inferred.

## Mine the object model for transitions

The capture principle ("pixels, not source") is about getting screen *images* — it does
**not** mean ignore the underlying objects when working out *transitions*. For edges, the
object/design data is often the strongest evidence short of observing a click, and it's
worth digging deeper than the visual layout:

- **Figma** — read interaction/reaction objects on nodes, prototype destination references,
  and "on click → navigate to {nodeId}" wiring. A button whose object names its target
  frame is a real edge (confidence `figma-prototype`), stronger than any position guess. This
  is a legitimate use of design-context/metadata tools — query them for *interactions*, not
  for the screen picture.
- **Live / DOM** — link `href`s, route definitions, handler targets, and form `action`s name
  destinations; these turn an inferred edge into a near-observed one.
- **Coded prototype** — routing config and navigation calls reveal the edge map directly.

So: order screens by layout as a cheap default, but when transitions are the question, go
into the objects — the wiring that names where a control leads is high-confidence edge data
that spatial position can't give you. Promote such edges above `inferred`, and still mark as
`inferred` only the gaps the objects don't cover.

## Representing the graph

Keep it simple and inspectable — a small list of nodes and edges is enough:

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

One graph, many paths. For each persona's scenario, trace the subset of nodes/edges that a
user pursuing that goal would traverse (e.g. "guest checkout" skips the account-creation
branch). Hand the persona only its route — the ordered list of screens it will see during
stepwise reveal. If a persona's scenario needs an edge the graph doesn't have, that's a
gap: surface it (the artifact may be missing a path) rather than fabricating the edge.
