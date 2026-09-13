---
title: Accessibility as a Design Constraint
confidence-note: WCAG 2.2 success criteria numbers and text (1.4.3, 1.4.11, 2.5.8, 2.4.7, 2.4.11) verified directly against w3.org this session. ARIA APG role/state/keyboard patterns for dialog, combobox, and tabs verified against w3.org this session. Apple HIG and Material touch-target numbers are ESTABLISHED from training knowledge (direct fetch was blocked by JS-rendering) — treat the specific pixel/point/dp figures as "generally cited as," and re-verify against the live HIG/Material sites before quoting them in a client-facing spec.
last_updated: 2026-09-12
---

# Accessibility

Governed by **Constitution #14**: accessibility is a design constraint —
like "must load in under 2 seconds" — not a checklist run after visual
design is finalized. This file exists so that contrast, touch targets,
focus order, and semantics are treated as *inputs* to decisions made in
month one (palette, type scale, component choice), not audit items found
in month six after they're expensive to fix.

## 1. Contrast: the actual math

**FOUNDATIONAL — WCAG 2.2, verified.**

- **SC 1.4.3 Contrast (Minimum), Level AA**: text and images of text must
  have a contrast ratio of **at least 4.5:1** against their background.
  **Large text** (≥18pt / 24px regular, or ≥14pt / ~18.66px bold) drops to
  **3:1**. Incidental text (disabled controls, purely decorative text,
  text that's part of a logo) is exempt.
- **SC 1.4.11 Non-text Contrast, Level AA**: UI components (input borders,
  button boundaries, custom checkbox/radio graphics, focus indicators) and
  meaningful graphical objects (icons that convey information, chart
  lines/segments) need **at least 3:1** against adjacent colors.
- **Design-time implication**: contrast is a constraint on the *palette*,
  not a per-instance fix. If a brand color is chosen in isolation and
  turns out to be 3.8:1 on white, every future use of that color for body
  text is broken — the fix is to constrain the palette definition itself
  (pick brand colors, then generate a full tint/shade ramp and verify
  which steps clear 4.5:1 on your actual background colors, before any
  screen is designed). This is precisely the "insufficiently contrasted
  brand color chosen in month one" failure Constitution #14 names.
- **Practical rule**: build your color ramp so that at minimum one
  "on-light" step and one "on-dark" step of every semantic hue (primary,
  success, warning, danger) is pre-verified at 4.5:1, and bake those as
  the only tokens allowed for text-on-color use (see
  `design-systems/design-tokens.md`) — don't leave contrast verification
  to whoever happens to use the color next.
- Don't rely on color alone to convey state or category (this is also
  Constitution #7) — colorblindness (~8% of men, ~0.5% of women have some
  form of red-green deficiency) means status must be redundantly encoded
  with icon, label, or pattern, not hue alone. Test palettes through a
  deuteranopia/protanopia simulator, not just a contrast checker.

## 2. Touch and pointer target size

**ESTABLISHED, cross-referenced across three sources with different
minimums — design to the largest one your context requires.**

- **WCAG 2.2 SC 2.5.8 Target Size (Minimum), Level AA — verified text**:
  "The size of the target for pointer inputs is at least 24 by 24 CSS
  pixels," except when: (a) **Spacing** — undersized targets pass if a
  24px-diameter circle centered on the target doesn't intersect another
  target's circle (equivalent rule of thumb: target size *s* plus gap *g*
  between adjacent targets must satisfy `s + g ≥ 24`); (b) **Equivalent**
  — an equally-sized alternative control achieves the same function
  elsewhere on the page; (c) **Inline** — the target sits inside a
  sentence/is constrained by surrounding text's line-height; (d)
  **User agent controlled** — size isn't author-modified; (e)
  **Essential** — a specific smaller presentation is legally or
  functionally required. This is a *minimum floor*, not a target to design
  toward.
- **Apple HIG** (ESTABLISHED from training knowledge, verify against live
  HIG before publishing a client spec): commonly cited minimum tappable
  area on iOS/iPadOS is **44×44 points**.
- **Material Design** (ESTABLISHED from training knowledge): commonly
  cited minimum touch target is **48×48dp**, with a recommended 8dp
  minimum spacing between adjacent targets.
- **Design-time implication**: touch target sizing is a layout-density
  constraint decided with the type scale and component sizing, not a
  later "make buttons bigger on mobile" patch. A dense desktop data table
  with 32px-tall row action icons needs either larger touch targets in its
  mobile/touch recomposition (see `responsive-design.md` §4/§5) or
  needs those actions moved into a per-row menu with an appropriately
  sized trigger — decide this when the table's interaction model is
  designed, not during touch QA.
- Visible target and *hit target* can differ (a small icon can have
  invisible padding extending its tappable area to the minimum) — this is
  the normal way to reconcile a visually compact design with the size
  floor, and should be a standard practice on every icon-only interactive
  element, not a special case.

## 3. Focus: order, visibility, and management

**FOUNDATIONAL — WCAG 2.2, verified.**

- **SC 2.4.7 Focus Visible, Level AA**: any keyboard-operable UI must have
  a visible indicator of keyboard focus. Never suppress the default focus
  ring (`outline: none`) without providing an equally or more visible
  custom replacement — this is one of the single most common and most
  damaging accessibility regressions in shipped interfaces.
- **SC 2.4.11 Focus Not Obscured (Minimum), Level AA** (new in 2.2): the
  focused element must not be entirely hidden by other content (a sticky
  header/footer covering it, for instance) — at least part of it must
  remain visible when focused.
- **Focus order** must follow a meaningful sequence — generally matching
  visual/reading order. A grid or multi-column layout built with CSS that
  visually reorders content (via `order` or absolute positioning) without
  reordering the DOM produces a focus order that contradicts what's seen,
  disorienting both keyboard and screen-reader users.
- **Focus management on dynamic UI** (dialogs, drawers, menus) — per the
  W3C ARIA Authoring Practices Guide (APG) dialog pattern, verified this
  session: on open, focus **moves to an element inside the dialog**;
  Tab/Shift+Tab **cycle within** the dialog (a focus trap); Escape closes
  it; on close, focus **returns to the element that invoked it** (unless
  that element no longer exists, in which case focus should go somewhere
  sensible and predictable). Initial focus placement depends on content:
  first focusable element by default, but a static heading with
  `tabindex="-1"` for complex/long content (so focus doesn't
  auto-scroll past context), or the least-destructive option for a
  confirm/destroy dialog (so a stray Enter doesn't confirm deletion).
- **Design-time implication**: any component with an open/close state
  (dialog, drawer, popover, menu, toast with actions) needs its focus
  behavior specified *as part of the component spec*, not left to
  whoever implements it — see `design-systems/components.md`.

## 4. Semantic HTML and ARIA — use native elements first

**FOUNDATIONAL.** The first rule of ARIA is not to use ARIA where a native
HTML element already provides the needed semantics, focus behavior, and
keyboard handling for free: `<button>` not `<div role="button">`,
`<nav>`/`<main>`/`<header>` landmarks, real `<table>` markup for tabular
data (not CSS grid divs pretending to be a table), native `<select>` where
its behavior is sufficient. ARIA is for the gap where no native element
exists (a combobox, a tab set, a tree, a custom slider) — and every custom
role you add commits you to implementing the *entire* keyboard and state
contract that role implies, not just the visual look.

Two real ARIA APG patterns verified this session, to standardize how the
skill designs these components (see `design-systems/components.md` for
the full library):

**Tabs** — `tablist` container; each tab is `role="tab"` with
`aria-selected="true"` on the active tab and `false` on the rest; each
panel is `role="tabpanel"`. Arrow keys move focus between tabs; Home/End
jump to first/last. Two valid activation models: *automatic* (a tab
activates as soon as it receives focus — requires cheap-to-render panels)
or *manual* (Space/Enter required to activate — safer when switching
loads expensive content). Pick one per component and be consistent.

**Combobox** — the input itself is `role="combobox"` with
`aria-expanded` (true/false), `aria-controls` (pointing at the popup),
and `aria-autocomplete` (`none`/`list`/`both`). The popup is typically an
implicit `listbox` of `option` elements. Critically: DOM focus **stays on
the input**; the currently-highlighted option is communicated via
`aria-activedescendant`, not by moving real focus into the popup — moving
real focus breaks the type-to-filter interaction and disorients screen
reader users. Down Arrow opens the popup or moves the "virtual" selection
down; Enter accepts; Escape closes.

## 5. Keyboard navigation as a first-class interaction mode

**FOUNDATIONAL.** Every interactive element and every custom component
must be fully operable by keyboard alone — not as a fallback for users who
can't use a mouse, but because keyboard operability is also what screen
readers, switch devices, and voice control depend on under the hood. A
design that "works" only via drag, hover, or a mouse-precision gesture has
already excluded a meaningful set of users and typically also fails
automated and manual audits. Every custom interactive pattern needs a
specified keyboard mapping *before* visual design is finalized — it's
frequently the keyboard mapping that reveals a proposed interaction is
overcomplicated (see Constitution #19).

## 6. Screen reader considerations

**ESTABLISHED.**
- Meaningful images need `alt` text describing their function/content;
  purely decorative images get `alt=""` (empty, not omitted) so screen
  readers skip them rather than announcing the filename.
- Icon-only buttons need an accessible name (`aria-label`, or visually
  hidden text) — an icon alone conveys nothing to a screen reader unless
  labeled.
- Live, asynchronously-updating regions (a toast, a form error summary, a
  live search result count) need `aria-live` (`polite` for non-urgent
  updates, `assertive` sparingly for urgent ones) so screen reader users
  learn about the update without having to re-scan the page.
- Form errors must be programmatically associated with their field
  (`aria-describedby` pointing at the error text, `aria-invalid="true"` on
  the field) — a red border and adjacent text alone are invisible to a
  screen reader unless linked.
- Heading structure (`h1`–`h6`) should reflect actual document hierarchy —
  screen reader users routinely navigate a page by jumping between
  headings, so skipped levels or headings chosen for their default font
  size rather than their semantic level break that navigation mode.

## 7. Reduced motion

**FOUNDATIONAL**, detailed in `knowledge/motion-interaction.md`. In
summary: honor `prefers-reduced-motion: reduce` by removing
non-essential motion (parallax, large-scale transform/translate
animations, auto-playing decorative animation) while preserving
functionally necessary state changes (an item disappearing on delete
still needs *some* signal, but can cross-fade or simply cut instead of
sliding/bouncing). Vestibular disorders can make large-scale motion
physically uncomfortable, not just distracting — this is a genuine
accessibility requirement, not a preference toggle to treat as optional.

## 8. Form accessibility

**ESTABLISHED.**
- Every input has a real, programmatically-associated `<label>` (not
  placeholder-as-label — placeholder text disappears on input, isn't
  reliably announced, and typically fails contrast requirements at the
  lighter tint most designs use for it).
- Group related fields (e.g. a set of radios, an address block) with
  `<fieldset>`/`<legend>` so the group's purpose is announced.
- Required fields are marked in a way that isn't color-only (an asterisk
  plus text, `aria-required="true"`), and the *pattern* for marking
  required vs. optional should be decided once as a system convention,
  not per form.
- Validate at a predictable, consistent trigger point (see
  Constitution #13 — consistency of *behavior*): validating character-by
  character while typing is usually premature and noisy; validating only
  on final submit is usually too late for a long form. A common, defensible
  default is on-blur validation for format errors plus a full re-check on
  submit — document whichever convention you pick and apply it everywhere.
- Autofill/autocomplete attributes (`autocomplete="email"`, etc.) should
  be set correctly — they benefit assistive tech and password managers
  alike, not just non-disabled users' convenience.

## 9. Empty, loading, and error states are accessibility surfaces too

**CONTEXTUAL.** A loading spinner with no accessible name announces
nothing to a screen reader user — pair it with visually-hidden status text
and `aria-live="polite"` (or `role="status"`). An error state that
communicates severity only via a red icon fails both contrast-independent
comprehension and screen reader users; pair icon + color + text every
time (see `design-systems/components.md` §Empty/Loading/Error States).

## 10. Deciding early: the constraint-not-checklist mandate in practice

Per Constitution #14, here is what "deciding early" concretely means at
each design stage:

- **Palette selection** (before any screen exists): verify the full ramp
  against 4.5:1/3:1 on your actual background tones, in light *and* dark
  mode, before locking brand colors. A palette that only works in light
  mode is not done.
- **Type scale**: base font size and line-height chosen with legibility
  and zoom-to-200%-reflow (SC 1.4.4/1.4.10) in mind, not purely
  brand-aesthetic size.
- **Component selection**: choosing a custom slider over a native
  `<input type=range">`, or a custom dropdown over `<select>`, is a
  decision that immediately obligates full ARIA/keyboard reimplementation
  — make that trade consciously (novelty must earn its cost, per
  Constitution #2/#19), not by default.
- **Density decisions** (Constitution #11): a high-density expert UI with
  small interactive targets must be paired with adequate spacing between
  them (WCAG's spacing exception) or a way to increase target size (a
  "comfortable" density mode) — decide this as part of the density
  decision, not as an afterthought once the dense layout is built.

## Common failure modes
- Contrast fixed by darkening text on a *single* screen after a designer
  notices it "looks light," rather than fixing the underlying token.
- `outline: none` shipped without a replacement focus style.
- Custom dropdown/combobox with no keyboard support beyond click.
- Icon-only buttons with no accessible name.
- Motion-heavy transitions with no `prefers-reduced-motion` handling.
- Placeholder text used as the only label.
- Color as the sole signal for validation state or category.

## Sources
- [WCAG 2.2 Quick Reference (1.4.3, 1.4.11)](https://www.w3.org/WAI/WCAG22/quickref/?showtechniques=143%2C146) — accessed 2026-09-12
- [WCAG 2.2 SC 2.5.8 Target Size (Minimum) — wcag22aa.org](https://wcag22aa.org/new-criteria/target-size/) — accessed 2026-09-12
- [W3C ARIA APG — Dialog (Modal) Pattern](https://www.w3.org/WAI/ARIA/apg/patterns/dialog-modal/) — accessed 2026-09-12
- [W3C ARIA APG — Combobox Pattern](https://www.w3.org/WAI/ARIA/apg/patterns/combobox/) — accessed 2026-09-12
- [W3C ARIA APG — Tabs Pattern](https://www.w3.org/WAI/ARIA/apg/patterns/tabs/) — accessed 2026-09-12
- Apple Human Interface Guidelines — Layout/touch target guidance (44×44pt commonly cited) — training knowledge, live fetch blocked by JS rendering; re-verify at https://developer.apple.com/design/human-interface-guidelines/
- Material Design — touch target guidance (48×48dp commonly cited) — training knowledge, live fetch blocked by JS rendering; re-verify at https://m3.material.io/
- Constitution principles #2, #7, #11, #13, #14, #16, #19 — `/home/claude/advanced-ui-skill/knowledge/ui-principles-2026.md`
