---
title: Color System
confidence-note: Contrast numbers are FOUNDATIONAL (normative WCAG text).
  Semantic-role architecture is ESTABLISHED practice across major design
  systems. Material 3 surface-tint/elevation section is FOUNDATIONAL,
  primary-source verified 2026-09-12 against Google's open-source token
  repos (see Sources). Restraint guidance re: AI-generated visual clichés
  is ESTABLISHED (converging critique from multiple 2025-2026 sources) but
  not a formal standard.
last_updated: 2026-09-12
status: living document — expand as research accumulates
---

# Color System

Per Constitution #7, color is the *least robust* hierarchy signal and should
reinforce structure that already works without it. This document is about
building a color system that behaves predictably once it's asked to do
real work: semantic roles, contrast math, surface/elevation, dark mode, and
— because this project cares specifically about not producing generic
AI-style UI (Constitution #21) — explicit restraint guidance.

## Semantic color roles, not just "primary/secondary"

**ESTABLISHED.** A mature color system names roles by *function*, not by
brand rank, because a token named for its function can be reused
correctly by anyone who didn't design it, while a token named `blue-500` or
even `secondary` tells you nothing about when to reach for it. A practical
role taxonomy, adapted from how Material 3, Spectrum, Carbon, and Polaris
all converge on similar structures:

- **Action roles**: `action-primary`, `action-secondary`, `action-danger`
  (and their hover/pressed/disabled states). These drive interactive
  elements — buttons, links, active tab indicators.
- **Feedback/status roles**: `status-success`, `status-warning`,
  `status-danger`, `status-info` — used for validation messages, badges,
  toasts, and status indicators. These carry semantic meaning independent
  of brand color (red means danger regardless of brand hue) — do not let a
  brand's accent color double as the danger color just because it happens
  to be red-adjacent; keep the status palette brand-independent so it stays
  legible and correctly-connoted everywhere.
- **Surface roles**: `surface`, `surface-raised`, `surface-sunken`,
  `surface-overlay` — the backgrounds that content sits on, layered to
  express depth (see Elevation below) rather than named after a single
  literal color, since the same role resolves to a different literal value
  in light vs. dark theme.
- **Border roles**: `border-default`, `border-subtle`, `border-strong`,
  `border-focus` — borders are a separate role family from surfaces because
  they're frequently adjusted independently for accessibility (see Non-text
  Contrast below) without wanting to touch surface colors.
- **Text-on-X roles**: `text-on-surface`, `text-on-action-primary`,
  `text-muted`, `text-disabled` — text color must be defined *relative to*
  the surface/fill it sits on, not as a single global "text color," because
  a color system with multiple surface tones (elevated card vs. page
  background vs. a colored action button) needs a guaranteed-legible text
  pairing for each one.
- **Neutral/gray scale**: the largest part of any real interface by area —
  10–13 steps from near-white to near-black is typical, used for surfaces,
  borders, and secondary text. Most of an interface's actual pixels are
  neutral, not brand-colored; a system that under-invests in its neutral
  ramp (2–3 grays) will visibly strain once real content density arrives.

Naming by role (not by literal hue or by brand rank like
"primary/secondary/tertiary" alone) is what lets a color system be
re-themed (dark mode, a white-label brand variant, a high-contrast mode)
without touching component code — the component asks for
`text-on-surface`, and only the token definition changes per theme.

## Accent color restraint

**ESTABLISHED, and directly relevant to Constitution #21.** A disciplined
UI typically uses one, rarely two, brand accent hues for interactive/action
purposes, with everything else carried by the neutral ramp and the
status-role colors. The failure mode this guards against: coloring
non-interactive UI (section backgrounds, decorative dividers, icon fills)
with the brand accent simply because it's available, which (a) desaturates
the accent's meaning as "this is actionable" (Constitution #7's
reinforcement principle — color should mean something, and if it's
everywhere it means nothing) and (b) is a primary driver of interfaces that
look artificially "branded" rather than genuinely functional. Reach for
neutral first; reach for accent only on elements that are actually
interactive or that specifically need attention.

## Contrast math (WCAG) — FOUNDATIONAL, normative

These are the actual current normative numbers (WCAG 2.2):

- **1.4.3 Contrast (Minimum), Level AA**: text and images of text must have
  a contrast ratio of at least **4.5:1** against their background, except
  **large text** (defined as ≥18pt / 24px regular weight, or ≥14pt / ~18.5px
  bold), incidental text, and logotypes, which need only **3:1**.
- **1.4.6 Contrast (Enhanced), Level AAA**: **7:1** for normal text, **4.5:1**
  for large text.
- **1.4.11 Non-text Contrast, Level AA**: UI components (input borders,
  button boundaries, focus indicators, icons conveying meaning) and
  graphical objects necessary to understand content need at least **3:1**
  contrast against their adjacent color(s). This is the criterion teams most
  often miss — it's easy to remember body-text contrast and forget that a
  1px light-gray input border on a white background can fail this
  criterion outright.
- **2.5.8 Target Size (Minimum), Level AA**: pointer targets must be at
  least **24×24 CSS pixels**, with defined exceptions (spacing — a 24px
  circle centered on a smaller target must not overlap another target;
  inline text links; user-agent-controlled controls; essential/legally
  required presentation; an equivalent alternate control elsewhere on the
  page). Practical math: for equal square targets of side `s` with gap `g`
  between them, `s + g ≥ 24` — a 20px icon button needs at least 4px of
  surrounding spacing to pass via the spacing exception even if the visible
  icon itself is under 24px.

**Design rule derived from these numbers:** pick every text-on-surface and
border-on-surface pairing in the token system *before* shipping components,
and verify each with an actual contrast calculation — not by eye. A brand
color chosen for its hue in month one and only checked for contrast in
month six (Constitution #14's exact failure example) routinely turns out to
fail 4.5:1 against white at the saturation the brand team wanted.

## Surface hierarchy: elevation via color, not just shadow

**ESTABLISHED**, and a genuine departure from earlier "flat design" and
earlier "everything is a drop shadow" eras. Two complementary techniques:

1. **Shadow/elevation** communicates *literal* z-axis stacking — a modal,
   menu, or tooltip that is floating above the page. Reserve it for content
   that is actually temporarily overlaid, per `visual-hierarchy.md`'s note
   that depth is an expensive signal.
2. **Surface tint/tone** communicates *layered* hierarchy without implying
   literal floating — a card, a raised panel, or a sunken well can be
   expressed by shifting its background luminance/tint slightly relative to
   the page background, rather than (or in addition to) a shadow. This
   matters most in **dark mode**, where shadows are nearly invisible
   against a dark page background — dark-theme elevation is primarily
   communicated by making higher surfaces progressively *lighter*, not by
   shadow.

   **FOUNDATIONAL, primary-source verified 2026-09-12 — corrects an
   earlier unverified claim in this file.** A previous pass of this
   document described Material 3's model as "an increasing *percentage*
   of tint color mixed in as elevation dp increases," reconstructed from
   training knowledge. Reading Google's actual open-source token/color
   source (`material-foundation/material-color-utilities`, the library
   `material-web`'s own tokens are generated from) shows the *current*
   spec does **not** work that way:
   - Elevation's z-axis distance is now conveyed by **shadow alone**, at
     six fixed dp levels (`level0`–`level5` = **0, 1, 3, 6, 8, 12dp**,
     per `tokens/versions/v0_192/_md-sys-elevation.scss` in
     `material-components/material-web`) plus a single `--md-elevation-shadow-color`
     token — there is no separate continuous "tint percentage" variable
     tied to dp in the current implementation.
   - What *reads* as "elevation via tint" is actually a set of **discrete,
     pre-computed surface-container colors** — `surface-container-lowest`,
     `-low`, `surface-container`, `-high`, `-highest` — each a fixed tone
     on the neutral (gray) tonal palette (light theme: tones 100 / 96 / 94
     / 92 / 90; dark theme: tones ~4 / 10 / 12 / 17 / 22, per
     `color_spec_2021.ts`'s `surfaceContainerLowest()` through
     `surfaceContainerHighest()`). A component picks the container role
     matching its intended stacking position; the "tint" is baked into
     that role's fixed color, not computed as `base-surface + tint-color
     × opacity(elevation)` at render time.
   - `surfaceTint` itself is a **single fixed color** (primary-palette tone
     40 in light mode, tone 80 in dark mode, from `surfaceTint()` in the
     same file) — a reference/accent color some platforms use to derive
     the container tones, not a per-elevation-level variable.
   - **Practical implication for this knowledge base**: describe M3
     "elevation via surface tone" as *role-based* (pick the right
     `surface-container-*` role for the stacking position you need) rather
     than as a formula you compute from a percentage and a dp value — the
     percentage-overlay mental model matches Material 2, not the current
     Material 3 web implementation.

## Borders vs. shadows vs. translucency vs. gradients vs. glass effects

Each of these is a distinct tool with a distinct cost, and per Constitution
#5/#9 the cheapest tool that does the job should win:

- **Borders** — cheap, precise, good for defining edges of interactive
  regions (inputs, table cells) where an exact boundary matters. Must
  independently satisfy 3:1 non-text contrast if conveying meaning (an
  input's boundary, an error state).
- **Shadows** — communicate literal elevation/floating; expensive
  (rendering cost is trivial, but *visual* cost is high — they draw the eye
  per `visual-hierarchy.md`). Overused shadows ("card inside card inside
  card," each with its own shadow) create visual mud rather than clarity.
- **Translucency** — appropriate when content genuinely overlays other
  content and a hint of what's underneath aids spatial continuity (a
  sheet sliding over a page, a scrim behind a modal). It is not a neutral
  "modern" background treatment for non-overlaying content.
- **Gradients** — legitimate for expressing depth, light source, or brand
  warmth in a *restrained*, purposeful way (a subtle top-to-bottom lightness
  shift on a button, a brand hero background); illegitimate as a default
  fill for buttons, headers, or cards with no represented light source or
  purpose — see the restraint section below.
- **Glass/blur effects** (backdrop-filter blur, "glassmorphism") —
  legitimate when content is literally layered over other moving/scrolling
  content and the blur aids readability of the foreground while preserving
  context of what's behind (an OS-level control center, a persistent nav
  bar over scrolling content). Illegitimate as a static decorative texture
  applied to cards or panels with nothing actually behind them to blur —
  in that case it's pure visual fashion with real costs: reduced contrast
  predictability (WCAG contrast math assumes a known background color;
  blur-over-arbitrary-content makes that unverifiable), rendering cost, and
  — per the restraint section below — it is one of the most recognizable
  markers of generic AI-generated UI.

## Restraint: why AI-generated UI overuses purple/blue gradients, glowing buttons, and glassmorphism

**ESTABLISHED** (converging critique across multiple independent 2025–2026
design-industry sources, not a single formal study). Per Constitution #21,
this is a specific, recognizable failure pattern with an identifiable
mechanism, not bad luck:

- **Why it happens**: generative models (and designers working fast without
  product-specific direction) default to the *statistically most common*
  pattern seen in training/reference material — and a huge fraction of
  recent SaaS marketing sites and AI-tool landing pages *do* use
  purple-to-blue or purple-to-cyan gradient heroes, glowing/neon button
  accents, and glass-panel cards, because those patterns were themselves
  trend-copied from a handful of influential products a few years earlier.
  The pattern is self-reinforcing: more of it in the training distribution
  produces more of it in generated output, independent of whether it suits
  a given product's content or personality (this is exactly Constitution
  #20's "trends are tools, not objectives" warning, now with a specific
  visual signature).
- **What it looks like concretely**: a hero section with a diagonal or
  radial purple→blue→cyan gradient background; buttons with an outer glow
  or gradient border that doesn't correspond to any actual light source or
  interaction state; every card given a semi-transparent, blurred
  "frosted glass" background regardless of whether anything is behind it to
  justify the blur; a uniform grid of icon-plus-heading-plus-text cards (a
  visual-hierarchy problem, not just a color one, per Constitution #6).
- **What disciplined use looks like instead**: derive the palette from the
  *product's actual brand and content* (Constitution #22 — a design
  decision is only as good as the product understanding behind it), not
  from "what looks like a modern SaaS product." A finance product's
  legitimate reasons to be calm and neutral-toned are different from a
  creative tool's legitimate reasons to be expressive — neither should
  default to purple-gradient-plus-glow because that's what's currently
  common. Concretely: pick one accent hue tied to the brand, not a
  gradient, unless the gradient is doing a specific representational job;
  use shadow/tint for elevation instead of glow; use blur only where
  content is genuinely layered; and run the "would this design look
  different if I actually looked at this product's content and users"
  test — if the answer is no, the visual choices were made without
  reference to the product (Constitution #21's diagnostic).

## Dark mode: not inverted light mode

**ESTABLISHED**, and worth stating explicitly because naive "invert the
lightness values" dark themes reliably look wrong. The core reason: color
perception is not linear, and a color that reads as a comfortable mid-tone
against a white background can look either washed-out or uncomfortably
vibrating against a black one — a phenomenon researchers call
**simultaneous brightness/lightness contrast**: the same color's *perceived*
brightness shifts depending on what surrounds it. Adobe's Spectrum team
found WCAG's mathematical contrast ratio and human *perceived* contrast are
measurably different things, and addressed it by computing target contrast
as a *percentage of available contrast* per theme (light vs. dark vs.
"darkest") rather than applying one fixed ratio target uniformly, using a
perceptually-informed color space (CAM02) and Stevens' Power Law to keep
lightness steps feeling perceptually even rather than mathematically even.

**Practical rules that follow:**
- Don't use pure black (`#000`) backgrounds with pure white text at full
  saturation for extended reading surfaces — the contrast is technically
  enormous (21:1) but the perceptual vibration is fatiguing; most mature
  dark themes use a very dark gray (not pure black) surface with an
  off-white (not pure white) text color.
- Desaturate and lighten brand/accent colors for dark mode rather than
  reusing the light-mode hex value — a saturated brand blue that looks
  crisp on white frequently looks muddy or loses contrast on a dark surface
  and needs a lighter, sometimes slightly different-hue variant.
- Elevation inverts: in light mode, higher elevation is often expressed by
  little more than shadow; in dark mode, shadow is nearly invisible, so
  elevation must be expressed primarily by making higher surfaces
  progressively *lighter* than the base background.
- Treat dark mode as a first-class palette with its own contrast
  verification pass, not a CSS filter or automatic inversion of the light
  palette.

## High-contrast modes

**CONTEXTUAL / under-researched, flag for follow-up.** OS-level
forced-colors/high-contrast modes (Windows High Contrast, `forced-colors:
active` in CSS, macOS Increase Contrast) override author colors with a
constrained system palette; a well-built color-token system that relies on
`currentColor` and standard form-control elements degrades gracefully under
these modes, while heavy custom styling (custom checkboxes, icon-only
buttons with no border) can become invisible or unusable. This needs a
dedicated deeper pass — noted honestly here as under-researched rather than
padded with unverified specifics.

## Sources

- [WCAG 2.2 — W3C Recommendation](https://www.w3.org/TR/WCAG22/) — 1.4.3, 1.4.6, 1.4.11 normative text and ratios. Accessed 2026-09-12.
- [wcag22aa.org — Target Size (Minimum) SC 2.5.8](https://wcag22aa.org/new-criteria/target-size/) — 24×24 CSS pixel requirement, five exceptions, spacing math. Accessed 2026-09-12.
- [Adobe Design — Reinventing Adobe Spectrum's Colors](https://adobe.design/stories/design-for-scale/reinventing-adobe-spectrum-s-colors) — semantic color roles, perceived-vs-mathematical contrast, CAM02/Stevens' Power Law, per-theme contrast-percentage approach, dark/darkest theme elevation reasoning. Accessed 2026-09-12.
- [SmoothUI — AI Design Slop: Why AI-Generated UI Looks Generic](https://smoothui.dev/blog/ai-design-slop) — purple/cyan gradient heroes, glassmorphism + glow, repetitive card grids, statistical-default mechanism for why generative tools converge on these patterns. Accessed 2026-09-12.
- [Constitution — UI Principles 2026, #7, #20, #21, #22](../knowledge/ui-principles-2026.md) — internal, framing for restraint and role-based color. Accessed 2026-09-12.
- [`material-components/material-web` — `tokens/versions/v0_192/_md-sys-elevation.scss`](https://raw.githubusercontent.com/material-components/material-web/main/tokens/versions/v0_192/_md-sys-elevation.scss) (raw source, fetched directly) — the six dp elevation levels (0/1/3/6/8/12dp). Accessed 2026-09-12.
- [`material-foundation/material-color-utilities` — `typescript/dynamiccolor/color_spec_2021.ts`](https://raw.githubusercontent.com/material-foundation/material-color-utilities/main/typescript/dynamiccolor/color_spec_2021.ts) (raw source, fetched directly) — `surfaceContainerLowest()` through `surfaceContainerHighest()` and `surfaceTint()` implementations, i.e. the actual current elevation/tint model. Accessed 2026-09-12.

**Weak spots / revisit:** Material 3's surface-tint-at-elevation section
was rewritten this pass after a primary-source fetch (see above) revealed
the previous "increasing tint percentage per dp" description was an
unverified reconstruction that actually matches Material 2, not the
current M3 web implementation — see the corrected explanation above. The
`m3.material.io` marketing site itself remains JS-gated and was not the
source of this fix; the open-source token/color repositories were.
High-contrast/forced-colors mode guidance is still explicitly flagged
above as needing a dedicated research pass — do not treat the one
paragraph here as sufficient.
