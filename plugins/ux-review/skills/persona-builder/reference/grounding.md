# Grounding — evidence, confidence, honesty

A persona must be **defensible**: point at any trait, ask "says who?", get evidence. This
file defines how traits attach to evidence, how confidence is set, and where the line between
observed and inferred sits.

## Every trait traces to a signal

Each non-trivial statement rests on one of:

- **direct evidence** — a quote, a ticket, a stat, an observed behavior from the materials,
- **reasonable inference** — a conclusion drawn from evidence, explicitly marked as
  inference,
- **assumption** — a guess with no backing (only acceptable in drafted personas, and
  labeled).

The persona *file* stays readable (it's pasted as identity into the review panel), so don't
clutter it with inline citations. Put the trait→source map in `_synthesis.md`, keyed by
persona.

## Quotes

- **Grounded persona:** use **real verbatims** from the materials.
- **Drafted persona:** you may write plausible lines, but mark the Voice section as
  illustrative, not sourced.

Never present an invented quote as if it came from a user.

## Confidence ladder

Set `confidence` per persona:

- **high** — multiple independent sources converge on this user type; goals, behaviors, and
  frustrations are all evidenced.
- **medium** — real evidence exists but is thin, single-source, or partly inferred.
- **low** — mostly inference or drafted from a brief; a hypothesis to validate.

And the boolean `grounded`:

- `true` — built from real user data (high/medium confidence).
- `false` — drafted from assumptions/brief (low confidence). The review panel will caveat
  findings from these personas.

Be honest. An over-confident persona launders a guess into an apparent fact and misleads
downstream decisions.

## The gaps list

In `_synthesis.md`, list user types the evidence hints at but doesn't substantiate, and the
data needed to confirm them.

## Bias notes

Each material type skews (tickets → unhappy users, NPS → extremes, sales notes → seller's
frame). Carry a short bias note per persona where a source skew shaped it.
