# Persona file format (output contract)

Output mirrors the bundled scaffold in `../assets/personas-scaffold/_TEMPLATE.md` so
[ux-review-panel](../../ux-review-panel/SKILL.md) reads the result without translation. One
file per persona, written to the project's `personas/<slug>.md`. If the project has no
`personas/` folder yet, scaffold it from `../assets/personas-scaffold/` (its `README.md`
holds the shared rules, `_TEMPLATE.md` the starting point) so the project gets the rules
doc too. Keep each persona file clean — it gets pasted as the persona's identity during a
stepwise walkthrough.

## Frontmatter

```yaml
---
name: "Maria, busy parent"      # short, memorable, human
grounded: true                  # true = real user data; false = drafted from brief
confidence: high                # high | medium | low (see grounding.md)
axis: "low tech comfort / high urgency"   # the one axis this persona stresses
scenarios:
  - "buy one item as a guest, in a hurry"
  # - optional second goal
---
```

`name`, `grounded`, `axis`, `scenarios` are what the review panel uses directly.
`confidence` is extra signal it can ignore. Add a `sources:` list only if you want it
visible in the file; otherwise keep provenance in `_synthesis.md`.

## Body

Same sections as the template, written as if briefing an actor:

```markdown
# <Name>

**One-line portrait:** <who they are + what they want, in one sentence.>

## Context
Where/when/device, what's going on around them, what pressure they're under.

## Goals & motivation
What they're trying to accomplish and why it matters — the job behind the clicks.

## Tech comfort & mental model
Fluency with this kind of interface; conventions they expect; where their model
mismatches the product.

## Frustrations & triggers
What makes them hesitate, distrust, or quit; the friction that loses *this* person.

## Voice
A couple of phrases in their real words (verbatim if grounded; illustrative if drafted)
so the walkthrough stays in character.
```

## Quality bar for the body

- **Reactable cold.** The portrait must let the persona walk screens naively — capture
  expectations and mental model, never knowledge of the "right" path.
- **Specific over generic.** "Distrusts forms that ask for a phone number before showing a
  price" beats "values privacy."
- **One axis, sharply.** Don't make every persona a well-rounded everyperson; each should
  fail and succeed differently from the others.

## `_synthesis.md` (the provenance companion)

One file for the whole set:

```markdown
# Persona synthesis — <project> (<date>)

## Run
Materials used (+ type/bias), evidence level (research-grounded | partial | drafted).

## Personas
For each: slug, primary axis, grounded/confidence, and the trait→evidence map
(which signals/quotes/sources back each major trait, what's inference, what's assumption).

## Coverage map
Which axes the set covers.

## Gaps
User types the evidence hints at but the set doesn't substantiate, and the data needed to
confirm them. This is the research agenda, not a failure.

## Changes (on re-run)
What new materials changed: traits revised, confidence bumped, conflicts surfaced.
```
