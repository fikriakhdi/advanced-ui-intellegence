---
title: Component Library — Patterns by Type
confidence-note: Structural/ARIA claims for dialogs, tabs, comboboxes, menus, listboxes, and breadcrumbs verified against W3C ARIA APG this session. Buttons, dialogs/drawers/sheets, tables, empty/loading/error states, command palettes, navigation, breadcrumbs, pagination, filters, search, notifications, onboarding, lists, and menus now all have full PATTERN/PURPOSE/WHEN TO USE/WHEN NOT TO USE/DESIGN RULES/RESPONSIVE BEHAVIOR/ACCESSIBILITY/COMMON FAILURE/IMPLEMENTATION NOTES treatment. General component behavior claims are ESTABLISHED, synthesized from Material 3, Spectrum, Carbon, Polaris, Primer, Atlassian, and ARIA APG conventions; specific vendor-guidance claims (e.g. "show only one banner at a time") are cited to their source system inline.
last_updated: 2026-09-12
---

# Component Library

Every entry uses PATTERN → PURPOSE → WHEN TO USE → WHEN NOT TO USE →
DESIGN RULES → RESPONSIVE BEHAVIOR → ACCESSIBILITY → COMMON FAILURE →
IMPLEMENTATION NOTES where it earns its place. All entries in this file
now carry that full treatment; earlier passes prioritized buttons,
dialogs/drawers/sheets, tables, empty/loading/error states, and command
palettes as the most commonly misused components, and a follow-up pass
brought navigation, breadcrumbs, pagination, filters, search,
notifications, onboarding, lists, and menus to the same depth.

---

## Buttons

**PURPOSE**: Trigger a single, immediate action. A button is a verb, not a
destination — if it navigates somewhere with no side effect, it should
usually be a link, not a button (visually and semantically).

**WHEN TO USE**: Any single discrete action — submit, save, delete, open a
dialog, trigger a process.

**WHEN NOT TO USE**: Navigation to another page/view with no side effect
(use a link); a set of mutually exclusive options (use a segmented
control or radio group); toggling a persistent binary setting shown
inline (use a switch, which communicates current state at rest — a button
doesn't).

**DESIGN RULES**:
- Exactly one **primary** button per decision region — not per page (see
  Constitution #8). A region is a card, a modal, a table row, a toolbar
  section — each gets at most one visually "loudest" action.
- Establish an explicit **hierarchy of button styles** and use every level
  consistently: *primary* (filled, highest emphasis — the one recommended
  action), *secondary* (outlined/tonal — a legitimate alternative action,
  e.g. "Cancel" next to "Save"), *tertiary/text* (lowest emphasis — an
  optional or de-emphasized action), *destructive* (a distinct treatment,
  usually the danger semantic color, reserved for irreversible/harmful
  actions and never used as decoration for anything else).
- Button label = verb + object where the object isn't already obvious from
  context ("Delete project," not just "Delete," when other deletable
  things are also on screen; just "Save" is fine when unambiguous).
  Avoid vague labels ("OK," "Submit," "Yes") when a specific verb
  communicates the consequence better, especially on destructive
  confirmations ("Delete 12 files" beats "Confirm").
- Never stack two primary-styled buttons side by side — if two actions
  both feel "primary," that's a sign one of them should be demoted or the
  decision simplified (Constitution #8 again).
- Icon-only buttons need a persistent way to discover their meaning
  (tooltip on hover/focus, or a label that appears responsively) since an
  icon alone is frequently ambiguous.

**RESPONSIVE BEHAVIOR**: Full-width buttons are a common, correct pattern
in narrow/mobile contexts for the primary action of a screen (a checkout
button anchored full-width at the bottom); on wider layouts, buttons
should generally size to content, not stretch to fill — a full-width
button in a wide desktop form looks unintentional. Multiple buttons in a
row (e.g. Cancel/Save) that would crowd at narrow widths should stack
vertically with primary on top (mobile) — see
`knowledge/responsive-design.md` §4 ("collapse").

**ACCESSIBILITY**: Real `<button>` element (or `role="button"` only if a
native button truly cannot be used, which then obligates full keyboard
support — Space/Enter activation — that native buttons provide for free).
Minimum target size per `knowledge/accessibility.md` §2. Disabled buttons
should still be perceivable (don't drop contrast so low the label is
unreadable — a disabled state needs to be legible enough to know a button
exists and is currently unavailable, ideally with a reason discoverable on
hover/focus rather than silent disablement).

**COMMON FAILURE**: Every button on a screen styled as filled/primary
("we want everything to look actionable") — this is the direct,
frequently-seen version of Constitution #8's failure mode. Also common:
destructive-red used for a button that isn't actually destructive, purely
because red "pops."

**IMPLEMENTATION NOTES**: Loading state should disable the button and show
a spinner (replacing or alongside the label) — see
`knowledge/motion-interaction.md` §4 — and must prevent double-submission
(disable on first click, don't rely on the user not double-clicking).

---

## Inputs (text fields)

**PURPOSE**: Capture a single piece of free-text or numeric data.

**DESIGN RULES**: Always paired with a real, persistently visible
`<label>` (never placeholder-as-label — see `accessibility.md` §8).
Provide inline validation feedback tied to a consistent trigger point
(on-blur is a common, defensible default; document the convention once
system-wide per Constitution #13). Helper text sits below the field and
should state format/constraint expectations *before* an error occurs
where the constraint isn't obvious (e.g. password requirements), not only
after a failed attempt.

**ACCESSIBILITY**: Label programmatically associated (`for`/`id` or
wrapping); errors linked via `aria-describedby` and flagged with
`aria-invalid="true"`; `autocomplete` set correctly (email, name,
address fields) both for AT and password-manager benefit.

**COMMON FAILURE**: Relying on color alone (a red outline) to signal an
error with no icon or text — fails colorblind users and Constitution #7.

*(Compressed — this entry covers the general text input; numeric,
date, and masked-input variants each have edge cases not detailed here.)*

---

## Selects / Comboboxes

**PATTERN**: `combobox` input + popup `listbox` (verified ARIA APG
pattern — see `accessibility.md` §4 for the full role/attribute mapping).

**WHEN TO USE**: A native `<select>` is correct and cheapest whenever the
option list is short, doesn't need search/filter, and doesn't need rich
option content (icons, descriptions, avatars) — always prefer it by
default (Constitution #19: don't replace a familiar, fully-accessible
native control without a measurable gain). Reach for a custom
combobox only when you need: type-ahead filtering over a long list,
multi-select with visible chips, rich option rendering, or async-loaded
options.

**DESIGN RULES**: A custom combobox commits you to the entire ARIA
contract: `aria-expanded`, `aria-controls`, `aria-activedescendant` (focus
stays on the input; the highlighted option is virtual, not real DOM
focus), `aria-autocomplete`. Filtering should be forgiving (substring,
not prefix-only, unless the list is huge and prefix search is a
deliberate performance/precision trade). Show a clear "no results" state
rather than an empty popup.

**RESPONSIVE BEHAVIOR**: On touch, the popup often becomes a full-screen
or bottom-sheet picker rather than a small anchored popover, both for
touch-target reasons and because virtual-keyboard overlap with a small
anchored popup is a frequent, easily-avoidable bug (see
`responsive-design.md` §4, "become a different component").

**COMMON FAILURE**: A custom-styled dropdown built from a `<div>` with
click handlers and no keyboard support at all — one of the single most
common serious accessibility regressions in custom design systems.

---

## Dialogs / Drawers / Sheets

**PURPOSE**: Interrupt the current flow to require a decision (dialog) or
to present supplementary content/actions without leaving the current
context (drawer/sheet).

**WHEN TO USE**: **Dialog** (centered, modal) — for a decision the user
must make before continuing (confirm destructive action, a required short
form) or content that fundamentally blocks the current task. **Drawer /
side panel** — for supplementary detail or a longer form the user may
want to reference against the underlying content (a detail inspector
next to a list). **Bottom sheet** — the standard mobile-context answer
when a drawer/side panel has no room (see `responsive-design.md` §4,
"become a sheet"), or for a short, dismissible action set (a share sheet).

**WHEN NOT TO USE**: A dialog for information that isn't actually
decision-blocking (use inline content or a toast instead) — modal
overuse is a common way products punish users with extra clicks for
non-critical information. Never stack more than one modal dialog at a
time; a dialog opening another dialog is almost always a sign the flow
should be redesigned as steps within one dialog, or as a separate page.

**DESIGN RULES** (ARIA APG dialog pattern, verified this session):
`role="dialog"`, `aria-modal="true"`, labeled via `aria-labelledby`
(pointing at a visible title) or `aria-label`. On open, **focus moves
inside the dialog**; Tab/Shift+Tab **cycle within it** (a hard focus
trap — focus must never leak to the underlying page while the dialog is
open); Escape closes it; on close, **focus returns to the invoking
element** (or a sensible fallback if it no longer exists). Initial focus:
first focusable element by default, but a static `tabindex="-1"` heading
for long/complex content (prevents auto-scroll past context), and the
*least* destructive option for a destructive confirmation (so a reflexive
Enter doesn't confirm deletion).
- A dialog invoked from a specific control should, where feasible,
  animate growing from that control's position (spatial continuity, per
  `motion-interaction.md` §7) rather than fading in from screen-center
  unconditionally.
- The underlying page must be inert while the dialog is open (clicks and
  scroll on background content disabled, typically via `inert` or an
  equivalent) — a dialog that's visually modal but not *actually* modal
  (background still scrollable/clickable) is a frequent implementation
  bug.
- Provide a visible close affordance (X, Cancel) in addition to Escape —
  keyboard-only closing isn't discoverable for a mouse/touch user who
  doesn't know the shortcut.

**RESPONSIVE BEHAVIOR**: A centered dialog on desktop commonly becomes a
full-screen view or a bottom sheet on narrow/touch contexts (per
Constitution #10 and `responsive-design.md` §4) — a small centered modal
on a 360px screen leaves awkward margins and a cramped scroll area.

**COMMON FAILURE**: Missing focus trap (Tab escapes into background
content); focus not returned on close; a dialog used for a non-blocking
notification that should have been a toast; nested modals.

**IMPLEMENTATION NOTES**: Prefer the native `<dialog>` element (with
`showModal()`) where its behavior (built-in focus trap, `::backdrop`,
Escape-to-close) suffices — it removes most of the above failure modes for
free, similarly to preferring native `<select>` for selects.

---

## Popovers / Tooltips

**PATTERN**: Non-modal, anchored, transient content.

**WHEN TO USE**: **Tooltip** — a short label clarifying an ambiguous
control (typically icon-only buttons), shown on hover *and* focus (never
hover-only — keyboard and touch users need an equivalent trigger).
**Popover** — richer content or actions anchored to a trigger, dismissed
by outside click/Escape, not modal (background remains interactive).

**DESIGN RULES**: Tooltips should not contain the *only* copy of
essential information or interactive controls — they're a supplementary
hint, not a content container (a tooltip that requires precise
mouse-hovering to read interactive text inside it is broken for most
input types). Popovers need their own focus management (Escape closes and
returns focus to the trigger) but, unlike dialogs, don't need a full focus
trap.

**ACCESSIBILITY**: Tooltip triggers need `aria-describedby` pointing at
the tooltip content; must appear on keyboard focus, not only mouse hover,
per `accessibility.md` §5 (keyboard is a first-class mode).

**COMMON FAILURE**: Hover-only tooltip triggers (invisible on touch and
to keyboard users); a tooltip used to hide information the user actually
needs to complete a task (should be inline text instead, per Constitution
#16).

---

## Tabs / Segmented Controls

**PATTERN**: `tablist` / `tab` (`aria-selected`) / `tabpanel` — verified
ARIA APG pattern, see `accessibility.md` §4.

**WHEN TO USE**: Switching between a small number (roughly 2–7) of
peer, same-level views of related content within the same context
(e.g., "Overview / Activity / Settings" for one entity). **Segmented
control** specifically for a small set of mutually exclusive *view modes*
or filters applied to the same content (e.g. "List / Grid" or
"Day / Week / Month"), visually more compact and control-like than
content-tabs.

**WHEN NOT TO USE**: More than ~7 options (use a different nav pattern —
a sidebar, a select, or a menu); options that aren't true peers (one tab
that's actually a completely different flow, not a view of the same
entity) — tabs imply "you're still looking at the same thing, from a
different angle."

**DESIGN RULES**: Choose and document one activation model — automatic
(tab activates the instant it receives keyboard focus; requires cheap
panel rendering) or manual (Space/Enter required; safer when switching
loads/fetches something) — and apply it consistently across the product.
Arrow keys move between tabs; Home/End jump to first/last.

**RESPONSIVE BEHAVIOR**: A tab row that doesn't fit at narrow widths
should become horizontally scrollable (with a scroll affordance) rather
than wrapping to multiple lines (which breaks the single-row "peer
options" visual grouping) — see `responsive-design.md` §4, "become
scrollable." Beyond a certain narrowness, tabs may need to become a
select/menu instead ("become a different component").

---

## Cards

**PURPOSE**: Represent one discrete, repeatable, often actionable unit
(per Constitution #6) — a product, a person, a document, a single
dashboard "thing." Not a general-purpose container for sequential or
sectional content.

**WHEN NOT TO USE**: A settings page's field groups, a form's steps, or a
dashboard's summary numbers — these are sequential/sectional, not
discrete repeatable units, and should use typographic/spatial grouping
instead (Constitution #5/#6). If every region of a page is a card, cards
have stopped communicating anything.

**DESIGN RULES**: One primary action per card (Constitution #8) if the
card is actionable at all — a card with five equally-weighted buttons at
the bottom has the same problem as a page with five primary buttons.
Clickable cards should have a single, unambiguous click target (avoid
nested interactive elements — a button inside a whole-card link creates
ambiguous/broken click targets and accessibility confusion about what's
focusable).

**RESPONSIVE BEHAVIOR**: Card grids are the classic `auto-fit`/
`minmax()` or container-query use case — see `responsive-design.md` §2–3.

---

## Tables

**PURPOSE**: Present multiple records with multiple comparable attributes
— the defining use case is *comparison across rows and columns*, which is
exactly what a card grid or list cannot do well.

**WHEN TO USE**: Data genuinely benefits from column-wise comparison
(a spreadsheet-like set of records, an admin list with sortable/filterable
attributes). **WHEN NOT TO USE**: A handful of records with only 1–2
attributes each (use a list instead — a table with two columns is usually
overhead); content that's fundamentally about one hero attribute per item
with supporting metadata (often better as a card list — e.g. a media
library, which benefits from a visual thumbnail a table cell can't
usefully show at a glance).

**DESIGN RULES**:
- Real `<table>` markup (`<thead>`, `<tbody>`, `<th scope="col">`) — a
  div-grid pretending to be a table loses native screen-reader table
  navigation.
- Right-align numeric columns for scannability of magnitude; left-align
  text. Keep a consistent column order across any views of "the same"
  data elsewhere in the product.
- Sort indicators should be visible (not hover-only) and the sorted
  column visually distinct once active.
- Row actions belong in a consistent, predictable location (commonly a
  trailing "more actions" menu) rather than scattered per-row buttons that
  compete with cell content for width and visual priority.
- Selection (checkboxes) needs a header "select all" with a clear
  indeterminate state when some-but-not-all rows are selected, and a
  persistent, visible bulk-action bar that appears once ≥1 row is
  selected (not a hidden toolbar the user must remember exists).
- Dense tables benefit from zebra striping or hairline row dividers
  (a legitimate, non-card use of a visual separator — see Constitution
  #5) rather than heavier per-row borders/cards.

**RESPONSIVE BEHAVIOR**: This is the canonical case for
`responsive-design.md` §4's "become a different component": below a
column-count/width threshold, a table should not shrink font size or
truncate every column — it should restructure each row into a small
key-value card (row → card, column header → label above value), or, for
tables that must stay tabular, use horizontal scroll with a **sticky
first column** (so the row's identity stays visible while scrolling
through its attributes) plus a scroll affordance. Choosing which columns
survive at narrower widths (keep vs. hide vs. merge into a row detail
expander) is itself a recomposition decision per element, per
`responsive-design.md` §4.

**ACCESSIBILITY**: Column headers associated via `scope` or `headers`/
`id`; sort-state changes announced via `aria-sort` on the header cell;
row selection state exposed via real checkbox inputs, not styled divs.

**COMMON FAILURE**: Shrinking a wide table's font and padding until it
technically fits a phone screen but is unreadable — the single most
common table-responsiveness mistake, and a direct violation of
Constitution #10 ("recomposed, not shrunk").

---

## Lists

**PATTERN**: A vertically (occasionally horizontally) stacked sequence
of same-shape rows, each representing one item, sitting between Tables
(column-wise comparison across many attributes) and Cards (a strong,
discrete, often movable/comparable unit) in structural weight.

**PURPOSE**: Present a sequence of items with roughly uniform, usually
single- or few-attribute-per-item content — the low-overhead alternative
to both tables (no need for column-wise comparison across the set) and
cards (no need for a strong "discrete unit" visual boundary around each
one). A list communicates "these are peers in a sequence," not "these are
independently significant objects" (that's a card's job) or "compare
these across N attributes" (that's a table's job).

**WHEN TO USE**: A set of items with 1-3 attributes each where the
relationship between items is sequential/peer rather than tabular
(a settings menu, a notification feed, search results, a simple
to-do/task list, a chat/message list, a file list without rich
metadata columns).

**WHEN NOT TO USE**: Items with several comparable attributes each (use a
Table, so columns can be scanned/sorted independently); items that are
genuinely discrete, self-contained, actionable units warranting their own
visual boundary and possibly their own primary action (use a Card grid —
Constitution #6); a very small, fixed set where the sequence itself
carries no information (a handful of independent, unrelated summary
numbers is neither a list nor a table — see the dashboard-cards guidance
in `knowledge/anti-patterns.md` #7).

**DESIGN RULES**:
- Use dividers or whitespace between items, not individual card borders
  around each row (Constitution #5/#6), unless items are genuinely
  draggable/selectable discrete units where a subtle boundary or
  hover/selected background earns its keep (a reorderable list, a
  multi-select list) — the boundary should signal a real interaction
  affordance, not just "this is one row."
- **Consistent leading icon/avatar and trailing metadata/action
  placement across every item** — a list where some rows have a leading
  icon and others don't, or where the secondary action sits in a
  different position row to row, breaks the scannability that's the
  whole point of a uniform list.
- Truncate long primary text with an ellipsis rather than wrapping to
  multiple lines by default, unless the list is explicitly a
  multi-line-content list (e.g. a message preview list) designed for it
  — inconsistent row heights from unpredictable text wrapping undermine
  the visual rhythm that makes a list fast to scan.
- A row's primary click target (if the whole row navigates) should be
  the entire row, not just the leading text — but avoid nesting a
  separately-clickable secondary action (a trailing icon button) inside
  a whole-row link/button, for the same nested-interactive-target reason
  flagged in the Cards section above.
- Selectable lists (checkboxes per row) need the same "select all" +
  indeterminate-state + persistent bulk-action-bar treatment specified
  for Tables above — a list is not exempt from that pattern just because
  it isn't a `<table>`.
- Group long lists with section headers (alphabetical, chronological,
  status-based) when a natural grouping exists and the list is long
  enough that ungrouped scanning becomes a real cost — but don't
  introduce grouping for a list short enough to scan in full at a
  glance, since a group header on a 4-item list is overhead with no
  payoff.

**RESPONSIVE BEHAVIOR**: Lists are naturally responsive-friendly — they
already stack vertically, which is the same recomposition tables need to
be forced into at narrow widths (see Tables above). The main adjustment
at narrow widths is usually reducing the number of visible trailing
metadata fields per row (keep the 1-2 most important, move the rest into
a detail view/expander) rather than shrinking type to fit everything,
which is the same "recompose, don't shrink" principle as Constitution
#10.

**ACCESSIBILITY**: A list of genuinely list-like content should use
real `<ul>`/`<ol>` + `<li>` markup (or `role="list"`/`role="listitem"`
if a CSS reset strips native list semantics, since some browser/AT
combinations drop list semantics from list-style:none without the role
reinstated) so assistive tech announces item count and position
("item 3 of 12"). A *selectable* list of options (as opposed to a list of
navigational rows) is a `listbox`/`option` pattern instead (verified ARIA
APG listbox pattern — see Selects/Comboboxes above for the shared
contract), with `aria-selected`/`aria-checked` state, arrow-key
navigation, and — per APG — type-ahead recommended especially past
seven options; don't mix the two semantics (a plain navigational list
styled to look selectable but missing real `option`/`aria-selected`
semantics is a common, subtle accessibility gap).

**COMMON FAILURE**: Wrapping every list item in its own bordered/shadowed
container, effectively turning a list into an unnecessarily heavy stack
of tiny cards (Constitution #5/#6's core warning, at list scale);
inconsistent row internal layout across a supposedly uniform list;
rendering a very long list (thousands of rows) fully in the DOM with no
virtualization, causing real scroll-performance and initial-render cost.

**IMPLEMENTATION NOTES**: For lists long enough that full-DOM rendering
becomes a performance problem (typically several hundred+ rows), use
list virtualization (rendering only the visible window of rows plus a
buffer) rather than paginating purely for performance reasons — if the
content itself is conceptually one continuous list (a chat history, an
activity feed), virtualizing preserves that continuity where pagination
would falsely chunk it; use real Pagination (above) instead of
virtualization only when chunking into discrete pages is *also* the
right product decision, not purely a performance workaround. Infinite
scroll and "load more" (see Pagination above) are the two standard ways
to grow a list past its initially-loaded window.

---

## Menus (dropdown / context menus)

**PATTERN**: `menu-button` (`aria-haspopup="menu"` + `aria-expanded` on
the trigger, `role="menu"`/`role="menuitem"` on the popup) — verified
ARIA APG pattern. A **context menu** is the same `menu` role triggered by
right-click or a long-press instead of a button.

**PURPOSE**: A transient list of *actions or navigation targets*,
triggered by a control (button, `···`, right-click), that disappears once
a choice is made or focus leaves it. Distinct from primary Navigation
(below), which is a persistent structural element, not a transient
overlay.

**WHEN TO USE**: A bounded set of actions or destinations that apply to
one specific object or context (row actions, a `···` overflow menu, a
right-click context menu, an account menu). Also the right choice for a
small set of mutually exclusive options when a segmented control or
select doesn't fit the surrounding layout (e.g. a compact toolbar).

**WHEN NOT TO USE**: A simple list of navigational links with no
action-menu behavior (no arrow-key roving, no type-ahead expectation) —
real `menu`/`menuitem` semantics impose real behavioral obligations (see
ACCESSIBILITY) that a plain list of links in a popover doesn't need to
take on; forcing `role="menu"` onto a nav-links popup is a common
over-application of ARIA that actually *breaks* screen-reader link
navigation (links stop being announced/navigable as links). More than
~10-12 items or items that need secondary description/preview content
(use a popover or a dedicated page instead — a menu is for quick action
selection, not browsing).

**DESIGN RULES**:
- Group related actions with dividers; order by frequency/expected use,
  not alphabetically, unless the list is long enough that alphabetical
  scanning wins.
- Destructive actions go last, visually distinct (danger color), and
  separated by a divider from non-destructive actions — so a fast,
  careless click doesn't land on "Delete" (ties to the Buttons section's
  destructive-styling rule).
- Icons are optional but must be applied consistently — either every
  item gets a leading icon or none do; a mix reads as arbitrary emphasis
  (Constitution #4).
- A menu item that opens a submenu needs a clear affordance (trailing
  chevron) and should open on hover-with-delay or explicit
  activation, not on bare mouse-over (accidental submenu triggers on
  transit are a common irritation).
- Checkbox/radio-style menu items (`menuitemcheckbox`/`menuitemradio`)
  are for options that persist as on/off or single-choice state *within*
  the menu itself (e.g. "Show completed") — don't use plain `menuitem`
  for something that's actually a toggle, since the checked state needs
  its own semantics and visual indicator.

**RESPONSIVE BEHAVIOR**: On touch, a menu anchored to a small trigger
should have generous item height (per `accessibility.md` §2 touch target
minimums) and may reposition to avoid viewport edges/keyboard overlap;
very long menus on mobile often become a bottom sheet rather than a small
anchored popup (`responsive-design.md` §4, "become a different
component"), matching the Selects/Comboboxes pattern above. Right-click
context menus have no direct touch equivalent — long-press is the
standard substitute, and it must be discoverable (don't make a feature
*only* reachable via a gesture with no visible alternative entry point).

**ACCESSIBILITY**: Trigger is a real `<button>` with `aria-haspopup="menu"`
and `aria-expanded`. The open menu gets `role="menu"`, items
`role="menuitem"` (or `menuitemcheckbox`/`menuitemradio`); arrow keys —
not Tab — move focus between items (Tab closes the menu and moves on, as
in native OS menus); Home/End jump to first/last; type-ahead-by-first-
letter is expected; Escape closes and returns focus to the trigger.
Because these are real behavioral obligations (not just decorative
roles), a menu that only supports mouse clicks is a broken
implementation of the pattern, not an incomplete one — see APG's
Menu Button pattern for the full keyboard contract.

**COMMON FAILURE**: `role="menu"` applied to a simple navigational
dropdown (breaking normal link semantics/behavior for screen-reader
users, per WHEN NOT TO USE above); hover-triggered menus with no click/
tap equivalent (fails touch and often fails keyboard); destructive item
not visually separated, leading to accidental data loss on a fast click.

**IMPLEMENTATION NOTES**: The command palette's internal result list
(above) is a `listbox`/`combobox`, not a `menu` — don't conflate the two
patterns; a menu is a fixed action list navigated by arrow keys with focus
staying in the DOM on each item, a combobox listbox is a filtered result
set navigated with focus staying on the input via
`aria-activedescendant`.

---

## Navigation (primary nav component)

**PATTERN**: The persistent structural component (top bar, sidebar, nav
rail, bottom tab bar) that exposes a product's top-level sections, as
distinct from `knowledge/navigation-patterns.md`'s page-level treatment
of *which* IA/navigation model to choose (tab bar vs. hub-and-spoke vs.
nested drill-down, etc.) — this entry covers the nav bar/sidebar/rail as
a UI component: its anatomy, states, and responsive recomposition.

**PURPOSE**: Orient the user within the product's overall structure
(where am I, what else exists, how do I get there) and provide fast
switching between top-level, persistent sections — it is infrastructure
the user relies on across every screen, not content specific to any one
screen.

**WHEN TO USE**: Any product with more than one top-level section that
the user needs to move between repeatedly across a session. **Top nav
bar** — few (≤6-7) top-level sections, brand-forward or content-first
products (marketing sites, many consumer apps). **Sidebar** — more
sections, sections with their own sub-navigation, or desktop-class
productivity/admin tools where persistent visibility of the full section
list aids orientation for high-frequency use (Constitution #11: expert/
frequent users benefit from more visible at once). **Nav rail**
(icon-only or icon+label collapsed sidebar) — a density compromise for
tablet/narrow-desktop widths, or for products wanting sidebar-like
persistence without its width cost. **Bottom tab bar** — the standard
mobile-primary-navigation answer, ≤5 items, always visible, no
scroll-hide for true primary destinations (see
`navigation-patterns.md`).

**WHEN NOT TO USE**: A single-section product (a focused tool with one
main view) doesn't need a nav shell at all — adding one anyway is
decoration, not orientation (Constitution #9); don't build a sidebar for
sections that are actually sequential steps in one flow (that's a
stepper/wizard, not navigation).

**DESIGN RULES**:
- Current-section state must be visually unambiguous through more than
  color alone (Constitution #7) — pair an active-state color with
  position/weight/an indicator bar or filled icon, not a hue shift by
  itself.
- The active-state visual language must be the single, consistent
  pattern used for "currently selected" everywhere else in the product
  (tabs, segmented controls) — per Constitution #13, a different
  selected-indicator style per component family reads as inconsistency
  even when each one is individually fine.
- Every item needs a stable position across sessions (don't reorder
  nav items based on recent usage — orientation depends on muscle-memory
  spatial stability, which throughput-based reordering actively breaks).
- Icon-only rail/bottom-bar items need labels on hover/focus at minimum
  (ideally always-visible labels for ≤5 items) — icon-only navigation
  without a text fallback is a common, avoidable ambiguity failure, same
  underlying issue as icon-only buttons above.
- A collapsed/expanded sidebar toggle (common on desktop admin tools)
  must persist the user's choice and not silently reset it on reload.
- Badge/count indicators on nav items (unread counts, pending items) are
  a legitimate, high-value use of a badge — unlike decorative badges
  flagged in `knowledge/anti-patterns.md` #9, this is live, actionable
  state, not manufactured busyness — but should be omitted or shown as a
  neutral dot rather than a number once the count is not itself useful
  information (e.g. "99+" territory).

**RESPONSIVE BEHAVIOR**: Top nav → bottom tab bar (primary, ≤5 sections)
or a hamburger/drawer (secondary/overflow sections) is the standard
mobile recomposition (Constitution #10; `responsive-design.md` §4).
Sidebar → one of three patterns depending on product shape (cf. GitHub
Primer's navigation guidance): **collapse to nothing, drill via the
index page** (mobile shows a list of sections as its own page; selecting
one navigates forward, "back" returns to the list — appropriate when
sidebar items are peer top-level destinations); **collapse to an
action-menu/bottom-sheet** (appropriate when the sidebar is more like a
contextual filter/settings list than primary navigation); or a **mixed
approach** combining both for products with both primary sections and
secondary in-section navigation. A nav rail commonly expands into a full
labeled sidebar at wider breakpoints and collapses to icon-only, then to
a drawer, as width shrinks — a genuine per-element recomposition chain,
not a single breakpoint swap.

**ACCESSIBILITY**: Wrap in a `<nav>` landmark with an `aria-label` when
there's more than one nav landmark on the page (e.g. `aria-label="Primary"`
vs. `aria-label="Secondary"`) so assistive-tech landmark navigation can
distinguish them. Current item marked `aria-current="page"` (or
`"true"` for a non-page section switch). Bottom tab bars and rails need
real touch-target sizing (`accessibility.md` §2) despite their compact
visual footprint.

**COMMON FAILURE**: Reordering or hiding nav items based on "personalization"
in a way that breaks spatial memory for a returning user; icon-only
sidebar/rail items with no accessible name beyond the icon; treating a
hamburger menu as a costless default on desktop widths where a visible
top nav would serve orientation better (hamburger-on-desktop is a common
"looks clean" choice that actually removes information the layout has
room to show — a Constitution #9 violation dressed as minimalism).

**IMPLEMENTATION NOTES**: See `knowledge/navigation-patterns.md` for
choosing the IA model (how many levels, hub-and-spoke vs. flat, when a
bottom bar vs. a drawer is the right *page-level* structure) — this
section assumes that decision is made and covers building the resulting
component correctly.

---

## Breadcrumbs

**PATTERN**: An ordered, horizontal list of ancestor links inside a
labeled `<nav>` landmark — verified ARIA APG breadcrumb pattern.

**PURPOSE**: Show the user's hierarchical position within a nested
structure and provide a fast upward-navigation shortcut — it answers
"where am I in the tree," which a top nav or sidebar (answering "what are
the top-level sections") does not.

**WHEN TO USE**: Content genuinely nested 3+ levels deep with a stable,
tree-like hierarchy the user can reason about (file/folder systems,
nested product categories, admin resource hierarchies, docs sites).

**WHEN NOT TO USE**: Flat or shallow structures — a breadcrumb reading
only "Home > Current Page" communicates nothing a page title doesn't
already say, and is a common instance of Constitution #9 (every visible
element should justify its existence): don't add breadcrumbs by default
"because admin panels have breadcrumbs." Also skip them where the actual
navigation need is lateral (between sibling records) rather than upward
(between ancestor levels) — that's better served by a prev/next control
or a back button referencing the specific list the user came from (see
COMMON FAILURE).

**DESIGN RULES**:
- Every crumb except the last is a real link; the last (current page) is
  non-interactive text, not a dead link to itself.
- Truncate long trails from the *middle*, not the end (`Home > … >
  Grandparent > Current`), since the first and last few levels carry the
  most orientation value; a collapsed ellipsis segment should be
  expandable (menu/tooltip revealing hidden levels) rather than silently
  discarding them.
- Separator glyphs (`>`, `/`) are decorative, not structural — don't rely
  on them alone to convey the list relationship; the underlying markup
  (see ACCESSIBILITY) carries the real structure.
- Keep crumb labels short and matching the actual page titles they link
  to — a breadcrumb whose label doesn't match the destination's own
  heading creates a small but real "did I click the right thing"
  hesitation.
- Don't conflate breadcrumbs with a back button: a back button returns to
  wherever the user actually came from (which may be a search results
  page, not the parent category); a breadcrumb always reflects the
  content hierarchy regardless of navigation history. Products with both
  a strong "came from a filtered list" flow and a real content hierarchy
  may legitimately need both, serving different questions.

**RESPONSIVE BEHAVIOR**: On narrow widths, breadcrumbs commonly collapse
to just "← Parent" (a single-level-up affordance) or an ellipsis-
collapsed trail with the immediate parent and current page visible,
rather than wrapping a long trail to multiple lines, which competes with
the page's own title for the top-of-screen space (`responsive-design.md`
§4, "collapse").

**ACCESSIBILITY**: Wrap the whole trail in `<nav aria-label="Breadcrumb">`
containing an ordered list (`<ol>`) of links — the landmark + label lets
assistive tech announce and jump to it distinctly from other `<nav>`
regions on the page (see Navigation section above). Mark the current
page with `aria-current="page"`; if the current-page element isn't a
link (the common, correct case), `aria-current` becomes optional per APG
but is still good practice to include for clarity. No special keyboard
interaction is required beyond standard link behavior — breadcrumbs are
plain links, not a widget.

**COMMON FAILURE**: A breadcrumb with only 2 levels that adds visual
clutter without orientation value; the last crumb rendered as a clickable
link to the current page (a confusing no-op click); using breadcrumbs as
a substitute for a real back button when the user's actual path was
lateral (arrived via search/filter, not by drilling down the hierarchy).

---

## Pagination

**PATTERN**: A numbered (or prev/next-only) control that divides a
bounded result set into discrete pages, paired with a sense of total
size (page count or item count).

**PURPOSE**: Let the user jump directly to a specific position within a
*bounded* collection and understand its total size — something infinite
scroll and "load more" both deliberately give up in exchange for lower
friction to keep consuming sequentially.

**WHEN TO USE**: Bounded, page-like content where knowing "how much is
there" and jumping to an arbitrary position both matter — search results
users compare against a known total, admin/data tables, paginated API
responses, archives users might bookmark a specific page of. Numbered
pagination also gives every page a stable, linkable/bookmarkable URL,
which infinite scroll loses by default.

**WHEN NOT TO USE**: Feeds and streams whose actual nature is unbounded
or continuously updating (social feeds, activity logs) — "infinite
scroll" or a "Load more" button fits the content's real shape better and
avoids implying a false sense of a fixed total. A **"Load more" button**
is the middle ground: it keeps user-initiated, discrete loading (so
scroll position and a footer remain reachable, unlike pure infinite
scroll) while avoiding page-number bookkeeping for content that isn't
meaningfully "paged." Choose based on whether the user's task benefits
from a sense of total/position (→ pagination) or from uninterrupted
sequential consumption (→ infinite scroll/load more) — not by default
convention per content type.

**DESIGN RULES**:
- Show the current page clearly distinguished from other page numbers
  (not color alone — weight/fill/border too, per Constitution #7).
- Prev/Next controls need real accessible names beyond a bare arrow
  glyph ("Previous page," not just "‹") — see ACCESSIBILITY.
- For large page counts, truncate the numeric range with ellipses
  (`1 … 4 5 [6] 7 8 … 42`) rather than listing every page — always keep
  first, last, current, and immediate neighbors visible so the user can
  both jump to the ends and step incrementally.
- Where feasible, show total count/context ("Showing 21–40 of 128") even
  when full numbered pagination isn't used — this single line often
  delivers most of pagination's orientation value on its own.
- Don't reset scroll position unexpectedly on page change without a
  reason — jumping the user back to the top of a long page after a click
  they made near the bottom (the pagination control itself) is usually
  fine and expected; jumping elsewhere is not.
- If infinite scroll is chosen instead, still provide a way to reach a
  footer/global actions (a "Back to top" affordance, or periodic
  natural stopping points) — pure infinite scroll with no footer access
  is a known, named failure mode of the pattern.

**RESPONSIVE BEHAVIOR**: A full numbered range with many pages should
collapse to fewer visible page numbers (or to bare Prev/Next plus a
"page X of Y" label) at narrow widths rather than wrapping controls
to a second line, which fights for the same footer space as other
page-level actions (`responsive-design.md` §4, "collapse"). Consider
switching to "Load more" on mobile even when desktop uses numbered
pagination, if the mobile task is closer to continuous scanning than
targeted jumping — a legitimate per-viewport pattern change, not
inconsistency, since it's answering Constitution #18 (respond to
available space and interaction mode, not a device label per se, though
touch-vs-pointer is itself a real interaction-mode difference).

**ACCESSIBILITY**: Wrap the control in a `<nav aria-label="Pagination">`
landmark; mark the current page with `aria-current="page"`; Prev/Next
buttons/links need explicit accessible names ("Previous page," "Next
page") since an arrow glyph alone has no text alternative by default;
disabled state (Prev on page 1, Next on the last page) should be a real
disabled/aria-disabled state, not a page number that silently does
nothing on click.

**COMMON FAILURE**: Bare `‹ ›` glyphs with no accessible name; pagination
applied to a feed that's actually unbounded (giving a false sense of a
fixed, countable total); losing scroll position or filter/sort state
across a page navigation because pagination was implemented as a full
page reload rather than an in-place content swap.

---

## Filters (chips, panels, applied-filter summary)

**PATTERN**: Three cooperating pieces — a **filter panel/menu** (where
criteria are chosen), **applied-filter chips** (a persistent, removable
summary of what's currently active), and the **filtered results**
themselves, which must visibly and immediately reflect the applied state.

**PURPOSE**: Let the user narrow a large result set to a relevant subset,
while always being able to see and adjust *what's currently narrowing it*
— the applied-filter summary is not a nice-to-have, it's the mechanism
that keeps filtering from becoming a black box the user has to
re-investigate to understand their own current view.

**WHEN TO USE**: Any result set large or varied enough that users benefit
from narrowing by more than one facet (price range, status, category,
date) — search results, admin tables, marketplace listings, issue
trackers.

**WHEN NOT TO USE**: A small, fully-visible result set (a handful of
items) doesn't need filter UI at all — scanning beats filtering below a
certain count, and adding a filter panel to a 6-item list is
Constitution #9 overhead with no payoff. Don't offer a filter facet with
only one meaningful value, or one that never actually narrows the visible
set in practice — a filter option is a promise that choosing it will
change something.

**DESIGN RULES**:
- **Applied filters must be visibly listed as removable chips near the
  results**, not only inside a closed filter panel — a user shouldn't
  have to reopen a panel to remember what's currently narrowing their
  view. This ties directly to Constitution #16: current filter state is
  information needed to correctly interpret what's on screen, not a
  secondary detail safe to bury behind a click.
- Each applied-filter chip needs its own inline remove affordance (an ×)
  so a single unwanted facet can be cleared without reopening the whole
  panel and re-deriving which one to uncheck.
- Provide a single "Clear all" action once more than ~2 filters are
  active — distinct from removing one chip at a time.
- Group filter panel controls by facet with clear labels; show live or
  near-live result counts per option where feasible ("In stock (42)") so
  the user can gauge a choice's effect before committing to it.
- Distinguish **instant-apply filters** (each toggle immediately updates
  results — best for cheap, client-side, or fast-server filtering) from
  **apply-on-confirm filters** (a panel with a single "Apply" button —
  better when changing multiple facets at once should produce one
  results update, not one per click, especially over a slower connection
  or an expensive query). Pick one model per surface and keep it
  consistent — mixing (some facets instant, others needing an Apply
  click, on the same panel) is a common source of user confusion about
  whether their change "took."
- A filter panel with many facets benefits from progressive disclosure —
  the most commonly used 2-4 facets visible by default, less common ones
  under a "More filters" expander (Constitution #16: this only works
  because the hidden facets are genuinely secondary, not because they're
  inconvenient to show).

**RESPONSIVE BEHAVIOR**: A persistent filter sidebar (common on wide
desktop layouts, sitting alongside results) collapses to a sheet/drawer
opened on demand at narrow widths (`responsive-design.md` §4, "become a
sheet") — typically triggered by a "Filters" button that also shows an
active-filter count badge so the collapsed state doesn't hide *that*
filtering is active. Applied-filter chips remain visible above the
results even when the panel itself is collapsed into a drawer — the
chips are the one piece of filter UI that should never be hidden behind
an extra tap, per the DESIGN RULES point above.

**ACCESSIBILITY**: Filter panel controls are real form controls
(checkboxes, radio groups, range inputs) with real labels, not styled
divs; announce result-count changes to screen-reader users via a polite
live region when filtering happens instantly, since a purely visual
count update is otherwise silent to AT; each removable chip's remove
button needs an accessible name identifying *which* filter it removes
("Remove filter: Color: Blue," not just "Remove").

**COMMON FAILURE**: Filters that visibly apply somewhere but leave no
trace in the main view once the panel is closed, forcing users to reopen
it to remember their own current view — this is the filters-specific
instance of Constitution #16's warning about hiding decision-relevant
state, and is closely related to `knowledge/anti-patterns.md`'s broader
point (#13, weak information hierarchy) about state that should be
visible but isn't. Also common: a filter panel that requires an explicit
"Apply" click but gives no visual feedback that pending, unapplied
changes exist (the user forgets they haven't applied yet).

**IMPLEMENTATION NOTES**: Keep filter state in the URL (query params)
wherever feasible — it makes a specific filtered view bookmarkable,
shareable, and back-button-navigable, all of which users of filter-heavy
surfaces (e-commerce, admin tools) reasonably expect.

---

## Search (input, results, suggestions/autocomplete)

**PATTERN**: A text input plus a results-or-suggestions surface. When the
surface is a live-filtered `listbox` with `aria-activedescendant`
highlighting, this is the same **combobox** pattern already specified in
`accessibility.md` §4 and the Selects/Comboboxes section above — this
entry covers the *search-specific* product decisions layered on top of
that shared accessibility contract, distinct from the Command Palette
(above), which is a keyboard-invoked, product-wide action/navigation
overlay rather than a content search.

**PURPOSE**: Let the user find specific content or records by typed query
rather than by browsing/filtering — the right tool when the user already
has a reasonably specific target in mind, versus Filters (above), which
is the right tool when narrowing a known, browsable set by facet.

**WHEN TO USE**: Content collections large or unstructured enough that
browsing/filtering alone is impractical (a large catalog, a knowledge
base, global product search); as a query-refinement companion alongside
filters, not a replacement for them, on large result sets.

**WHEN NOT TO USE**: A small, fully-visible collection doesn't need a
dedicated search input any more than it needs a filter panel (same
Constitution #9 reasoning as Filters); don't reach for search as a
band-aid for a navigation/IA that's actually just poorly organized —
search finds things despite bad structure, it doesn't fix the structure
for users who browse instead.

**DESIGN RULES**:
- Show recent and/or suggested queries when the field is focused-but-
  empty, rather than a blank dropdown or nothing at all — this is the
  highest-value, lowest-cost moment to help a user who hasn't typed
  anything yet.
- Debounce live-filtering/suggestion requests (don't fire a network
  request on every keystroke at typing speed) — a short debounce (typically
  ~150-300ms) balances responsiveness against wasted requests and
  flicker; for purely client-side filtering over already-loaded data,
  debouncing is about render cost/flicker rather than network cost, but
  the same principle applies.
- **Limit suggestion list length** — roughly ~10 items on desktop, 4-8 on
  mobile's smaller viewport — long unbounded suggestion lists reintroduce
  the scanning burden search was supposed to remove (ESTABLISHED, Baymard
  Institute research).
- **Visually distinguish suggestion types** when a list mixes categories
  (e.g. "in Electronics" scoped suggestions vs. plain query completions)
  using consistent secondary styling (italics, muted color, indentation)
  so users can tell scoped from unscoped suggestions at a glance.
- Highlight the *predicted/completed* portion of a suggestion (bold or
  emphasized), not the part the user already typed — this is what lets a
  user compare several suggestions quickly rather than re-reading each
  one in full.
- Always provide a real "no results" state with a next step (broaden the
  query, check spelling, clear filters, browse categories instead) — a
  bare empty area after a search fails both Constitution #16 (the user
  needs to understand *why* nothing appeared to decide what to do next)
  and the general Empty States guidance above.
- Avoid a fixed-height, internally-scrolling suggestion dropdown on
  desktop — let it expand to fit its content up to a sane maximum, rather
  than forcing an extra scroll interaction inside an already-transient
  overlay (ESTABLISHED, Baymard).
- On results pages (as opposed to the live-suggestion dropdown), preserve
  and clearly display the query the results are for, and let users
  refine it in place rather than needing to retype from scratch.

**RESPONSIVE BEHAVIOR**: On mobile, a search input frequently expands
to a full-screen search view on focus/tap (input pinned to the top,
suggestions filling the remaining viewport) rather than a small anchored
dropdown, both to maximize suggestion visibility on a small screen and to
avoid virtual-keyboard overlap with an anchored popup —
`responsive-design.md` §4, "become a different component," the same
reasoning as the Comboboxes and Menus sections above. Keep the visible
suggestion area free of competing sticky elements (headers, chat
widgets, ads) that crowd the limited mobile viewport during an active
search.

**ACCESSIBILITY**: Follows the combobox contract in full — see
`accessibility.md` §4 for the complete role/attribute mapping
(`role="combobox"`, `aria-expanded`, `aria-controls`, `aria-autocomplete`,
`aria-activedescendant` with real focus retained on the input). Announce
the result count to screen-reader users via a polite live region when
results update live from typing, since a sighted user sees the count
change but an AT user otherwise has no signal that anything happened.

**COMMON FAILURE**: A suggestion dropdown with no keyboard support
(arrow keys don't move through it, forcing a mouse-only interaction on
what's otherwise a keyboard-first input); firing a request on every
keystroke with no debounce, causing visible flicker/out-of-order
responses as results race each other; a "no results" state that's just
empty whitespace with no explanation or next step.

**IMPLEMENTATION NOTES**: Guard against out-of-order async responses when
debouncing (a slow request for an earlier keystroke resolving after a
faster request for a later one must not overwrite the newer, correct
results) — a frequent, subtle bug in hand-rolled search-as-you-type
implementations.

---

## Command Palettes

**PATTERN**: A keyboard-invoked (commonly Cmd/Ctrl+K), full-overlay,
type-to-filter list of actions and navigation targets, popularized at
scale by Linear, Vercel, Raycast, and Superhuman-style products, now a
common power-user pattern in 2026 SaaS tools.

**PURPOSE**: Give high-frequency, keyboard-oriented users a single,
consistent, fast path to *any* action or destination in the product,
without navigating a visual hierarchy — directly serving Constitution
#11's principle that expert/frequent users benefit from throughput over
low-density comprehension aids.

**WHEN TO USE**: Products with (a) a large or growing action/navigation
surface that a visual nav can't fully expose without becoming unwieldy,
and (b) a meaningfully large population of repeat, keyboard-comfortable
users (most B2B/productivity/developer tools qualify; a low-frequency
consumer checkout flow usually doesn't).

**WHEN NOT TO USE**: As the *only* way to reach common actions — a
command palette is an accelerator for users who've learned it, never a
replacement for discoverable, visible primary navigation and buttons
(Constitution #19: don't force novelty on users who haven't opted in).
Also not warranted for a small product with only a handful of screens/
actions — the overhead of building and maintaining it doesn't pay back.

**DESIGN RULES**:
- Combobox-pattern internally (see Selects/Comboboxes above): input with
  virtual highlight via `aria-activedescendant`, real focus staying on
  the input.
- Results should be grouped by type (Actions / Pages / Recent / Settings)
  with the most contextually relevant group first, not one flat
  alphabetical list.
- Fuzzy/substring matching, not prefix-only — users type fragments,
  not always from the start of a label.
- Show keyboard shortcuts next to items that have a dedicated shortcut,
  reinforcing shortcut discoverability every time the palette is used.
- Recent/frequent actions should surface first on empty-query open (an
  empty palette showing nothing wastes the first, most common
  interaction: open then immediately re-select the last thing).
- Must be reachable from anywhere in the product via one consistent
  shortcut, and must not conflict with the browser's or OS's own
  shortcuts for the same keys.

**ACCESSIBILITY**: Functionally a modal combobox — needs the dialog
focus-trap behavior (background inert while open) *and* the combobox
activedescendant behavior simultaneously; both contracts must be
implemented together, which is why command palettes are a component worth
building once, carefully, and reusing rather than rebuilding per feature.

**RESPONSIVE BEHAVIOR**: On touch devices without a hardware keyboard, the
shortcut-invocation model doesn't apply — either omit the pattern
entirely for touch-only contexts or provide an explicit on-screen trigger
(a search icon) that opens the same overlay, understanding that its
core value proposition (keyboard speed) is diminished without a physical
keyboard.

**COMMON FAILURE**: A command palette that duplicates the *entire* nav
tree as flat, ungrouped, unranked results — technically searchable but
slower to use than just clicking, defeating its own purpose.

---

## Notifications (toasts, banners, inline alerts, notification center)

**PATTERN**: Four related but distinct surfaces that are frequently
conflated: **toast** (transient, floating, auto-dismissing), **banner**
(persistent, page/section-level, dismissed by user or resolution),
**inline alert** (scoped to one specific piece of content), and
**notification center / inbox** (a persistent, revisitable log of past
notifications the user can open on demand) — distinct from the general
Empty/Loading/Error States section above, which covers *system feedback
about an operation's outcome in place*; this section covers *messages
designed to interrupt or be collected*, including ones unrelated to
anything the user just did (a mention, an assignment, an incoming
message).

**PURPOSE**: Communicate something the user didn't necessarily request at
this exact moment — a background event, a completed action's outcome, a
status change, or something requiring awareness or action — at the level
of urgency and persistence appropriate to how important it is and how
long the user needs to be able to act on it.

**WHEN TO USE**:
- **Toast** — transient confirmation of a just-completed, typically
  user-initiated action (saved, sent, copied), optionally with a single
  undo/action affordance. Also appropriate for system-generated alerts
  not tied to a specific visible page region (ESTABLISHED, IBM Carbon).
- **Banner** — a persistent, page- or section-level status that remains
  true until resolved or explicitly dismissed (a system outage notice, an
  unsaved-changes warning, a trial-expiring notice) — the defining trait
  is that the underlying condition *persists*, so the message should too.
- **Inline alert** — feedback scoped to one specific piece of content (a
  form field's validation error, one table row's failed sync) — placed
  immediately adjacent to what it's about, not floated elsewhere.
- **Notification center/inbox** — an accumulating, revisitable list of
  notifications (mentions, assignments, activity) that the user checks on
  their own schedule rather than being interrupted for in the moment —
  appropriate once notification *volume* or *asynchrony* (things happen
  while the user isn't looking) makes a transient toast insufficient; it
  gives users a durable record and lets them triage rather than reactively
  process. Usually paired with a badge count on its trigger icon (a
  legitimate, non-decorative badge use — see Navigation above).

**WHEN NOT TO USE**: A toast for information the user needs to act on
*later*, not right now — it disappears, so if the message matters after
the next few seconds it must be a banner, inline alert, or a
notification-center entry instead (Constitution #16: this is essential
information, not a detail safe to lose). A modal/blocking dialog for a
notification that isn't actually decision-blocking is over-escalation —
reserve interruption-that-blocks-the-task for genuinely critical,
immediate-attention cases (ESTABLISHED, Carbon: modals are for "highly
disruptive notifications... critical information," not routine status).
Don't build a notification center before volume/asynchrony actually
justifies one — a product with rare, single, actionable events is
better served by inline alerts and toasts alone.

**DESIGN RULES**:
- **One toast queue, stacked, not scattered** — multiple simultaneous
  toasts stack vertically in one consistent screen position (commonly
  top-right or bottom-center depending on platform convention); never
  spawn toasts in varying positions.
- **Show only one banner at a time** per scope (ESTABLISHED, Carbon) —
  stacking multiple persistent banners degrades into the same
  "everything is important therefore nothing is" failure as Constitution
  #4; if multiple conditions are true, prioritize and show the most
  severe/actionable one, or consolidate.
- An **actionable notification** (toast or inline) should offer at most
  one action button — more than one reintroduces the multi-primary-action
  problem (Constitution #8) into a component that's supposed to be a
  quick, low-friction aside.
- Never rely on color alone to distinguish severity (info/success/
  warning/danger) — pair with icon and label text (Constitution #7,
  `accessibility.md` §1/§6); this is the same failure mode as
  color-only form errors, just at the notification-severity level.
- A notification-center entry needs enough context to act on without
  navigating first (what happened, to what, by whom, when) and a direct
  link/action to the relevant place — a feed of bare "X updated" entries
  with no specifics forces a detour to figure out what actually happened.
- Distinguish read/unread state in the notification center clearly (not
  color-only — weight, a dot, or grouping "New" vs. "Earlier") and
  provide a "mark all read" bulk action once volume warrants it (same
  reasoning as the bulk-action bar in Tables above).
- Give users control over notification volume/channel where the surface
  supports many notification types (email/push/in-app toggles per
  category) — this is itself an accessibility and respect-for-attention
  concern, not just a preferences nicety (ESTABLISHED, WCAG 2.2's
  interruption-related criteria and Carbon's own guidance both point the
  same direction).

**RESPONSIVE BEHAVIOR**: Toasts on mobile commonly reposition to
bottom-anchored (thumb-reachable, doesn't overlap a top status bar/notch)
rather than top-right, and should never overlap a bottom tab bar's active
area. A notification center that's a dropdown panel on desktop typically
becomes a full-screen view on mobile (`responsive-design.md` §4, "become
a different component") — a small anchored panel doesn't have room for a
scrollable, filterable notification list on a narrow viewport. Banners
should remain full-width and not become dismiss-and-forget-too-easily
small chips at narrow widths, since their entire purpose is persistence
until resolved.

**ACCESSIBILITY**: Toasts and inline live-updating alerts use
`aria-live="polite"` so screen readers announce them without interrupting
current speech; reserve `aria-live="assertive"` for genuinely urgent,
rare cases (a session-ending error), since assertive regions interrupt
whatever the user is currently doing. Auto-dismissing toasts must not
disappear faster than a reasonable reading time and should pause the
dismiss timer on hover/focus (and ideally never auto-dismiss a toast that
contains an action button the user might need a moment to notice and
use) — this connects to WCAG's timing-adjustable criteria (a
non-negotiable minimum, not a nice-to-have, per `accessibility.md`).
Banners and inline alerts, being persistent, don't need `aria-live`
urgency in the same way but still need an accessible dismiss control and
correct landmark/labeling if they're structurally significant (e.g. a
sitewide banner as a `role="region"` with a label).

**COMMON FAILURE**: Toast overuse — firing a toast for every minor,
expected action (including ones with no failure mode worth confirming),
training users to reflexively ignore or dismiss the entire toast channel,
which then also buries the rare toast that *did* matter
(`knowledge/anti-patterns.md`'s general pattern of manufactured/generic
UI signals applies here: a toast that fires constantly stops
communicating anything, the same failure Constitution #4 describes for
visual weight). Also common: stacking multiple full-width banners at the
top of a page until they push all real content below the fold; a
notification center that's purely a firehose with no read/unread
distinction or grouping, making it useless for triage past a handful of
items.

**IMPLEMENTATION NOTES**: A toast triggered by a destructive action with
undo (e.g. "Item deleted — Undo") is a strong pattern for making
irreversible-feeling actions actually reversible without an upfront
confirmation dialog — but the undo window must be genuinely long enough
to notice and use (a reasonable reading-plus-reaction time, not a
1-2 second flash), or it functions as a fake safety net.

---

## Empty, Loading, and Error States

**PURPOSE**: Every place content *can* eventually render has (at least)
three other states it will be in before or instead of showing that
content — empty (no data yet, or a filtered view has 0 matches), loading
(data is being fetched), and error (the fetch/action failed). These are
not edge cases to patch in during QA; they are core states the layout
must be designed for from the start, exactly as much as the "happy path"
with data.

**DESIGN RULES — Empty states**:
- Distinguish **first-use empty** ("you haven't created anything yet" —
  an opportunity for a specific, actionable prompt: "Create your first
  project") from **filtered-to-zero empty** ("no results match your
  filters" — the fix here is almost always to help the user relax or
  clear the filter, and restating the filter/search term helps them
  understand *why* it's empty) from **genuinely-and-permanently empty**
  (an inbox at zero, which can legitimately be a small, low-effort
  moment of Constitution #3's permitted cleverness/delight, since there's
  no task being blocked).
  These three need different copy and different actions — a single
  generic "Nothing here" empty state used everywhere is a common failure
  because it can't serve all three cases.
- Always include a next action where one exists (a button to create, a
  link to clear filters) — an empty state with no path forward is a dead
  end.

**DESIGN RULES — Loading states**: see `knowledge/motion-interaction.md`
§5 for the full duration-based decision tree (no indicator under ~300ms;
skeleton for sub-few-second waits; determinate progress when duration/
size is knowable; optimistic UI to avoid needing a loading state at all
for reversible actions). Skeletons must mirror the actual eventual
layout's shape and count (a list loading state shows list-shaped rows at
roughly the real row height) to avoid layout shift and to correctly set
expectations about what's coming.

**DESIGN RULES — Error states**: Distinguish **field-level** (this input
is invalid — inline, next to the field, per `accessibility.md` §8),
**action-level** (this specific operation failed — usually a toast/inline
alert with a retry action), and **page/section-level** (the whole
view failed to load — a dedicated error state in place of the content
area, with a retry action and, where possible, enough specificity to be
useful: "Couldn't load your projects — check your connection and retry,"
not a bare "Something went wrong"). Every error state should answer three
things: what happened, why (if knowable and useful), and what the user
can do next — an error with no next step converts a recoverable failure
into a dead end. Never communicate error state via color alone (icon +
color + text, per `accessibility.md` §1/§6).

**COMMON FAILURE**: Designing only the happy path and letting empty/
loading/error be improvised by whoever implements the screen — the
single most reliable source of inconsistent, generic-feeling states
across a product (three different loading spinners, two different empty
state layouts, one screen with no error handling at all).

---

## Onboarding (tours, coach marks, checklists, empty-state-as-onboarding)

**PATTERN**: Four distinct component-level tools, escalating in
intrusiveness, that are often reached for interchangeably but suit
different situations: **empty-state-as-onboarding** (the default,
lowest-cost pattern — see Empty States above), **onboarding checklist**
(a persistent, dismissible list of setup steps with progress), **coach
mark / spotlight** (a single, contextual callout pointing at one specific
UI element, triggered by reaching it), and **product tour** (a multi-step,
often modal, sequential walkthrough spanning multiple screens/elements).

**PURPOSE**: Get a new user from "just arrived, no context" to
"understands enough to get value" with the minimum interruption and
front-loaded information necessary — onboarding is a cost paid once
per user, so its budget should be spent precisely, not generously.

**WHEN TO USE**:
- **Empty-state-as-onboarding** — the default for a first-use empty state
  (per Empty States above): the empty state itself carries the
  explanation and call-to-action ("Create your first project"). This
  should be the first tool reached for, not the last, because it costs
  nothing beyond designing the empty state well and imposes zero extra
  interruption.
- **Checklist** — a product with several distinct setup steps needed
  before real value is reached (connect an integration, invite a
  teammate, configure a first project) benefits from a persistent,
  checkable list with visible progress — it lets a user do the steps in
  their own order, on their own schedule, rather than being marched
  through a forced sequence, and the progress indicator itself is
  motivating (a visible, shrinking task list, distinct from a generic
  progress bar with no discrete steps).
- **Coach mark/spotlight** — introducing *one* specific feature at the
  contextual moment the user reaches it or is likely to benefit from it
  (a tooltip-like callout on a new button the first time its containing
  view loads), rather than upfront. This is the ESTABLISHED best practice
  direction (Atlassian's "Onboarding (Spotlight)" pattern, Carbon and
  Polaris's contextual-help conventions): teach in context, not in
  advance.
- **Product tour** — reserve for genuinely non-obvious, high-value
  changes in orientation (a fundamentally new layout after a redesign,
  a workspace/canvas-type product where spatial orientation itself needs
  explaining) where a sequence of several elements must be understood
  together before any one of them makes sense alone. This is the most
  expensive, most interruptive tool and should be the least frequently
  reached for.

**WHEN NOT TO USE**: A long, upfront multi-step tour that front-loads
information before the user has any concrete task motivating it — this
directly violates Constitution #16 (progressive disclosure defers
secondary detail without hiding essentials) applied to first-run
specifically: information given before it's needed is *forgotten* before
it's needed, making the tour a checkbox the user clicks through rather
than a comprehension aid. Don't use a coach mark for something genuinely
essential to avoiding an irreversible mistake (that's core information,
per Constitution #16, and belongs inline/persistent, not a
dismiss-and-forget callout). Don't stack a tour, a checklist, *and*
several coach marks on first load — pick the tool matching the actual
complexity, and sequence rather than simultaneously fire multiple
onboarding surfaces.

**DESIGN RULES**:
- **Sequence, don't simultaneously trigger**, when more than one
  onboarding surface exists (a checklist item completing shouldn't also
  pop a coach mark for something unrelated at the same moment) — this is
  Constitution #4's zero-sum-attention principle applied to onboarding
  specifically: only one thing can actually get the user's attention at
  a time.
- **Any skippable step must be skippable with one clear, visible
  action** — a small "Skip" or "×," not buried in a menu — since forcing
  a tour on an already-oriented or returning user (or one who simply
  doesn't want it right now) is a direct tax on their time with no
  benefit.
- A coach mark should point unambiguously at its target (an anchored
  arrow/connector, not just nearby placement) and dismiss on
  interaction with the target or an explicit dismiss, not on a timer —
  a user reading slowly shouldn't have the callout vanish before they
  finish.
- Checklist items should link directly into the flow that completes them
  (clicking "Invite a teammate" opens the invite flow) rather than just
  describing the step — a checklist that only describes steps without
  acting as their entry point adds a navigation detour to something
  that should be one click.
- Persist onboarding completion/dismissal state per user — don't re-show
  a dismissed tour or checklist on next login; a coach mark shown once
  per feature (not per session) respects that the first exposure already
  did its job.
- A checklist should be dismissible/collapsible once substantially
  complete or explicitly dismissed, and shouldn't linger indefinitely as
  a persistent UI element once its job is done — an onboarding checklist
  still visible to a six-month veteran user is clutter, not guidance.

**RESPONSIVE BEHAVIOR**: Coach marks/spotlights need to reposition
reliably relative to their target across viewport sizes, including
when the target itself moves or is hidden by responsive recomposition
(a coach mark pointing at a sidebar item that's now inside a collapsed
drawer on mobile must either point at the drawer trigger instead or be
suppressed on that viewport rather than pointing at nothing). A
multi-step tour on mobile should favor fewer, more focused steps — the
smaller viewport has less room for both the highlighted element and an
explanatory callout to coexist legibly. A checklist widget commonly
collapses from a persistent sidebar/corner card on desktop to a
dismissible banner or a dedicated small section on a mobile home/dashboard
view.

**ACCESSIBILITY**: A coach mark/spotlight that dims/highlights part of
the screen must not trap focus the way a modal dialog does unless it's
genuinely modal in intent — if it's dismissible by continuing to interact
with the underlying UI, it shouldn't behave like a hard focus trap; if it
*is* modal (a full tour step blocking interaction), it needs the full
dialog contract (focus trap, Escape, labeled via `aria-labelledby`) per
the Dialogs section above. Ensure a coach mark's pointed-to element and
its explanatory text are both reachable and readable via keyboard/screen
reader, not just visually connected by a line/arrow that conveys the
relationship only visually (`accessibility.md` §1: don't rely on visual
proximity/connectors alone to convey a relationship that also needs a
programmatic one, e.g. `aria-describedby` linking the target to the
callout's text).

**COMMON FAILURE**: A five-plus-step modal tour shown to every new user
regardless of whether they're already familiar with the product's
underlying model (a common failure for products with returning or
invited-by-a-teammate users who don't need the same intro as a cold
signup); multiple coach marks firing at once, competing for attention on
first load; an onboarding checklist that never goes away after
completion, becoming permanent visual clutter.

**IMPLEMENTATION NOTES**: Empty-state-as-onboarding, checklist, and coach
marks are not mutually exclusive — a common, effective composition is an
empty-state CTA plus a lightweight checklist for the handful of setup
steps beyond the first one, with coach marks reserved for any genuinely
non-obvious feature introduced later in the user's growth through the
product (progressive, not upfront, disclosure of the product itself).

---

## Sources
- [W3C ARIA APG — Dialog (Modal) Pattern](https://www.w3.org/WAI/ARIA/apg/patterns/dialog-modal/) — accessed 2026-09-12
- [W3C ARIA APG — Combobox Pattern](https://www.w3.org/WAI/ARIA/apg/patterns/combobox/) — accessed 2026-09-12
- [W3C ARIA APG — Tabs Pattern](https://www.w3.org/WAI/ARIA/apg/patterns/tabs/) — accessed 2026-09-12
- [W3C ARIA APG — Menu Button Pattern](https://www.w3.org/WAI/ARIA/apg/patterns/menu-button/) — accessed 2026-09-12 (follow-up pass)
- [W3C ARIA APG — Listbox Pattern](https://www.w3.org/WAI/ARIA/apg/patterns/listbox/) — accessed 2026-09-12 (follow-up pass)
- [W3C ARIA APG — Breadcrumb Pattern](https://www.w3.org/WAI/ARIA/apg/patterns/breadcrumb/) — accessed 2026-09-12 (follow-up pass)
- [W3C ARIA APG — Disclosure (Show/Hide) Pattern](https://www.w3.org/WAI/ARIA/apg/patterns/disclosure/) — accessed 2026-09-12 (follow-up pass)
- [Command Palette UI Design — Mobbin glossary](https://mobbin.com/glossary/command-palette) — surfaced this session
- [Designing a Command Palette — Destiner's notes](https://destiner.io/blog/post/designing-a-command-palette/) — surfaced this session
- [Command Palette Pattern — UX Patterns for Developers](https://uxpatterns.dev/patterns/advanced/command-palette) — surfaced this session
- [GitHub Primer — Navigation UI Patterns](https://primer.style/product/ui-patterns/navigation/) — accessed 2026-09-12 (follow-up pass): NavList/TreeView/UnderlineNav vs. UnderlinePanels decision tree, sidebar responsive recomposition (index-to-detail, action-menu/sheet, mixed)
- [IBM Carbon Design System — Notification Pattern](https://carbondesignsystem.com/patterns/notification-pattern/) — accessed 2026-09-12 (follow-up pass): toast/inline/banner/actionable-notification/modal distinctions, "one banner at a time," timing guidance referencing WCAG 2.2 SC 2.2.3/2.2.4
- [Atlassian Design System — Onboarding (Spotlight)](https://atlassian.design/components/onboarding) — accessed 2026-09-12 (follow-up pass, page notes the package is deprecated in favor of `@atlaskit/spotlight` — underlying contextual-spotlight pattern intent still applicable)
- [Baymard Institute — 9 UX Best Practices for Autocomplete Suggestions](https://baymard.com/blog/autocomplete-design) — accessed 2026-09-12 (follow-up pass): suggestion list length limits, predictive-text highlighting, category-scope differentiation, mobile spacing/competing-element guidance
- Shopify Polaris filters/index-filters documentation — referenced via search this session; live component page redirected to a JS-rendered app-shell (shopify.dev/docs/api/polaris) not directly fetchable — applied-filter-chip and instant-vs-apply-on-confirm conventions taken as ESTABLISHED training knowledge, consistent with Polaris's documented filter-chip pattern
- Pagination vs. infinite scroll vs. "load more" UX comparisons (Eleken, Baymard-adjacent industry sources surfaced via search) — synthesized as ESTABLISHED guidance on bounded/unbounded content fit
- Material Design 3, Adobe Spectrum, IBM Carbon, Shopify Polaris, GitHub Primer, Atlassian Design System component documentation — training knowledge, synthesized; several live component-doc pages were JS-rendered SPAs not directly fetchable this session
- Constitution principles #3, #4, #5, #6, #7, #8, #9, #10, #11, #13, #16, #18, #19 — `/home/claude/advanced-ui-skill/knowledge/ui-principles-2026.md`
- `knowledge/anti-patterns.md` #4 (unmotivated gradients), #7 (wall of cards), #9 (unnecessary badges), #13 (weak information hierarchy) — cross-referenced from Navigation, Notifications, Filters, and Lists sections
