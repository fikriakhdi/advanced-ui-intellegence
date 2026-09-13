---
title: Spacing & Layout
confidence-note: Base-grid values (4px/8px) and named breakpoints are
  CURRENT BEST PRACTICE drawn from named systems. Container-query support
  and CSS Grid/Flexbox division of labor are ESTABLISHED current practice.
  The grid-breaking guidance is FOUNDATIONAL reasoning per Constitution #18.
last_updated: 2026-09-12
status: living document — expand as research accumulates
---

# Spacing & Layout

Per Constitution #18, responsive design should respond to available space
and content, not device labels — and per Constitution #5, whitespace is a
grouping tool, not empty space to be filled. This document covers the
mechanics (base grids, Grid vs. Flexbox, container queries) and the judgment
call the constitution insists on: when a rigid grid serves the content and
when breaking it serves the content better.

## The base-unit grid (4px / 8px) — why it matters, not just what it is

**FOUNDATIONAL reasoning, CURRENT BEST PRACTICE numbers.** A spacing scale
built from multiples of a single base unit (commonly 4px or 8px) exists for
the same reason a type scale exists (see `typography.md`): it converts an
infinite space of possible pixel values into a small, composable set, so
that "the gap between these two elements" is always a decision from a known
menu (4, 8, 12, 16, 24, 32, 48, 64...) rather than an arbitrary number
someone typed once. This has three concrete payoffs:

1. **Predictable rhythm.** When every gap, padding, and margin in a product
   is a multiple of the same base unit, spacing relationships stay visually
   consistent even as layouts vary — a proxy for the proximity/grouping
   signal in `visual-hierarchy.md` staying legible across the whole product,
   not just one screen.
2. **Fewer decisions, more consistency (Constitution #15).** A design
   system that defines "8px, 16px, 24px" as the available gaps for a card's
   internal padding means no one has to decide between 14px and 15px and
   16px — that decision was made once, system-wide.
3. **Cross-platform alignment.** 4px and 8px both divide evenly into common
   icon sizes (16, 24, 32px) and into most default type line-heights,
   reducing the sub-pixel rounding/rendering inconsistencies that occur when
   spacing and content sizes don't share a common divisor.

**8px vs. 4px**: 8px-based scales (IBM Carbon's "2x grid," many product
design systems) are common for general UI because 8px is coarse enough to
keep the scale small while still fine enough for most layout needs; a
4px "half-step" is commonly layered in for icon-adjacent spacing, border
widths, and fine adjustments inside compact components (Carbon explicitly
calls its finer unit a "mini unit" used *within* the coarser 2x rhythm,
not as a replacement for it). GOV.UK's scale is a useful real-world
counter-example showing the values needn't be pure powers of two — its
scale runs 5 / 10 / 15 / 20 / 25 / 30 / 40 / 50 / 60px on large screens,
built from a 5px base instead, with a *different*, more compressed set of
values on small screens (5/10/15/15/15/20/25/30/40) rather than naive
linear scaling — again, a system deciding by content/context rather than
mechanical proportion.

## Optical vs. mathematical spacing

**ESTABLISHED.** A spacing scale is a starting menu, not a replacement for
eyes. Two adjustments recur even in disciplined 8px-scale systems:

- **Optical centering**: an icon inside a circular button, or a triangular
  play glyph inside a control, frequently needs a few pixels of
  intentional off-center placement to *look* centered, because
  mathematical bounding-box centering doesn't account for a shape's visual
  weight distribution.
- **Compounding gaps at edges**: when two spacing values stack (e.g., a
  card's internal padding plus the gap between cards), the *visual* gap a
  user perceives is the sum, and sums from a mathematically clean scale
  don't always look clean — a designer should look at the resulting
  composition, not just verify each individual value is "on the grid."

The grid should be treated as the default that saves 95% of spacing
decisions from being reinvented, not as a rule that overrides visual
judgment in the remaining 5%.

## CSS Grid vs. Flexbox: when each actually applies

**ESTABLISHED, frequently confused.** The dividing line is *dimensionality
and content-awareness*, not general "modernness" of one over the other:

- **Flexbox** is for **one-dimensional** distribution — a row or a column
  where items should distribute, wrap, or align along a single axis and
  where the *content* (not a predefined grid) should determine sizing. Use
  it for: a toolbar's buttons, a nav bar's items, a tag list that wraps, a
  form row's fields, anything where "however many items there are, lay
  them out along this line and let them size to their content or share
  space evenly."
- **CSS Grid** is for **two-dimensional** layout — when rows *and* columns
  need to relate to each other, or when a layout needs explicit named
  regions (header/sidebar/content/footer) that should hold their position
  independent of source order. Use it for: page-level app shells, card
  grids where item width and row height should both be controlled, form
  layouts with aligned label/input columns across multiple rows, dashboard
  layouts with irregular panel spans (a metric tile spanning 2 columns next
  to two 1-column tiles).
- **They compose**: a Grid-based page shell commonly contains
  Flexbox-based components inside each grid area (a Flexbox toolbar inside
  a Grid-defined header region) — the choice is per-region, not
  page-wide.

## Container-based layout and container queries

**ESTABLISHED current practice, real browser support as of this research
date.** CSS Container Queries (`@container`) let a component respond to the
width of its *containing element* rather than the viewport — directly
operationalizing Constitution #18's point that "mobile-narrow" is a
property of available space, not device class. A component dropped into a
sidebar, a split-view pane, or a dashboard's narrow column can adopt its
narrow-layout styling there even on a wide desktop viewport, and its
roomy-layout styling when placed full-width, without any JavaScript.

**Practical guidance:**
- Container queries are supported in all current major evergreen browsers
  (Chrome, Edge, Firefox, Safari) as of this research date, including
  container query *units* (`cqw`, `cqh`, `cqi`) for sizing content
  relative to the container rather than the viewport — treat them as safe
  for new product work rather than an experimental feature needing a
  fallback, unless a product specifically must support very old browser
  versions.
- A container query requires the *parent* to declare `container-type:
  inline-size` (or `size`) — a common early mistake is querying a
  component's own width without realizing the containment context has to
  be established one level up.
- Use container queries for genuinely reusable components that get placed
  in variable-width contexts (a card component used both in a 3-column
  grid and a narrow sidebar); continue using viewport media queries for
  page-level, structural layout decisions (does the whole page have a
  sidebar at all, does global nav collapse to a hamburger) where the
  relevant "container" really is the viewport.
- Container queries do not replace fluid/intrinsic sizing (flex-wrap, CSS
  Grid's `auto-fit`/`minmax`, `clamp()`) — many layouts that used to be
  reached for with a media query or (recently) a container query don't
  need a query at all if built with intrinsically responsive CSS from the
  start; reach for queries when a genuine *discrete* layout change is
  needed (switch from a horizontal card to a stacked one), not for
  continuous scaling.

## Max-width strategies and full-bleed sections

**FOUNDATIONAL reasoning, tying directly to `typography.md`'s measure
guidance.** A page's outer container width and a piece of content's
*reading* width are different decisions and should be controlled
independently:

- A body-copy column should be constrained to a measure-appropriate
  max-width (see `typography.md` — roughly 60–75 characters, which in
  practice is often somewhere around 640–75ch depending on font and size)
  regardless of how wide the viewport or the page shell is.
- A page's overall content region commonly caps around 1200–1400px on very
  wide viewports (GitHub Primer's guidance is representative: content
  regions generally cap at their `xlarge` breakpoint, 1280px, specifically
  "so the content region doesn't render paragraphs with too many words per
  line" — i.e., the page-level max-width exists in service of the same
  measure concern, not as an arbitrary brand choice) — but a page can
  legitimately have some regions capped tighter (the reading column) and
  others full-bleed (a hero image, a data table that benefits from all
  available width, a full-width divider).
- **Full-bleed sections** are the deliberate exception to a capped content
  width — used for imagery, section-background color changes, or wide
  data surfaces (tables, charts, timelines) that genuinely benefit from
  using the full viewport rather than being constrained to match the
  reading column. The judgment call is per-section: does *this* content
  benefit from width, or does width hurt it (per the measure argument)?

## Asymmetric, editorial, and modular layouts

**CONTEXTUAL.** Not every layout should be a uniform, symmetric grid.
Editorial and marketing contexts in particular benefit from asymmetric
column splits (a 60/40 or 70/30 division rather than 50/50), deliberate
misalignment as a compositional device, and modular/mosaic grids where
tile sizes vary to create visual rhythm and signal relative importance
(a large tile for the lead story, smaller tiles for secondary ones — using
*size within the grid* as a hierarchy signal per `visual-hierarchy.md`,
rather than relying on a uniform grid plus color to differentiate
importance). This is legitimate, deliberate design — not sloppiness —
when it's used to express a real content hierarchy or brand personality,
and illegitimate when it's asymmetry for its own sake with no underlying
reason (Constitution #1's decoration-without-hierarchy failure, applied to
grid structure instead of type or color).

## When intentional grid-breaking helps — and when rigid grids matter

Per Constitution #18, this knowledge base explicitly rejects "everything
must align to one rigid grid" as a universal rule. The actual judgment
call:

**Rigid, consistent grids matter more when:**
- Content is **tabular or comparative** — a data table, a pricing
  comparison, a settings list — where the user's task depends on scanning
  down columns and comparing values at consistent positions. Any
  misalignment here actively costs comprehension (Constitution #11's
  throughput argument for dense, professional UI).
- The screen is used **frequently by the same user** — an IDE, a
  dashboard, an admin panel — where muscle memory and predictable element
  position (Constitution #19 — familiar patterns) speed up repeat use more
  than any compositional interest would add.
- Multiple **independent teams or content types** populate the same
  template (a CMS-driven page, a marketplace listing grid) — a rigid grid
  is what keeps arbitrary content from breaking the layout.

**Intentional grid-breaking helps more when:**
- The context is **editorial or narrative**, read once or infrequently
  (a landing page, a case study, a long-form article) where visual
  interest and pacing are part of the communicative goal, not friction
  against a repeat-use task.
- One element genuinely **deserves outsized attention** — a hero product
  shot, a lead headline — and breaking it out of the column grid (bleeding
  past the text measure, overlapping an adjacent section) is being used
  *as a hierarchy signal*, consistent with `visual-hierarchy.md`'s
  "position/scale as free hierarchy signals" — not decoration for its own
  sake.
- The brand's personality is explicitly expressive/editorial (Constitution
  #17) and the product context (marketing site, not the underlying
  transactional app) supports spending that novelty cost (Constitution #2).

**The test that resolves ambiguous cases**: would breaking the grid here
change how fast or accurately the user completes their actual task? If yes
(a table, a form, a dashboard), keep it rigid. If the task is closer to
"read and be persuaded/informed" than "scan and compare precisely," grid-
breaking is available as a legitimate tool, to be spent deliberately per
Constitution #2 rather than applied by default.

## Sources

- [GitHub Primer — Layout foundations](https://primer.style/product/getting-started/foundations/layout/) — breakpoint table, padding-by-breakpoint, max-width-for-measure rationale. Accessed 2026-09-12.
- [IBM Carbon Design System — 2x Grid](https://carbondesignsystem.com/guidelines/2x-grid/overview/) — 8px "mini unit" base, fluid/fixed/hybrid grid types, breakpoint/column/margin table. Accessed 2026-09-12.
- [GOV.UK Design System — Spacing](https://design-system.service.gov.uk/styles/spacing) — 5px-based responsive spacing scale, small vs. large screen values. Accessed 2026-09-12.
- Container query browser-support and practical-guidance search results (LogRocket "Container queries in 2026," multiple 2026 CSS-guide posts) corroborate current-generation evergreen-browser support; **no single canonical spec source was deep-fetched in this pass** — flagged for a follow-up direct fetch of MDN's container-queries browser-compatibility data before treating the support claim as fully verified.
- [Constitution — UI Principles 2026, #18](../knowledge/ui-principles-2026.md) — internal, the direct source for this file's anti-rigid-grid framing. Accessed 2026-09-12.

**Weak spots / revisit:** container-query browser support was corroborated
via search snippets rather than a direct MDN/caniuse fetch — treat the
"safe for new work" claim as ESTABLISHED-but-unverified-at-source and
confirm with a caniuse.com or MDN fetch before publishing this as a hard
recommendation. Modular/mosaic editorial grid guidance is reasoned from
`visual-hierarchy.md` principles rather than sourced from a named design
system in this pass.
