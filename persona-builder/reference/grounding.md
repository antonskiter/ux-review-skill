# Grounding — evidence, confidence, honesty

The value of a persona here is that it's **defensible**. Anyone should be able to point at a
trait and ask "says who?" and get evidence. This file defines how traits attach to evidence,
how confidence is set, and where the line between observed and inferred sits.

## Every trait traces to a signal

When drafting a persona, each non-trivial statement should rest on one of:

- **direct evidence** — a quote, a ticket, a stat, an observed behavior from the materials,
- **reasonable inference** — a conclusion drawn from evidence, explicitly marked as
  inference,
- **assumption** — a guess with no backing (only acceptable in drafted personas, and
  labeled).

The persona *file* stays readable (it's pasted as identity into the review panel), so don't
clutter it with inline citations. Put the trait→source map in `_synthesis.md` instead, keyed
by persona. The discipline is real even though the audit trail lives next door.

## Quotes

- **Grounded persona:** use **real verbatims** from the materials. A genuine "do I *have* to
  make an account just to buy one thing?" is worth more than any invented line.
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

Be honest. An over-confident persona is worse than a humble one — it launders a guess into
an apparent fact and misleads every downstream decision.

## The gaps list

Grounding includes naming what you *can't* support. In `_synthesis.md`, list user types the
evidence hints at but doesn't substantiate, and data you'd need to confirm them. This turns
a persona set into a research agenda, not a false claim of completeness.

## Bias notes

Each material type skews (tickets → unhappy users, NPS → extremes, sales notes → seller's
frame). Carry a short bias note per persona where a source skew shaped it, so readers weight
it correctly.
