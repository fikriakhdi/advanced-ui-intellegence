---
title: System-Level Patterns — How Components Combine Into Flows
confidence-note: ESTABLISHED synthesis of recurring flow-level patterns observed across mature SaaS/consumer products (list-detail, master-detail, wizard/stepper, dashboard, settings, CRUD table+drawer). Distinct in scope from design-systems/components.md, which covers single-component behavior — this file is about combinations of components across a full user flow.
last_updated: 2026-09-12
---

# System-Level Patterns

`design-systems/components.md` specifies how one component behaves.
This file specifies how *combinations of components, across a flow*,
should be structured — the layer where most real product design actually
happens, and where Constitution #15's composition mandate is proven or
disproven in practice.

## 1. List → Detail

**PATTERN**: A list/table of records paired with a detail view of one
selected record.

**WHEN TO USE**: Browsing a collection where inspecting one item is a
common, expected next step (an inbox, a CRM's contact list, a file
browser).

**VARIANTS BY AVAILABLE SPACE** (this is a direct application of
`responsive-design.md`'s content-not-device framing, not three unrelated
designs):
- **Wide layout — master-detail (two-pane)**: list occupies a narrower
  left column, detail renders in the remaining space, selection updates
  detail in place without navigation. Appropriate above roughly
  1024–1200px where both panes can be usefully sized.
- **Narrow layout — full-screen drill-down**: selecting a list item
  navigates to a full-screen detail view with a back affordance,
  because there isn't width for two useful panes simultaneously. This
  is a "become a different component" recomposition
  (`responsive-design.md` §4) applied at the flow level, not just to one
  component.
- **Intermediate/split-view (tablet)**: two-pane if width allows, with
  the detail pane defaulting to empty/placeholder state until a selection
  is made, or collapsing to list-only until selection.

**DESIGN RULES**: The list should retain visible selection state even
while the detail pane is focused (so the user always knows which record
they're viewing); deep-linkable detail URLs so a specific record can be
shared/bookmarked/reloaded directly, independent of layout mode; keyboard
users should be able to move selection (arrow keys) without leaving the
list, mirroring desktop file-browser conventions users already know
(Constitution #19).

## 2. CRUD table + create/edit drawer

**PATTERN**: A table of records (per `components.md` §Tables) with
create/edit performed in a side drawer or dialog rather than a full page
navigation.

**WHEN TO USE**: Records with a moderate number of fields where staying
in the context of the surrounding list (seeing it update live, comparing
against other rows) is valuable — most admin/back-office CRUD screens.
**WHEN TO PREFER A FULL PAGE INSTEAD**: Records with many fields, multiple
sections, or a genuinely multi-step creation flow — cramming a large form
into a drawer produces its own scroll-within-scroll problems; a full page
(or `responsive-design.md` "become a different component" applied to the
drawer itself) is more honest about the task's actual size.

**DESIGN RULES**: The drawer opening should use spatial continuity from
the triggering row/button (`motion-interaction.md` §7); on successful
save, the underlying table should update **optimistically or immediately
on confirmed success** and the drawer should close, ideally with a brief
toast confirmation — avoid leaving the user staring at a static drawer
after save with no clear next step. Unsaved-changes protection (a
confirm-before-discard prompt) is required once any field has been
touched — silently discarding entered data on accidental dismiss is a
common, damaging failure in this pattern specifically.

## 3. Wizard / multi-step flow

**PATTERN**: A sequence of steps, each gating progress on completing the
prior step, culminating in a final action (checkout, account setup, a
complex configuration task).

**WHEN TO USE**: A task genuinely has sequential dependencies (step 2's
options depend on step 1's choice) or is long enough that breaking it up
reduces perceived and actual cognitive load (Constitution #11 —
comprehension cost matters most for novice/occasional users, and a wizard
is fundamentally a device for reducing that cost at the price of more
clicks). **WHEN NOT TO USE**: A form with no real sequential dependency,
where a wizard only adds friction (extra clicks, inability to see the
whole picture) without a comprehension benefit — a single well-organized
page with clear sections usually beats an unnecessary wizard for
experienced or motivated users.

**DESIGN RULES**: Show progress (a step indicator: numbered steps,
segmented progress bar, or both) so the user always knows how much
remains — this is a Constitution #16 requirement (the user needs this to
form correct expectations, not just a decoration). Allow backward
navigation without losing entered data. Validate each step before
allowing "Next" for hard requirements, but avoid re-validating
already-passed steps unnecessarily when moving backward and forward.
State should persist across a browser refresh for any wizard expected to
take more than a minute or two (draft-saving), since abandoning and
restarting a multi-minute flow from scratch is a severe cost.

## 4. Dashboard (KPI + drill-down)

**PATTERN**: A top-level summary view (KPI tiles, charts, key lists)
that each link to a more detailed view of the same data.

**DESIGN RULES**: KPI tiles are sectional/summary content, not discrete
actionable "cards" in the Constitution #6 sense — resist wrapping every
tile in a heavy bordered card by default; typographic grouping in a
consistent grid is usually sufficient (Constitution #5), reserving an
actual card treatment for tiles that are genuinely independently
actionable (e.g., each one links to a different detail page and benefits
from a distinct clickable boundary). Every summary number should be
scannable in under a second — meaning at most one comparison signal per
tile (a trend arrow + percentage vs. prior period is common and
sufficient; five different comparison stats crammed into one tile
recreates Constitution #4's "everything bold = nothing bold" failure at
the tile level). Charts need the same information hierarchy discipline as
any other content — a dashboard where every chart is equally large and
equally colorful has the same problem as a page where every button is
primary.

## 5. Settings

**PATTERN**: Grouped, typically sectional (not card-based, per
Constitution #6) configuration screens, often with a persistent
left-hand section nav on wide layouts.

**DESIGN RULES**: Group settings by *what the user is trying to
accomplish* (Notifications, Billing, Security), not by the underlying
data model's structure — a settings taxonomy that mirrors backend schema
rather than user mental models is a recurring, avoidable failure. Changes
should have a clear, consistent save model applied uniformly across the
whole settings area (either autosave-on-change with a confirmation toast,
or an explicit Save button per section) — mixing the two models across
different settings sections in the same product violates Constitution
#13's behavioral-consistency requirement and actively causes data-loss
mistakes (a user assumes autosave everywhere, navigates away from a
Save-button section, loses changes).

**RESPONSIVE BEHAVIOR**: Persistent settings section-nav sidebar becomes
a top-level list of section links on narrow layouts, with each section
its own full-screen drill-down (`responsive-design.md` §4, "become a
sheet" / "become a different component" depending on depth).

## 6. Notification / activity feed

**PATTERN**: A reverse-chronological stream of discrete events, each
potentially linking to the entity it concerns.

**DESIGN RULES**: Group by recency (Today / Yesterday / This week / Older)
rather than an undifferentiated infinite list — this gives scannable
structure to what's otherwise a flat stream. Unread/read state must be
visually distinct without relying on color alone (weight, a dot indicator
plus color, not color alone — Constitution #7). Mark-as-read behavior
should be predictable and consistent (on view vs. on explicit action) and
documented once as a system convention rather than decided per feed.

## 7. Onboarding-to-first-value

**PATTERN**: The specific flow from account creation to the user
experiencing the product's core value for the first time — the
highest-leverage flow in most products and the one most often over- or
under-designed.

**DESIGN RULES**: Minimize the distance between account creation and a
first meaningful action inside real (or realistic sample) content —
front-loaded, content-free tours delay this without benefit (Constitution
#16, applied to first-run: don't hide the essential "how do I get value"
information behind an unrelated tour). Prefer seeding a working example
(sample data, a pre-filled first project) over an empty first-run screen
wherever the product allows it, since an empty state's "create your
first X" call to action is inherently one click short of a user
*experiencing* the product, while a working example gets them there
immediately.

## Sources
- Synthesized from recurring flow-level patterns across mature SaaS/consumer product design (list-detail, master-detail, CRUD-with-drawer, wizard, dashboard, settings) — training knowledge, general design practice, not tied to one fetched source this session.
- Constitution principles #4, #5, #6, #11, #13, #15, #16 — `/home/claude/advanced-ui-skill/knowledge/ui-principles-2026.md`
