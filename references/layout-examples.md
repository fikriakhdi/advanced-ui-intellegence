---
title: Layout Examples — Specific Page & Screen Decisions
confidence-note: Grounded in observed product structure and published design write-ups (see Sources); layout details evolve — verify against the live product before treating specifics as current pixel-for-pixel fact.
last_updated: 2026-09-12
---

# Layout Examples (Specific Page/Screen Structures)

Every entry names a transferable structural principle, not a template to
clone. See `knowledge/anti-patterns.md` #22–#23 for what happens when a
specific layout is copied without checking product fit.

---

## Linear's issue list + inline filter bar

**OBSERVATION.** Rather than a search/filter panel that opens as a
separate modal or sidebar, Linear renders active filters as small,
directly-editable chips inline in the toolbar above the issue list; the
list itself uses a dense single-column row layout (not a card grid) with
consistent left-aligned columns for status, priority, assignee, and
title.

**WHY IT WORKS.** Filtering is a high-frequency action for this user, so
it's placed at zero navigation depth (no modal to open/close) and the
chips make the current filter state permanently visible rather than
hidden inside a dialog the user has to reopen to check. The row-based
list (not cards) supports fast vertical scanning of many items at once —
exactly the density Constitution #11 recommends for expert/high-frequency
use.

**UNDERLYING PRINCIPLE.** For a frequently-adjusted, frequently-checked
piece of state (an active filter, a sort order), keep both the control
and its current value inline and persistently visible rather than behind
a disclosure — collapse only state that's checked rarely.

**WHEN TO REUSE THE PRINCIPLE.** Any dense list/table view for expert
users who filter/sort often — admin panels, ops dashboards, inbox-like
tools.

**WHEN NOT TO COPY IT.** A consumer browse/search experience (an
e-commerce catalog, a marketplace) usually benefits from a more visible,
larger filter panel/sidebar precisely because occasional users need the
options to be discoverable, not just editable once found (Constitution
#16) — inline chips assume the user already knows filtering exists.

---

## Stripe's pricing/billing page: total-cost-first layout

**OBSERVATION.** Stripe's own published guidance for billing pages (see
Sources) recommends structuring the page so the total cost and what's
included are visible before any payment fields, with plan comparison
presented as a small number of clearly differentiated columns rather than
a long feature-matrix table.

**WHY IT WORKS.** Constitution #16 requires that anything which would
change a user's decision if revealed must not be hidden — price and
inclusions are exactly that category. Placing them first removes the
single biggest cause of checkout abandonment (surprise cost) and
front-loads the information the user actually needs to decide, rather
than making them commit attention to a form before knowing what they're
committing to.

**UNDERLYING PRINCIPLE.** Order a transactional page by what the user
must know to decide, not by the sequence convenient for data entry —
decision-critical information (total price, what's included, cancellation
terms) precedes and is at least as prominent as the input fields
collecting payment.

**WHEN TO REUSE THE PRINCIPLE.** Any checkout, subscription, or
commitment flow (signup with a paid tier, a booking with cancellation
terms, a contract-length commitment).

**WHEN NOT TO COPY IT.** For genuinely variable/quote-based pricing
(enterprise sales, custom contracts) a fixed total-cost-first layout is
impossible to build honestly — the correct move there is transparency
about *how* pricing is determined and a clear next step (contact sales),
not a fabricated total.

---

## Notion's flexible block-based page layout

**OBSERVATION.** A Notion page has no fixed template structure — it is a
vertical stack of independently-typed blocks (heading, paragraph, table,
toggle, embed) that the user assembles and rearranges via drag handles,
with no separate "layout mode" distinct from the editing mode.

**WHY IT WORKS.** Because editing and viewing use the same direct-
manipulation surface, there's no context switch between "building the
page" and "using the page" — the layout literally is the content
structure the user chose, so it can be exactly as dense or sparse as this
specific document needs, unlike a fixed template that forces every
document into the same shape regardless of fit. This is a direct
consequence of the data model, not just a UI choice: Notion's own
engineering blog (see Sources) describes every block — paragraph,
heading, table, and even the page itself — as sharing one uniform
structure with parent/child position pointers, so "layout" is literally
the same tree that stores the content, with no separate layout
representation to keep in sync.

**UNDERLYING PRINCIPLE.** When content structure genuinely varies
per-instance (every user's document is structurally different), let
layout be a direct, in-place consequence of content composition rather
than a separate template chosen from a fixed list.

**WHEN TO REUSE THE PRINCIPLE.** Authoring tools, wikis, flexible
reporting/dashboard builders — anywhere the shape of the content isn't
knowable in advance.

**WHEN NOT TO COPY IT.** Anywhere content structure *is* knowable and
consistent across instances (a settings page, a product detail page, an
order-confirmation screen), a fixed, purpose-built layout will always
communicate hierarchy more reliably than a freeform block stack — freeform
composition trades consistency and predictability for flexibility the
use case doesn't need (Constitution #13).

---

## GitHub's "Files changed" split: file tree + persistent diff

**OBSERVATION.** GitHub's PR review layout keeps a collapsible file-tree/
list on one side (with per-file changed/viewed indicators) and the diff
itself as the large, persistent central content, rather than paging
through files one at a time in a single-file-at-a-time view.

**WHY IT WORKS.** Code review requires both an overview (how many files,
which ones are risky, what have I already reviewed) and deep focus on one
file's content at a time — the split layout serves both without a modal
or page navigation, and the per-file viewed-state directly persists
progress across an interrupted review session, matching how long reviews
actually happen (in bursts, not one sitting).

**UNDERLYING PRINCIPLE.** When a task has both a navigation/overview
need and a deep-focus need on the same data, use a persistent split
(list/tree + detail) rather than forcing either an overview-only or
detail-only view, and persist per-item progress state explicitly.

**WHEN TO REUSE THE PRINCIPLE.** Any review/inbox-style task across many
items with meaningful per-item state (email triage, moderation queues,
multi-file editing tools, translation-review tools).

**WHEN NOT TO COPY IT.** For a small, fixed number of items (2–3) a
persistent list/tree pane wastes space and adds a navigation layer that
isn't needed — a simpler tabbed or sequential view serves a short,
fixed set better (Constitution #9 — the list pane must justify its
screen share).

---

## Airbnb search results: map + list split

**OBSERVATION.** Airbnb's desktop search results page splits the viewport
between a scrollable list of listings and a persistent, synchronized map
— hovering a list item highlights its map pin and vice versa — rather
than putting the map behind a toggle or on a separate page.

**WHY IT WORKS.** Location is one of the most decision-relevant
attributes for a travel booking (Constitution #16 — this is exactly the
kind of information that changes the decision and must not be hidden),
and keeping both views simultaneously visible and linked lets a user
reason spatially and by list criteria (price, rating) at the same time
without losing context switching between the two.

**UNDERLYING PRINCIPLE.** When two representations of the same data set
(spatial and list/ranked) are both decision-critical, keep them
simultaneously visible and cross-linked rather than choosing one as
default and hiding the other behind a toggle.

**WHEN TO REUSE THE PRINCIPLE.** Real-estate search, local-service
marketplaces, event/venue discovery — any search where location is a
first-order decision factor alongside rankable attributes.

**WHEN NOT TO COPY IT.** On mobile, or for search where location isn't a
primary decision factor (most e-commerce, B2B SaaS discovery), a
permanent split view isn't warranted — Airbnb itself collapses to a
toggleable single view on narrow viewports precisely because there isn't
room to do both well (Constitution #10, recompose don't shrink) and
forcing a map for non-spatial decisions is unjustified complexity
(Constitution #9).

---

## Slack's three-pane workspace layout

**OBSERVATION.** Slack's desktop layout is a persistent three-pane
structure: a narrow workspace/server switcher, a channel/DM list, and the
active conversation — all visible simultaneously, with the active
conversation getting the majority of horizontal space.

**WHY IT WORKS.** The three panes map directly to the three levels of the
actual navigation model (which workspace, which conversation, what's
being said) and keeping all three visible at once means switching
context (to another channel) never requires losing your place in the
level above it — you can always see what workspace and channel list
you're in while reading a conversation, supporting the orientation
requirement in Constitution #9 ("where am I, what can I do"). Slack's
own product blog (see Sources) frames the channel list itself as solving
a specific navigation problem email doesn't: a new team member "starts
with an empty inbox" in an email-based team, but can scroll a channel's
full history to get oriented — the persistent middle pane is what makes
that history reachable without leaving the surrounding navigation
context.

**UNDERLYING PRINCIPLE.** When navigation has a genuine fixed hierarchy
depth (2–3 levels) that users move between constantly, render all levels
as persistent, simultaneously-visible panes rather than collapsing
upper levels behind a back button or hamburger menu — the cost of
losing orientation on every switch outweighs the screen space saved.

**WHEN TO REUSE THE PRINCIPLE.** Any tool with a stable 2–3-level
navigation hierarchy visited many times per session (email clients, IDEs,
project-management tools with a workspace/project/item structure).

**WHEN NOT TO COPY IT.** On mobile viewports, or for navigation
hierarchies deeper than 2–3 levels, a persistent multi-pane layout
doesn't fit — recomposing to a stack-based single-pane-at-a-time
navigation (Constitution #10) is correct there; permanently exposing
three panes on a narrow screen would just recreate the "shrink, don't
recompose" anti-pattern.

---

## Figma's canvas + right-side properties panel

**OBSERVATION.** Figma keeps the design canvas as the dominant central
element and puts all object-specific controls (position, fill, effects,
layout) in a consistently-positioned right-side panel that updates to
reflect the current selection, rather than a toolbar that changes
position or a modal per property type.

**WHY IT WORKS.** The canvas is the actual work — it must stay maximally
large and stable — while properties are inherently *about* whatever is
currently selected, so anchoring them to a fixed, predictable location
that simply updates its content on selection change avoids both losing
canvas space to a shifting toolbar and avoids a modal that would
interrupt direct manipulation.

**UNDERLYING PRINCIPLE.** In a direct-manipulation tool, keep the primary
work surface maximized and stable, and represent selection-dependent
properties in one fixed, predictable location whose *content* changes
with selection — not one whose *position* changes.

**WHEN TO REUSE THE PRINCIPLE.** Any canvas/editor tool (image editors,
video editors, diagramming tools, page builders) where a primary spatial
work surface coexists with per-object property editing.

**WHEN NOT TO COPY IT.** For non-spatial editing tasks (a text document,
a form builder without visual layout freedom) there's no canvas to
protect, so a fixed side panel is often unnecessary overhead — inline
editing controls near the content usually serve better (Constitution #9).

---

## Superhuman's single-pane, command-driven inbox

**OBSERVATION.** Superhuman deliberately keeps a single reading pane (not
a 3-pane email client layout) and drives navigation between messages
through keyboard commands and a command palette rather than sidebar
folders and preview panes.

**WHY IT WORKS.** For a keyboard-first triage workflow, a single focused
pane removes any temptation to click around, and forces (in a good way)
the archive/snooze/reply-then-advance rhythm that is the entire value
proposition — every extra visible pane would be a place attention could
leak to instead of processing the next message. Superhuman paired this
layout decision with a matching go-to-market one: per First Round Review
and Lenny's Newsletter (see Sources), the company ran manual, personal
1-on-1 onboarding calls (at peak, a 20-person team doing this full time)
specifically to teach the single-pane, keyboard-driven rhythm before a
user's first unassisted session — the layout only works if the habit is
actually installed, so Superhuman treated installing it as a product
investment, not a documentation problem.

**UNDERLYING PRINCIPLE.** When a workflow is fundamentally sequential and
speed-critical (process item, advance to next, repeat), a narrower,
single-focus layout that removes alternative attention targets can
outperform a layout that maximizes simultaneous visibility.

**WHEN TO REUSE THE PRINCIPLE.** Triage-style workflows: support-ticket
queues, moderation queues, review/approval flows, flashcard-style
learning apps.

**WHEN NOT TO COPY IT.** For reference or comparison tasks (comparing two
emails, cross-referencing a folder while composing) a single-pane layout
actively removes needed capability — this only fits genuinely linear,
one-at-a-time processing tasks, not tasks that require holding multiple
items in view at once.

---

## Sources

- [Billing page design that converts](https://stripe.com/resources/more/designing-a-billing-page-that-converts-tips-for-better-payment-experiences) — Stripe, accessed 2026-09-12
- [Checkout page design optimisation for e-commerce](https://stripe.com/gb/resources/more/how-to-design-an-ecommerce-checkout-page-that-converts-more-sales) — Stripe, accessed 2026-09-12
- [The Elegant Design of Linear.app](https://telablog.com/the-elegant-design-of-linear-app/) — Tela Blog, accessed 2026-09-12
- [How Notion Crafts a Personalized Onboarding Experience](https://www.candu.ai/blog/how-notion-crafts-a-personalized-onboarding-experience-6-lessons-to-guide-new-users) — Candu Blog, accessed 2026-09-12
- [Reviewing proposed changes in a pull request](https://docs.github.com/en/pull-requests/how-tos/review-pull-requests/reviewing-proposed-changes-in-a-pull-request) — GitHub Docs, accessed 2026-09-12
- [Case Study: How Airbnb Increased Bookings by 25% with 3 Trust-Building UX Changes](https://raw.studio/blog/how-airbnb-increased-bookings-by-25-with-3-trust-building-ux-changes/) — Raw.Studio, accessed 2026-09-12
- [How Figma Uses 3 Collaboration UX Patterns to Drive Product Adoption](https://raw.studio/blog/figma-ux/) — Raw.Studio, accessed 2026-09-12
- [How Superhuman Built an Engine to Find Product Market Fit](https://review.firstround.com/How-Superhuman-Built-an-Engine-to-Find-Product-Market-Fit) — First Round Review (Rahul Vohra), non-paywalled, accessed 2026-09-12
- [Superhuman's secret to success: Ignoring most customer feedback, manually onboarding every new user...](https://www.lennysnewsletter.com/p/superhumans-secret-to-success-rahul-vohra) — Lenny's Newsletter, accessed 2026-09-12
- [Superhuman's Secret 1-on-1 Onboarding Revealed](https://growth.design/case-studies/superhuman-user-onboarding) — Growth.Design, accessed 2026-09-12
- [Exploring Notion's Data Model: A Block-Based Architecture](https://www.notion.com/blog/data-model-behind-notion) — Notion's own engineering blog, accessed 2026-09-12
- [Six ways that channels can transform your work](https://slack.com/blog/productivity/six-ways-that-channels-can-transform-your-work) — Slack's own blog, accessed 2026-09-12 (replaces "direct product observation" for the workspace-layout rationale)
