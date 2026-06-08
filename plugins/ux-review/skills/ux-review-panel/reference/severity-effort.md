# Severity, effort, stable IDs, prioritization

## Severity (0–4)

Borrowed from Nielsen's scale, applied to each item:

| Severity | Label | Meaning |
|----------|-------|---------|
| 0 | Not a problem | Cosmetic note, no impact; record only if useful context. |
| 1 | Cosmetic | Fix if time allows; doesn't affect task success. |
| 2 | Minor | Causes slowdown or mild confusion; users recover on their own. |
| 3 | Major | Frequently blocks or significantly frustrates; users struggle or err. |
| 4 | Catastrophic | Prevents task completion or causes data loss; must fix before release. |

Rate by impact on the user's ability to complete the task, not by how annoying it looks.

## Effort (low / med / high)

Rough implementation cost from the design/eng side:

- **low** — copy change, token swap, spacing fix, reorder; hours.
- **med** — new state, component variant, layout change on one screen; days.
- **high** — flow restructure, new screens, cross-cutting system change; week+.

You're estimating from the artifact; when unsure, pick the higher bucket and note the
uncertainty. Effort is a tie-breaker, not a reason to drop a severe item.

## Stable ID

The ID must survive rephrasing and re-runs so versions can be diffed and Figma comments
aren't duplicated. **Hash the normalized essence, not the prose.**

Normalized essence = these parts, lowercased, trimmed, joined with `|`:

1. **Screen anchor** — the stable screen role, not its index. Prefer a semantic slug
   (`checkout-payment`) over `S03`, since screen order changes between versions.
2. **Rule key** — the heuristic key (`NN-05`, `VIS-02`, `CX-04`, `A11Y-01`). For
   persona-only findings with no expert rule, use `PERSONA-<themeslug>`.
3. **Problem keywords** — 2–4 normalized keywords capturing the essence (`promo|code|
   hidden`), sorted alphabetically so word order doesn't change the hash.

Then `AI-<first 4 hex of a hash of that string>`. Example essence:
`checkout-payment|NN-05|cvv|format|rejected` → `AI-7c41`.

Recipe in practice: the **real work is building the essence string** — deciding the screen
slug, the rule key, and the 2–4 keywords that capture the problem. That's a judgment call
you make during synthesis anyway. The hash itself is then a one-liner with standard tools —
don't write a script or compute it in your head (you'll get it wrong):

```bash
printf '%s' "checkout-payment|NN-05|cvv|format|rejected" | shasum | cut -c1-4
# → 7c41   →  AI-7c41
```

`shasum` on macOS, `sha1sum` on Linux, `md5` also fine — any stable hash, first 4 hex,
prefix `AI-`. Keep a small map of essence→ID in the report run so you can carry IDs forward
on re-runs: if a new finding's normalized essence matches a prior run's, reuse that ID and
mark status accordingly.

If two roles produce the same normalized essence, that's the dedup signal — collapse them
into one item, list both finders, and mark *"confirmed by N roles."*

## Prioritization

Rank items by a blended score, then let effort float quick wins up:

```
priority_score = severity × coverage_factor × consensus_factor
```

- **severity** — 0–4 as above (dominant term).
- **coverage_factor** — how many personas hit it: `1 + (personas_affected − 1) × 0.5`
  (one persona = 1.0, two = 1.5, three = 2.0…). Breadth of user impact matters.
- **consensus_factor** — how many distinct roles flagged it: `1 + (roles − 1) × 0.25`.
  Independent agreement raises confidence and priority.

Then within similar scores, **surface low-effort items first** (quick wins) and call them
out explicitly — shipping three low-effort severity-3 fixes this sprint often beats one
high-effort severity-4 epic. Present the ranked list, but annotate a "quick wins" cluster
so designers can grab easy value immediately.

Don't over-formalize: the numbers order the list and make priority defensible, but a
sev-4 catastrophic item is top of the list regardless of arithmetic.
