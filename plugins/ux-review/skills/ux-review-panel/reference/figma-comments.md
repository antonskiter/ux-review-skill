# Figma pin-comments (premium path)

Optional. Only when **both**: input was Figma, **and** a write channel exists (Figma MCP
with comment support, or the Figma REST Comments API with a token). This writes to an
external shared resource — **get explicit consent before posting**, and offer a dry-run.

**Probe the write channel at the start, not at the end.** The Figma MCP often has no
comment-write tool. When the input is Figma, check up front whether a write channel exists
(MCP comment tool, or a REST token the user can provide). If none, say so early: "I can't
post comments to the file directly — pin-comments would need a Figma REST token; otherwise
findings live in the report with screen-anchors."

## Gate

1. Input is Figma? If not → not applicable, skip.
2. Write channel available? Check for a Figma MCP comment tool or a REST token. None →
   **fallback**: screen-anchors + `action-items.md` only; tell the user plainly that you
   couldn't post to the canvas and why.
3. Consent → ask: *"Post N findings as pin-comments to the Figma file? Or do a dry-run
   (show exactly what I'd post, post nothing)?"* Default to dry-run if unsure.

## Choosing the write channel

- **Comments attributed to the user** come from the REST Comments API with the *user's*
  token (`POST /v1/files/:key/comments`, pinned via `client_meta.node_id` + `node_offset`).
- **Plugin/MCP-created comments** are a fallback — they post, but typically not as the user.
  Mention this tradeoff when offering the option.
- **Token handling, non-negotiable:** never accept a token pasted into chat and never echo
  it. If a token is needed, hand the user a small script that reads it from their
  environment and makes the request. Confirm via dry-run before anything runs.

## Mapping an action-item → a pin

A comment is pinned to a coordinate on a frame. For each item:

- **Frame** — the Figma node for the screen (keep the screen→node map captured in Phase
  1/2).
- **Coordinate** — the offending element's location in the frame. Use the metadata position
  if you have it; otherwise pin to a sensible anchor (element region, or frame top-left) and
  name the element in the text so it's findable.
- **Text** — the same essence as the md, so canvas and doc stay in sync.

## Comment text format

```
[AI-7c41] sev3 · NN-05 · Payment
CVV field rejects valid 4-digit Amex codes; no format hint shown.
→ Accept 3–4 digits; add inline format example. Found by: usability, persona "Maria".
```

- Lead with the **same stable ID** as `action-items.md` — prevents duplicates on re-runs and
  lets designers cross-reference.
- Include **severity** and the **rule key**.
- One line of essence, one line of recommendation.

## Re-run discipline

- Before posting, reconcile against existing comments: an item whose ID already appears on
  the canvas is **not** reposted.
- New items → post. Resolved items → optionally add a short "resolved in vN" reply rather
  than deleting. Never bulk-delete others' comments.

## Dry-run mode

Produce the exact `{frame, coordinate, text}` list you *would* post, render it in chat (and
optionally append to the report), and post nothing. Lets the user approve placement and
wording before anything touches the file.
