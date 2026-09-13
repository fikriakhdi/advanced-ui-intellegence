---
title: UI Principles 2026 — The Constitution
confidence: FOUNDATIONAL (unless marked otherwise per-item)
last_updated: 2026-09-12
status: living document — expand as research accumulates
---

# The UI Constitution

This is the durable core the rest of the knowledge base must not
contradict. Everything in `knowledge/`, `design-systems/`, `references/`,
and `workflows/` is downstream of these principles. When a specific
pattern doc and this constitution appear to conflict, the constitution
wins, and the pattern doc should be revised.

A principle earns a place here only if it explains a **why**, not just a
**what**. "Use good spacing" is not a principle; it's a placeholder for
one. Every entry below should let you predict a design decision you
haven't seen yet.

Confidence tags used throughout this knowledge base (see
`research/research-log.md` for the full taxonomy):
**FOUNDATIONAL** > ESTABLISHED > CONTEXTUAL > EMERGING > EXPERIMENTAL >
VISUAL TREND. Every item below is FOUNDATIONAL unless noted.

---

## 1. Hierarchy before decoration

A screen's first job is to tell the eye where to go first, second, third.
Decoration (color accents, shadows, illustration, gradients) can *express*
hierarchy but cannot *create* it — if hierarchy doesn't already exist in
the underlying structure (what matters most, given the user's goal right
now), no amount of styling will produce it. Design decoration decisions
only after the information architecture and hierarchy are settled.
**Failure mode:** a beautifully styled screen where everything competes
for attention because styling was used to differentiate elements that
were never actually prioritized against each other.

## 2. Function before visual novelty

A novel interaction or visual treatment is a cost (learning curve,
implementation risk, accessibility risk) that must be paid back in
comprehension or task speed. Default to the familiar solution; spend
novelty deliberately, on the one or two moments in a product where it
earns its keep (see principle 19).

## 3. Clarity before cleverness

Clever copy, clever iconography, and clever layout tricks all impose an
interpretation tax on the user. In any interface used more than once,
that tax is paid every session. Clarity compounds; cleverness decays into
irritation. Cleverness is acceptable in low-frequency, high-delight
contexts (a 404 page, an empty state, onboarding) where the interpretive
cost is paid once and the payoff is affective, not functional.

## 4. Not every element can be visually important

Visual weight is zero-sum on a single screen. If ten elements are all
bold, colored, and boxed, none of them reads as more important than the
others — the user experiences this as noise, not richness. Before adding
emphasis (bold, color, size, a badge, a border) ask what it's being
prioritized *against* — emphasis without a losing counterpart is not
prioritization, it's decoration wearing prioritization's clothes.

## 5. Prefer meaningful whitespace over unnecessary containers

Whitespace is a grouping and separation tool that costs nothing to render
and never competes visually with content. A border or a card background
is a stronger, heavier separator — reach for it when whitespace and
alignment alone can't do the job (dense tabular UI, drag targets,
distinguishing genuinely unrelated regions), not as the default way to
organize a page. See principle 6 and `knowledge/anti-patterns.md` for
"card inside card inside card."

## 6. Do not put everything inside cards

A card says: "this is a discrete, self-contained, often actionable unit"
(a product, a person, a document). It's the wrong container for content
that is simply *sequential* or *sectional* — a settings page's field
groups, a dashboard's KPI row, a form's steps. Overusing cards flattens
this signal: once everything is a card, "card" stops communicating
anything, and the UI gains visual weight (borders, shadows, radius,
padding multiplied by n) without gaining structure. Default to
typographic and spatial grouping; use a card when the content is a
genuine repeatable, comparable, or movable unit.

## 7. Establish hierarchy through typography, spacing, position, and scale before color

Color is the least robust hierarchy signal: it fails for colorblind
users, degrades under bad lighting or cheap displays, and carries
cultural/semantic baggage that competes with pure "importance" signaling
(red reads as *danger*, not just *big*). Typography (size, weight),
spacing (proximity, isolation), position (top/left in LTR reads first,
center reads as the protagonist), and scale should carry hierarchy on
their own; color should then *reinforce* a hierarchy that already works
in grayscale. Test: desaturate the screen — if hierarchy collapses, it
was never really there.

## 8. One region should not contain multiple competing primary actions

A "primary action" is a claim about what the user should do next in that
region. Two primary-styled buttons side by side make that claim twice,
canceling both — the user has to do the prioritization work the UI
should have done. Every region gets at most one primary action; everything
else is secondary, tertiary, or a link. This is a stronger constraint
than "one primary button per screen" — it applies per decision region
(a card, a modal, a table row), not just per page.

## 9. Every visible element should justify its existence

Each element on screen has a rendering cost, a scanning cost (the eye
must pass over or around it), a maintenance cost, and — for interactive
elements — a decision cost. An element earns its place by advancing the
user's current goal, supporting orientation (where am I, what can I do),
or being required by law/platform convention (legal links, required
form fields). "It looks nice" or "it fills the space" are not
justifications; empty space is not a problem to be solved with content
(see principle 5).

## 10. Mobile interfaces should be recomposed, not shrunk

A desktop layout encodes decisions that assumed a cursor, hover, a wide
viewport, and low interruption. Shrinking it preserves those decisions
in a context where none of the assumptions hold (touch, no hover, narrow
viewport, high interruption). Recomposition means re-deciding, per
element: **keep, move, collapse, hide, merge, become scrollable, become
a sheet, or become a different component** — see
`knowledge/mobile-patterns.md`. This is a redesign step, not a CSS
media-query step, even though CSS is how it gets implemented.

## 11. Information density should reflect user intent and frequency of use

A novice, occasional, or task-focused user benefits from low density
(more whitespace, fewer visible options, progressive disclosure) because
their cost is *comprehension*. An expert, high-frequency, professional
user benefits from higher density (more visible at once, fewer clicks,
less animation) because their cost is *throughput* — see a trading
terminal or an IDE versus a consumer onboarding flow. Density is a
product decision informed by who's using the screen and how often, not a
brand-wide aesthetic choice (see `knowledge/content-density.md`).

## 12. Motion should communicate state, causality, continuity, or spatial relationships

Motion is justified when it answers one of: *where did this come from /
go to* (spatial continuity — a modal growing from the button that opened
it), *what changed* (a value updating, an item being removed), *what's
happening right now* (loading, processing), or *what caused what*
(a dependent field appearing because a checkbox was ticked). Motion with
none of these jobs is decorative and, because it adds latency to
perceived responsiveness, is a net cost. See
`knowledge/motion-interaction.md`.

## 13. Components should remain consistent without making every product look identical

Consistency operates at the level of *behavior and structure*: a button
always affords being pressed the same way, a modal always traps focus
the same way, a form always validates at the same trigger points. It
does not require identical visual treatment across products — color,
type, radius, and voice are where brand personality legitimately lives.
Confusing "consistent system" with "identical look" is why so many
products built on the same popular component library feel
interchangeable; the fix is a distinct token layer on top of shared
structural/behavioral patterns, not different structural patterns per
product.

## 14. Accessibility is a design constraint, not a final checklist

Contrast ratios, touch target sizes, focus order, and semantic structure
constrain the design space the same way "must load in under 2 seconds"
or "must work on a 5-inch screen" do — they are inputs to the design
decision, not a QA pass applied after the visual design is finalized.
Treating it as a checklist run at the end produces designs that "mostly"
pass by accident and get expensively reworked when they don't (an
insufficiently contrasted brand color chosen in month one, discovered in
month six). See `knowledge/accessibility.md`.

## 15. Design systems should enable composition, not restrict creativity

A design system's job is to make *common* decisions once (spacing scale,
color roles, type scale, interaction states) so that creative effort can
concentrate on the decisions that are actually specific to this product
and this screen. A system has failed when it makes it *harder* to build
the right thing for an unusual case than to build the wrong thing from
its component library. Systems should expose primitives and composition
rules, not just a closed set of finished components — see
`design-systems/composition.md`.

## 16. Progressive disclosure should reduce complexity without hiding essential information

Progressive disclosure defers *secondary* detail, edge cases, and
advanced options — it must never defer the information the user needs to
form correct expectations or avoid an irreversible mistake (price before
checkout, destructive-action consequences, required fields). The test:
if hiding something behind a click *changes the user's decision* when
revealed, it was hidden wrongly. If it only reduces initial scanning
load without changing the decision, it's a good candidate.

## 17. Visual styling should reinforce product personality without reducing usability

Personality (playful vs. serious, dense vs. spacious, warm vs.
clinical) is legitimate and valuable — it's how products differentiate
and how users judge trust and fit. But personality choices are
subordinate to comprehension and task completion: a playful illustration
style is fine on an empty state, wrong on an error message a user needs
to act on urgently. When personality and usability conflict on a
specific screen, usability wins on that screen; personality still gets
expressed elsewhere.

## 18. Responsive design should respond to available space and content, not device labels

"Mobile," "tablet," and "desktop" are proxies for *viewport width*, which
is itself a proxy for the thing that actually matters: how much space is
available to this specific component right now. A component embedded in
a sidebar, a split view, or a floating panel can be "mobile-narrow" on a
1440px-wide desktop monitor. Container queries make this literal;
even without them, design breakpoints as content/layout inflection
points ("this grid drops from 3 columns to 2 at X"), not as a mapping to
"phone / tablet / desktop." See `knowledge/responsive-design.md`.

## 19. Familiar interaction patterns should only be replaced when the alternative is meaningfully better

Every deviation from convention (custom scrollbars, a non-standard form
control, a novel navigation gesture) costs users a moment of
recalibration and costs the product a support/onboarding burden. That
cost is worth paying only when the replacement produces a *measurable*
improvement in speed, error rate, or capability — not merely a
different or "more interesting" look. Default to the boring, correct,
well-understood pattern; deviate deliberately and rarely.

## 20. Trends are tools, not design objectives

A visual trend (glassmorphism, a specific gradient palette, a particular
corner-radius fashion) is a tool that may or may not fit a given
product's personality, content, and constraints — it is never itself the
goal. "Make it look like [trendy product]" is a category error: the
goal is always a specific outcome for a specific user, and a trend is
adopted only when it serves that outcome. See
`research/emerging-patterns.md` for what's currently trending versus
durable, and `knowledge/anti-patterns.md` for how trend-chasing produces
generic AI-style UI.

---

## Amendments (added during research, not in the seed list)

### 21. Generic AI-generated UI is a specific, recognizable failure mode — not bad luck

Certain patterns (see `knowledge/anti-patterns.md`) recur across
AI-generated interfaces specifically because they are locally-plausible,
individually-defensible defaults that a system without genuine product
context reaches for repeatedly: centered hero + 3-icon-grid landing
pages, cards-for-everything, purple/blue gradients, and generic stat
tiles. Recognizing "this looks AI-generated" is itself a design skill —
it means recognizing *decisions that were made without reference to this
specific product's content and hierarchy*. The fix is always the same:
go back to the actual content and the actual information priority for
*this* product, not a different visual style.
**Confidence: ESTABLISHED** (based on cross-referencing multiple design
critiques of AI tooling output — see research log).

### 22. A design decision is only as good as the product understanding behind it

This constitution and the rest of the knowledge base describe *how* to
reason about hierarchy, density, motion, and composition — but every one
of those reasoning chains starts from an honest answer to "what is this
product, who uses it, what are they trying to do, how often." Skipping
that analysis (see `workflows/analyze-product.md`) and jumping to
component selection is the single most common route to a generic or
wrong design, regardless of how sophisticated the component-level
knowledge is.
