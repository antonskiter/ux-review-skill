# Severity, effort, stable IDs, prioritization

## Severity (0–4)

Nielsen's scale, applied per item:

| Severity | Label | Meaning |
|----------|-------|---------|
| 0 | Not a problem | Cosmetic note, no impact; record only if useful context. |
| 1 | Cosmetic | Fix if time allows; doesn't affect task success. |
| 2 | Minor | Causes slowdown or mild confusion; users recover on their own. |
| 3 | Major | Frequently blocks or significantly frustrates; users struggle or err. |
| 4 | Catastrophic | Prevents task completion or causes data loss; must fix before release. |

Rate by impact on task completion, not by how annoying it looks.

## Effort (low / med / high)

Implementation cost from the design/eng side:

- **low** — copy change, token swap, spacing fix, reorder; hours.
- **med** — new state, component variant, layout change on one screen; days.
- **high** — flow restructure, new screens, cross-cutting system change; week+.

When unsure, pick the higher bucket and note the uncertainty. Effort is a tie-breaker, not
a reason to drop a severe item.

## Stable ID

The ID must survive rephrasing and re-runs so versions diff cleanly and Figma comments
aren't duplicated. **Hash the normalized essence, not the prose.**

Normalized essence = these parts, lowercased, trimmed, joined with `|`:

1. **Screen anchor** — the stable screen role, not its index. Prefer a semantic slug
   (`checkout-payment`) over `S03`; screen order changes between versions.
2. **Rule key** — the heuristic key (`NN-05`, `VIS-02`, `CX-04`, `A11Y-01`). For
   persona-only findings with no expert rule, use `PERSONA-<themeslug>`.
3. **Problem keywords** — 2–4 normalized keywords (`promo|code|hidden`), sorted
   alphabetically so word order doesn't change the hash.

Then `AI-<first 4 hex of a hash of that string>`. Example:
`checkout-payment|NN-05|cvv|format|rejected` → `AI-7c41`.

The real work is building the essence string — screen slug, rule key, 2–4 keywords. The
hash is a one-liner; don't script it or compute it by hand:

```bash
printf '%s' "checkout-payment|NN-05|cvv|format|rejected" | shasum | cut -c1-4
# → 7c41   →  AI-7c41
```

`shasum` on macOS, `sha1sum` on Linux, `md5` also fine — any stable hash, first 4 hex,
prefix `AI-`. Keep a map of essence→ID across the run: if a new finding's essence matches a
prior run's, reuse that ID and mark status accordingly.

If two roles produce the same essence, that's the dedup signal — collapse into one item,
list both finders, mark *"confirmed by N roles."*

## Prioritization

Rank by a blended score, then float quick wins up:

```
priority_score = severity × coverage_factor × consensus_factor
```

- **severity** — 0–4 as above (dominant term).
- **coverage_factor** — personas hit: `1 + (personas_affected − 1) × 0.5` (one = 1.0, two =
  1.5, three = 2.0…).
- **consensus_factor** — distinct roles flagging it: `1 + (roles − 1) × 0.25`.

Within similar scores, **surface low-effort items first** and annotate a "quick wins"
cluster. The numbers order the list, but a sev-4 catastrophic item is top regardless of
arithmetic.
