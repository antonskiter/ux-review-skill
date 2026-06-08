# Personas

Project-side personas for the `ux-review-panel` skill. The skill ships experts; **personas
live here, in the project**, because they're grounded in *your* users and research, not in
the tool.

One persona per file (`*.md`, frontmatter + body). Drop them in this folder; the skill
discovers them automatically (it also checks `research/` and will ask if it finds none).
Use [_TEMPLATE.md](_TEMPLATE.md) as the starting point.

## Shared rules for all personas

These hold for every persona, so individual files don't repeat them:

1. **Grounded over invented.** Prefer personas built from real research (interviews,
   analytics, support tickets). If a persona is drafted from a brief without research, set
   `grounded: false` — the skill will label its findings *"not grounded in research"* so
   nobody over-trusts them.
2. **A goal-bearing user, not a demographic.** What this person is *trying to do* and
   *why* matters more than age/income. Lead with motivation, context, and mindset.
3. **Honest friction, including unflattering.** A useful persona gets confused, impatient,
   misreads labels, gives up. Personas that glide through perfectly find nothing.
4. **Stays naive during a run.** The skill walks each persona through screens **one at a
   time** and never shows it other personas, the experts, the spec, or anyone's findings.
   Write the portrait so it can react cold — don't bake in knowledge of the "right" path.
5. **Distinct from the others.** Each persona should stress a *different* axis (tech
   comfort, urgency, accessibility need, domain expertise, device). Overlapping personas
   waste a sub-run. Aim for coverage, not a crowd.
6. **Tied to a scenario.** Every persona carries at least one concrete goal the skill turns
   into a walkthrough scenario ("buy one item as a guest, in a hurry"). Vague personas
   produce vague routes.

## What the skill reads from a persona

- `name`, the one-line portrait, and the body → pasted into the persona sub-run as its
  identity (see the skill's `prompts/persona-walkthrough.md`).
- `scenarios` → each becomes a stepwise walkthrough with its own route through the flow
  graph.
- `grounded` → controls the research caveat on findings.

Everything else in the body (frustrations, tech comfort, context) shapes how the persona
reacts. Write it as if briefing an actor, not filling a database.
