---
title: Accessibility Audit
last_updated: 2026-09-12
---

# Workflow: Accessibility Audit

**Purpose.** Audit a design or implementation for accessibility as a
design constraint, not a final QA pass (Constitution #14). This
workflow gives the procedure; the detailed, citable standard reference
is being written in parallel at `knowledge/accessibility.md` — treat
that file as the authority on specific numeric thresholds and exact
WCAG success-criterion numbers once it exists, and flag any conflict
between this file's checklist and that one in favor of the knowledge
file.

**Note on WCAG citations:** this workflow structures the audit around
the WCAG POUR principles (Perceivable, Operable, Understandable,
Robust) at a general, practitioner level. Specific success-criterion
numbers (e.g. "1.4.3 Contrast (Minimum)") should be pulled from
`knowledge/accessibility.md` when citing exact conformance claims —
**follow-up flagged:** this workflow should be cross-checked against
that file once written, and updated with precise SC references rather
than the general descriptions used here.

---

## Structure: POUR

### Perceivable — can users perceive the content and UI, regardless of sense?
- [ ] **Contrast** — text and meaningful icons meet at least AA-level
  contrast against their background (normal text ≈4.5:1, large text
  ≈3:1, as a working threshold pending exact figures from
  `knowledge/accessibility.md`); this includes text over images/
  gradients and disabled-looking-but-actually-interactive elements.
- [ ] Color is never the *only* signal for meaning (error states, chart
  series, status) — paired with icon, label, or pattern (Constitution
  #7).
- [ ] Images/icons conveying information have text alternatives; purely
  decorative ones are marked as such (not exposed to assistive tech).
- [ ] Text can be resized/reflowed without loss of content or function.

### Operable — can users operate every control, regardless of input method?
- [ ] **Touch targets** meet a minimum comfortable size (~44×44px
  working baseline) with adequate spacing between adjacent targets,
  especially at narrow/touch breakpoints (cross-reference
  `workflows/responsive-audit.md`).
- [ ] **Keyboard navigation** — every interactive element is reachable
  and operable via keyboard alone, in a logical sequence, with no
  keyboard trap.
- [ ] **Focus order** — tab order matches visual/logical reading order,
  not DOM accident order; a visible, clear focus indicator exists on
  every focusable element (never `outline: none` without a replacement).
- [ ] No content relies on a timing threshold, hover-only reveal, or a
  gesture that has no non-gesture alternative.
- [ ] **Reduced motion** — respects `prefers-reduced-motion`; motion
  that's justified per Constitution #12 still has a reduced/no-motion
  fallback that preserves the same information.

### Understandable — can users understand the content and how to use it?
- [ ] **Semantic structure** — headings in a real, non-skipping
  hierarchy (h1→h2→h3), landmarks/regions used correctly (nav, main,
  aside), lists marked as lists, tables marked as tables with headers
  associated to cells — not divs styled to look like these.
- [ ] **Screen reader labels** — every control has an accessible name
  (visible label, `aria-label`, or equivalent) that describes its
  purpose, not just its icon; icon-only buttons especially checked.
- [ ] **Form errors** — errors are announced (not color/icon only),
  associated with their field programmatically, describe what's wrong
  and how to fix it (not just "invalid"), and appear without requiring
  the user to guess which field failed.
- [ ] Language, labels, and instructions are consistent across the
  interface (the same action isn't called two different things in two
  places).

### Robust — does it work reliably across assistive technology and platforms?
- [ ] Valid, well-formed markup; ARIA used only to fill real gaps, not
  layered on top of already-correct semantic HTML (ARIA can break
  things if misapplied — prefer native semantics first).
- [ ] Custom components (custom dropdowns, tabs, modals, sliders) match
  their platform's expected keyboard/ARIA pattern, not an invented one
  (Constitution #19 applies to accessibility patterns too).
- [ ] Dynamic content changes (toasts, live-updating values, loading
  states) are announced via appropriate live regions, not silently
  updated.

---

## Practical checklist summary (quick pass)

- [ ] Contrast
- [ ] Touch targets
- [ ] Focus order
- [ ] Semantic structure
- [ ] Keyboard navigation
- [ ] Screen reader labels
- [ ] Reduced motion
- [ ] Form errors

Use this quick list for a fast pass; use the full POUR structure above
for a real audit before shipping.

---

## Procedure

1. Run the quick-pass checklist first on the whole screen/flow.
2. For anything that fails or is uncertain, go to the relevant POUR
   section for the specific check and detail.
3. Report every failure in the same PROBLEM / WHY IT MATTERS / SEVERITY
   / RECOMMENDED CHANGE / EXPECTED IMPROVEMENT format as
   `workflows/critique-ui.md`, so findings merge cleanly into a
   broader critique. Accessibility failures that block task completion
   for any user should default to Critical severity, not Minor — this
   matches Constitution #14's framing of accessibility as a constraint
   on the design space, not a nice-to-have.
4. Log any threshold or citation used from this workflow that should
   really come from `knowledge/accessibility.md` once it's written, so
   the two files can be reconciled — see `research/research-log.md`.

## Done when

The quick-pass checklist and all four POUR sections have been checked
against the actual implementation (not just the visual design — focus
order and screen reader labels require inspecting markup/DOM, not just
looking at the screen), and every failure is logged with severity.
