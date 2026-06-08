# Report templates

Two files in `ux-review/<date>-<target>-<version>/`. `action-items.md` is the deliverable;
`report.md` is the supporting narrative. Both use the **same stable IDs** as anchors so
they cross-link, and so re-runs can diff.

---

## action-items.md

The main artifact — prioritized, scannable, each row anchored by its stable ID.

```markdown
# Action items — <target> <version> (<date>)

Prioritized by severity × user coverage × role consensus, quick wins surfaced.
Run mode: <one line, e.g. "static PNG set, no interactivity">.

## Quick wins (low effort, do these first)

- <a id="AI-7c41"></a>**AI-7c41** · sev3 · low · Payment · CVV rejects valid 4-digit codes
  → accept 3–4 digits + format hint. *Found by: usability, persona "Maria". Status: new.*

## Prioritized list

### AI-2f9a · sev4 · high · Checkout · Cart total changes silently after promo
<a id="AI-2f9a"></a>
- **Screen:** [S04-checkout](screens/S04-checkout.png)
- **Severity:** 4 (catastrophic — users feel deceived, abandon)
- **Effort:** high
- **Confirmed by:** 3 roles (usability NN-01, cx-journey CX-04) + persona "Alex"
- **Issue:** Total updates without explanation when a promo is applied; no line item.
- **Recommendation:** Show an itemized breakdown; animate the change; label the discount.
- **Status:** still-open (was AI-2f9a in v1).

### AI-7c41 · sev3 · low · Payment · CVV rejects valid 4-digit codes
… (one block per item, ordered by priority) …
```

Keep each item's fields consistent: **ID · screen · severity · effort · issue ·
recommendation · who found it · status.** Status is one of `new / still-open / fixed /
regressed` relative to the prior run in this series (omit on a first run).

---

## report.md

The UX-research narrative. Sections:

```markdown
# UX review — <target> <version> (<date>)

## Run limitations
What was analyzed, what couldn't be reached (interactivity, hidden states, real data),
assumptions made, and which findings are higher-confidence vs. inferred. Be honest — this
calibrates how everything below is read.

## Panel
- Personas: <names + one-line portraits + their scenario>. Note if any were drafted
  "not grounded in research".
- Experts: <which roles ran>; accessibility on/off.

## Flow graph
Brief node/edge summary (linear or branched), with edge confidence. Inline the graph
listing or a small diagram.

## Per-screen walkthrough
For each screen, in flow order:

### S04 — Checkout
![S04 checkout](screens/S04-checkout.png)
- **Persona reactions** (as interview snippets): what each persona predicted, where the
  reveal mismatched, misclicks, "where is X?" moments — quote them.
- **Expert findings:** bullet per finding with rule key, linking to its action-item:
  see [AI-2f9a](action-items.md#AI-2f9a).
- **Spec:** matches / diverges / not implemented / beyond spec (if a spec was provided).

## Persona journeys (as UX interviews)
One short narrative per persona — their goal, their path, where they stalled, how they
felt. This is the qualitative heart; write it like research notes, not a checklist.

## Spec reconciliation (if applicable)
Table or list of screen ↔ requirement with the four categories; flag unsure mappings.

## Cross-cutting themes
Patterns spanning screens (e.g. "inconsistent button hierarchy throughout"), each tied to
its action-item ID.

## Appendix: Figma comments (if posted or dry-run)
The {frame, coordinate, text} list that was posted, or would be posted in dry-run.
```

### Anchors and cross-linking

- Each finding in `report.md` links to its item: `[AI-2f9a](action-items.md#AI-2f9a)`.
- Each item in `action-items.md` carries `<a id="AI-xxxx"></a>` and links back to its
  screen image. Stable IDs are the through-line connecting list ↔ screen ↔ Figma pin ↔
  next version.
- **No broken links.** Every `![](screens/Sxx.png)` must point at a file that actually
  exists in `screens/` (MCP screenshots are inline by default — materialize them, see
  [input-acquisition.md](input-acquisition.md)). If a screen couldn't be saved, reference it
  by source (node-id / URL) instead of writing a dead file link.
