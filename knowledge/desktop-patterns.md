---
title: Desktop Patterns
confidence-note: Synthesized from cross-platform desktop HIGs, accessibility standards (WCAG hover/focus requirements), and observed conventions in professional tools (IDEs, creative/office suites); numeric and standards claims are FOUNDATIONAL, workflow claims are ESTABLISHED unless noted.
last_updated: 2026-09-12
---

# Desktop Patterns

Desktop is not "mobile with more room" — it has affordances mobile
structurally lacks (a precise pointer, hover, a keyboard always present,
large stable viewports, multiple windows) and this file is about designing
*for* those affordances rather than just having space left over after a
mobile-first design. This complements Constitution #10: desktop is the
other end of the recomposition spectrum, and #18 (respond to available
space, not device labels) — a "desktop pattern" really means "a
generous-space, pointer-and-keyboard-present pattern," which can also
appear in a maximized tablet browser window or a large in-app panel.

## Cursor Precision and What It Unlocks

**FOUNDATIONAL.** A mouse/trackpad pointer resolves to roughly a single
pixel, provides continuous hover state as it moves, and separates
"pointing at" from "activating" (hover vs. click) — none of which touch
input can do (a finger is ~10mm wide, has no hover state, and pointing and
activating are the same event).

**WHAT THIS UNLOCKS**
- **Hover previews** — revealing more detail (a tooltip, an expanded
  thumbnail, a preview pane) without committing to a click/navigation.
- **Fine drag interactions** — resizing a column to the pixel, drawing a
  precise selection marquee, fine-grained slider control — all rely on
  sub-pixel-feeling precision touch cannot match.
- **Hover-scoped affordances** — showing row actions (edit/delete icons)
  only when the pointer is over that row, keeping the resting UI quiet
  (Constitution #5's "meaningful whitespace" applies here: hover-reveal is
  a way to keep density high without permanent visual noise).
- **Right-click context menus** — a whole secondary-action surface that
  costs zero permanent screen space.
- **Cursor feedback** — changing cursor shape (resize handles, grab/grabbing,
  not-allowed) as a low-cost, continuous signal of what an element affords,
  something touch has no equivalent for.

## Hover-Dependent Affordances Must Degrade Gracefully

**ACCESSIBILITY / FOUNDATIONAL.** WCAG 2.1 Success Criterion 1.4.13
(Content on Hover or Focus) and general accessibility guidance require that
anything revealed on hover must also be revealed on keyboard focus, must be
dismissible without moving the pointer, and must remain visible while the
user is interacting with it. More fundamentally for this project's
constitution: **hover must never be the *sole* means of discovering that a
function exists** (this is the desktop instance of the same principle
mobile gesture-only interactions violate — see
`knowledge/mobile-patterns.md`).

**DESIGN RULES**
- Any hover-revealed action (row actions, hover toolbars on an image/card)
  needs a keyboard-reachable equivalent: focus state shows the same
  affordance, and/or the action is also reachable via a persistent overflow
  menu (a "⋯" button) for touch/keyboard-only/screen-reader users.
  Touchscreen laptops and tablets running desktop-pattern web apps have no
  hover at all — the persistent fallback is not optional.
  **FOUNDATIONAL.**
  {ties to Constitution #14 — accessibility as a design constraint, not a
  checklist item to patch on later.}
- Hover states should have a short, deliberate delay or none at all for
  reveal, but dismiss promptly on pointer-out — a lagging hover state feels
  broken, an instant one causes flicker when moving across a dense row of
  elements.
- Tooltips triggered by hover must not be the only place critical
  information lives (Constitution #16) — a tooltip explaining what an icon
  *does* is fine; a tooltip containing the only copy of a required warning
  is not.

## Multi-Pane / Split-View Layouts

**PATTERN.** Two or more independently-scrollable regions shown
side-by-side: a list-detail split (email client, file manager), a
source-preview split (code editor + rendered output), or a three-pane
layout (navigation | list | detail).

**PURPOSE.** Let the user see context and content simultaneously instead of
navigating back and forth, which is exactly the tradeoff desktop's width
affords and mobile structurally cannot (this is why multi-pane is a prime
"become a different component" target when recomposing to mobile —
typically becomes a stack of full screens with back navigation).

**WHEN TO USE.** The user's task inherently involves *comparing or
correlating* two views (an item in a list against its detail, code against
its rendered result) and doing so repeatedly across many items in one
session — the setup cost of a split pane pays for itself over many
list→detail transitions.

**WHEN NOT TO USE.** A single, deep, sequential task (a checkout, a
wizard) has no need for a persistent side pane; forcing a split-view
there just steals width from the one pane that matters.

**DESIGN RULES**
- Make the divider between panes draggable/resizable (see below) — a fixed
  split forces every user into one designer's guess about proportion.
- Persist the user's chosen split ratio and open/closed pane state across
  sessions where the tool is used repeatedly (professional/expert context,
  Constitution #11) — re-deriving your preferred layout every session is a
  throughput tax on exactly the users density/persistence is meant to serve.
- The detail pane should update from a single click/selection in the list
  pane, not a separate navigation action — the entire point of the pattern
  is eliminating that round-trip.

**RESPONSIVE BEHAVIOR.** Below a width threshold (container-query, per
Constitution #18) a multi-pane layout should collapse to a single pane with
navigation, rather than rendering unreadably narrow panes side by side.

## Dense Toolbars

**PATTERN.** A row (or rows) of compact icon/label controls exposing many
actions in a small vertical footprint — common in creative tools, office
suites, admin consoles.

**WHEN TO USE.** High-frequency, expert users (Constitution #11) who need
many actions reachable in one click/scan without navigating a menu tree,
and who will learn icon meanings through repetition.

**WHEN NOT TO USE.** Occasional-use consumer software, or a toolbar
attempting to expose every possible action "just in case" — an overloaded
toolbar with 40 icons is not information-dense, it's noise (Constitution
#4: not everything can be visually important, and an unprioritized toolbar
makes everything compete equally).

**DESIGN RULES**
- Group by function with visual separators (whitespace or hairlines per
  Constitution #5), not by arbitrary order.
- Icon-only buttons need tooltips (with a keyboard-accessible path — see
  hover rules above) and accessible labels; never rely on icon
  recognizability alone for anything non-universal.
- Provide an overflow ("more") affordance for lower-frequency actions
  rather than perpetually widening the toolbar — this is the desktop
  analog of progressive disclosure (Constitution #16), applied to command
  surfaces instead of content.
- Reflect current state in the toolbar itself (a pressed/active toggle
  state for bold/italic, a highlighted current tool) so the toolbar also
  functions as a status readout, not just an input surface.

## Keyboard Shortcuts and Command-Driven Workflows

**ESTABLISHED**, strongly reinforced in current SaaS tools (Linear,
Superhuman-style email, Vercel dashboard, most IDEs) as a professional-
throughput accelerant, consistent with Constitution #11: expert/frequent
users trade a learning cost for a large ongoing speed gain.

**DESIGN RULES**
- Every shortcut should have a discoverable, mouse-driven equivalent
  (Constitution #19 discoverability, mirroring the hover-affordance rule
  above) — shortcuts accelerate, they should not gatekeep.
- Follow platform/ecosystem shortcut conventions (Cmd/Ctrl+K for command
  palette/search, Cmd/Ctrl+S for save, Esc to close/cancel) rather than
  inventing new bindings for well-established actions — this is
  Constitution #19 directly: deviating from a nearly-universal convention
  needs to earn that cost back.
- Surface relevant shortcuts contextually (in tooltips, in a menu item's
  trailing hint, in the command palette itself) rather than only in a
  separate "keyboard shortcuts" help screen nobody visits unprompted.
- A command palette (Cmd/Ctrl+K) has become the default "escape hatch" for
  command-driven workflows in modern SaaS tools (Linear, Vercel, Raycast,
  Notion) — see `knowledge/navigation-patterns.md` for its role as a
  navigation pattern. **EMERGING → ESTABLISHED** as of 2026: common enough
  in professional tools to be an expected affordance, not yet universal
  enough to assume users already know the shortcut without a visible
  trigger (a search icon or button that also opens it).

## Resizable Panels

**PATTERN.** Draggable dividers between regions (sidebar/content,
list/detail, panel/canvas) that let the user reallocate space.

**DESIGN RULES**
- Give the drag handle a generous hit area (wider than its visual hairline)
  and a hover/cursor change (`col-resize`/`row-resize`) so its affordance is
  discoverable without instruction — this is a cursor-precision-unlocked
  affordance that has no mobile equivalent (touch has no hover to reveal it
  progressively, so a mobile equivalent typically becomes a different
  component — a fixed proportion or a modal, not a drag divider).
- Enforce sensible min/max widths so a pane can't be dragged to
  uselessness (0px) or to swallow the whole window; collapsing a pane
  fully should be an explicit action (a collapse button) with a clear way
  back, not an accidental drag endpoint.
- Persist the resized state (see multi-pane section) for returning users.

## Right-Click / Context Menus

**PATTERN.** A menu of actions scoped to the specific element under the
pointer, triggered by right-click (or long-press as the closest mobile
analog, though long-press context menus on mobile are typically shorter and
touch-optimized, not a direct port).

**WHEN TO USE.** Power-user shortcuts to actions that are *also* available
elsewhere (a toolbar, a menu bar, a hover-revealed row action) — per the
hover-affordance rule, a context menu should accelerate, not be the sole
path to a function, since it's fundamentally undiscoverable without prior
knowledge that right-click does something here.

**DESIGN RULES**
- Scope the menu tightly to the clicked element's plausible actions — a
  context menu with generic, always-identical options regardless of what
  was clicked defeats its own purpose.
- Never override the browser's/OS's native context menu without a strong
  reason and a clear affordance that this surface *is* interactive (e.g., a
  visible "⋯" hint) — silently hijacking right-click on ordinary content
  (like disabling text-selection context menus) is a well-known
  anti-pattern that erodes trust.

## Multi-Window Considerations

**CONTEXTUAL.** Relevant mainly to native desktop apps and browser-based
tools that support opening secondary windows (a video call in its own
window, a document detached from a workspace, a "pop out" panel).

**DESIGN RULES**
- State that matters across windows (auth, a shared document's edits,
  notification counts) must sync in near-real-time — a secondary window
  showing stale state is worse than not offering the feature.
- Give each window a distinguishable title/icon so window-switchers
  (Cmd+Tab, Alt+Tab, taskbar) let the user tell them apart.
- Reserve multi-window for content that genuinely benefits from being
  reference-visible alongside other apps (a video call, a reference panel,
  a small utility) — don't multiply windows for flows that are actually
  sequential and belong in one window's navigation.

## Sources

- WCAG 2.1 Success Criterion 1.4.13 Content on Hover or Focus — W3C
- [Navigation rail – Material Design 3](https://m3.material.io/components/navigation-rail/guidelines)
- [Designing a Command Palette | Destiner's notes](https://destiner.io/blog/post/designing-a-command-palette/)
- [Command Palette UI Design: Best practices, Design variants & Examples | Mobbin](https://mobbin.com/glossary/command-palette)
- Cross-referenced against observed conventions in Linear, Vercel, and modern IDE toolchains (2026).
