# Material intake

Lightest probe first, then ask. Classify the run's evidence level — it sets the
`grounded` flag and confidence on every persona. Don't manufacture richness from thin
input.

## Classify the run

- **research-grounded** — real user data exists (interviews, tickets, surveys, analytics,
  recordings). Personas can be `grounded: true`, confidence up to high.
- **partial** — some real data plus meaningful gaps, or secondhand data (sales/marketing
  framing, stakeholder beliefs). Mixed: ground what you can, flag the rest.
- **drafted** — only a brief, a product description, or assumptions. All personas
  `grounded: false`, confidence low, clearly labeled as hypotheses to validate.

## Material types and how to read each

- **Interview transcripts / user research notes** — richest source. Pull goals, verbatim
  quotes, observed behavior, emotional moments. Highest grounding weight.
- **Support tickets / chat logs** — frustrations, failure points, real language. Skewed
  toward problems (happy users don't write in) — note that bias.
- **Survey results** — quantify how common a trait is; free-text gives voice. Watch for
  leading questions.
- **Analytics / funnels / session data** — behavior without the *why*. Good for segmenting
  by what people do; pair with qualitative for motivation.
- **NPS / reviews / app-store verbatims** — strong emotion, real phrasing, selection bias
  toward extremes.
- **Sales / CS call notes** — goals and objections, but filtered through a seller's lens.
- **Existing personas / segments / CRM** — a starting structure; verify against fresher
  evidence rather than inheriting uncritically.
- **Product brief / PRD / pitch** — context and intended audience, not actual users. Use
  for drafted personas; mark as assumptions.

## How structured is the input? Adopt vs. synthesize

Before clustering from raw signals, check whether the materials **already carry a persona
structure**. Reuse what's there instead of re-deriving it.

Detection cues (any of these → likely structured):

- a persona doc / deck with named users and attributes,
- a segmentation table or audience matrix (segments × traits),
- CRM/product segments, named user tiers, or JTBD groups,
- a brief that explicitly enumerates the target user types.

When structured input is present:

- **Adopt the given set** as the persona skeleton — keep their names, segments, and intended
  axes. Don't invent a parallel set from scratch.
- **Map** each to the output format (frontmatter + body sections).
- **Verify and enrich** against any raw evidence also present: confirm each claimed trait
  has support, attach real quotes/signals, and raise or lower confidence accordingly. A
  given persona that the raw data contradicts is a finding — surface the conflict, don't
  silently "fix" it.
- **Set `grounded` by backing, not by format.** A formatted persona with no evidence behind
  it is still `grounded: false`.
- **Note unbacked given personas** in the gaps/changes section; don't drop them quietly.

When input is semi-structured (some segments, some raw) → adopt the structured part, cluster
the rest, then reconcile so you don't produce two personas describing the same user.

When input is fully raw → synthesize from signals as in
[synthesis.md](synthesis.md).

## Light probe → ask

1. Read what's directly provided or pointed to (a folder, a paste, a file).
2. If it's clearly thin for the ask, **stop and offer**, plainly:
   - proceed with **drafted** personas from what exists (flagged as hypotheses),
   - point you to where richer data lives (research repo, ticket export, analytics),
   - **narrow scope** to only the user types the current evidence actually supports.
3. Don't fetch external sources or stand up tooling on your own to fill gaps; ask.

## Record for `_synthesis.md`

- which materials were used (and their type/bias),
- the run's evidence level,
- what was missing, and which personas are consequently lower-confidence or drafted.
