---
title: Content Density
confidence-note: Direct expansion of Constitution #11; grounded in observed professional-tool density conventions (IDEs, trading terminals, admin panels such as those built on Carbon/Polaris/enterprise systems) versus consumer-app conventions. Density-mode mechanics are ESTABLISHED; specific pixel/row-height numbers are CONTEXTUAL (vary by design system).
last_updated: 2026-09-12
---

# Content Density

This file expands Constitution #11 directly: **information density should
reflect user intent and frequency of use, not a brand-wide aesthetic
choice.** Density is frequently treated as a purely visual/taste decision
("we want a clean, spacious look" vs. "we want a dense, data-rich look").
That framing is backwards — density is a *product* decision downstream of
who the user is, how often they use the screen, and what their task costs
are, and only then expressed visually.

## The Core Variable: Comprehension Cost vs. Throughput Cost

Every user paying attention to a screen is paying one of two costs
(Constitution #11's framing, made explicit):

- **Comprehension cost** — the effort to understand what's on screen and
  what it means. Low density reduces this cost by showing fewer things at
  once, with more whitespace and progressive disclosure.
- **Throughput cost** — the effort/time to get through a large volume of
  known, understood information or repeated actions. High density reduces
  this cost by showing more at once, minimizing clicks/scrolling/context-
  switches per unit of work done.

**DESIGN RULE.** Identify which cost dominates for a given user and task
*before* choosing a density level. A novice user on their first visit to a
feature is paying comprehension cost — favor low density. An expert running
the same report every morning for two years is paying throughput cost —
favor high density. Designing both audiences to the same density
optimizes for neither.

## Density as a Function of Expertise, Frequency, and Task Type

**FOUNDATIONAL**, restating and operationalizing Constitution #11:

- **Expertise.** A novice needs labels spelled out, generous spacing
  between choices, and fewer simultaneous options (reduces the chance of
  the wrong choice and the cognitive load of scanning). An expert has
  already internalized the layout and benefits more from compression than
  from continued hand-holding — persistent verbose labels and wide
  spacing become throughput friction once the user no longer needs the
  scaffolding.
- **Frequency of use.** A screen visited once (onboarding, a rare
  settings page) should optimize for first-encounter comprehension even at
  the cost of extra scrolling/clicks. A screen visited dozens of times a
  day (an inbox, a queue, a trading blotter) should optimize for getting
  through many rows/decisions fast, because the cumulative cost of low
  density multiplies by every visit.
- **Task type.** Decision-heavy tasks (comparing options, making a judgment
  call) benefit from more breathing room around each option even for
  experts, since density here would compress the very information being
  weighed. Execution-heavy tasks (processing a queue of known-shape items)
  benefit from density because the "decision" per item is small and
  repetitive — the cost is in volume, not per-item difficulty.

## Compact vs. Comfortable Modes

**ESTABLISHED** pattern in professional software (enterprise admin tools,
spreadsheet apps, some IDEs and dashboards): offering the *same* screen in
two (or more) density settings, typically user-selectable, rather than
forcing one density choice on every user of a shared tool.

**WHEN TO OFFER A DENSITY TOGGLE.** The same screen genuinely serves both a
high-frequency expert audience and a low-frequency/occasional audience (a
shared admin table used daily by an ops team and occasionally by a
manager checking one record) — a toggle lets each audience self-select
rather than forcing a single compromise density that serves neither well.

**WHEN NOT TO BOTHER.** The screen has one clear, uniform audience — adding
a density toggle here is unnecessary complexity/maintenance burden
(Constitution #9) for a choice nobody would actually make differently.

**DESIGN RULES**
- A density toggle should primarily change **spacing and row height**, not
  hide/show different information — changing what information is visible
  between modes turns a density preference into a features gap, which is a
  different and riskier product decision.
- Persist the user's chosen density as a preference (not re-asked every
  session) — this is itself a throughput consideration: re-choosing your
  preferred density every visit is exactly the kind of repeated friction
  density settings exist to eliminate.
- Compact mode still needs to meet minimum accessible touch-target and
  contrast requirements (Constitution #14) — "compact" must not become
  "inaccessible"; on touch-primary devices, compact-mode row heights
  should not fall below the platform's minimum touch target even if visual
  padding is reduced (see `knowledge/mobile-patterns.md`'s 44/48px
  guidance) — pack visual whitespace tighter, not the tappable area.

## Information Scent vs. Overload

**FOUNDATIONAL.**

Information scent is the user's ability to judge, from what's currently
visible, whether continuing to scan or click in a given direction will
lead to what they want. Density and information scent are in tension in
both directions:

- **Too sparse** and information scent suffers because too little is
  visible at once to judge direction — a user has to click into things one
  at a time to learn anything, which is its own throughput cost even for
  low-frequency users.
- **Too dense** and information scent suffers because everything visible
  competes for attention with equal weight (Constitution #4) — the signal
  the user actually needs is buried in noise even though it's technically
  "on screen."

**DESIGN RULE.** The right density maximizes information scent for the
target user, not the raw amount of information shown — cramming more onto
a screen is only a throughput win if the user can still tell at a glance
which of the many visible things matters to them right now (this is why
high-density professional tools still rely heavily on typographic
hierarchy, per Constitution #7, rather than density serving as an excuse to
flatten hierarchy).

## Professional/Expert Tools vs. Consumer/Occasional-Use Tools as Density Anchors

Useful reference anchors, not rules to copy uncritically — the point is the
*reasoning*, not the specific pixel values of any one product:

- **IDEs, trading terminals, admin/ops consoles.** Extremely high density
  is not a stylistic choice here — it's a direct response to expert,
  full-time, multi-hour-per-day usage where even small per-glance savings
  compound massively over a career of use. Minimal whitespace, small type
  (but never below accessible minimums), abbreviations understood by the
  trained audience, dense multi-column layouts, and minimal animation
  (per Constitution #12 — motion adds latency, which is a direct cost to
  throughput-optimized users) are all justified by this same frequency/
  expertise logic, not by an aesthetic preference for "technical-looking"
  UI.
- **Consumer apps, onboarding flows, marketing sites, occasional-use
  utilities (tax software used once a year, a rarely-visited settings
  page).** Low density is justified by the opposite logic: the user has no
  built-up mental model to lean on, is paying comprehension cost almost
  entirely, and low frequency means there's no large multiplier making
  small per-visit frictions expensive in aggregate — spending a little
  more space and a little more hand-holding per visit is cheap because
  there are few visits.

**DESIGN RULE — the anti-pattern to avoid in both directions.** Applying
consumer-app spaciousness to a professional daily-use tool produces a
product that feels "friendly" in a demo but frustrates its real, frequent
users with excessive scrolling and clicking for routine work. Applying
professional-tool density to a consumer or first-use product produces a
screen that overwhelms and intimidates the exact audience it's trying to
onboard. Neither is a stylistic failure — both are a failure to match
density to Constitution #11's actual variables (who, how often, what
task) rather than a house visual style.

## Sources

- This file is a direct expansion of `knowledge/ui-principles-2026.md`
  Principle 11; no external claim here contradicts or extends beyond that
  principle's scope.
- Cross-referenced against observed density conventions in enterprise
  design systems (Carbon, Polaris) and professional developer/trading
  tooling as category anchors (2026).
