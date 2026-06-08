# Figma pin-comments (premium path)

Optional. Only when **both** are true: the input was Figma, **and** a write channel exists
(Figma MCP with comment support, or the Figma REST Comments API with a token). This writes
to an external, shared resource — **get explicit consent before posting**, and offer a
dry-run.

## Gate

1. Input is Figma? If not → not applicable, skip.
2. Write channel available? Check for a Figma MCP comment tool or a REST token. None →
   **fallback**: screen-anchors + `action-items.md` only; tell the user plainly that you
   couldn't post to the canvas and why.
3. Consent → ask: *"Post N findings as pin-comments to the Figma file? Or do a dry-run
   (show exactly what I'd post, post nothing)?"* Default to dry-run if unsure.

## Mapping an action-item → a pin

A Figma comment is pinned to a coordinate on a frame. For each item:

- **Frame** — the Figma node for the screen the item lives on (you captured node IDs in
  Phase 1/2 when reading from Figma; keep the screen→node map).
- **Coordinate** — the location of the offending element within that frame. Use the element
  position if you have it from metadata; otherwise pin to a sensible anchor (the element's
  region, or top-left of the frame) and name the element in the text so it's findable.
- **Text** — the same essence as the md, so canvas and doc stay in sync.

## Comment text format

```
[AI-7c41] sev3 · NN-05 · Payment
CVV field rejects valid 4-digit Amex codes; no format hint shown.
→ Accept 3–4 digits; add inline format example. Found by: usability, persona "Maria".
```

- Lead with the **same stable ID** as `action-items.md` — this is what prevents duplicates
  on re-runs and lets designers cross-reference.
- Include **severity** and the **rule key**.
- One line of essence, one line of recommendation.

## Re-run discipline

- Before posting, reconcile against existing comments: an item whose ID already appears on
  the canvas is **not** reposted.
- New items → post. Items now resolved → optionally add a short "resolved in vN" reply
  rather than deleting, so history is preserved. Never bulk-delete others' comments.

## Dry-run mode

Produce the exact list of `{frame, coordinate, text}` you *would* post, render it in the
chat (and optionally append to the report as an appendix), and post nothing. This lets the
user approve placement and wording before anything touches the file.
