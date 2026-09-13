---
title: Responsive Design — Space, Not Devices
confidence-note: Framework in "The Recomposition Decision" section is treated as authoritative elsewhere in this knowledge base (referenced by Constitution #10 and #18) — revise with care. Container query mechanics verified against 2026 practical guides; breakpoint numbers below are industry-common starting points, not universal law (per Constitution #18, they must always yield to actual content inflection points).
last_updated: 2026-09-12
---

# Responsive Design

Governed by **Constitution #18** (respond to available space and content, not
device labels) and **Constitution #10** (mobile interfaces are recomposed,
not shrunk). This file gives the operational framework for both.

## 1. Breakpoints are content decisions, not device decisions

**FOUNDATIONAL.** A breakpoint is the point at which a specific layout stops
working for its content — a grid whose cards get too narrow to read, a
table that starts truncating every column, a nav bar whose items start
wrapping. It is discovered by looking at *this* content in *this* layout at
shrinking widths until something breaks, not by copying "mobile: 375,
tablet: 768, desktop: 1440."

Device-labeled breakpoints fail for three concrete reasons:
- **Device diversity**: phone widths now range roughly 320–430px, tablets
  600–1280px (a tablet in portrait can be narrower than a phone in
  landscape), and "desktop" spans 1024px laptops to 3440px ultrawides. Any
  single number claimed to represent "tablet" is already wrong for a large
  fraction of tablets.
- **Context, not device**: the same 1440px browser window might have your
  component in a full-width page, a 320px sidebar panel, a 400px modal, or
  a 240px dashboard tile. Device width tells you nothing about the space
  actually available to the component you're styling.
- **Multitasking and OS-level layout**: split-screen multitasking on
  tablets, floating/resizable windows on desktop OSes, and foldables mean
  even "the device" doesn't map to one width during a single session.

**ESTABLISHED starting reference numbers** (use as a first draft, then
verify against real content — never ship without checking where *your*
content actually breaks): common industry inflection clusters sit around
~480px (small phone), ~640px (large phone / phone landscape), ~768–834px
(small tablet / split view), ~1024–1080px (tablet landscape / small
laptop), ~1280–1440px (laptop/desktop), ~1920px+ (large desktop). Treat
these as places to *check*, not values to hardcode blindly.

## 2. Container queries: query the space you actually have

**ESTABLISHED, rapidly maturing.** `container-type` and `container-name`
(with `@container` rules) let a component respond to the width of its
*containing element*, not the viewport. This is the direct technical
answer to Constitution #18: a card component can be written once and
correctly compress its layout whether it's rendered full-width, in a
3-column grid, or in a 280px sidebar — because it queries its own box, not
`window.innerWidth`.

```css
.card-container { container-type: inline-size; container-name: card; }

@container card (min-width: 480px) {
  .card { grid-template-columns: 120px 1fr; }
}
@container card (max-width: 479px) {
  .card { grid-template-columns: 1fr; }
}
```

**When to reach for container queries vs. media queries:**
- **Container queries** for any component that is reused in more than one
  layout context (cards in a grid, in a list, in a sidebar; a data table
  that also renders inside a drawer) — anywhere the component's available
  width is decoupled from viewport width.
- **Media queries** remain correct for page-level, viewport-scoped
  decisions that have no "container" to speak of: overall page grid column
  count, whether a persistent sidebar exists at all, global nav layout
  (top bar vs. bottom tab bar), font-size base adjustments tied to reading
  distance assumptions (phone-in-hand vs. desktop-at-arm's-length).
- Container query units (`cqw`, `cqh`, `cqi`) let type and spacing scale
  fluidly *with the container* rather than the viewport — useful for
  components (a hero inside a variable-width card) but should be capped
  with `clamp()` so text never becomes illegibly small or absurdly large.
- Practical rule for 2026 codebases: default new reusable components to
  `container-type: inline-size`; reserve media queries for the app shell
  (nav, page grid, global chrome).

## 3. Fluid layout before fixed breakpoints

**ESTABLISHED.** Most of a layout's responsiveness should come from
inherently flexible techniques — not from a wall of breakpoint overrides:
- **`grid-template-columns: repeat(auto-fit, minmax(240px, 1fr))`** lets a
  card grid add/remove columns automatically as width changes, with zero
  breakpoints, as long as the item's minimum comfortable width is known.
- **`clamp(min, preferred, max)`** for type size, spacing, and container
  widths gives continuous scaling instead of stepped jumps at breakpoints
  (`font-size: clamp(1.25rem, 1rem + 1vw, 1.75rem)`).
- **Flexbox `flex-wrap`** for anything that should degrade gracefully by
  wrapping (tag lists, toolbar actions, filter chips) rather than needing
  an explicit "mobile version."
- Breakpoints and discrete layout swaps (see below) are for changes fluid
  techniques *cannot* express — a genuine structural change, not a size
  change (e.g., a sidebar becoming a bottom sheet is not something
  `clamp()` can do).

## 4. The Recomposition Decision Framework (authoritative)

Per **Constitution #10**: shrinking a desktop layout preserves decisions
that assumed a cursor, hover, wide viewport, and low interruption — none
of which hold on a narrow/touch context. Recomposition means re-deciding
the fate of *every element*, individually, using this ordered set of
options. Apply it element-by-element, not to the page as a whole — a
single page will use several of these for different regions.

For each element, ask, in this order:

1. **Keep** — the element works as-is at the new size with no change
   (common for single-column body content, most buttons, most icons).
   Default to this; every other option is a deviation that must be
   justified by an actual problem at the new size.

2. **Move** — the element is still needed and still the same component,
   but its *position* relative to other elements changes because the
   surrounding layout changed (a secondary action moves from beside a
   heading to below it; a filter panel moves from a persistent left rail
   to above the results list).

3. **Collapse** — the element's full content is still available but its
   *default visible state* shrinks (a multi-column form field group
   collapses to one column; an expanded sidebar collapses to icon-only
   with labels on demand; a always-open filter panel collapses to
   closed-by-default with a toggle). Distinguish from "hide": collapsed
   content is one interaction away and its presence is signaled.

4. **Merge** — two or more elements that were visually/structurally
   separate at the larger size combine into one at the smaller size (three
   separate KPI cards become one horizontally-scrollable strip; a
   toolbar's five icon buttons merge into a single "more actions" menu
   button holding all five as menu items). Merging is a legitimate
   response to limited width, but per Constitution #16 it must not hide
   information the user needs to form a correct decision — only secondary
   or convenience actions should merge into an overflow menu, never a
   primary action or a piece of information required before proceeding.

5. **Hide** — the element is genuinely removed from this context, with no
   equivalent surfaced (a decorative illustration, a "last updated by"
   metadata line on a dense list). Justify against Constitution #16:
   hiding is only correct if it doesn't change what the user needs to know
   to make their next decision correctly. If removing it *would* change
   that decision, it must become collapsed or merged instead of hidden.

6. **Become scrollable** — a layout that fit without scrolling at the
   larger size (a wide data table, a horizontal stat row, a multi-tab
   toolbar) becomes a scrollable region at the smaller size rather than
   wrapping or shrinking each item illegibly. Correct default for: wide
   tables (horizontal scroll with a sticky first column), horizontal
   card carousels, wide toolbars. Must signal scrollability (partial next
   item visible, a scroll shadow/fade, or a scrollbar) — a scrollable
   region with no affordance reads as "that's everything."

7. **Become a sheet** — a persistently visible panel (a sidebar, an
   inspector, a filter rail, a secondary nav) becomes a bottom sheet or
   slide-over that opens on demand at the smaller size, because there is
   no longer room for two simultaneous columns. This is the standard
   answer for "desktop has a permanent right-hand detail panel; mobile
   doesn't have the width for two columns."

8. **Become a different component entirely** — the interaction model
   itself changes because the desktop pattern assumed capabilities the
   narrow/touch context doesn't have. Canonical cases: a `<table>` becomes
   a stack of key-value cards (each row → one card, each column header →
   a label above its value) once there are too few columns worth of width
   to preserve tabular scanning; a hover-triggered tooltip becomes a
   tap-triggered popover or an always-visible caption (no hover on touch);
   a multi-column mega-menu becomes a full-screen drill-down nav; a
   drag-to-reorder list becomes a list with explicit up/down move buttons
   or a long-press-triggered reorder mode. This is the most expensive
   option — reach for it only when 1–7 genuinely cannot express the needed
   change — but refusing it (forcing a hover-dependent or wide-table
   pattern onto touch/narrow) is a worse failure than the cost of building
   two components.

**How to use this list**: for every distinct element or region in a
design, pick exactly one of these eight outcomes at each breakpoint you've
identified via §1. Write the decision down (a "recomposition table":
element × breakpoint × outcome) before implementing — this turns "make it
responsive" from a vague CSS task into a concrete, reviewable design
artifact, and is the deliverable that proves recomposition happened rather
than shrinking.

## 5. Touch vs. pointer, independent of screen size

**ESTABLISHED.** Use `(hover: hover) and (pointer: fine)` /
`(hover: none) and (pointer: coarse)` media features to detect input
capability, not width — a touchscreen laptop and a mouse-connected tablet
both break the "narrow = touch" assumption. Practical implications:
- Never make critical functionality hover-only reachable when
  `pointer: coarse` is possible.
- Increase target size and target spacing under `pointer: coarse` (see
  `knowledge/accessibility.md` for exact numbers — WCAG 2.2 SC 2.5.8, Apple
  HIG, Material touch target guidance).
- Drag-and-drop and hover-preview interactions need a coarse-pointer
  equivalent (long-press, explicit buttons) — see "become a different
  component" above.

## 6. Responsive typography and spacing scales

**ESTABLISHED.** Don't just shrink type uniformly — the *type scale ratio*
itself can compress at narrow widths (headings that are 2.5x body size on
desktop may only need to be 1.8x on mobile, because there's less
horizontal competition for attention and less need for a heading to "win"
against a wide page). Spacing scales similarly compress (desktop's 64px
section gaps might become 32px on mobile) but should stay on the same
underlying spacing token scale (see `design-systems/design-tokens.md`) —
responsive behavior is expressed by choosing a different token at a
breakpoint, not by inventing one-off pixel values.

## 7. Common failure modes

- **Squint test failure**: resizing the browser slowly from 1440px to
  360px and watching *only* for the moment something visually breaks,
  without asking whether the interaction model still makes sense (a table
  that "fits" at 500px by shrinking font to 10px has not been correctly
  recomposed — it should have become cards).
- **Uniform shrink**: every element scales down proportionally
  ("responsive" implemented as `transform: scale()` or globally smaller
  padding) rather than genuinely recomposing per §4 — produces a cramped
  miniature of the desktop layout instead of a layout designed for the
  space.
- **Breakpoint proliferation**: five, six, seven breakpoints each nudging
  one property by a few pixels, none tied to an actual content inflection
  point — a sign that fluid techniques (§3) should have been used instead.
- **Orphaned desktop-only content**: content hidden at narrow widths via
  `display: none` with no equivalent path to it (violates Constitution
  #16 if that content affects a decision).
- **Ignoring the input-capability axis**: assuming narrow viewport implies
  touch and wide viewport implies mouse+hover; see §5.

## Sources
- [The Ultimate Guide to CSS Container Queries in 2026](https://dev.to/nickbenksim/the-ultimate-guide-to-css-container-queries-in-2026-1ndi) — accessed 2026-09-12
- [MDN: CSS Container Queries](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_container_queries) — accessed 2026-09-12 (background knowledge, not directly fetched this session)
- [MDN: clamp()](https://developer.mozilla.org/en-US/docs/Web/CSS/clamp) — background knowledge
- Constitution principles #10, #16, #18 — `/home/claude/advanced-ui-skill/knowledge/ui-principles-2026.md`
