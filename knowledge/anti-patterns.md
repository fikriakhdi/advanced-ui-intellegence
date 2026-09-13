---
title: Anti-Patterns — Generic and AI-Generated UI Failure Modes
confidence-note: Synthesis grounded in real 2024–2026 design criticism of AI-generated interfaces (see Sources), cross-referenced with long-standing UI-design literature (card UI pitfalls, hierarchy theory) predating the AI-tooling wave. Per-item confidence is noted inline where it varies from the default (ESTABLISHED).
last_updated: 2026-09-12
---

# Anti-Patterns

This file exists because Constitution #21 says recognizing "this looks
AI-generated" is itself a design skill, and the project owner has flagged
this file as the most load-bearing one in the knowledge base. **Do not
soften these judgments.** A design that matches one of these patterns is
not automatically bad — several entries below have a genuine "when it's
appropriate" case — but every instance must be justified against *this
product's* content and hierarchy, never adopted by default. If asked to
critique generated UI, this file's verdicts are correctly harsh; praising
work that matches these patterns without the justification present is a
critique failure, not tact.

Each entry uses: **PATTERN / WHY AI GENERATES IT / WHY IT IS WEAK / WHEN IT
MAY ACTUALLY BE APPROPRIATE / HOW TO IMPROVE IT.**

---

## 1. Card inside card inside card

**PATTERN.** A page background is a card. Inside it, a section is a card.
Inside that, each row or stat is its own card. Three or more nested
bordered/shadowed/radius'd containers stacked inside each other, each
adding its own padding.

**WHY AI GENERATES IT.** A card is a locally-safe, self-contained unit
that "looks structured" without requiring the model to reason about the
actual relationship between the content pieces. Nesting is the path of
least resistance when a generator is composing a page bottom-up from
components rather than top-down from an information architecture:
each new group of content gets wrapped because wrapping is the default
"I don't know what else to do with this" move.

**WHY IT IS WEAK.** Every nested border/shadow is a hierarchy signal
(Constitution #7), and stacking them communicates "these are increasingly
separate/important things" when the actual relationship is usually the
opposite — the inner content is *part of* the outer content, not
independent of it. Padding compounds: by the third nesting level a
significant fraction of the viewport is dead space spent on chrome, not
content (see daverupert.com's "pitfalls of card UIs" — once one thing has
a border, everything else demands one too, in a hierarchy arms race that
dilutes all of them). On mobile this is worse: double/triple padding eats
horizontal space that was already scarce.

**WHEN IT MAY ACTUALLY BE APPROPRIATE.** Two levels can be legitimate when
the outer container is a true layout region (a page, a modal, a sidebar
panel) and the inner elements are genuinely discrete, comparable units
(a list of product cards inside a "results" panel). It stops being
appropriate the moment the inner card has no sibling to be compared
against, or the outer wrapper contains only one inner card.

**HOW TO IMPROVE IT.** Collapse to the minimum nesting that still signals
real groupings. Replace inner "cards" that aren't independently
actionable/comparable with typographic and spatial grouping (heading +
spacing, a hairline divider, indentation) per Constitution #5 and #6. Ask,
for each nested boundary: if I deleted this border, would the reader lose
information about what belongs together? If not, delete it.

---

## 2. Excessive rounded containers

**PATTERN.** Every container — button, input, card, modal, tag, avatar
frame, image thumbnail — uses the same large border-radius (commonly
12–24px), applied uniformly regardless of the container's size, function,
or content.

**WHY AI GENERATES IT.** Large radii read as "modern software" in the
training distribution (post-2020 SaaS and mobile-app defaults, plus
default component-library values like shadcn/ui's rounded-lg/xl); a
uniform radius token is also the easiest value to reuse without any
per-component reasoning.

**WHY IT IS WEAK.** Radius should scale with element size and signal
function, not apply as a flat aesthetic coat. A 24px radius on a 32px-tall
button starts to look like a pill (see #3) with no chin; the same 24px on
a 600px-wide card looks conservative. Applied identically everywhere, it
also erases a hierarchy signal: sharper corners for structural/dense
elements (tables, toolbars) versus softer corners for
inviting/content-forward elements (marketing cards, onboarding) is a real
tool that gets thrown away when everything gets one value.

**WHEN IT MAY ACTUALLY BE APPROPRIATE.** A consistent radius scale (e.g.
4/8/12/16px tied to element size) is good practice and part of a coherent
design system (Constitution #13). The anti-pattern is a *single* radius
value applied with no scale logic, not radius itself.

**HOW TO IMPROVE IT.** Define a radius scale proportional to control
height (roughly 15–25% of the shorter dimension is a reasonable heuristic
for controls; large surfaces can use a fixed larger token). Let brand
personality pick a point on a spectrum (sharp = precise/technical,
e.g. Linear's tighter radii; soft = friendly/consumer, e.g. many
mobile-social apps) deliberately, once, and apply it as a scale — not a
single magic-number reflex.

---

## 3. Excessive pill-shaped elements

**PATTERN.** Buttons, tags, filter chips, nav items, and badges are all
fully rounded (radius = 50% of height), producing a page dominated by
capsule shapes.

**WHY AI GENERATES IT.** Pills read as "friendly" and are a very common
default in component libraries and marketing-site templates; a model
optimizing for "looks like good modern design" over-samples this because
it's heavily represented in landing-page training data specifically.

**WHY IT IS WEAK.** Pills lose their signaling value when overused —
their actual job is to mark something as a small, discrete, often
removable/selectable token (a filter, a tag, a status). When every button
and nav item is also a pill, the visual language can no longer
distinguish "this is a tag" from "this is a primary action," which
directly conflicts with Constitution #4 and #8 (differentiated visual
weight per role). A page of all-pill shapes also reads as visually
"soft"/low-stakes even for content that should feel structured or
authoritative (financial dashboards, admin tools).

**WHEN IT MAY ACTUALLY BE APPROPRIATE.** Pills are the *correct* shape for
their canonical uses: filter/tag chips, status indicators, toggle
segments, small counts. A pill-shaped primary CTA button is a legitimate
brand choice (many consumer apps use it) as long as it's not the *only*
shape in the system.

**HOW TO IMPROVE IT.** Reserve full-round pills for tokens/tags/status
and toggle controls; give structural actions (primary/secondary buttons,
nav items) a shape from the radius scale in #2 instead, so pill-vs-not
itself becomes a hierarchy/role signal rather than decoration.

---

## 4. Random or unmotivated gradients

**PATTERN.** A purple-to-blue (or purple-to-cyan, or pink-to-orange)
gradient applied to hero backgrounds, button fills, icon badges, or text,
with no connection to brand color, content, or state — "AI-generated
purple gradient" is now specific enough to be a named, recognized meme in
design criticism circles.

**WHY AI GENERATES IT.** Gradients (particularly purple/blue/violet
ranges) are heavily represented in 2021–2024 SaaS marketing sites, the
exact era most heavily scraped for training data, and gradients are also
a cheap way to make a flat, undifferentiated layout look "designed"
without requiring any actual content-driven decision — multiple 2026
critiques (Smoothui, Impeccable.style's "missing design vocabulary for
agents," prg.sh's "Why Your AI Keeps Building the Same Purple Gradient
Website") converge on this exact pattern as the single most-cited AI
design tell.

**WHY IT IS WEAK.** A gradient with no relationship to the brand palette
or the content it decorates carries zero information — it's pure
decoration wearing a "modern design" costume. Because it's now so
strongly associated with generated/templated output, it actively signals
"nobody made a deliberate choice here" to a design-literate viewer, which
is the opposite of the intended effect. It also frequently fails contrast
requirements at the point where it transitions (text over the darker end
of the gradient passes, text over the lighter end doesn't).

**WHEN IT MAY ACTUALLY BE APPROPRIATE.** A gradient that is derived from
the actual brand palette (its two color-role tokens, not an arbitrary
purple/blue pair), used sparingly (one hero, one accent), and checked for
contrast across its full range is a legitimate device — see Stripe's or
Linear's occasional restrained gradient use tied to their own specific
brand hues, not a generic default.

**HOW TO IMPROVE IT.** Derive any gradient from the product's actual
defined palette (Constitution #7's "color reinforces, doesn't create,
hierarchy" applies here too — a gradient should reinforce something,
e.g. depth on a hero illustration, not stand alone as content). If the
product has no strong color identity yet, that's the actual problem to
solve — don't paper over an undefined palette with a decorative gradient.

---

## 5. Giant hero text without purpose

**PATTERN.** A landing page opens with a very large (often 64–96px+)
headline, frequently vague marketing copy ("Scale without limits," "The
future of X," "Your all-in-one platform for Y"), sitting alone in a tall,
mostly empty viewport-height section.

**WHY AI GENERATES IT.** "Big centered headline + subhead + CTA" is the
statistically dominant opening pattern across SaaS marketing sites, so
it's the default a generator reaches for absent specific product content;
vague copy fills the slot when the model doesn't have (or isn't given)
a real, specific value proposition to put there.

**WHY IT IS WEAK.** Scale should communicate importance (Constitution #7),
but importance is relative — a headline that's huge because "hero
headlines are huge" rather than because this message is more important
than everything else on the page is decoration mimicking hierarchy. Vague
copy paired with huge type is worse than vague copy at normal size: it
maximizes the screen real estate spent on saying nothing specific to
*this* product, actively pushing the specific, differentiating
information further down or off the fold entirely. Mania Design and
SmoothUI's slop critiques both single out generic headlines as a direct
symptom of skipped product understanding (Constitution #22).

**WHEN IT MAY ACTUALLY BE APPROPRIATE.** Large type is warranted when the
single message really is the most important thing on the page and is
specific — a product launch's actual name and positioning, a number that
matters (pricing, a metric users care about), or a brand moment where
personality (Constitution #17) is deliberately being spent. The test:
would a person who reads only this line understand something true and
specific about the product?

**HOW TO IMPROVE IT.** Write the headline last, after deciding what this
specific product's single most important claim is; if that claim can't be
made in one specific, falsifiable sentence, the copy problem is upstream
of the type-size problem. Size the type to the actual weight the message
deserves relative to the rest of the page, not to a template default.

---

## 6. Meaningless statistics

**PATTERN.** Dashboards or landing pages display large numbers ("10,000+
users," "99.9% uptime," "2.5x faster") with no baseline, comparison,
trend, or action attached — a stat tile that is purely decorative.

**WHY AI GENERATES IT.** Numeric social proof is a known trust-building
device (real products do use it well), so it's over-represented in
training data as "things good landing pages have"; a generator reproduces
the *form* (big number + small label) without the underlying requirement
that the number mean something to the specific viewer.

**WHY IT IS WEAK.** A stat with no context fails to answer the two
questions a number-on-a-screen must answer: *compared to what* (a
baseline, a prior period, a competitor, a target) and *so what* (what
should I do differently knowing this). "10,000+ users" tells a visitor
nothing actionable and nothing comparative — it's a shape that resembles
evidence without being evidence. On dashboards this is worse because it's
functional UI, not marketing: a KPI tile with no trend arrow, no
comparison period, and no drill-down is decoration dressed as data (see
also #21, decorative charts).

**WHEN IT MAY ACTUALLY BE APPROPRIATE.** A bare number is fine when the
number is inherently self-explanatory and the actionability is genuinely
not needed at that moment (a small "247 people are viewing this listing"
urgency signal on a marketplace, where the *comparison* is implicit —
more is more urgent — and no action beyond "decide faster" is expected).

**HOW TO IMPROVE IT.** For dashboards: always pair a value with a
comparison (vs. last period, vs. target, vs. peer) and, where relevant, a
clear next action. For marketing stats: replace vanity metrics with
specific, falsifiable, comparison-bearing claims ("40% fewer support
tickets after 30 days" beats "trusted by thousands").

---

## 7. Excessive dashboard cards ("wall of cards")

**PATTERN.** A dashboard composed entirely of a uniform grid of
equally-sized bordered cards — 6, 9, or 12 of them — each holding one
metric, chart, or list, with no visual distinction between a critical
metric and a minor one.

**WHY AI GENERATES IT.** A grid of cards is the easiest way to lay out an
unordered bag of "things the dashboard should show" without doing the
harder work of ranking them; it also mirrors the literal training-data
prevalence of dashboard-template screenshots and admin-panel kits, which
are built to demo *breadth* of components, not a specific product's
priorities.

**WHY IT IS WEAK.** This is Constitution #4 in its purest form: if nine
cards all have the same size, border, shadow, and padding, none of them
reads as more important, so the user must do the prioritization work that
the dashboard should have done for them. It also directly ignores
Constitution #11 (density should reflect the user's frequency and
expertise) — a wall of identically-weighted cards is neither low-density
enough for a novice (too much simultaneous information, no guidance on
where to look) nor high-density enough for an expert (too much wasted
chrome for the actual data-per-pixel).

**WHEN IT MAY ACTUALLY BE APPROPRIATE.** A genuinely flat set of
comparable, independent metrics that a user scans rather than reads in
order (a status-monitoring wall where every service is equally
important until one fails) can legitimately be a uniform grid.

**HOW TO IMPROVE IT.** Rank the content before laying it out: one
hero metric or chart gets primary size/position, secondary metrics get a
denser row or list treatment, tertiary detail gets progressive disclosure
(Constitution #16). Vary size and treatment to match actual importance,
not grid-fill convenience.

---

## 8. Repeated icon + title + description grids ("3 feature cards")

**PATTERN.** A landing-page section with 3 (or 4, or 6) identical cards,
each containing a centered icon in a colored circle, a bold short title,
and one line of description — used to list "features" with no
differentiation in visual treatment between them.

**WHY AI GENERATES IT.** This is possibly *the* most common SaaS
landing-page section shape in the training distribution, so much so that
critiques of AI-generated design (see Sources) name it specifically as a
tell; it's also trivially generatable from a bulleted feature list without
requiring any judgment about which feature matters most.

**WHY IT IS WEAK.** By construction, this layout gives every feature
identical visual weight (Constitution #4), which is almost never true —
products have a hero capability and supporting ones. Icons chosen for
abstract concepts ("collaboration," "security," "speed") tend toward
generic stock-icon meaning (a shield, a lightning bolt, two overlapping
circles) that carries no product-specific information — the icon becomes
decoration that must be read past, not through. Three or six of these in
a row also produce a repetitive scanning rhythm that discourages reading
any single one closely.

**WHEN IT MAY ACTUALLY BE APPROPRIATE.** A true feature-parity comparison
(e.g., "here are the three plan tiers and what each includes," or a
genuinely flat set of integrations/capabilities where no ranking exists)
can legitimately use a repeated-card grid — the pattern is wrong when
applied to content that actually has a hierarchy, not wrong in principle.

**HOW TO IMPROVE IT.** Decide whether the features actually have a
hierarchy; if so, give the primary one a distinct, larger treatment
(often with a real screenshot or interactive demo instead of an icon) and
demote the rest to a lighter list or smaller secondary grid. Replace
generic stock icons with product-specific visuals (actual UI, a real
diagram) wherever possible.

---

## 9. Unnecessary badges

**PATTERN.** Small colored labels ("New," "Beta," "Pro," "Popular," "AI")
attached to features, plans, or nav items pervasively, often on content
where the label adds no decision-relevant information.

**WHY AI GENERATES IT.** Badges are a compact way to imply status,
recency, or tiering — all popular in SaaS pricing pages and feature lists
in the training data — and require no layout reasoning: they drop onto
existing content without restructuring anything, an especially seductive
low-effort way to introduce "importance" and "the most important
element" without doing the real hierarchy work in Constitution #1 and #4.

**WHY IT IS WEAK.** Constitution #4 again: if every third element has a
badge, badges stop reading as exceptional and become wallpaper. An
"AI" badge stapled onto a feature not because AI does something the user
needs to know about but because it's currently a marketable word is a
badge that misrepresents priority. Badges also frequently duplicate
information already conveyed by position or grouping (a "Popular" badge
on the plan that's already visually emphasized by being in the middle
column and taller).

**WHEN IT MAY ACTUALLY BE APPROPRIATE.** A badge earns its place when it
changes a decision the user is about to make and that information isn't
available elsewhere on screen — "Beta" warning of instability before
someone adopts a feature for production use, or a genuine "New" on the
one thing that shipped this week in a nav a returning user scans daily.

**HOW TO IMPROVE IT.** Before adding a badge, ask what decision it
changes and for whom; if the answer is "it makes this look more
exciting," cut it. Cap simultaneous badges so the ones that remain still
mean something.

---

## 10. Every section centered

**PATTERN.** Every block on a page — headline, body copy, feature
sections, forms, even data tables — is horizontally centered, producing a
narrow, symmetrical column running down the middle of a much wider
viewport.

**WHY AI GENERATES IT.** Center alignment is the "safe," symmetrical
default that requires no layout decision — it looks intentional even when
no intention was applied, and it's extremely common in the marketing-page
segment of training data (where hero-centric centered layouts genuinely
are common and correct).

**WHY IT IS WEAK.** Constitution #7 notes that position carries hierarchy
information (left/top reads first in LTR, center reads as "the
protagonist"). If *everything* is centered, "centered" stops meaning
"this is the protagonist" — it's used up on content that isn't. Centered
body text specifically hurts readability at wide measure (line lengths
vary line to line with no consistent left edge for the eye to return to),
and centered data/tabular content actively fights scanability (F-pattern
reading, per the card-UI critique) since numbers and labels no longer
align in a column.

**WHEN IT MAY ACTUALLY BE APPROPRIATE.** True hero moments (a landing
page's opening statement, an empty state, a confirmation screen, a modal
with a single decision) are legitimately centered — the content *is* the
protagonist of that screen. Short-line marketing copy (one to two lines)
also centers fine.

**HOW TO IMPROVE IT.** Reserve centering for genuine single-focus moments;
default dense/informational/tabular content to left alignment with a
strong, consistent left edge, and let asymmetric layouts (content left,
supporting visual right, or a sidebar + main split) carry hierarchy that
centering can't.

---

## 11. Excessive whitespace

**PATTERN.** Generous padding and margins applied so uniformly and
heavily that related content is pushed apart until relationships between
elements become ambiguous, or a data-dense professional tool is given the
same airy spacing as a consumer landing page, forcing excessive
scrolling/paging to see what an expert user needs to see at a glance.

**WHY AI GENERATES IT.** "More whitespace = more premium/modern" is a
widely reproduced heuristic in template and marketing-site training data,
and it's applied as a flat multiplier rather than as a proximity tool,
because proximity-based spacing (Constitution #5) requires knowing which
elements are actually related — a judgment a generator without product
context can't make, so it defaults to "generous everywhere."

**WHY IT IS WEAK.** Whitespace's actual job is to communicate grouping
through proximity (Gestalt proximity principle) — elements close together
read as related, far apart as unrelated. Uniformly large gaps flatten
this signal exactly like uniformly small gaps do: if the space between a
label and its value is the same as the space between two unrelated
sections, proximity stops communicating structure. This directly
violates Constitution #11 for expert/high-frequency tools: a trading
terminal or admin table padded like a marketing page forces scrolling and
reduces throughput for a user whose actual cost is speed, not comprehension.

**WHEN IT MAY ACTUALLY BE APPROPRIATE.** Generous whitespace is correct
for low-frequency, high-comprehension-cost contexts — onboarding, a
pricing page, an empty state, a single-decision screen — where the user's
real cost is understanding, not throughput (Constitution #11).

**HOW TO IMPROVE IT.** Set spacing based on relationship strength, not a
flat brand value: tight for tightly related pairs (label/value,
icon/text), progressively looser for section boundaries. Match the
overall density to the user profile from Constitution #11 before choosing
a spacing scale, not after.

---

## 12. Insufficient whitespace

**PATTERN.** Content packed edge-to-edge with minimal margin, inconsistent
or nonexistent gaps between unrelated elements, and no breathing room
around focal content — the inverse failure of #11.

**WHY AI GENERATES IT.** This shows up less from "AI slop" specifically
(which trends toward the opposite, #11) and more from naive
information-maximizing generation — when asked to "fit everything in" or
working from a content-first without-layout-pass approach, filling
available space with content reads as thoroughness.

**WHY IT IS WEAK.** Without margin between unrelated elements, proximity
grouping (see #11) collapses the other direction: everything appears
related because nothing is separated, so the eye can't segment the page
into scannable chunks. It also frequently produces failing touch-target
spacing on mobile (adjacent interactive elements too close together to
tap reliably) — an accessibility problem per Constitution #14, not merely
an aesthetic one.

**WHEN IT MAY ACTUALLY BE APPROPRIATE.** Genuinely dense professional
tools for expert/high-frequency users (spreadsheets, trading terminals,
log viewers, IDEs) legitimately minimize whitespace to maximize
information-per-viewport, per Constitution #11 — the key difference from
this anti-pattern is that a well-built dense UI still uses *consistent,
deliberate* small gaps and alignment, not an absence of spacing logic.

**HOW TO IMPROVE IT.** Apply a spacing scale even at high density — small
values are still values, applied consistently and tied to relationship
strength (see #11) — rather than omitting spacing altogether. Check touch
targets against platform minimums regardless of density.

---

## 13. Weak information hierarchy

**PATTERN.** Multiple headings at the same size/weight, or body text and
labels indistinguishable in weight from headers; no clear "read this
first" path; several elements compete for the first glance.

**WHY AI GENERATES IT.** Establishing real hierarchy requires ranking
content by the user's actual goal (Constitution #1, #22), which needs
product/user context a generator often lacks or isn't given; absent that
ranking, a flat, evenly-weighted type scale is the "safe" default that
never looks obviously wrong in isolation, only in context.

**WHY IT IS WEAK.** This is the direct violation of Constitution #1 and
#7: hierarchy must exist in the underlying structure before styling can
express it, and typography/spacing/position/scale are the primary tools
for expressing it. A screen that fails the "desaturate it" test (does
hierarchy survive without color) usually also fails a "squint at it" test
— if everything is roughly the same size and weight, squinting shows a
uniform gray field with no clear entry point. See `knowledge/visual-hierarchy.md`
for the full hierarchy toolkit this anti-pattern violates.

**WHEN IT MAY ACTUALLY BE APPROPRIATE.** Rarely — flat hierarchy is
appropriate only for content that is genuinely non-sequential and equally
weighted for the user's task (e.g., a simple alphabetical index/glossary),
never for anything the user is meant to scan in priority order.

**HOW TO IMPROVE IT.** Do the ranking exercise explicitly before touching
type: list the content, rank it by what the user needs first/second/third
for their actual current goal, then map that rank to a type/weight/
position scale. Run the desaturation and squint tests on the result.

---

## 14. Random floating decorative elements

**PATTERN.** Abstract blobs, dots, grid patterns, or geometric shapes
scattered across a hero or background with no relationship to the content
they surround, often semi-transparent and positioned "artistically" in
corners.

**WHY AI GENERATES IT.** Decorative background elements are extremely
common in template/marketing-site training data as a way to make an
otherwise plain section feel "designed"; they're also low-risk to
generate because they don't need to be correct about anything — they
carry no information, so they can't be wrong.

**WHY IT IS WEAK.** Every visible element should justify its existence
(Constitution #9) — decorative shapes with no tie to content or brand
mark fail that test outright: they exist purely because "sections need
visual interest," which is not a justification. They add scanning cost
(the eye must process and discount them) and rendering/maintenance cost
for zero informational or brand payoff, and at their most generic
(gradient blobs, dot grids) they're now recognizable specifically as
AI/template filler rather than as intentional design.

**WHEN IT MAY ACTUALLY BE APPROPRIATE.** Decorative elements that are
actually brand assets (a consistent illustration style, a signature
pattern that recurs across the product as identity, not filler) or that
support a specific storytelling beat (an illustration reinforcing what an
empty state literally means) earn their place — the test is whether
removing the element loses something specific, not just "visual
interest" in the abstract.

**HOW TO IMPROVE IT.** Cut decoration that fails the "does removing this
lose something specific" test. Where visual interest is genuinely needed,
prefer content-derived decoration (a real product screenshot, real data,
an on-brand illustration system) over generic abstract shapes.

---

## 15. Inconsistent border radius

**PATTERN.** Different corner radii used arbitrarily across similar
components — one card at 8px, another at 16px, buttons at 6px, modals at
20px — with no evident system or rationale.

**WHY AI GENERATES IT.** Without a defined token system, each component
is generated somewhat independently (especially across multiple
generation passes/prompts), and each pass picks a plausible-looking value
in isolation rather than referencing a shared scale.

**WHY IT IS WEAK.** Inconsistency here isn't expressive (unlike varying
radius by size/function deliberately, see #2) — it reads as sloppiness
because there's no logic a user could infer. It undermines Constitution
#13's requirement that components behave/appear consistently at the
structural level, and it's one of the fastest tells of ungoverned,
un-systemized output because real design systems fix this in a token
file on day one.

**WHEN IT MAY ACTUALLY BE APPROPRIATE.** Never, as *inconsistency* — the
closest legitimate cousin is a deliberate radius *scale* (see #2), which
looks similar at a glance but is systematic and repeatable, not arbitrary.

**HOW TO IMPROVE IT.** Define 2–4 radius tokens tied to a rule (by
component size or role) and audit every container against that scale;
replace one-off values with the nearest token.

---

## 16. Excessive shadows

**PATTERN.** Heavy, dark, or multiple stacked box-shadows applied to
many/most elements — cards, buttons, inputs, even text — producing a page
that looks like it's covered in sticky notes floating above the
background.

**WHY AI GENERATES IT.** Shadow is a cheap, purely additive way to imply
depth and "elevation" (a real, meaningful concept in some design
languages, e.g. Material) without needing to reason about which elements
actually need to appear elevated (a genuinely floating modal, a dragged
item) versus which are flat, in-flow content.

**WHY IT IS WEAK.** Elevation is a hierarchy/state signal (this thing is
temporarily above the normal flow: a dropdown, a modal, a dragged card,
a hover state) — applying it permanently and everywhere removes its
ability to signal that, exactly like #2 and #3's overuse problem. Heavy
shadows also add visual noise and, stacked with borders and radius (see
#17), compound rendering weight and cognitive noise together.

**WHEN IT MAY ACTUALLY BE APPROPRIATE.** Real elevation states — an open
dropdown, a modal over a scrim, a dragged item, a hover-lift on an
actionable card — legitimately use shadow, ideally with a restrained scale
tied to actual z-index/elevation level, not a flat default.

**HOW TO IMPROVE IT.** Reserve shadow for elements that are actually
elevated above the base layer; use a small elevation scale (e.g. 2–3
levels) instead of a single heavy default; let static, in-flow content be
flat.

---

## 17. Too many borders

**PATTERN.** Every card, section, input, and table cell has a visible
1px border, often in combination with a background-color change and a
shadow — three separation techniques used simultaneously where any one
would do.

**WHY AI GENERATES IT.** Borders are the most explicit, "obviously
correct-looking" way to show a boundary, so absent a considered spacing/
grouping strategy, a generator reaches for the strongest tool available
by default rather than the lightest tool sufficient for the job.

**WHY IT IS WEAK.** This is a direct instance of Constitution #5's
guidance inverted: reach for a border only when whitespace/alignment
can't do the job. When everything has a border, the eye can no longer use
"has a border" as a signal for "this is a distinct region" (again, the
"hierarchy arms race" the card-UI critique describes) — and combining
border + fill + shadow on the same edge is visually redundant, tripling
the rendering cost of a signal that a single technique already conveyed.

**WHEN IT MAY ACTUALLY BE APPROPRIATE.** Borders earn their place in
genuinely dense tabular/grid UI where whitespace alone can't disambiguate
adjacent cells (spreadsheets, comparison tables), or to distinguish
truly unrelated adjacent regions (a sidebar from a main content area).

**HOW TO IMPROVE IT.** Pick one separation technique per boundary
(whitespace, OR a border, OR a background shift) rather than layering
all three; reserve borders for the cases in Constitution #5.

---

## 18. Poor mobile adaptation (shrink, not recompose)

**PATTERN.** The mobile layout is the desktop layout scaled down: the same
multi-column grid squeezed into one narrow column, the same hover-
revealed controls now permanently hidden (because there's no hover),
the same dense toolbar wrapped into three overflowing rows, side-by-side
elements stacked in their original left-to-right visual order regardless
of priority.

**WHY AI GENERATES IT.** Responsive CSS (media queries, flex-wrap,
percentage widths) makes "technically responsive" trivial to produce
without redesigning anything, so a generator satisfies the letter of
"be responsive" by making elements not overflow, without doing the
redesign work of deciding what changes.

**WHY IT IS WEAK.** This is a direct, named violation of Constitution
#10: a desktop layout encodes cursor/hover/wide-viewport/low-interruption
assumptions that don't hold on mobile, and simply shrinking preserves
those wrong assumptions in the new context. Hover-only affordances become
undiscoverable and unreachable (there is no hover on touch), dense
multi-column data becomes horizontally scrollable or illegibly tiny, and
information priority that made sense at 1440px (a sidebar visible
alongside content) often needs to become sequential, collapsible, or
sheet-based at 375px.

**WHEN IT MAY ACTUALLY BE APPROPRIATE.** Never as an end state — the
constitution frames recomposition as mandatory. A *simple* single-column
content page (an article, a settings list) may end up looking like "the
desktop layout, narrower" simply because a single column was already the
right structure at both widths — that's convergence, not the anti-pattern,
as long as it was arrived at by evaluating the content, not by skipping
the evaluation.

**HOW TO IMPROVE IT.** For every element, explicitly decide: keep, move,
collapse, hide, merge, become scrollable, become a sheet, or become a
different component (Constitution #10's own list) — do this per element,
not as one blanket "make it fit" pass. Treat hover-dependent affordances
as needing a touch-visible alternative from the start, not a mobile
afterthought.

---

## 19. Arbitrary or ungrounded color choices

**PATTERN.** Colors picked because they "look nice together" in isolation
— a palette with no defined semantic roles (this blue always means
"primary action," this red always means "destructive"), so the same hue
appears for unrelated purposes across the product, or semantically loaded
colors (red, green) are used decoratively.

**WHY AI GENERATES IT.** Generating a plausible-looking palette (a
primary hue plus a few accents) is easy; defining and *consistently
enforcing* a semantic role system for that palette across an entire
product requires state that a single generation pass, or a series of
disconnected prompts, typically doesn't maintain.

**WHY IT IS WEAK.** Constitution #7 places color last in the hierarchy
toolkit specifically because it's the least robust signal — and an
ungrounded palette squanders even that limited robustness by making color
carry contradictory meanings (if the brand's decorative accent color is
also used for a "success" state and a "new" badge, color stops helping
users predict anything). Red/green used decoratively is worse: it borrows
danger/success connotations users can't turn off, creating false alarms
or false reassurance.

**WHEN IT MAY ACTUALLY BE APPROPRIATE.** A rich decorative palette is
fine in purely illustrative/brand contexts (marketing imagery, editorial
content) that don't carry functional/semantic weight — the anti-pattern
is specifically ungrounded color in functional UI where users infer
meaning from it.

**HOW TO IMPROVE IT.** Define semantic roles first (primary action,
destructive, success, warning, informational, disabled, plus brand
neutrals) and derive the decorative palette from what's left over, never
the reverse. Audit for accidental reuse of semantic hues in non-semantic
contexts.

---

## 20. Fake complexity (UI that looks sophisticated but isn't functional)

**PATTERN.** Elaborate visual treatment — multi-panel layouts, dense
iconography, layered depth, animated transitions, settings-looking
toggles — applied to a product whose actual functional surface is much
simpler, so the interface *looks* like an advanced professional tool
without the underlying feature depth to justify it.

**WHY AI GENERATES IT.** "Sophisticated-looking" dashboards and admin
panels are heavily represented in template libraries (they demo well),
so a generator without a real functional spec to work from over-produces
visual complexity as a proxy for perceived quality/premium-ness.

**WHY IT IS WEAK.** This is Constitution #9 violated at the level of an
entire interface, not just one element: complexity that doesn't map to
real functionality costs the user comprehension and orientation effort
for capabilities that don't exist, and it erodes trust the moment they
discover the sophistication is cosmetic (a settings panel with toggles
that don't do anything meaningfully different, a multi-step wizard for a
task that needed one field). It's also a growth liability: every
cosmetically-implied capability becomes a support ticket or a broken
expectation.

**WHEN IT MAY ACTUALLY BE APPROPRIATE.** Deliberately "growing into"
visual sophistication ahead of feature parity can be a legitimate
staged-rollout strategy (the shell communicates a roadmap) — but only
when disclosed as such (a visible "coming soon," a disabled state with an
honest label), never presented as fully functional.

**HOW TO IMPROVE IT.** Match visual complexity to actual functional depth;
where the product is simple, let the UI be simple and confident about it
rather than padding it with cosmetic sophistication. Where advanced
capability really exists, verify every visual affordance actually does
something before shipping it.

---

## 21. Decorative charts

**PATTERN.** Charts and sparklines included for visual interest — a
smooth wavy line, a bar chart, a donut — where the underlying data is
either not real, not labeled, not readable at the rendered size, or not
tied to any decision the viewer needs to make.

**WHY AI GENERATES IT.** A small chart or sparkline strongly signals
"data product" / "analytics" in training data (dashboard templates in
particular are full of them), so it gets added as a genre marker even
when no specific, real, decision-relevant series exists to plot.

**WHY IT IS WEAK.** Same failure as #6 (meaningless statistics) applied
to visualization: a chart is a hierarchy and comprehension tool, not a
decoration, and an unlabeled or illegibly-scaled chart fails both the
"compared to what" and "so what" tests. It's worse than a bare number in
one respect — it implies more analytical rigor than a number does, so
its emptiness is a bigger trust violation when discovered (a smooth,
pretty sparkline the user later realizes never had real axis values).
See `knowledge/data-visualization.md` if present for chart-selection and
labeling principles this violates.

**WHEN IT MAY ACTUALLY BE APPROPRIATE.** A genuinely decorative,
explicitly non-data illustration that merely evokes "movement" or
"activity" (e.g., an abstract waveform on a music app's marketing page)
is fine *as long as it's not placed where users would reasonably expect
it to represent their real data* — the failure is specifically decorative
charts masquerading as functional data in a data-bearing context.

**HOW TO IMPROVE IT.** Every chart in a functional (non-marketing)
context needs real data, a labeled axis or clear unit, and an answer to
what decision it supports; if none of those exist yet, ship the metric as
plain labeled text instead of a chart, or wait until real data exists.

---

## 22. Generic SaaS landing-page template applied without regard to the product

**PATTERN.** The entire page follows: hero (headline + subhead + CTA +
screenshot) → logo bar ("trusted by") → 3-feature-icon grid (#8) →
testimonial carousel → pricing table (3 tiers, middle one "Most Popular")
→ FAQ accordion → footer — applied regardless of whether this product is
B2B or consumer, technical or non-technical, pre-launch or mature,
free or enterprise-sales-only.

**WHY AI GENERATES IT.** This exact sequence is the dominant structural
shape of SaaS marketing sites in training data, to the point that
multiple 2026 critiques treat it as the landing-page equivalent of the
purple gradient — the single most reliable "this was templated" tell.
It's attractive to generate because each section is independently
well-understood and composable without needing a specific go-to-market
story.

**WHY IT IS WEAK.** The template encodes assumptions (self-serve signup,
comparable competitors worth a feature-grid pitch, existing social proof
worth a logo bar, a pricing model that fits 3 flat tiers) that many
products don't actually have — an enterprise-sales, pre-revenue, or
highly technical product forcing its story into this shape ends up
saying less about itself than a bespoke structure would, exactly the
generic-because-untethered-from-content failure Constitution #21 and #22
describe. Applied uncritically, it also means every section gets the
generic sub-patterns above (#5, #6, #8) stacked together, compounding the
problem.

**WHEN IT MAY ACTUALLY BE APPROPRIATE.** The structure exists because it
genuinely fits a common case well: a self-serve product with real social
proof, comparable tiered pricing, and a feature set that breaks cleanly
into 3 headline capabilities. For that specific situation, this order of
information is a reasonable, well-tested default — the anti-pattern is
applying it *by default*, not the structure itself being wrong.

**HOW TO IMPROVE IT.** Start from Constitution #22: what is this product,
who buys it, how do they buy it, what do they need to believe before they
will act? Build the page's section order and content from those answers;
keep any section from the template only where it actually serves the
answer, cut or replace the rest (no logo bar without real logos to show;
no pricing table if sales is the only path to purchase; no testimonial
carousel with quotes that could apply to any product).

---

## 23. Blindly copying popular landing-page structures

**PATTERN.** Reproducing a specific admired product's page structure or
visual system (Linear's issue-tracker chrome, Stripe's docs typography,
Notion's minimalist marketing style) on an unrelated product because it's
well-regarded, rather than because its underlying rationale transfers.

**WHY AI GENERATES IT.** Well-known, widely-praised products are
over-represented in training data and in prompts themselves ("make it
look like Linear/Stripe/Notion"), so a generator (or a designer under
deadline pressure) reaches for a recognizable, proven-looking shape
instead of deriving one from the product at hand.

**WHY IT IS WEAK.** Every entry in `references/` in this knowledge base is
explicit that the *principle*, not the surface style, is what transfers —
copying Linear's dense, keyboard-first, monochrome aesthetic onto a
consumer product aimed at infrequent, non-technical users imports a
density and interaction model tuned for expert daily users (Constitution
#11) into a context where it will alienate its actual audience. This is
the direct violation Constitution #20 names: "make it look like [trendy
product]" is a category error because the goal is a specific outcome for
a specific user, not a resemblance to something admired.

**WHEN IT MAY ACTUALLY BE APPROPRIATE.** Adopting a *specific transferable
principle* (not the surface style) from an admired product, when that
product's context genuinely matches — e.g., a developer tool borrowing
Linear's command-palette-first navigation because its users are also
expert, high-frequency, keyboard-comfortable — is exactly how the
`references/` files in this knowledge base are meant to be used.

**HOW TO IMPROVE IT.** Whenever a reference product is invoked, name the
underlying principle and check its "when NOT to copy it" conditions
against the current product before reusing anything; if the match fails,
find a different principle or derive one from this product's own
constraints instead of the borrowed surface style.

---

## Sources

- [AI Design Slop: Why Every AI-Built Interface Looks the Same (And How to Fix It)](https://mohitphogat.medium.com/ai-design-slop-why-every-ai-built-interface-looks-the-same-and-how-to-fix-it-bf874e0b470c) — Medium, accessed 2026-09-12
- [AI Design Slop: Why AI-Generated UI Looks Generic — and the Fix](https://smoothui.dev/blog/ai-design-slop) — SmoothUI, accessed 2026-09-12
- [Spot the Slop: A UI Designer's Guide to Fixing AI Defaults](https://www.mania.design/blog/spot-the-slop-a-ui-designers-guide-to-fixing-ai-defaults/) — Mania Design, accessed 2026-09-12
- [AI-Generated UI Is Making Every Product Look the Same: Here's the Real Cost](https://www.drawbackwards.com/blog/ai-generated-ui-making-every-product-look-the-same) — Drawbackwards, accessed 2026-09-12
- [Why Your AI Keeps Building the Same Purple Gradient Website](https://prg.sh/ramblings/Why-Your-AI-Keeps-Building-the-Same-Purple-Gradient-Website) — prg.sh, accessed 2026-09-12
- [The missing design vocabulary for agents ("Spot the Slop")](https://impeccable.style/slop/) — impeccable.style, accessed 2026-09-12
- [AI Slop Fonts and Gradients: The Tells That Give Away AI Design](https://www.925studios.co/blog/ai-slop-design-tells) — 925 Studios, accessed 2026-09-12
- [avoid-ai-design (Claude Code skill for auditing AI design slop)](https://github.com/funboy322/avoid-ai-design) — GitHub, accessed 2026-09-12
- [Pitfalls of Card UIs](https://daverupert.com/2018/04/pitfalls-of-card-uis/) — Dave Rupert, accessed 2026-09-12
- [Why AI-Generated UI Looks Good But Often Feels Generic](https://medium.com/@cssamithpitigala/why-ai-generated-ui-looks-good-but-often-feels-generic-020a9b1b8492) — Medium, accessed 2026-09-12
- Internal: `knowledge/ui-principles-2026.md` (Constitution, especially #1, #4–#11, #20–#22) — this project, accessed 2026-09-12
