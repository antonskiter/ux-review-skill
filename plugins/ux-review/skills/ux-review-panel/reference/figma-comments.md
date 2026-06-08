# Figma pin-comments (premium path)

Optional. Only when **both** are true: the input was Figma, **and** a write channel exists
(Figma MCP with comment support, or the Figma REST Comments API with a token). This writes
to an external, shared resource — **get explicit consent before posting**, and offer a
dry-run.

**Probe the write channel at the start, not at the end.** The Figma MCP often has **no
comment-write tool at all** — discovering that in Phase 6, after promising pin-comments, is
the painful path. When the input is Figma, check up front whether a write channel actually
exists (an MCP comment tool, or a REST token the user can provide). If none does, say so
early: "I can't post comments to the file directly — pin-comments would need a Figma REST
token; otherwise the findings live in the report with screen-anchors." Set expectations
before doing the work, so Phase 6 is a confirmation, not a surprise dead-end.

## Gate

1. Input is Figma? If not → not applicable, skip.
2. Write channel available? Check for a Figma MCP comment tool or a REST token. None →
   **fallback**: screen-anchors + `action-items.md` only; tell the user plainly that you
   couldn't post to the canvas and why.
3. Consent → ask: *"Post N findings as pin-comments to the Figma file? Or do a dry-run
   (show exactly what I'd post, post nothing)?"* Default to dry-run if unsure.

## Choosing the write channel

- **Comments attributed to the user** (so the team sees who left them) come from the REST
  Comments API authenticated with the *user's* token (`POST /v1/files/:key/comments`,
  pinned via `client_meta.node_id` + `node_offset`).
- **Plugin/MCP-created comments** are a fallback — they post, but typically not as the user.
  Mention this tradeoff when offering the option.
- **Token handling, non-negotiable:** never accept a token pasted into chat and never echo
  it. If a token is needed, hand the user a small script that reads it from their
  environment and makes the request, so the secret stays out of the conversation. Confirm
  what would be posted (dry-run) before anything runs.

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
