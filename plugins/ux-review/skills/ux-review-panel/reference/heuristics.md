# Heuristics catalog

Shared reference for all experts. Each heuristic has a **stable key** — cite the key on
every finding so synthesis can dedup by rule. Add new heuristics here with a new key; never
invent wording inside a role file.

## Contents

- [Nielsen usability heuristics (`NN-*`)](#nielsen-usability-heuristics)
- [Task-flow rules (`NN-*` extensions)](#task-flow-rules)
- [Visual design (`VIS-*`)](#visual-design)
- [Customer journey (`CX-*`)](#customer-journey)
- [Accessibility (`A11Y-*`)](#accessibility)

## Nielsen usability heuristics

| Key | Heuristic | The check |
|-----|-----------|-----------|
| NN-01 | Visibility of system status | The system keeps users informed about what's going on through timely, appropriate feedback (loading, saved, progress, current location). |
| NN-02 | Match between system and real world | Speaks the users' language; words, phrases, concepts, and order follow real-world conventions, not system internals. |
| NN-03 | User control and freedom | Clearly marked exits, undo, redo, cancel; users can leave unwanted states without penalty. |
| NN-04 | Consistency and standards | Same words/actions/situations mean the same thing; follows platform conventions. |
| NN-05 | Error prevention | Designs that prevent problems before they occur — constraints, confirmations, good defaults, forgiving formats. |
| NN-06 | Recognition rather than recall | Elements, actions, options are visible; the user isn't forced to remember info across screens. |
| NN-07 | Flexibility and efficiency of use | Accelerators and shortcuts for experienced users; lets users tailor frequent actions. |
| NN-08 | Aesthetic and minimalist design | No irrelevant or rarely-needed info competing with the essentials. |
| NN-09 | Help users recognize, diagnose, recover from errors | Error messages in plain language, state the problem, suggest a solution. |
| NN-10 | Help and documentation | Help is available, findable, task-focused, concrete when needed. |

## Task-flow rules

Usability-expert extensions beyond the ten classics:

- **NN-11 Sensible defaults** — most likely choice preselected; user confirms rather than
  constructs.
- **NN-12 State coverage** — every screen has empty / loading / error / success variants
  designed, not just the happy path.
- **NN-13 Reversibility of commitment** — actions with consequences (pay, delete, submit)
  are previewable and, where possible, reversible or confirmable.

## Visual design

| Key | Rule | The check |
|-----|------|-----------|
| VIS-01 | Hierarchy | Visual weight matches importance; one clear primary action per screen; readable type scale. |
| VIS-02 | Consistency | Repeated elements share treatment across screens (buttons, icons, radii, shadows, labels). |
| VIS-03 | Alignment & spacing | Consistent grid and spacing rhythm; intentional whitespace, no arbitrary gaps. |
| VIS-04 | Color use | Purposeful palette; semantic colors (error/success/warning) applied consistently. |
| VIS-05 | Token adherence | Values map to the design system / tokens, not one-off hardcoded values. |
| VIS-06 | Density & legibility | Comfortable text sizes, line lengths, and information density. |
| VIS-07 | Iconography | Icons are recognizable, consistent in style and metaphor, and labeled where ambiguous. |
| VIS-08 | Motion & feedback styling | Visual feedback (hover, pressed, selected, disabled) is present and distinguishable. |

## Customer journey

| Key | Rule | The check |
|-----|------|-----------|
| CX-01 | Continuity | Each step follows naturally; state and context carry across transitions. |
| CX-02 | Effort curve | Effort is paced sensibly; no work spike right before the goal. |
| CX-03 | Expectation setting | Progress and next-step are always knowable (steppers, previews, counts). |
| CX-04 | Emotional touchpoints | Commitment and risk moments are handled with reassurance and clarity. |
| CX-05 | Drop-off risk | Identifies where and why a real user would abandon. |
| CX-06 | Recovery paths | Broken journeys (error, empty, timeout) offer a graceful way back in. |

## Accessibility

WCAG 2.1 AA-oriented. Mark each finding **confirmed** (measurable from the artifact) or
**needs live check** (requires running product or markup).

| Key | Rule | Threshold / check |
|-----|------|-------------------|
| A11Y-01 | Contrast | Text ≥ 4.5:1 (≥ 3:1 large); UI/graphics ≥ 3:1. (WCAG 1.4.3 / 1.4.11) |
| A11Y-02 | Target size | Interactive targets ≥ 44×44 px. (WCAG 2.5.5) |
| A11Y-03 | Color not sole signal | Meaning has a non-color cue too. (WCAG 1.4.1) |
| A11Y-04 | Text legibility | Adequate size/spacing; essential text is real text, not baked into images. (1.4.4 / 1.4.5) |
| A11Y-05 | Labels & semantics | Inputs have visible labels; programmatic name/role correct. (1.3.1 / 4.1.2) |
| A11Y-06 | Focus & keyboard | Visible focus, logical order, no traps. (2.4.7 / 2.1.1–2.1.2) — needs live check |
| A11Y-07 | Alternatives | Alt text, captions, transcripts for non-text content. (1.1.1 / 1.2.x) |
| A11Y-08 | Reflow & zoom | Content usable at 200% zoom / 320px reflow without loss. (1.4.10) — needs live check |
