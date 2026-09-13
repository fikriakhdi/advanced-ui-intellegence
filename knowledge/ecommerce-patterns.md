---
title: Ecommerce Patterns
confidence-note: Grounded in Baymard Institute large-sample checkout benchmarking (180+ sites) and Shopify Polaris admin/storefront conventions; conversion statistics are ESTABLISHED empirical findings, not opinion, but are benchmark averages — treat specific percentages as directional, re-verify for a given product/audience before citing to stakeholders as guaranteed lift.
last_updated: 2026-09-12
---

# Ecommerce Patterns

Ecommerce UI is judged directly on conversion, so this file leans harder on
cited empirical research (primarily Baymard Institute) than on general
design heuristics. Constitution #16 (progressive disclosure must not hide
decision-relevant information) is the throughline: nearly every checkout
failure mode below is a case of hiding something — a fee, a requirement, a
delivery date — that changes the user's decision when finally revealed.

## Product Listing / Grid Patterns

**ESTABLISHED.**

**DESIGN RULES**
- Each product card needs, at minimum: image, name, price, and — where
  relevant to the category — rating/review count. Anything beyond that
  (badges, quick-add, color swatches) should be judged per Constitution #9:
  does it help this category's decision, or is it decoration.
- Grid density should scale with category browsing behavior: high-
  consideration categories (furniture, electronics) benefit from larger
  images and fewer columns (more visual detail per item, since users
  compare fewer items more carefully); low-consideration/high-volume
  categories (basics, groceries) tolerate denser grids (more items
  scannable per screen) — this is Constitution #11's density principle
  applied to browsing intent rather than user expertise.
- Show price prominently and consistently positioned across all cards in a
  grid — inconsistent price placement forces re-scanning per item.
- Quick-add-to-cart from the grid (skipping the PDP) is appropriate for
  low-risk, single-variant items; items with required variant selection
  (size, color) should route to the PDP or an inline variant picker rather
  than adding an ambiguous/default variant silently.

## Product Detail Page (PDP) Structure

**ESTABLISHED.**

**DESIGN RULES**
- Price, primary product image, and the add-to-cart action belong "above
  the fold" (or immediately reachable) — this is the PDP's one primary
  action per Constitution #8, and everything else on the page is in
  service of that decision.
- Variant selection (size/color/etc.) must be resolved *before* or *at* the
  add-to-cart action, with unavailable combinations clearly disabled
  rather than silently accepted and failing later.
- Shipping/delivery estimate and return policy should be visible on the PDP
  itself, not deferred to checkout — these are decision-relevant facts per
  Constitution #16 (see checkout findings below on delivery-date framing).
- Reviews, detailed specs, and related/complementary products belong
  below the primary decision area — supporting detail, not competing with
  the primary action for attention (Constitution #1).

## Cart and Checkout Flow

This is the highest-stakes surface in ecommerce UI; Baymard Institute's
large-scale checkout benchmark (180+ leading sites, updated through 2025)
found **zero sites achieved a "perfect" checkout UX score**, with 64% of
desktop and 63% of mobile checkouts rated mediocre or worse — meaning even
mature, well-funded checkouts routinely violate these findings.
**ESTABLISHED**, cite Baymard directly for any of the following claims.

**DESIGN RULES, with sourced findings**
- **Guest checkout must be the visually prominent option.** Baymard found
  62% of sites fail to make guest checkout the most prominent choice,
  and forced account creation is a leading, avoidable abandonment cause —
  roughly 18% of surveyed users reported abandoning an order specifically
  over being forced to create an account.
- **Show delivery date, not shipping speed.** "Delivery on April 4th" is
  unambiguous; "2 Business Days" requires the user to do date math and
  guess the cutoff — a small framing change with outsized clarity benefit.
- **Combine fulfillment options in one interface.** 52% of sites require
  navigating back to the cart to switch between pickup, local delivery,
  and shipping — present all applicable fulfillment methods together at
  the point of decision.
- **Mark both required AND optional fields explicitly**, not just one or
  the other. Baymard found 32% of test participants skipped a required
  field when only *optional* fields were labeled — an asymmetric-labeling
  failure, not a carelessness failure. See `knowledge/forms-inputs.md`.
- **Keep password/account requirements minimal at checkout** — a 6-8
  character minimum is sufficient; complex composition rules are linked to
  meaningful abandonment (19% among users resetting a forgotten password
  mid-checkout).
- **Give adaptive, specific error messages** for payment/field errors
  ("Your card number is incomplete") rather than a generic "Invalid card" —
  Baymard found 94% of sites still use unhelpful generic messages. This is
  the ecommerce instance of the general error-message-quality rule in
  `knowledge/forms-inputs.md` and `knowledge/states-feedback.md`.
- **Use stepper/plus-minus controls plus a text field for quantity**, not a
  bare text field alone — bare text quantity fields are especially
  error-prone on mobile numeric entry.
- **Explain why a phone number is required** if collected at checkout
  (order updates, delivery coordination) — unexplained requests for phone
  numbers measurably increase hesitation.
- **Never surprise the user with new costs at the final step.** Unexpected
  shipping fees/taxes revealed only at the last checkout step are one of
  the best-documented, highest-impact abandonment causes across ecommerce
  UX research generally — show a running total (including estimated
  tax/shipping where feasible) as early as the cart, not just before final
  payment.

**COMMON FAILURE.** Treating checkout as a place to *add* friction
(account walls, upsells, newsletter opt-ins inserted mid-flow) — every
extra field, decision, or interruption in checkout is a re-litigation of
the "should I still do this" question the user already answered by
starting checkout. Constitution #16 applies directly: nothing about the
purchase's true cost or terms should be a late reveal.

## Filters and Faceted Search

**ESTABLISHED**, cross-referenced against Shopify Polaris's admin filtering
component conventions (index filters) as a structural model, adapted here
to storefront faceted search.

**DESIGN RULES**
- Promote only the 2-3 most commonly used facets (category, price range,
  a category-specific attribute like size) as immediately visible controls;
  push long-tail facets (brand, material, rating) into an expandable "More
  filters" section — mirrors Constitution #16's progressive disclosure.
- Show the live, updated result count as filters are applied, ideally
  before the user commits/closes a filter panel — lets the user course-
  correct (loosen an over-narrow filter) without a round trip.
- Display active filters as removable chips/pills near the result list, so
  the current filtered state stays visible and reversible without
  reopening the filter panel — directly reusable across desktop sidebar and
  mobile sheet forms of the same pattern (see `knowledge/mobile-patterns.md`
  "Mobile Filters").
- On mobile, faceted filtering *becomes a sheet* (Constitution #10) rather
  than a persistent sidebar — there is no room for a permanent filter rail
  at phone widths.
- Preserve filter/sort state in the URL so filtered results are shareable
  and survive back-navigation from a PDP.

## Pricing and Promotion Display

**ESTABLISHED.**

**DESIGN RULES**
- If showing a discount, show the original price, the discounted price,
  and the discount itself (percentage or amount) together — any one alone
  is a weaker, more ambiguous signal.
- Be conservative with urgency/scarcity messaging ("Only 2 left," countdown
  timers) — genuine scarcity is a legitimate, high-value signal; fabricated
  or perpetual scarcity messaging is a well-documented trust-eroding
  dark pattern once users notice it repeats regardless of actual stock.
- State whether a displayed price includes or excludes tax/shipping
  unambiguously near the price itself, not only in fine print — this
  directly prevents the late-cost-reveal failure described in the checkout
  section.

## Trust Signals

**ESTABLISHED.**

**DESIGN RULES**
- Place trust signals (secure-checkout badges, return policy, reviews,
  recognizable payment logos) near the decision point they support —
  payment security badges near the payment form, return policy near
  add-to-cart, not clustered only in a footer no one scrolls to during the
  actual decision.
- Real, specific review content (with review count) outweighs a generic
  star rating alone — a 4.5-star rating with "(1,204 reviews)" is more
  credible than the stars alone, and both outweigh no social proof.
- Avoid stacking so many trust badges/logos that the page reads as
  defensive rather than confident — per Constitution #4, a wall of trust
  badges undermines its own signal once it looks like anxious over-
  justification rather than a couple of well-placed, credible marks.

## Mobile Commerce Specifics

See `knowledge/mobile-patterns.md` for the general framework; ecommerce-
specific applications:

- Sticky "Add to Cart" bar on the PDP once the user scrolls past the
  primary buy box, so the action stays reachable through long product
  description/review content (see mobile "Sticky Actions").
- Checkout on mobile should minimize typing wherever possible: platform
  autofill for address and payment (Apple Pay/Google Pay/saved cards),
  numeric keyboards for numeric fields, and fewer total form fields than
  the desktop equivalent by combining fields where feasible (e.g., a
  single full-name field instead of first/last, where the backend can
  support it) — every avoided keystroke has outsized value on mobile
  input.
- Mobile checkout should preserve the same "guest checkout prominent, no
  surprise costs" findings above — the abandonment causes are not desktop-
  specific, and mobile's higher input friction makes the cost of any
  unnecessary field or step even higher.

## Sources

- [Checkout UX Best Practices 2025 – Baymard Institute](https://baymard.com/blog/current-state-of-checkout-ux)
- [Index filters — Shopify Polaris](https://polaris-react.shopify.com/components/selection-and-input/index-filters)
- [Filters — Shopify Polaris](https://polaris.shopify.com/components/filters)
- [Data table — Shopify Polaris React](https://polaris-react.shopify.com/components/tables/data-table)
