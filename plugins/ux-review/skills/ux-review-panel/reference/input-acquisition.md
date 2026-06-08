# Input acquisition

Lightest probe first, then ask. **Do not stand up heavy processes and never install
tools.** Screenshots are always a valid, sufficient input — offer that fallback freely.
Record what you worked with for the report's "run limitations" section.

## The rule of escalation

1. Try the lightest thing already available.
2. If it yields too little, **stop and offer the user options** — don't silently escalate
   or install anything.
3. Whatever you settle on, note its limits (no interactivity, no error states, etc.).

**MCP calls can hang** — a single screenshot/export can stall for minutes on a large node.
Shrink scope (lower resolution, one node not a page), retry once or twice, and if it still
won't return, skip that screen, note it, move on. Honestly-flagged partial coverage beats a
stalled review.

## Capture principle: pixels, not source

A UX review needs **rendered images of screens** — what the eye sees. Tools that touch a
design or page expose two outputs: a **render** (image) and the **source behind it** (code,
markup, DOM, design data, base64 blobs). For capturing screens, use the render. Pulling
source to reconstruct images burns context and dead-ends.

For any input type:

- **Use the tool that returns a rendered image directly** — whatever the channel calls it
  (screenshot/snapshot/export-as-image). Source-level output (markup, component code,
  metadata, tokens, interaction wiring) is for *other* questions — copy text, token values,
  and the transitions that build the flow graph (see [flow-graph.md](flow-graph.md)) — never
  for the picture.
- **Capture one screen at a time** at ≈1280–1440px on the long edge. Large multi-frame
  exports time out or overflow the tool response.
- **Degrade gracefully** — on timeout, lower resolution and retry; keep going
  screen-by-screen.
- **Order by flow, not source order.** XML/DOM/layer order is *not* flow order. Order by
  spatial canvas position (left→right / top→bottom) or observed navigation. See
  [flow-graph.md](flow-graph.md).

**Anti-pattern:** harvesting base64 / binary image data in chunks to rebuild an image — it
overflows the response and almost always fails. If the only image path is a source dump,
switch tools or ask the user for exports.

### Inline screenshots vs. files on disk (important)

Many screenshot tools — Figma MCP `get_screenshot`, some browser MCPs — return the image
**inline into context, not as a file on disk.** You can see it, but `report.md`'s
`![](screens/S01.png)` links then point at files that don't exist.

After capturing each screen, **materialize it into `screens/`**:

- If the tool exposes an export/render **URL** (Figma's image-export endpoint, a browser's
  saved-capture path), fetch it with `curl` and save to `screens/Sxx-name.png`.
- Do this per screen as you go, with the same degrade-on-timeout discipline; don't batch.
- **Only if materializing is genuinely impossible** (no URL, no write access) → do **not**
  write fabricated `screens/` links. State in the report that screens are reproducible by
  source reference (node-id / URL), embed inline where the medium allows, and note it in run
  limitations.

Verify before finishing: every image link in `report.md` resolves to a file in `screens/`.

## By input type

### Live URL / web product

- **Browser integration connected** (Chrome MCP, chrome-devtools, or similar) → use it:
  real interactivity, real states, observed transitions. Capture each state with the
  browser's **screenshot** action — not by scraping the DOM.
- **Not connected** → light markup fetch is fine for *text* (structure, copy), but it isn't
  a screen capture. Don't render pixels from markup.
- **Can't render the actual screens** → STOP and offer:
  - send screenshots (valid and often enough),
  - connect the browser integration for interactive review,
  - use a headless browser tool *if already installed* (don't install one).

### Figma

With the Figma MCP connected:

1. **`get_metadata`** on the node from the link → frames in that section/page.
2. **Order frames by canvas position** (X, then Y). Layer order is not flow order.
3. For each frame, capture with `get_screenshot` (by node-id, long edge ≈1280–1440) → inline
   into `screens/`, one frame at a time.

Use `get_design_context` / design-data tools for source-level questions — copy text, token
values, and **interaction/prototype wiring for the flow graph** (see
[flow-graph.md](flow-graph.md)) — **not** for the screen image. Screenshot tool for the
picture, design-data tools for what's wired underneath. Don't chunk base64 out of an export
call.

- **MCP not connected** → ask which is convenient: attach PNG exports, provide a Figma REST
  token, or connect the MCP. Don't guess a token or file key, and don't accept a token
  pasted into chat (see [figma-comments.md](figma-comments.md)).

### Static (PNG / folder of exports)

- Work with the images directly — baseline path, fully supported.
- **Missing an expected state** (error, empty, success) → ask for it rather than imagining
  the screen.

### Prototype (Figma prototype, InVision-style, coded prototype)

- Reachable through a connected browser/Figma channel → observe transitions by clicking;
  enriches the flow graph with real edges.
- Otherwise treat as static + a user-provided transitions list, or ask.

## Always record

In the report's **run limitations** section, state plainly:

- what you analyzed (which screens, which channel),
- what you could **not** reach (interactivity, hidden states, real data, motion),
- assumptions about transitions or states,
- which findings are higher-confidence vs. inferred.
