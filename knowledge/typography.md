---
title: Typography
confidence-note: Numeric scales below (ratios, px values) are CURRENT BEST
  PRACTICE from named systems, current as of research date — treat exact
  numbers as ESTABLISHED reference points to adapt, not FOUNDATIONAL law.
  The reasoning about why scales/measure/line-height work is FOUNDATIONAL.
last_updated: 2026-09-12
status: living document — expand as research accumulates
---

# Typography

Typography is usually the single highest-leverage design decision in a
product: most screens are mostly text, and type carries most of the
hierarchy work before color or depth ever gets involved (Constitution #7).
This document separates the durable *why* from the current *what*.

## Type scales: durable principle

**FOUNDATIONAL:** A type scale exists to make size decisions *finite and
composable* — a small, deliberately-chosen set of sizes that recur across a
product, rather than an ad-hoc size picked per screen. Without a scale,
teams accumulate dozens of near-duplicate sizes (15px here, 16px there, 17px
somewhere else) that carry no meaning and cannot be reasoned about as a
system. A scale is not primarily about aesthetics (though a well-chosen
ratio produces pleasant proportion) — it's about giving every text element
a *role* (display, headline, title, body, label, caption) that a designer or
engineer can pick from without inventing a new size.

### Ratio-based scales (CURRENT BEST PRACTICE / ESTABLISHED)
Classical typographic scales borrow musical-interval ratios applied
multiplicatively to a base size:

| Name | Ratio | Notes |
|---|---|---|
| Minor second | 1.067 | Very subtle steps; rare in UI, used for dense data tables where levels must be barely distinguishable |
| Minor third | 1.2 | Common for compact/dashboard UI |
| Major third | 1.25 | A common default for general product UI — a good balance between too-subtle and too-dramatic |
| Perfect fourth | 1.333 | Editorial/marketing contexts wanting clearer jumps |
| Golden ratio | 1.618 | Rare in interface type; more common in display/marketing headline systems |

Mechanically: pick a base (commonly 16px, matching browser/OS default body
size — see accessibility note below), then multiply up and divide down by
the ratio to generate a small number of steps (5–8 is typical for a full
product; more than ~8 distinct sizes usually indicates the scale isn't
actually being respected). **CONTEXTUAL** — the right ratio depends on
content density and brand personality, not a universal "best" ratio.

### Named-role scales (what real systems ship) — ESTABLISHED
Rather than expose a raw ratio, mature design systems ship a fixed table of
named roles. Material Design 3's type scale is a good concrete reference
point (values below, regular weight unless noted):

| Role | Size | Weight |
|---|---|---|
| Display Large | 57px | Regular |
| Display Medium | 45px | Regular |
| Display Small | 36px | Regular |
| Headline Large | 32px | Regular |
| Headline Medium | 28px | Regular |
| Headline Small | 24px | Regular |
| Title Large | 22px | Regular |
| Title Medium | 16px | Medium |
| Title Small | 14px | Medium |
| Body Large | 16px | Regular |
| Body Medium | 14px | Regular |
| Body Small | 12px | Regular |
| Label Large | 14px | Medium |
| Label Medium | 12px | Medium |
| Label Small | 11px | Medium |

Notice this is *not* a single clean ratio end to end — steps compress at
the small end (11/12/14/16) and expand at the large end (36/45/57), because
legibility and information density matter more than mathematical purity at
small sizes, while dramatic jumps read fine at display sizes. This is the
practical lesson: **treat a stated ratio as a generating heuristic, not a
constraint you must honor at every step.**

GOV.UK's type scale is a useful contrasting reference for content-heavy,
accessibility-first contexts — its scale (large screens) runs roughly
80 / 48 / 36 / 27 / 24 / 19 / 16px, with a *different*, larger set of sizes
on small screens (53 / 32 / 27 / 21 / 21 / 19 / 16px) rather than naively
scaling the same numbers down — because GOV.UK's research found smaller
screens need type to stay *larger*, not shrink further, for reading
accessibility. Every line-height in the GOV.UK scale is a multiple of 5px
specifically to keep vertical rhythm consistent when type sizes change.
**ESTABLISHED**, and a direct rebuttal to the naive assumption that mobile
type should always be smaller than desktop type.

### How many sizes should a real product use?
**ESTABLISHED, and worth stating bluntly given Constitution #4's zero-sum
framing applied to type:** most well-designed product UIs use far fewer
distinct type styles than an unconstrained scale would generate — commonly
4–7 in active use (one or two display/headline sizes, one body size, one or
two label/caption sizes, maybe one large marketing display size reserved
for landing content). A screen using ten different font sizes has usually
confused "many pieces of content" with "many levels of importance" —
per Constitution #4, if everything is differentiated, nothing is.

## Fluid typography (`clamp()`) — CURRENT BEST PRACTICE

`clamp(min, preferred, max)` lets a size scale continuously with viewport
width instead of jumping at fixed breakpoints, which suits display/headline
text (large sizes with a lot of headroom to fill on wide screens) far better
than body text (where the size delta across viewports is small and
breakpoint-based sizing is simpler to reason about). A typical pattern:

```css
font-size: clamp(1.75rem, 1.2rem + 2.5vw, 3.5rem);
```

**Design rules:**
- Express min/max bounds in `rem`, not `px`, so they still respect the
  user's browser font-size preference.
- Reserve fluid scaling for headline/display text; keep body and label text
  on the fixed scale — the reading-size difference across viewports for
  body copy is small enough that fluid scaling adds complexity without a
  visible benefit, and it's easier to guarantee line-length (measure)
  correctness with a fixed size plus a max-width container.
- **Accessibility caveat (real, not hypothetical):** `clamp()` maximums, and
  any `vw`-based term, can prevent a user from reaching 200% zoom as
  required by WCAG 1.4.4 (Resize Text) if the max bound caps growth too
  early. Test actual browser zoom to 200%, not just resize-the-window,
  before shipping a fluid scale on body-adjacent text. **ESTABLISHED risk**,
  frequently overlooked.

## Line height, measure (line length), and tracking

**FOUNDATIONAL — line height:** Line height should increase (relatively) as
font size decreases and decrease (relatively) as font size increases. Small
text needs proportionally more line height to remain scannable (roughly
1.4–1.6x font size for body copy is a common comfortable range); large
display text needs proportionally less (often 1.05–1.2x) because the
letterforms themselves provide enough visual separation and excess leading
starts to visually disconnect wrapped lines from each other. GOV.UK's
practice of quantizing every line-height to a 5px multiple is a good
concrete technique for keeping vertical rhythm consistent when several type
sizes coexist on one page.

**FOUNDATIONAL — measure (line length):** Reading comprehension and speed
both degrade outside roughly 45–75 characters per line (character count,
not pixel width) for body text; the commonly cited sweet spot is around
60–75 characters. Too narrow (under ~45 characters) forces the eye to make
too many line-return saccades per idea; too wide (over ~75–90) makes it
easy to lose the correct next line on the return sweep. This is *why*
`max-width` on text containers is a real design decision, not an arbitrary
constraint (see `spacing-layout.md`) — a full-bleed paragraph on an
ultrawide monitor is a measure failure, not a "responsive" success.

**ESTABLISHED — tracking (letter-spacing):** Large display type generally
benefits from slightly *negative* tracking (tighter) because default font
metrics are tuned for body-text sizes and look loose at 48px+; small
all-caps labels benefit from slightly *positive* tracking (looser) because
tight caps become harder to distinguish letter-by-letter at small sizes.
Body text at normal sizes should generally be left at the font's default
tracking — hand-tuning body tracking is a common over-engineering trap with
little payoff.

## Weight and the type-style vocabulary

**ESTABLISHED:** A disciplined product typically defines a small, closed
vocabulary of *type styles* (not just sizes) — each combining a role
(display/headline/title/body/label/caption/numeric), a size, a weight, a
line-height, and sometimes a tracking value — and every piece of UI text
picks from that vocabulary rather than combining size+weight+color ad hoc.
This is the typographic analog of Constitution #15 (systems make common
decisions once). A common, workable vocabulary:

- **Display** — largest, marketing/hero contexts, low frequency of use.
- **Headline/Title** — page and section headers, 2–3 sizes.
- **Body** — the working text size(s), 1–2 sizes (a "body" and a smaller
  "body small" for secondary/dense contexts).
- **Label** — UI chrome text: button labels, form labels, tab labels, tags.
  Often medium/semibold weight at a small size, since it needs to read as
  "interactive/structural" rather than "content."
- **Metadata/Caption** — timestamps, helper text, footnotes — smallest,
  lowest emphasis, often a muted color (see `color-system.md`).
- **Numeric/tabular** — see below.

### Numeric and tabular type — CONTEXTUAL, easy to get wrong
Numbers that appear in columns (financial tables, dashboards, any UI where
values are compared vertically) should use **tabular figures** (fixed-width
digits, `font-variant-numeric: tabular-nums` in CSS) so that digits align
vertically across rows — proportional figures cause numbers to visually
"jitter" left-right as values change, which reads as instability in exactly
the contexts (finance, monitoring) where stability matters most. Numbers
that appear inline in prose should generally use proportional (default)
figures, which look more typographically natural next to surrounding text.
This distinction is frequently missed because most body text doesn't need
it and most engineers don't know the CSS property exists.

## Apple Dynamic Type text-style scale

**ESTABLISHED, cross-referenced secondary sources — re-verified 2026-09-12,
still not a direct primary-source table quote.** A second research pass
confirmed Apple's own current documentation no longer publishes exact
point sizes as text: the API reference pages for `UIFont.TextStyle` cases
(`largeTitle`, `body`, etc.) — fetched successfully this pass via their
`.md` documentation-source variant, which *is* plain static content
unlike the JS-rendered marketing HIG pages — describe each style's
*purpose* only ("The font style for large titles") with no point-size
table, and the `UIFont.TextStyle` overview page lists all eleven/twelve
style cases but likewise omits sizes. The HIG's own Typography and Motion
pages remain JS-rendered SPAs that return only navigation/metadata even
via the `.md` suffix trick — that trick works for `/documentation/...`
API-reference pages, not for `/design/human-interface-guidelines/...`
pages.

Given that, the table below is corroborated across two independent
secondary sources rather than quoted from Apple text directly — one data
point (`.headline` = 17pt Semi-Bold) was independently confirmed by both
sources, which is the basis for treating the rest of the table as
reliable:

| Text style | Point size (Large / default) | Weight |
|---|---|---|
| `.largeTitle` | 34 | Regular |
| `.title1` | 28 | Regular |
| `.title2` | 22 | Regular |
| `.title3` | 20 | Regular |
| `.headline` | 17 | Semibold |
| `.body` | 17 | Regular |
| `.callout` | 16 | Regular |
| `.subheadline` | 15 | Regular |
| `.footnote` | 13 | Regular |
| `.caption1` | 12 | Regular |
| `.caption2` | 11 | Regular |

All sizes are at the default "Large" content-size category — every style
scales up/down from these as the user changes their Dynamic Type setting,
via `UIFontMetrics`/`preferredFont(forTextStyle:)`, which is the
documented (and now primary-source-confirmed via the `scaling-fonts-automatically`
API guide) mechanism apps should use instead of hardcoding these numbers.

## Context-specific guidance

- **Dashboards / data-dense UI**: fewer, smaller type-scale steps (per
  Material's compression at the low end); prioritize a highly legible body
  size (often 13–14px given viewing distance and density) over dramatic
  headline treatment; use weight and tabular numerals, not size, to
  differentiate metric emphasis (Constitution #11 — throughput over
  comprehension-via-whitespace).
- **Editorial / long-form content**: generous measure control (a real
  `max-width` on the text column, not just the page), larger body size
  (17–20px is common for long-form reading, larger than typical app body
  text), more generous line-height (1.5–1.7), and a clearer display/headline
  jump since the type *is* the product here.
- **Mobile**: per Constitution #10, don't simply shrink desktop sizes —
  GOV.UK's finding that small screens need *larger*, not smaller, text is a
  useful corrective to the instinct to compress everything on mobile. Body
  text below ~16px on mobile web additionally risks iOS Safari's
  automatic-zoom-on-focus behavior for form inputs.
- **Marketing/landing**: this is where fluid, dramatic display type earns
  its cost — high visual impact, low information density, read once.

## Common failure modes

- **Too many text styles.** The most common typographic failure in
  practice — a page with 8–12 distinct size/weight combinations where 4–5
  named styles would do the same communicative job with far less noise
  (Constitution #4 applied to type).
- **Excessive sizes chasing "impact."** Oversized headline type on content
  that doesn't warrant marketing-grade visual weight (a settings page
  title styled like a landing-page hero) inflates perceived importance
  without content to back it up — a Constitution #1 violation (decoration
  without underlying hierarchy).
- **Ignoring measure.** Full-width body paragraphs on wide viewports;
  fixable with a `max-width` on the text container independent of the
  page's overall width (see `spacing-layout.md`).
- **Proportional numerals in tables.** Digits that shift horizontally as
  values update, undermining scanability in exactly the UI (financial,
  monitoring) that needs it most.
- **Fluid type breaking 200% zoom.** A `clamp()` max bound set too low,
  silently failing WCAG 1.4.4 for users who need larger text.

## Sources

- [Material Design 3 — Typography](https://m3.material.io/styles/typography/type-scale-tokens) and secondary cheatsheet [Material 3 (You) Typography Cheatsheet, Egor Tarasov](https://medium.com/@vosarat1995/material-3-you-typography-cheatsheet-ffc58c540181) — type scale role/size/weight table. Accessed 2026-09-12.
- [GOV.UK Design System — Type scale](https://design-system.service.gov.uk/styles/type-scale) — large/small-screen size tables, 5px line-height rhythm rationale. Accessed 2026-09-12.
- [Smashing Magazine — Modern Fluid Typography Using CSS Clamp](https://www.smashingmagazine.com/2022/01/modern-fluid-typography-css-clamp/) — clamp() mechanics, viewport-scaling formulas, WCAG 1.4.4 zoom caveat. Accessed 2026-09-12.
- [Apple Human Interface Guidelines — Typography](https://developer.apple.com/design/human-interface-guidelines/typography) — attempted fetch again 2026-09-12 (direct URL, `.md` variant); still JS-rendered, no extractable data. Apple's own API reference (`developer.apple.com/documentation/uikit/uifont/textstyle/*.md`) *was* fetchable this pass but does not publish point sizes as text.
- [iosfontsizes.com](https://www.iosfontsizes.com/) and [Use Your Loaf — Using a Custom Font with Dynamic Type](https://useyourloaf.com/blog/using-a-custom-font-with-dynamic-type/) — cross-referenced secondary sources for the Dynamic Type point-size table; independently agree on `.headline` = 17pt Semibold, which anchors confidence in the rest of the table. Accessed 2026-09-12.
- General typographic measure/line-height guidance (45–75 character line length, line-height inverse relationship to size) reflects long-standing, widely-replicated typographic practice (Bringhurst-derived) rather than a single 2026 source — **under-cited, revisit with a current authoritative citation** (e.g. a current WCAG or Baymard readability study).

**Weak spots / revisit:** Apple HIG's marketing/guideline pages remain
JS-gated after a second attempt with varied URLs (direct, `.md` suffix);
the Dynamic Type point-size table above is now cross-referenced across two
independent secondary sources with one point-corroborated data value
rather than a direct Apple-text quote — an honest upgrade from "unverified
training knowledge" but still short of a primary-source table fetch.
Tracking guidance is ESTABLISHED-by-convention rather than backed by a
specific 2026 source in this pass.
