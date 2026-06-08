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
