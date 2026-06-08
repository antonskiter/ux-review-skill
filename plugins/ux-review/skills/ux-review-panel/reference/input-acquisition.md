# Input acquisition

Lightest probe first, then ask. **Do not stand up heavy processes and never install
tools.** Screenshots are always a valid, sufficient input — offer that fallback freely.
Record what you actually worked with for the report's "run limitations" section.

## The rule of escalation

1. Try the lightest thing already available.
2. If that yields too little, **stop and offer the user options** — don't silently
   escalate to a heavier method or install anything.
3. Whatever you settle on, note its limits (no interactivity, no error states, etc.).

## By input type

### Live URL / web product

- **Browser integration already connected** (Chrome MCP, chrome-devtools, or similar) →
  use it. This is best: real interactivity, real states, observed transitions for the flow
  graph.
- **Not connected** → light `WebFetch` / `curl` for the markup to understand structure and
  copy. This gives you text and DOM, not rendered states or interactivity.
- **Markup alone is too little** → STOP and offer, as a plain choice:
  - send screenshots (valid and often enough),
  - connect the browser integration for interactive review,
  - use Playwright *if it's already installed* (don't install it).

### Figma

- **Figma MCP connected** → use it directly (screens via `get_screenshot`, structure via
  `get_metadata`, prototype links for the flow graph, variables for token checks).
- **Not connected** → ask which is convenient: attach PNG exports, provide a Figma REST
  token, or connect the MCP. Don't guess a token or a file key.

### Static (PNG / folder of exports)

- Work with the images directly — this is the baseline path and fully supported.
- **Missing a state** you'd expect (error, empty, success) → ask for it rather than
  imagining the screen.

### Prototype (Figma prototype, InVision-style, coded prototype)

- If reachable through an already-connected browser/Figma channel → observe transitions by
  clicking; this enriches the flow graph with real edges.
- Otherwise treat as static + a transitions list the user provides, or ask.

## Always record

In the report's **run limitations** section, state plainly:

- what you analyzed (which screens, which channel),
- what you could **not** reach (interactivity, hidden states, real data, motion),
- any assumptions you made about transitions or states,
- consequently, which findings are higher-confidence vs. inferred.

Honesty here is load-bearing: designers weigh recommendations by how the review was run.
