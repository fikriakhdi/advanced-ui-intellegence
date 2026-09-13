---
title: Redesign UI
last_updated: 2026-09-12
---

# Workflow: Redesign UI

**Purpose.** Improve an *existing* interface, as opposed to generating
one from scratch (`workflows/generate-ui.md`). Redesign carries a risk
generation doesn't: an existing interface usually has real users who
have already learned it, and "improving" it can destabilize working
patterns and habits even while fixing genuine problems. This workflow
sequences change to avoid that.

---

## Step 1: Reverse-engineer the current IA and hierarchy

Before judging anything, describe what's actually there, neutrally:

- Map the existing navigation, screens, and sections back into the
  Goals → Tasks → Information Groups shape from
  `workflows/information-architecture.md`, worked in reverse. What
  user goal does each existing section seem to serve, even if
  clumsily?
- Identify the *de facto* information priority — what's actually
  prominent on screen — versus what the product's real priority should
  be per a fresh `workflows/analyze-product.md` pass. The gap between
  these two is usually where the real problems are.
- Note existing conventions users have plausibly already learned:
  where things live, what a particular color or icon has come to mean
  in this product, any non-standard-but-established pattern.

## Step 2: Diagnose with critique-ui.md

Run `workflows/critique-ui.md` in full against the current interface.
This is the mandatory diagnostic step before any redesign work — it
produces the scored, weakness-by-weakness input this workflow sorts in
Step 3. Do not skip straight to "what I'd change" impressions; use the
structured critique output.

## Step 3: Sort findings into three buckets

Every critique finding and every reverse-engineered observation from
Step 1 goes into exactly one bucket:

- **Must preserve** — things that work and that users rely on, even if
  they wouldn't be the first choice on a blank slate. This includes:
  navigation locations for high-frequency tasks, muscle-memory
  shortcuts, terminology the user base has adopted, and any pattern
  that scored acceptably in critique even if not optimal. Constitution
  #19 governs this bucket directly: familiar patterns are only worth
  replacing when the alternative is *meaningfully* better, not just
  different.
- **Should fix** — genuine problems from the critique, especially
  anything marked Critical or Major severity: broken hierarchy,
  accessibility failures, information priority that doesn't match the
  real product priority, competing primary actions. These are not
  optional and should be scheduled even if disruptive.
- **Could improve** — Minor-severity critique items, aesthetic
  refinements, and nice-to-haves that don't fix a real problem. These
  are the first things to cut or defer if the redesign needs to be
  scoped down.

A finding that seems to belong in more than one bucket usually means
it needs to be split: e.g. "the reports table" might have a "must
preserve: column order users have memorized" part and a "should fix:
contrast on the muted rows fails AA" part.

## Step 4: Sequence the changes

Do not ship all three buckets at once. Sequence by risk and payoff:

1. **Should-fix items that don't touch preserved patterns** first —
   these are close to free: real improvement, minimal destabilization
   (e.g. a contrast fix, a spacing correction, adding a missing label).
2. **Should-fix items that do touch preserved patterns** next, and
   only after confirming via Step 3 that the value of fixing outweighs
   the disruption — consider a transitional approach (keep the old
   entry point working while introducing the new one, a visible "what
   changed" note, a gradual rollout) rather than a silent swap.
3. **Could-improve items** last, and only if budget/scope allows —
   never let polish items delay or get bundled with should-fix items
   in a way that makes the release riskier than it needs to be.
4. **Never bundle an unrelated visual refresh with a structural IA
   fix** in one pass if avoidable — it becomes impossible to tell users
   (or yourself, in a later critique) which change caused which effect,
   good or bad.

## Step 5: Re-validate

After changes are made, run `workflows/critique-ui.md` again on the
new version — not just the changed sections, since a Should-fix change
can have hierarchy ripple effects on Must-preserve sections nearby.
Also re-run `workflows/responsive-audit.md` and
`workflows/accessibility-audit.md` if the changed sections touch
layout or semantics. Confirm every Should-fix item from Step 3 is
actually resolved, and confirm no Must-preserve item was
inadvertently broken in the process — the latter is the specific
failure mode this whole workflow exists to prevent.

## A note on confidence

Reverse-engineering "why does this exist" from an existing interface
without access to its original rationale, usage data, or its users is
inherently uncertain. Where Step 1's guess about a Must-preserve
pattern can't be verified (no usage data, no access to real users),
say so explicitly and default to caution — treat it as Must-preserve
unless the critique found it Critical, rather than assuming it's safe
to change because it looks suboptimal on inspection alone.
