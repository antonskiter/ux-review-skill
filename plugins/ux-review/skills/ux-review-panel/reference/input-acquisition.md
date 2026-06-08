# Input acquisition

Lightest probe first, then ask. **Do not stand up heavy processes and never install
tools.** Screenshots are always a valid, sufficient input — offer that fallback freely.
Record what you actually worked with for the report's "run limitations" section.

## The rule of escalation

1. Try the lightest thing already available.
2. If that yields too little, **stop and offer the user options** — don't silently
   escalate to a heavier method or install anything.
3. Whatever you settle on, note its limits (no interactivity, no error states, etc.).

## Capture principle: pixels, not source

Whatever the input, a UX review needs **rendered images of screens** — what a user's eye
sees. Most tools that touch a design or a page expose *two* kinds of output: a **render**
(an image of the screen) and the **source behind it** (code, markup, DOM, design data,
base64 blobs). For capturing screens you want the render. Pulling source to *reconstruct*
images burns context and usually dead-ends.

So, for any input type:

- **Use the tool that returns a rendered image directly.** Whatever the channel calls it
  (a screenshot/snapshot/export-as-image action), that's the one. Source-level output
  (markup, component code, design metadata, tokens, interaction wiring) is the right tool
  for *other* questions — copy text, token values, and the transitions that build the flow
  graph (see [flow-graph.md](flow-graph.md)) — just never as the way to get the *picture*.
- **Capture one screen at a time** at a sensible resolution (≈1280–1440px on the long
  edge). Don't try to export the whole set in a single call — large multi-frame exports
  time out or overflow the tool response.
- **Degrade gracefully.** If a capture times out on a big screen, lower the resolution and
  retry; keep going screen-by-screen rather than failing the whole batch.
- **Order by what the eye/flow shows, not by source order.** Source/markup order (XML
  order, DOM order, layer order) is *not* the flow order. Order screens by their spatial
  position on the canvas (left→right / top→bottom) or by observed navigation. See
  [flow-graph.md](flow-graph.md).

**Anti-pattern — don't do this:** harvesting base64 / binary image data in chunks through
tool responses to rebuild an image. It doesn't fit the response, it's slow, and it almost
always fails. If the only image path is a source dump, that's a signal to switch tools or
ask the user for exports, not to grind through bytes.

## By input type

### Live URL / web product

- **Browser integration already connected** (Chrome MCP, chrome-devtools, or similar) →
  use it. This is best: real interactivity, real states, observed transitions for the flow
  graph. Capture each state with the browser's **screenshot** action (pixels), per the
  capture principle — not by scraping the DOM to rebuild the page.
- **Not connected** → light markup fetch to understand structure and copy is fine for
  *text*, but it isn't a screen capture. Don't try to render pixels from markup.
- **Can't render the actual screens** → STOP and offer, as a plain choice:
  - send screenshots (valid and often enough),
  - connect the browser integration for interactive review,
  - use a headless browser tool *if it's already installed* (don't install one).

### Figma

A concrete instance of the capture principle. With the Figma MCP connected:

1. **`get_metadata`** on the node from the link → the list of frames in that section/page.
2. **Order the frames by canvas position** (X, then Y). Figma's source/layer order is not
   the flow order — the visual left-to-right arrangement usually is.
3. For each frame, capture with the **screenshot tool** (`get_screenshot` by node-id,
   long edge ≈1280–1440) → inline images into `screens/`, one frame at a time.

Use `get_design_context` / design-data tools for the *source-level* questions they're good
at — copy text, token values, and **interaction/prototype wiring for the flow graph** (see
[flow-graph.md](flow-graph.md)) — but **not** to obtain the screen image (they return
code/data, the wrong shape for capturing pixels, and it wastes context). Two different jobs:
screenshot tool for the picture, design-data tools for what's wired underneath. Do not chunk
base64 out of an export call.

- **MCP not connected** → ask which is convenient: attach PNG exports, provide a Figma REST
  token, or connect the MCP. Don't guess a token or a file key, and don't accept a token
  pasted into chat (see [figma-comments.md](figma-comments.md) for safe token handling).

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
