---
title: Mobile Patterns
confidence-note: Synthesized from Apple HIG, Material Design 3, and cross-referenced UX research current as of 2026; numeric values (touch targets, safe areas) are platform-sourced and FOUNDATIONAL, while trend-adjacent items are tagged individually.
last_updated: 2026-09-12
---

# Mobile Patterns

This file operationalizes Constitution #10 (mobile interfaces are recomposed,
not shrunk) and #11 (density follows intent/frequency). Every section below
is an instance of the same underlying move: identify what a desktop layout
assumed (cursor, hover, wide viewport, low interruption, two-handed input)
and re-decide the element under touch, narrow-viewport, high-interruption
conditions.

## The Recomposition Framework (Constitution #10)

> **Canonical version:** `knowledge/responsive-design.md` §4 ("The
> Recomposition Decision Framework") is the authoritative statement of this
> framework — it was developed independently in parallel with this file and
> the two are consistent in substance (same eight outcomes, same
> element-by-element application) but not identical in wording. Treat any
> future edit to the framework as an edit to responsive-design.md first,
> then reconcile this restatement to match. What follows here is the
> mobile-specific application, not a competing definition.

When adapting a component or region from a wide/desktop layout to a narrow
one, classify every piece of it into exactly one of these outcomes — do not
default to "shrink it":

- **Keep** — the element survives unchanged (a primary CTA, a page title).
- **Move** — relocates to a place that makes sense in the new layout (a
  secondary action moves from inline to an overflow menu; a filter panel
  moves from a persistent sidebar to a triggered sheet).
- **Collapse** — remains present but starts closed/summarized, expandable on
  demand (an accordion section, a "show more" row).
- **Hide** — removed entirely at this size because it's not essential here
  (decorative imagery, a secondary metric that only matters at a glance on a
  large monitor).
- **Merge** — two or more desktop elements combine into one mobile element
  (separate "sort" and "filter" bars become one "Filter & Sort" sheet
  trigger).
- **Become scrollable** — a layout that showed everything at once (a
  side-by-side table, a multi-column form) becomes a single scrollable
  column, or a horizontally-scrollable strip for peer items (category chips,
  a carousel of cards).
- **Become a sheet** — a desktop modal, popover, or inline panel becomes a
  bottom sheet that the user can drag, because sheets respect thumb reach
  and one-handed use better than centered modals.
- **Become a different component entirely** — the desktop pattern has no
  honest mobile equivalent and is replaced by a pattern native to touch (a
  hover-triggered tooltip becomes a tap-triggered info affordance; a
  multi-column comparison table becomes a stacked card-per-item list with a
  "compare" toggle).

Apply this per element, not per screen — a single screen typically uses five
or six of these outcomes simultaneously. **FOUNDATIONAL.**

## Thumb Reach and Ergonomic Zones

On a phone held one-handed, the reachable zone is roughly the bottom third
of the screen without the hand shifting grip; the top third (especially top
corners, opposite the holding hand) is the hardest to reach, and the middle
third is reachable but requires more effort/stretch than the bottom.
**ESTABLISHED** (based on longstanding thumb-zone ergonomics research,
reconfirmed across current phone sizes which have only grown, making this
more true, not less).

**DESIGN RULES**
- Primary, frequent actions (navigation, the main CTA, compose/create)
  belong in the bottom third.
- Destructive or rarely-used actions can live in the top-right/top-left,
  where accidental taps are less likely precisely because reaching there is
  effortful.
- Never place the *only* way to complete the primary task at the very top
  of a tall screen with no secondary access point lower down (e.g., a
  sticky bottom "Continue" button restating a top-of-screen primary action).

## Bottom Navigation vs. Tab Bars

**PATTERN.** A bottom tab bar is a fixed, always-visible set of top-level
destinations (Apple HIG: iOS tab bars).

**PURPOSE.** Give the user a persistent, one-tap way to jump between the
app's handful of top-level sections, and to communicate "you are here."

**WHEN TO USE.** 3–5 top-level, roughly equal-weight destinations that a
user moves between frequently, non-hierarchically (Home, Search, Inbox,
Profile). Apple HIG guidance: keep the tab count small, about five or fewer,
with a minimum touch target around 44pt per tab. **FOUNDATIONAL** (Apple HIG).

**WHEN NOT TO USE**
- More than 5-6 destinations — this is a sign the IA needs a hub, not a
  bigger tab bar. Overflowing into a "More" tab is an acceptable escape
  valve but is itself a signal to reconsider the architecture.
- Destinations that are steps in a sequence (checkout, onboarding) — that's
  a progress/stepper pattern, not a tab bar.
- One-off/contextual actions — those belong in the screen's own toolbar or
  a floating action, not permanently promoted to global navigation.

**RESPONSIVE BEHAVIOR.** On wider layouts (tablet, foldable unfolded, or a
desktop web view of the same product), the tab bar is a candidate to
*become a different component*: promote it to a side navigation rail, since
the wider viewport removes the thumb-reach justification for bottom
placement and a rail costs less vertical space.

**ACCESSIBILITY.** Tab bar items need a text label (icon-only tab bars fail
recognition for infrequent users and assistive-technology users alike);
respect Dynamic Type/user font-size settings without truncating labels into
ambiguity; each tab is a distinct accessible element with its selected
state announced.

**COMMON FAILURE.** Treating the tab bar as a dumping ground for anything
that seems "important," inflating it past 5 items, or using it for a
single, sequential flow.

## Bottom Sheets

**PATTERN.** A panel that slides up from the bottom edge, partially or fully
covering the screen, draggable to intermediate detents (Material Design 3:
standard and modal bottom sheets).

**PURPOSE.** Present supplementary content or a focused task without fully
leaving the current context — the equivalent of a desktop popover/modal but
shaped for one-handed reach and touch dismissal (drag down / tap scrim).

**WHEN TO USE.** Filters, item details, a share sheet, a secondary form, a
comment composer, quick actions on a long-pressed item — anything that
would be a modal or popover on desktop is a strong candidate to *become a
sheet* on mobile (Constitution #10's "become a sheet" outcome).

**WHEN NOT TO USE.** Content the user needs to reference *while* interacting
with the page behind it for an extended period (rare on mobile since screen
real estate doesn't allow true side-by-side); a sheet that requires so much
scrolling internally that it might as well be its own full screen — at that
point, push a new screen instead.

**DESIGN RULES**
- Support at least a partial-height detent (peek) and a full-height detent
  when content warrants it; let the user drag between them.
- A modal bottom sheet needs a scrim and traps focus like any modal; a
  non-modal (standard) sheet does not block interaction with the rest of
  the screen and has no scrim.
- Always provide an explicit close affordance (not drag-to-dismiss alone) —
  some users won't discover the gesture, and it's not available to
  switch-control or screen-reader users.

**ACCESSIBILITY.** Sheets must be reachable and dismissible via
assistive-technology-compatible controls, not gesture-only; focus moves
into the sheet on open and returns to the trigger on close (modal sheets).

**COMMON FAILURE.** A bottom sheet with no visible drag handle or close
button, relying entirely on a swipe gesture a user must discover by
accident.

## Gesture Interactions

**ESTABLISHED**, with a strict corollary from Constitution #19: gestures
that replace a *visible, discoverable* control are riskier than gestures
that *supplement* one, because a gesture has zero visual affordance by
default.

**DESIGN RULES**
- Every meaningful gesture (swipe-to-delete, swipe-to-archive, long-press
  for context menu, pull-to-refresh) should have a discoverable, visible
  alternative reachable without knowing the gesture exists — a menu button,
  an edit mode, a manual refresh control.
- Reserve system-reserved gesture zones (edge-swipe-back on iOS/Android)
  for their platform meaning; a custom edge-swipe interaction fights muscle
  memory and the OS itself.
- Swipe actions on list rows should reveal their icon/label progressively
  as the user drags, not require a full swipe to know what will happen.

**ACCESSIBILITY.** Gestures must never be the *sole* means of accomplishing
an action (this generalizes Constitution #14): screen-reader users navigate
by heading/element, not by spatial swipe, and motor-impaired users may not
be able to perform multi-finger or precise-timing gestures at all.

## Safe Areas and Notches

**FOUNDATIONAL.** Modern phones reserve system space (status bar, home
indicator, camera cutouts, rounded corners) that content must not be
obscured by or crowd against. Use the platform's safe-area insets
(`env(safe-area-inset-*)` on web, `safeAreaLayoutGuide` on iOS) rather than
hardcoded padding — the inset value differs by device and orientation.

**DESIGN RULES**
- Fixed bottom bars (tab bars, sticky CTAs) must add the bottom safe-area
  inset to their padding so content isn't flush against the home indicator.
- Full-bleed media may extend under the status bar/notch, but interactive
  controls and text must stay clear of it.
- Landscape orientation moves the safe-area geometry to the sides (notch or
  camera cutout may sit left/right) — a layout that only accounted for
  portrait safe areas breaks in landscape.

## Keyboard Handling and Avoidance

**PATTERN/FAILURE PAIR.** The on-screen keyboard covers roughly a third to
half of a phone's viewport when open. A layout that doesn't account for
this hides the very field being edited, the submit button, or validation
messages.

**DESIGN RULES**
- The focused input should scroll into view above the keyboard automatically
  (not require the user to manually scroll while typing).
- A form's primary submit action should either float above the keyboard
  (an accessory/toolbar bar) or be reachable by scrolling without dismissing
  the keyboard.
- Avoid opening a keyboard automatically on screen entry unless the entire
  screen's purpose is that one field (a search screen) — an unexpected
  keyboard is disorienting and covers content the user hasn't seen yet.
- Match the keyboard type to the input (`type="email"`, `type="tel"`,
  numeric pad for OTP/amounts) — this is also a *forms* concern, see
  `knowledge/forms-inputs.md`.

## Mobile Forms

Forms are one of the highest-friction mobile surfaces because typing on
glass is slow and error-prone. See `knowledge/forms-inputs.md` for the full
treatment; mobile-specific rules:

- One logical field group visible per screen-height where the form is long;
  a multi-step wizard often *becomes* the mobile version of a single dense
  desktop form (recomposition: "become a different component").
- Full-width inputs and buttons — there is no room for a desktop-style
  inline label-left-of-field layout at phone widths; labels move above
  fields (recomposition: "move").
- Autofill and platform autocomplete (address, payment, OTP autofill from
  SMS) are far more load-bearing on mobile than desktop — wire up the
  correct `autocomplete`/`textContentType` values or the effort savings
  don't happen.

## Mobile Tables and Tabular Data

A desktop data table cannot survive unchanged below roughly 600px of width.
Recomposition options, in order of how much structure they preserve:

1. **Become scrollable (horizontal)** — keep the table shape, pin the first
   (identifying) column, let the rest scroll sideways. Preserves row/column
   relationships exactly; costs discoverability (users may not realize more
   columns exist) and comparison-across-rows becomes awkward once scrolled.
2. **Collapse columns into a card per row** — each row becomes a small card
   showing the 2-3 most important fields, with the rest revealed on tap
   (expand) or on a detail screen. Best when rows represent a "thing" a user
   reasons about as a unit (an order, a contact, a product) rather than
   values compared column-by-column.
3. **Merge secondary columns into a single summary line** — e.g., "Status ·
   Due Mar 12 · $240" as one line under the primary field, instead of three
   separate columns. Good middle ground for lists that are still
   list-shaped, not deeply comparative.
4. **Hide entirely and provide a filter/sort to reach what a hidden column
   would have shown** — appropriate only for genuinely secondary columns
   (an internal ID), never for anything the user needs to make a decision
   (Constitution #16).

**DESIGN RULE.** Decide by asking "does the user need to compare this value
*across rows* at a glance, or *within one row* as detail?" Cross-row
comparison data should stay in the scrollable-table or summary-line
approach; within-row detail data is safe to collapse to a card.

## Content Prioritization Under Constraint

Because vertical scrolling is cheap on mobile but *visual scanning budget*
before a user gives up is small, content prioritization must be explicit and
ruthless — apply Constitution #1 (hierarchy) and #9 (every element must
justify itself) more aggressively on mobile than desktop, since there is no
peripheral-vision slack to hide a low-priority element in.

**DESIGN RULE.** For any screen, rank content into: what the user came for
(must be visible without scrolling or within one scroll), what supports
that decision (visible with modest scrolling), and what's reference-only
(behind a tap: collapse, move to a secondary screen, or hide).

## Touch Target Sizing (real numbers)

- Apple HIG: minimum touch target roughly **44×44pt**.
- Material Design / Android accessibility guidance: minimum touch target
  **48×48dp**.
- WCAG 2.2 Success Criterion 2.5.8 (Target Size, Minimum): **24×24 CSS px**
  minimum for the *target itself* (with exceptions for inline text links and
  spacing-based exceptions), and WCAG 2.5.5 (AAA) recommends **44×44 CSS
  px**. **FOUNDATIONAL.**

**DESIGN RULE.** Design to the stricter of the platform guideline and 44px
for anything that is a user's primary means of triggering an action;
visually small icons (e.g., a 16px icon) still need their *tappable* hit
area padded out to the full 44/48px target — the visual size and the hit
target size are independent.

## Scrolling Behavior

- Vertical scroll is the default, expected interaction; don't fight it with
  custom scroll-jacking or intercepted momentum.
- Horizontal scroll is acceptable *only* for a clearly-bounded, peer set of
  items (categories, a carousel) that the user understands has more content
  off-screen — never for primary reading content, and never nested inside
  another horizontal scroll region (users cannot reliably control two
  orthogonal-feeling swipe zones stacked together).
- Pull-to-refresh is a well-understood, low-cost affordance for
  "check for new content" on feeds/inboxes — but it must not be the *only*
  way to refresh (Constitution #19's discoverability concern; pair with a
  manual refresh control or automatic background refresh).

## Sticky Actions

**PATTERN.** A primary action (Add to Cart, Continue, Save) pinned to the
bottom of the viewport, remaining visible while the body content scrolls.

**WHEN TO USE.** Long scrollable content (a product page, a long form, an
article with a "Subscribe" goal) where the primary action would otherwise
scroll out of reach, and the action applies to the *whole* screen/entity
rather than to a specific scrolled-past element.

**WHEN NOT TO USE.** Short screens that already fit the action in view
without scrolling — a sticky bar there just wastes permanent vertical space
and duplicates content. Also avoid stacking a sticky bottom bar on top of
the platform's own bottom tab bar without accounting for combined height
and safe-area insets.

**DESIGN RULES.** Give the sticky bar a subtle separation (shadow or hairline)
from content so it doesn't look like it's part of the scrolling body; ensure
it never obscures the field/content a user needs to see to decide on the
action (e.g., don't let a sticky "Add to Cart" bar cover a variant selector
that's still relevant).

## Mobile Search

- Search entry points on mobile trade off between a persistent, always-
  visible search bar (costs vertical space on every screen) and a search
  icon that expands into a full search experience on tap (costs one extra
  tap, saves space). Choose the persistent bar when search is the primary
  way users use the product (marketplaces, note apps); choose the
  icon-trigger when search is secondary to browsing/navigation.
- On activation, mobile search should typically *become a different
  component*: a dedicated full-screen search experience (recent searches,
  suggestions, scoped filters) rather than an inline expanding text field,
  because the keyboard already claims most of the vertical space.
- Show recent/suggested queries before the user types anything — an empty
  search screen with just a cursor wastes the highest-intent moment in the
  product.

## Mobile Filters

Desktop faceted filtering commonly lives in a persistent sidebar (see
`knowledge/ecommerce-patterns.md`). On mobile this sidebar has nowhere to
live permanently, so it **becomes a sheet**, triggered from a "Filter"
button, typically paired with the result count updating live inside the
sheet ("Show 128 results") so the user doesn't need to close the sheet to
learn whether their filters produced a usable result set (this pairs with
Constitution #16 — the consequence of the filter choice must be visible
before the user commits to it).

**DESIGN RULE.** Show currently-applied filters as removable chips above the
result list even after the sheet closes, so the filtered state remains
visible and reversible without reopening the sheet.

## Responsive Nav Collapse

The desktop-to-mobile transition for primary navigation typically follows:
top nav bar with visible links → **becomes** a hamburger-triggered drawer
or a bottom tab bar. Which one:

- **Bottom tab bar** when there are ≤5 flat, frequently-visited top-level
  destinations (see above).
- **Hamburger/drawer** when there are many destinations, deep hierarchy, or
  infrequently-used sections (settings, account, help) that don't deserve
  permanent bottom-bar real estate — often used *alongside* a tab bar for
  the "More"/overflow case rather than as the sole nav mechanism.

**COMMON FAILURE.** Hiding primary, frequently-used navigation behind a
hamburger purely to "clean up" the header — this trades a small aesthetic
win for a real cost in discoverability and taps-to-destination for the
things users do most (see `knowledge/navigation-patterns.md`).

## Sources

- [Tab Bars — Apple Human Interface Guidelines (mirrored)](https://github.com/sankalpaacharya/apple-human-interface-skills/blob/main/skills/components/tab-bars.md)
- [iOS Tab Bar: A Complete UX and Design Guide for 2026](https://uiuxdesigning.com/ios-tab-bar/)
- [Bottom sheets – Material Design 3](https://m3.material.io/components/bottom-sheets/guidelines)
- [Navigation rail – Material Design 3](https://m3.material.io/components/navigation-rail/guidelines)
- [Navigation bar – Material Design 3](https://m3.material.io/components/navigation-bar/guidelines)
- WCAG 2.2 Success Criteria 2.5.5 (Target Size, Enhanced) and 2.5.8 (Target Size, Minimum) — W3C
