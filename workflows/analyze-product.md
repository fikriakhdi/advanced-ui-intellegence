---
title: Analyze Product
last_updated: 2026-09-12
---

# Workflow: Analyze Product

**Purpose.** Before any layout, component, or visual decision is made,
build an honest, specific understanding of the product, its users, and
their intent. This workflow is the mandatory first stage of any design
task — see Constitution #22 ("A design decision is only as good as the
product understanding behind it"). It exists because the most common
route to a generic or wrong design is skipping this step and jumping
straight to component selection.

**Hard rule:** design generation (see `workflows/generate-ui.md`) must
not start until every question below has an explicit answer — even a
provisional one. If the user's brief doesn't supply an answer, ask, or
state the assumption being made and flag it as an assumption. An
assumption made silently is a design decision made without product
understanding, which is exactly what Constitution #22 forbids.

---

## The questions

Work through these in order. Later answers should be checked against
earlier ones for consistency (e.g. a "casual, infrequent" user paired
with a "dense, expert" density claim is a contradiction — resolve it,
don't carry it forward).

1. **Product** — What is this, in one sentence? Not the feature list —
   the thing itself and the category it competes in or replaces.
2. **Primary user** — Who is the main person using this screen/flow?
   One primary persona, named specifically (not "users" — "a merchant
   running a live-shopping stream," "a support agent triaging tickets").
3. **Primary goal** — What is this user trying to accomplish, in this
   specific screen or flow, right now? Not the product's mission — the
   task at hand.
4. **Secondary goals** — What else might they plausibly want to do here,
   in rough order of likelihood? These become secondary/tertiary
   actions, not competing primaries (Constitution #8).
5. **Frequency of use** — Multiple times a day, daily, weekly, once?
   Frequency drives how much can be assumed as "already learned" versus
   needs to be self-explanatory each time (Constitution #2, #11).
6. **Environment** — Focused desk session, on-the-go/interruptible,
   background/ambient monitoring, high-stress/time-pressured? This
   affects tolerance for density, motion, and required precision.
7. **Device** — What device(s) is this actually used on, and which is
   primary? Not "responsive" as an afterthought — which device the
   *primary* workflow happens on shapes the base design (Constitution
   #10, #18).
8. **Information hierarchy** — If the user could see only one number or
   fact on this screen, what must it be? Rank the next 4-6 things after
   that. This ranking is the single most load-bearing artifact of this
   workflow — it feeds IA (`workflows/information-architecture.md`)
   directly.
9. **Content density** — Given frequency and user expertise (Q5, Q2),
   should this screen be low-density/spacious (novice, occasional,
   comprehension-limited) or high-density/compact (expert, frequent,
   throughput-limited)? See Constitution #11.
10. **Navigation requirements** — How many distinct areas/sections does
    this product have? Flat or does it need grouping/nesting? Is
    navigation persistent (used constantly) or occasional (settings,
    rarely visited)?
11. **Primary actions** — What is the one action this screen most wants
    the user to take? (Per Constitution #8, this must resolve to one
    per decision region, not one globally if the screen has multiple
    independent regions.)
12. **Secondary actions** — What supporting actions exist, and how much
    visual weight do they deserve relative to the primary action?
13. **Brand personality** — Where does this product sit on axes like
    professional↔playful, quiet↔bold, warm↔clinical, dense↔spacious?
    Pick a position, don't default to neutral — neutral-by-default is
    how generic AI UI happens (Constitution #21).
14. **Accessibility requirements** — Any known constraints beyond
    baseline (low-vision users, older user base, one-handed/mobile-only
    use, regulatory requirements e.g. WCAG AA/AAA, public sector)? Even
    "no known special requirements" should be stated explicitly so it
    isn't silently dropped later — see `knowledge/accessibility.md`.
15. **Technical constraints** — Framework/design-system already in use,
    performance budget, offline requirements, existing brand/token
    system to respect, legacy browser support, embed context (iframe,
    native webview)?

---

## From answers to strategy

Once all 15 are answered, synthesize a **Design Strategy** — a short,
falsifiable statement, not a mood board. It should contain, at minimum:

- **Information priority** — the ranked list from Q8, numbered. This is
  what `workflows/information-architecture.md` consumes directly.
- **Design character** — 4-6 adjectives derived from Q13 + Q9 + Q6, each
  one earning its place because it predicts a decision (e.g. "medium-
  density" predicts fewer whitespace-heavy hero sections; "fast to scan"
  predicts typography/scanning-pattern choices over illustrative ones).
- **Primary action model** — the one primary action per key region,
  stated explicitly, so later screens can be checked against it.
- **Density and motion posture** — one line each, derived from Q9, Q5,
  Q6, Q12 (Constitution #9, #11, #12).
- **Constraints carried forward** — the technical and accessibility
  constraints (Q14, Q15) that will gate later choices (e.g. "must hit
  WCAG AA," "no custom fonts, must use existing token set").

A strategy that is just restated adjectives with no traceable answer
behind them ("modern, clean, delightful") has failed this workflow —
every adjective must trace back to a specific answer above.

---

## Worked example

**Brief:** "Build a dashboard for merchants running live-shopping
streams to track how they're doing."

| # | Question | Answer |
|---|----------|--------|
| 1 | Product | Live-commerce merchant dashboard |
| 2 | Primary user | Merchant/creator running live sales streams |
| 3 | Primary goal | Understand business performance and operate commerce activity |
| 4 | Secondary goals | Manage products, review affiliate performance, respond to activity/alerts |
| 5 | Frequency | Daily, often multiple times a day during/after streams |
| 6 | Environment | Mixed — focused review sessions and quick glances between streams |
| 7 | Device | Desktop primary (back-office work), mobile secondary (quick checks) |
| 8 | Information hierarchy | 1. Revenue 2. Orders 3. Live performance 4. Products 5. Affiliates 6. Activity |
| 9 | Content density | Medium-to-high — user is a repeat, semi-expert operator, not a first-time visitor |
| 10 | Navigation | ~6 sections, flat (no need for deep nesting at this scale) |
| 11 | Primary actions | Per-region: "view live session" on the live card, "restock/edit" on a product row |
| 12 | Secondary actions | Export report, filter by date range, manage affiliate payouts |
| 13 | Brand personality | Professional, modern, operational — not playful, not consumer-cute |
| 14 | Accessibility | Baseline WCAG AA; used in bright retail environments so contrast matters more than usual |
| 15 | Technical constraints | Existing internal design system tokens; must work at 1280px minimum desktop width |

**Resulting Design Strategy:**

> **PRODUCT:** Live-commerce merchant dashboard
> **PRIMARY USER:** Merchant/creator
> **PRIMARY INTENT:** Understand business performance and operate
> commerce activities
> **INFORMATION PRIORITY:** 1. Revenue 2. Orders 3. Live performance
> 4. Products 5. Affiliates 6. Activity
> **DESIGN CHARACTER:** Professional, modern, operational, medium-
> density, fast to scan, minimal decorative noise.
> **PRIMARY ACTION MODEL:** One clear operational action per card/region
> (view live, edit product); no page-level competing CTAs.
> **DENSITY/MOTION POSTURE:** Medium-high density; motion limited to
> state changes (live indicator pulse, value updates) — no decorative
> transitions.
> **CONSTRAINTS CARRIED FORWARD:** WCAG AA minimum, existing token
> system, desktop-first at ≥1280px with a recomposed (not shrunk) mobile
> view per Constitution #10.

This strategy — not the original one-sentence brief — is what
`workflows/information-architecture.md` and `workflows/generate-ui.md`
should receive as input.
