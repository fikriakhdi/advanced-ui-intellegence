---
title: Critique UI
last_updated: 2026-09-12
---

# Workflow: Critique UI

**Purpose.** A rigorous, honest evaluation of a rendered interface —
whether just generated or existing. This is the CRITIQUE stage of
`workflows/generate-ui.md` and the diagnostic step
`workflows/redesign-ui.md` depends on.

**The core rule: do not automatically praise generated work.** A
critique that defaults to "looks great!" is not a critique, it's a
courtesy — and it's actively harmful here because it prevents the
system from ever improving its own output. The critic must be willing
to say things like "visually attractive but structurally weak" or
"usable but generic." A critique with zero identified weaknesses on a
first pass should be treated as suspicious, not as a good sign — re-
examine with `knowledge/anti-patterns.md` open before concluding the
design is actually strong.

---

## Dimensions to evaluate

Score each 1-10 where a numeric scale is useful (most dimensions);
some are better captured as pass/fail or a short judgment. A score
without a reason attached is worthless — every score must be
justifiable by pointing at something specific in the interface.

1. **Visual hierarchy** — Does the eye land on the right thing first,
   second, third? Test per Constitution #7: does hierarchy survive
   desaturation?
2. **Information architecture** — Does the structure match user goals
   (cross-check against `workflows/information-architecture.md`
   output if available)?
3. **Task clarity** — Can the user tell what to do next, and what the
   one primary action per region is (Constitution #8)?
4. **Typography** — Type scale, weight usage, line length, line
   height; is a real scale used or are sizes ad hoc?
5. **Spacing** — Consistent scale, meaningful proximity/grouping
   (Constitution #5), no arbitrary gaps.
6. **Alignment** — Are elements aligned to a shared grid/baseline, or
   do things almost-but-not-quite line up (a strong "ungenuine" tell)?
7. **Color** — Role-based and restrained, or decorative/arbitrary?
   Does color reinforce a hierarchy that already exists without it
   (Constitution #7)?
8. **Component consistency** — Same component behaves/looks the same
   everywhere it appears; deviations are intentional, not accidental.
9. **Information density** — Right density for the stated user
   frequency/expertise (Constitution #11), not uniformly sparse or
   uniformly packed.
10. **Interaction clarity** — Are affordances legible (what's
    clickable, what's disabled, what's loading)? Is motion justified
    per Constitution #12?
11. **Responsive behavior** — Spot-check narrow/wide; defer full
    coverage to `workflows/responsive-audit.md`.
12. **Accessibility** — Spot-check contrast/focus/semantics; defer
    full coverage to `workflows/accessibility-audit.md`.
13. **Product appropriateness** — Does this actually fit *this*
    product's stated user, frequency, and environment (from
    `workflows/analyze-product.md`), or could it be swapped onto any
    other product unchanged? The latter is a failure even if every
    other dimension scores well.
14. **Brand personality** — Does it express the design character
    decided in ANALYZE, or default to neutral-generic?
15. **Visual originality** — Is there at least one deliberate,
    specific design decision, or is this assembled entirely from
    default patterns? Cross-reference `knowledge/anti-patterns.md` —
    centered-hero-plus-icon-grid, purple/blue gradient defaults,
    cards-for-everything, and generic stat tiles are the primary
    lookup table for spotting genericness. If three or more anti-
    patterns from that file are present, this dimension scores low
    regardless of how polished the execution is.
16. **Implementation quality** — Does the code match the visual intent
    (no CSS hacks fighting the design), is it maintainable, does it
    use the defined system tokens rather than one-off values?

---

## Reporting format

For **every identified weakness**, report all five fields — a
weakness reported without all five is incomplete and should be
finished before the critique is considered done:

- **PROBLEM** — what's wrong, specific and pointing at something real
  (not "spacing could be better" — "the 8px gap between the KPI label
  and value is smaller than the 16px gap between unrelated KPI cards,
  inverting the intended grouping").
- **WHY IT MATTERS** — which principle or user consequence this
  connects to (cite a Constitution number or workflow where relevant).
- **SEVERITY** — Critical (breaks comprehension/task completion or an
  accessibility requirement) / Major (noticeably weakens the design or
  reads as generic) / Minor (polish-level).
- **RECOMMENDED CHANGE** — a concrete, actionable fix, not a
  restatement of the problem.
- **EXPECTED IMPROVEMENT** — what gets better if the change is made,
  stated so it can be checked after REVISE.

## Overall verdict

After scoring all dimensions and listing weaknesses, write one or two
plain sentences that would survive being read by someone who built the
thing — including the honest ones: "Visually polished and on-brand,
but the IA doesn't reflect the stated information priority — revenue
is buried below three lower-priority sections. Fix that before
anything else." A verdict that only lists positives, or that hedges
every criticism into invisibility, has failed this workflow's purpose.

## When nothing is actually wrong

It's possible for a design to genuinely score well across the board.
When that happens, say so plainly, but only after checking:
- Every "should preserve" familiar pattern was a *deliberate* choice
  per Constitution #19, not an unexamined default.
- `knowledge/anti-patterns.md` was actually consulted, not skipped.
- At least one dimension was scored below 8 with a reason, or an
  explicit note explains why none was — a uniform 9/10 across sixteen
  dimensions is itself a signal the critique wasn't rigorous enough.
