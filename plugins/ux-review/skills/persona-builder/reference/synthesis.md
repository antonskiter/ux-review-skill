# Synthesis — signals to personas

Turn a pile of signals into a small set of distinct personas. Goal: **coverage of real
behavioral axes**, not a tidy headcount.

## 1. Normalize signals

Each signal is one observation tagged with its source: `{type, observation, source,
quote?}` where type ∈ goal / behavior / frustration / context / mental-model / trait.
Strip duplicates that say the same thing from the same source; keep duplicates from
*different* sources — repeat across sources is what raises confidence later.

## 2. Affinity clustering

Group signals that describe the same kind of user. Cluster by **what drives behavior**, not
surface demographics — two people of different ages who both abandon at unexpected cost
belong together; two same-age users with opposite goals don't.

Lay out frustration and goal signals first (they discriminate users best), let clusters form
around them, then attach context/behavior/trait signals to the cluster they fit.

## 3. Decide the persona set by axes

A persona earns inclusion by stressing an **axis** the others don't. Common discriminating
axes:

- **goal / job-to-be-done** (what they're trying to accomplish)
- **tech comfort / fluency** with this kind of interface
- **urgency / context** (leisurely vs. under pressure; desktop vs. mobile-on-the-go)
- **expertise / domain knowledge**
- **accessibility need**
- **trust / stakes** (first-time skeptic vs. committed returning user)

Rules of thumb:

- **Merge** clusters that differ only cosmetically — same behavior, same frustrations.
- **Split** a cluster carrying two contradictory behaviors (e.g. "wants speed" and "reads
  everything carefully" are two people).
- **Target 3–5 personas, preferably an odd number.** More than that and they blur; fewer
  may miss a segment. Odd sets break ties downstream: ux-review-panel weights findings by how
  many personas hit them, and a 3-of-5 majority beats a 50/50 split. Let the evidence set the
  number — if it only honestly supports two distinct types, ship two and flag that the panel
  may need a tie-breaker — but prefer 3 or 5 over 2 or 4 when the data supports it.
- A cluster backed by one weak signal isn't a persona — it's a hypothesis. Either mark it
  drafted/low-confidence or fold it into the gaps list.

## 4. Name the axis per persona

For each surviving cluster, record the **one axis it primarily represents** — this becomes
the `axis` frontmatter field. If two personas claim the same primary axis, you have a merge
or a re-split to do.

## 5. Propose the set before drafting

Surface your read and get a confirm before writing full files. Keep it compact — one block
per candidate:

```
Proposed personas from these materials:

1. "Maria, busy parent" — axis: low tech comfort / high urgency — grounded, high
   Why: 6 interviews + 12 tickets show guest-checkout abandonment under time pressure.
2. "Dmitri, power buyer" — axis: efficiency / repeat use — grounded, medium
   Why: analytics segment of frequent reorderers; thin qualitative, one interview.
3. (candidate) "First-time mobile skeptic" — axis: trust / mobile — drafted, low
   Hinted by 3 NPS verbatims; not enough to stand alone — include as draft, or leave as a gap?

Confirm, drop, merge, split, rename, or add — then I'll write the files.
```

For the **adopt path** (structured input), frame it as confirmation, not synthesis:
"Here are the N personas already defined in your materials; I'll format and enrich them
against the raw data — anything to change first?" Note any given persona the evidence
doesn't back, and any user type the raw data reveals that the given set misses.

## 6. Hand off to drafting

Once confirmed, for each persona gather: its signals (with sources), its primary axis, and
at least one concrete goal → scenario. Pass that to Phase 3 drafting; traits get written with
their evidence attached (see [grounding.md](grounding.md)).
