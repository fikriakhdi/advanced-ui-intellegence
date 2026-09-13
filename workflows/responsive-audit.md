---
title: Responsive Audit
last_updated: 2026-09-12
---

# Workflow: Responsive Audit

**Purpose.** Systematically audit a design or implementation across
viewport/container sizes, using the recomposition model from
Constitution #10 and #18 rather than a "does it shrink without
breaking" check. See `knowledge/responsive-design.md` and
`knowledge/mobile-patterns.md` for the underlying pattern knowledge
this audit applies — this workflow is the procedure, those files are
the reference.

**Framing reminder (Constitution #18):** audit by *available space to
the component*, not by device label. A component can be "mobile-narrow"
inside a desktop sidebar or split view. Breakpoints should be checked
as content/layout inflection points, not as a phone/tablet/desktop
checklist alone — though device-label checks remain a useful sanity
pass for full-page layouts.

---

## The recomposition framework

For every component being audited, at every width/breakpoint where its
available space changes meaningfully, decide explicitly which of these
applies — never leave it as an unexamined "same as before, just
smaller":

- **Keep** — unchanged; still works at this width.
- **Move** — relocates (e.g. a sidebar filter moves above the content).
- **Collapse** — condenses but stays visible (a multi-column form
  becomes single-column; a toolbar's labels drop, icons remain).
- **Hide** — removed at this width, with the information/action still
  reachable elsewhere (never hidden with no path back — check against
  Constitution #16's progressive-disclosure test).
- **Merge** — multiple elements combine into one (several icon buttons
  become one overflow menu).
- **Become scrollable** — a fixed grid/table becomes a scrollable
  region (horizontal scroll for a wide table, rather than breaking its
  columns).
- **Become a sheet** — a popover/dropdown/side panel becomes a bottom
  sheet or full-screen modal at narrow widths, matching touch
  ergonomics.
- **Become a different component** — the pattern itself changes (a
  data table becomes a stacked card list; tabs become a select/accordion).

A component that has never been assigned one of these eight and is
just "the desktop version, scaled" has not actually been audited — that
is precisely the failure Constitution #10 names ("recomposed, not
shrunk").

---

## Procedure: component-by-component, breakpoint-by-breakpoint

1. **Enumerate components** in the screen/flow being audited (nav,
   primary content sections, cards, tables, forms, modals/overlays,
   any floating UI).
2. **Enumerate breakpoints** relevant to this design — derived from
   where its own layout actually needs to change (content-driven per
   Constitution #18), typically checked at minimum: narrow
   (~360-400px), medium (~768px), wide (~1024-1280px), and any custom
   inflection point the design defines (e.g. "3-column grid drops to 2
   at 900px"). Also check any known non-full-page contexts (embedded
   in a sidebar, a split view, a resizable panel) if applicable.
3. **For each component × breakpoint cell**, fill in:

   | Component | Breakpoint | Recomposition strategy | Verified? | Notes/issues |
   |---|---|---|---|---|
   | e.g. Data table | ≤480px | Become a different component (stacked cards) | ☐ | Sort control needs a new home — check it's not lost |

4. **Cross-cutting checks** at every breakpoint, independent of any
   single component:
   - [ ] No horizontal scroll on the page body itself (only intentional
     `overflow-x` containers — tables, code blocks, carousels).
   - [ ] Touch targets meet minimum size at touch-capable widths (cross-
     reference `workflows/accessibility-audit.md`'s touch-target check).
   - [ ] No content or primary action becomes unreachable at any
     audited width (everything hidden per the framework has a path
     back).
   - [ ] Information hierarchy (Constitution #1, #7) survives the
     recomposition — the most important content is still first/most
     prominent at every width, not just at the original design width.
   - [ ] Density adjusts sensibly (Constitution #11) — a narrow viewport
     on a high-density expert tool may still warrant more density than
     a consumer app's narrow view, don't over-simplify by reflex.
   - [ ] Motion/transition used for the recomposition itself (e.g. a
     panel sliding into a sheet) is justified per Constitution #12,
     not decorative.
5. **Resize-drag test** (when auditing a live implementation, not just
   static designs): drag the viewport slowly across each breakpoint
   boundary and watch for layout jumps, content reflow flashes, or a
   component getting stuck between two recomposition states.

## Reporting

For each cell marked with an issue, report it in the same PROBLEM /
WHY IT MATTERS / SEVERITY / RECOMMENDED CHANGE / EXPECTED IMPROVEMENT
format used in `workflows/critique-ui.md`, so responsive findings can
be merged into a broader critique pass without translation.

## Done when

Every component has an assigned recomposition strategy at every
audited breakpoint, every cross-cutting check passes, and any open
issues are logged with severity — not silently left for "someone to
notice later."
