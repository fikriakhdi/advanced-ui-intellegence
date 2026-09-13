---
title: Navigation Patterns
confidence-note: Cross-referenced against Material Design 3, NN/g pagination/infinite-scroll research, and observed command-palette conventions in modern SaaS (Linear, Vercel, Raycast); tradeoff claims for pagination/infinite-scroll are ESTABLISHED empirical NN/g findings, command-palette-as-navigation is EMERGING/ESTABLISHED depending on product category.
last_updated: 2026-09-12
---

# Navigation Patterns

Navigation exists to answer three questions continuously: where am I, what
else is here, and how do I get back. Every pattern below is evaluated
against how well it answers those three questions for its context — this
operationalizes Constitution #1 (hierarchy) and #9 (justify every element)
specifically for wayfinding.

## Primary Navigation Models

**Top nav (horizontal bar).** Best for a small number of flat, top-level
sections, marketing sites, and content sites where navigation is
secondary to the page content itself. Scales poorly past roughly 5-7 top-
level items before it needs a "More" overflow or a mega-menu, at which
point it's worth asking whether the IA actually needs a different model.

**Side nav (vertical sidebar).** Best for applications with many sections,
nested/hierarchical structure, and desktop-width, persistent-session usage
(dashboards, admin panels, IDEs) — the vertical list scales to far more
items than a horizontal bar without crowding, and a sidebar's presence
doubles as a permanent "where am I in the whole app" map, satisfying the
orientation job better than a top bar can for deep IAs. Collapsible/
icon-only rail states are a legitimate density tradeoff (Constitution #11)
for expert users who've memorized icon meanings.

**Tab bar (bottom, mobile).** Best for 3-5 flat, frequently-used mobile
destinations — see `knowledge/mobile-patterns.md` for full treatment; it is
the recomposed form side-nav or top-nav typically takes at phone widths
(Constitution #10).

**DESIGN RULE.** Choose the model by the actual shape of the IA (flat vs.
deep, few vs. many sections) and the device/context, not by what looks
current — a side nav forced onto a 4-item flat site adds a permanent
sidebar's worth of visual weight for no orientation benefit a top nav
wouldn't already provide (Constitution #9).

## Hub-and-Spoke vs. Flat vs. Nested IA

- **Flat** — every section is a direct sibling, reachable in one navigation
  action from anywhere (via a persistent nav). Appropriate when sections
  are genuinely independent and roughly equal in importance/frequency of
  use (most consumer apps' top-level tabs).
- **Hub-and-spoke** — a central hub (a dashboard, a home screen) from which
  the user branches into a task, completes it, and returns to the hub
  before starting the next one. Appropriate when tasks are largely
  independent of each other and the user's mental model is genuinely
  "return to home base between tasks" (many admin tools, a settings area
  branching from a main app).
- **Nested/hierarchical** — sections contain subsections contain further
  subsections, requiring drill-down navigation (breadcrumbs, back
  buttons) rather than a flat jump. Appropriate when the content itself is
  genuinely hierarchical (a file system, a product catalog with
  categories/subcategories) — forcing hierarchy where the content isn't
  actually hierarchical just adds unnecessary navigation depth
  (Constitution #19's cost of deviating — here, the cost of imposing
  unwarranted structure).

**DESIGN RULE.** Match the model to the actual relationship between
sections — the most common mistake is defaulting to deep nesting for
content that's actually flat, adding clicks with no orientation benefit.

## Breadcrumbs

**PATTERN.** A horizontal trail showing the current page's position in a
hierarchy (Home > Category > Subcategory > Product), each segment a link
back up the chain.

**WHEN TO USE.** Genuinely hierarchical/nested IAs (see above) — ecommerce
category trees, documentation sites, deep admin drill-downs (dashboards'
drill-down paths, per `knowledge/dashboard-patterns.md`). Breadcrumbs solve
"where am I" and "how do I go back up (not just one step back)" better
than a browser back button, which only knows navigation history, not IA
structure.

**WHEN NOT TO USE.** Flat IAs with no real hierarchy (breadcrumbs with only
2 segments, "Home > Current Page," add clutter with no wayfinding value);
as the *sole* navigation mechanism (breadcrumbs supplement primary nav,
they don't replace it — a user landing directly on a deep page via search
still needs another way to explore laterally).

**DESIGN RULES.** Every segment except the current page is a link; the
current page segment is not a link (or is visually distinct/non-
interactive) since linking to the page you're already on is meaningless;
truncate long trails from the middle, not the end, preserving both the
root and the immediate context.

## Pagination vs. Infinite Scroll vs. Load-More

**ESTABLISHED**, direct NN/g findings.

- **Pagination (numbered pages).** Preserves a stable, returnable,
  bookmarkable position ("page 3 of 12"); lets users jump non-sequentially;
  gives a concrete sense of total content size. Best for goal-directed
  finding tasks where a user might want to return to a specific spot, or
  where reaching the footer/end of content matters (e-commerce category
  browsing where "did I see everything" matters, search results).
- **Infinite scroll.** Removes the interruption of a page load, well-suited
  to browsing/discovery feeds where there's no strong "I want item #340
  specifically" need and no natural, meaningful endpoint (social feeds,
  image discovery). NN/g's well-documented weaknesses: users lose their
  scroll position on navigating away and back, the footer becomes
  effectively unreachable, and users lose a sense of how much content
  exists or how much they've already seen.
- **Load-more (a button, not automatic).** A middle ground NN/g
  specifically recommends as an alternative to pure infinite scroll: gives
  the low-interruption benefit of scroll-continuation while keeping the
  action explicit (so position/footer aren't silently swallowed) and
  giving the user a natural pause point.

**DESIGN RULE.** Choose by task type: goal-directed/returnable tasks →
pagination; low-stakes discovery/browsing feeds → infinite scroll or
load-more; when in doubt, load-more is the safer middle default since it
keeps most of infinite scroll's benefit without its worst failure modes.
Whichever is chosen, preserve scroll position on back-navigation — this is
often a bigger real-world usability factor than the pattern choice itself.

## Search-as-Navigation

**ESTABLISHED.** For content-heavy or deep IAs, search is frequently faster
than browsing a nav tree, and should be treated as a first-class navigation
method, not a fallback for users who "couldn't find it in the menu."

**DESIGN RULES.** Make the search entry point visually prominent and
consistently placed (not buried); support partial/fuzzy matching and
show results progressively as the user types where feasible; surface
recent/popular queries when the field is empty (see
`knowledge/mobile-patterns.md` "Mobile Search" for the touch-specific
form); route search results through the same information hierarchy/context
cues as browsed content (breadcrumb or category label on each result) so
arriving via search doesn't strand the user without orientation.

## Command Palettes as a Navigation Pattern

**EMERGING → ESTABLISHED** in professional/SaaS tools as of 2026 (Linear,
Vercel, Raycast, Notion, most modern IDEs): a keyboard-triggered (commonly
Cmd/Ctrl+K), text-driven overlay that combines navigation ("go to
Settings"), search ("find this document"), and action invocation
("create new issue") in one interface.

**WHEN TO USE.** High-frequency, expert users (Constitution #11) in a
content- or feature-rich application, where memorized text commands
outpace mousing through a nav tree — the palette is fundamentally a
throughput tool for people who already roughly know what they want by
name.

**WHEN NOT TO USE.** Low-frequency consumer products or first-time-user
flows — a command palette assumes the user already has a mental model of
what commands/destinations exist, which a novice does not yet have; for
those users it's an invisible feature at best, not a replacement for
visible primary navigation (Constitution #19 discoverability).

**DESIGN RULES.** Always provide a visible trigger (a search icon/button
showing the shortcut hint) in addition to the keyboard shortcut — an
undiscoverable Cmd+K-only feature fails the majority of users who never
learn it exists; keep results categorized (Pages, Actions, Recent) so a
single flat list of mixed result types doesn't become its own hierarchy
problem; a command palette supplements, it does not replace, a visible
primary navigation structure.

## Wayfinding / Orientation Cues

**FOUNDATIONAL**, the connective thread across every pattern above.

**DESIGN RULES**
- Always indicate the current location within persistent navigation (an
  active/selected state on the current nav item) — a nav that doesn't
  show current position answers "what else is here" but not "where am I."
- Page titles/headings should match or closely echo the nav label the user
  clicked to get there — a mismatch (clicked "Billing," landed on a page
  titled "Payment Settings") creates a moment of doubt about whether
  navigation worked correctly.
- Provide a clear, consistent way back (a back button, a breadcrumb, a
  logo-to-home link) from every non-trivial depth — never strand a user on
  a page with only forward-moving links.

## Sources

- [Alternatives to Pagination on Product-Listing Pages – NN/g](https://www.nngroup.com/articles/alternatives-pagination-listing-pages/)
- [Infinite Scrolling: When to Use It, When to Avoid It – NN/g](https://www.nngroup.com/articles/infinite-scrolling-tips/)
- [3 Alternatives to Infinite Scrolling (Video) – NN/g](https://www.nngroup.com/videos/alternatives-to-infinite-scrolling/)
- [Navigation rail – Material Design 3](https://m3.material.io/components/navigation-rail/guidelines)
- [Designing a Command Palette | Destiner's notes](https://destiner.io/blog/post/designing-a-command-palette/)
- [Designing Effective Breadcrumbs Navigation — Smashing Magazine](https://www.smashingmagazine.com/2022/04/breadcrumbs-ux-design/)
