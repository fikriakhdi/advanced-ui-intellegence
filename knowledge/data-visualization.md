---
title: Data Visualization
confidence-note: Chart-selection guidance synthesized from Datawrapper's applied chart-type framework and general statistical-graphics consensus; accessibility requirements are FOUNDATIONAL (WCAG); "decorative charts" anti-pattern naming is ESTABLISHED via cross-referencing dashboard/dataviz critique.
last_updated: 2026-09-12
---

# Data Visualization

A chart's only job is to make a comparison, trend, or relationship easier
to see than the underlying numbers would be on their own. Every rule here
follows from that: if a chart doesn't make something easier to see than a
well-formatted number or table would, it shouldn't be a chart (Constitution
#9 applied to visualization specifically).

## Chart Type Selection by Data Shape and Question

Select the chart by first naming the question being asked, then picking
the form that answers it most directly — not by which chart type looks
most sophisticated or is available in the charting library's gallery.

- **Trend over time** — line chart is the default, intuitive baseline for
  continuous time series. Bar/column charts work for a small number of
  discrete time points (quarterly, yearly) where "connecting" points into a
  line would imply continuity that doesn't exist between distinct periods.
  Area charts extend a line chart to also show composition changing over
  time; use small multiples (one panel per series) instead of overlaying
  many lines once a single chart would have more than ~4-5 overlapping
  series, since overlapping lines beyond that become unreadable.
- **Comparison across categories (absolute values)** — bar charts are the
  default; they let a human compare lengths accurately in a way pie/donut
  charts cannot. Grouped bars compare subcategories without hiding the
  totals; stacked bars show a whole split into parts, at some cost to
  comparing the middle segments' individual lengths precisely.
- **Composition / share of a whole** — pie and donut charts read
  immediately as "these are percentages of one whole" but sacrifice
  precision: a genuine 3% difference between two slices is close to
  invisible in a pie chart but obvious in a bar chart. Prefer a simple bar
  chart whenever the *comparison between parts* matters more than the
  "this sums to one whole" gestalt; reserve pie/donut for a small number of
  categories (2-4) with genuinely large differences, or when the "single
  whole" framing itself is the point being communicated (e.g., "we hit our
  target," shown as a filled ring).
- **Relationships between two variables** — scatter plots show correlation/
  distribution between two continuous variables; bubble charts add a third
  dimension via marker size. When points overlap heavily, a 2D
  density/heatmap view clarifies where the data actually concentrates —
  but both scatter plots and heatmaps demand more numeracy from the viewer
  than a bar or line chart, so reserve them for analytical/expert contexts
  (Constitution #11) rather than a general-audience dashboard.
- **Flows and transitions between categories** — Sankey/alluvial diagrams
  show how volume moves between stages or how members shift categories
  over time (funnel stages, user-segment migration). These are
  high-complexity, low-frequency chart types — use them only when a
  simpler bar/line genuinely cannot represent the flow relationship, since
  they impose real interpretation cost (Constitution #3).
- **Single-value / KPI display** — a well-formatted number with
  context (trend arrow, sparkline, comparison) often communicates faster
  than any chart at all for a single metric — see
  `knowledge/dashboard-patterns.md` on avoiding "meaningless statistics";
  a chart is not required just because the surface is a "dashboard."

## Avoiding "Decorative Charts"

**ANTI-PATTERN: decorative charts.** A chart added because a section
"needed some visual interest" or because dashboards are expected to be
visually rich, rather than because it answers a specific question more
clearly than the number/table it replaces. This is the data-visualization
instance of Constitution #9 (every element must justify its existence) and
#20 (trends are tools, not objectives) — a gratuitous donut chart showing a
single 73%/27% split, styled elaborately, communicates strictly less than
the sentence "73% of users are on the free plan," at a much higher
rendering and scanning cost. **ESTABLISHED.**

**DESIGN RULES**
- Before adding a chart, state the question it answers in one sentence
  ("how has revenue trended over the last 12 months") — if that sentence
  is better answered by a single number or a short sentence, don't chart
  it.
- Prefer the simplest chart type that answers the question — a bar chart
  that looks "plain" but is instantly readable beats a radial/3D/novel
  chart type that requires the viewer to learn a new visual grammar for a
  single use.
- Never add chart junk (3D extrusion, drop shadows on bars, decorative
  background gradients, excessive gridlines) — these are visual-novelty
  costs (Constitution #2) with no comprehension benefit and a well-
  documented history of actively distorting perceived values (3D pie/bar
  charts are the canonical example: perspective distorts the visual size
  of slices/bars relative to their true values).

## Labeling and Legends

**FOUNDATIONAL.**

**DESIGN RULES**
- Label data directly on the chart (a line's endpoint, a bar's value) in
  preference to a separate legend requiring the eye to look up a
  color-to-category mapping elsewhere — this reduces the "decoding" step
  the viewer must perform, especially valuable when there are only a
  handful of series.
- When a legend is necessary (many categories, or repeated small
  multiples), keep it close to the chart and order it to match the visual
  order of the data (e.g., legend order matches the stacking order in a
  stacked bar) rather than an arbitrary/alphabetical order that forces
  cross-referencing.
- Every axis needs a label stating its unit — an unlabeled y-axis with
  just numbers leaves the viewer guessing whether it's dollars, count, or
  percentage.
- Title the chart with the *insight or question*, not just the dataset
  name, when the chart is meant to stand alone in a report ("Revenue grew
  18% after the March price change" as a title communicates more upfront
  than "Monthly Revenue").

## Color Use in Charts

Ties directly to `knowledge/color-system.md`'s semantic color guidance.

**DESIGN RULES**
- Use a consistent categorical palette across a whole product/report so
  the same category always reads as the same color across charts —
  inconsistent color-to-category mapping between charts forces
  re-learning per chart.
- Reserve semantic colors (red/green/amber) for values that actually carry
  that semantic meaning (a threshold breach, above/below target) — don't
  spend "danger red" on an arbitrary category in a categorical palette
  where it will misleadingly imply a problem exists.
- Never encode meaning in color alone (Constitution #7 and WCAG 1.4.1) —
  pair color with position, direct labels, pattern/texture, or shape so a
  colorblind viewer or a black-and-white print of the chart still
  communicates the finding. This is also why a data table alternative
  (below) matters even when the chart itself is well-designed.
- Use a sequential palette (single hue, increasing lightness/saturation)
  for ordered/quantitative data (e.g., a heatmap of intensity) and a
  diverging palette (two hues meeting at a neutral midpoint) specifically
  for data with a meaningful zero/neutral point (e.g., above/below
  average) — a categorical (qualitative, unordered-hue) palette is wrong
  for either of these cases since it implies no order where one exists.

## Dashboards vs. Reports: Context Differences

**CONTEXTUAL.**

- **Dashboards** are checked repeatedly, often at a glance, by users who
  already have context — charts should be immediately scannable, favor
  familiar chart types over precise/dense ones, and prioritize "is this
  normal or not" over deep analytical detail (see
  `knowledge/dashboard-patterns.md`).
- **Reports** are read more slowly, often once, by a reader who may lack
  the dashboard user's built-up context — charts can afford more
  annotation, more explanatory titles, and can justify a more complex
  chart type (a scatter plot, a Sankey) if the accompanying text walks the
  reader through it, since the report's format allows for that
  explanation in a way a glanced-at dashboard tile doesn't.

**DESIGN RULE.** Don't port a report-appropriate complex chart directly
into a dashboard tile without the surrounding explanatory context it
depended on in the report — the same chart type is not equally
appropriate in both surfaces.

## Accessibility of Charts

**FOUNDATIONAL**, ties to `knowledge/accessibility.md`.

**DESIGN RULES**
- Provide the underlying data as an accessible table (visually hidden if
  needed, or a "view as table" toggle) as an alternative to every
  meaningful chart — screen reader users cannot perceive a rendered
  chart's shape, and a table is the reliable fallback that preserves the
  actual information (Constitution #14: accessibility is a design
  constraint, not an afterthought toggle bolted on later).
- Never rely on color alone to distinguish series or convey meaning (see
  Color Use section above) — this is a WCAG 1.4.1 requirement, not merely
  a nicety.
- Ensure sufficient contrast between chart elements and background,
  including gridlines and axis text, not just the data marks themselves.
- Interactive charts (hover tooltips, click-to-filter) need a keyboard-
  operable equivalent (tab to a data point and see its tooltip via focus,
  not hover alone) — this mirrors the hover-affordance rule in
  `knowledge/desktop-patterns.md`.
- Provide meaningful alt text or an adjacent text summary stating the
  chart's key takeaway for any chart embedded as a static image, since
  alt text cannot convey a full data table but can convey the finding.

## Sources

- [A friendly guide to choosing a chart type – Datawrapper Blog](https://www.datawrapper.de/blog/chart-types-guide)
- WCAG 2.1 Success Criterion 1.4.1 Use of Color — W3C
- Cross-referenced against `knowledge/dashboard-patterns.md` vanity-metrics/anti-pattern research (this knowledge base)
