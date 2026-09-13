---
name: advanced-ui-intelligence
description: >
  Use when the user asks to design, build, redesign, or critique a user
  interface (web or mobile) — a screen, flow, component, dashboard,
  landing page, app, or design system — and wants senior-level design
  judgment rather than a default template. Also use when asked to audit
  an existing interface for responsiveness, accessibility, or "does this
  look generic/AI-generated" concerns.
---

# Advanced UI Intelligence

This skill orchestrates a local knowledge base of UI/UX design reasoning
(`knowledge/`, `design-systems/`, `references/`, `workflows/`,
`research/`) so that UI work draws on real design judgment instead of
generic defaults. Read this file fully before starting; it tells you
what to read next and when — the knowledge base is too large to load
into context all at once, so this file is a router, not a summary.

**The one rule that matters most:** never go straight from a request to
code. Every failure mode this skill exists to prevent — generic layouts,
purple-gradient SaaS templates, cards around everything, hierarchy that
collapses under scrutiny — traces back to skipping product understanding
and jumping to implementation. See `knowledge/ui-principles-2026.md`
Constitution #22 and its amendment #21.

---

## Step 0 — Which task is this?

Route to one of four modes. Most requests are GENERATE.

| User is asking to... | Mode | Entry workflow |
|---|---|---|
| Build/design a new screen, flow, component, or product UI | **GENERATE** | `workflows/generate-ui.md` |
| Improve/fix/modernize an existing interface | **REDESIGN** | `workflows/redesign-ui.md` |
| Get an honest evaluation of a design (theirs or one you just made) | **CRITIQUE** | `workflows/critique-ui.md` |
| Check a design across screen sizes or for accessibility specifically | **AUDIT** | `workflows/responsive-audit.md` and/or `workflows/accessibility-audit.md` |

If the request is ambiguous (e.g. "make this dashboard better" could be
REDESIGN or CRITIQUE-then-REDESIGN), default to running CRITIQUE first —
you cannot redesign responsibly without first diagnosing what's actually
wrong, and `workflows/redesign-ui.md` requires it as an input anyway.

A request that names a component in isolation ("make me a pricing
table") still goes through GENERATE — just briefly. Constitution #22
applies at every scale; a one-component request still deserves a fast
pass through UNDERSTAND/ANALYZE (a sentence or two, stated explicitly),
not a skip straight to markup.

---

## Step 1 — Always load these three first

Regardless of mode, these are small, foundational, and everything else
depends on them:

1. `knowledge/ui-principles-2026.md` — the Constitution. Every
   downstream file cites these 22 principles by number; you need the
   numbers to mean something.
2. `knowledge/anti-patterns.md` — the 23 named failure modes. This is
   your lookup table for "does this look generic/AI-generated," used in
   both COMPOSE (to avoid) and CRITIQUE (to detect).
3. The confidence taxonomy (defined in the Constitution's header):
   FOUNDATIONAL > ESTABLISHED > CONTEXTUAL > EMERGING > EXPERIMENTAL >
   VISUAL TREND. When you cite a claim from any knowledge file, its tag
   tells you how much weight to give it — an EXPERIMENTAL or VISUAL
   TREND claim should be applied cautiously and disclosed as such if it
   materially shapes a decision, not treated as equal to FOUNDATIONAL.

---

## Step 2 — Run the mode's workflow

### GENERATE
Follow `workflows/generate-ui.md`'s 13-stage pipeline (UNDERSTAND →
DELIVER) exactly, in order. That file names which knowledge-base path to
draw on at each stage — don't pre-load files for stages you haven't
reached yet. The two stages worth flagging here because they're the
ones people are tempted to skip under time pressure:
- **ANALYZE** (stage 2) runs `workflows/analyze-product.md` in full —
  15 questions to a Design Strategy. Nothing after this stage may
  proceed without it existing, even in abbreviated form.
- **CRITIQUE** (stage 10) runs `workflows/critique-ui.md` in full
  against your own output. Do not skip this because you just built the
  thing and believe it's good — see that workflow's explicit
  anti-praise rule.

### REDESIGN
Follow `workflows/redesign-ui.md`. It requires a CRITIQUE pass as its
diagnostic step — run `workflows/critique-ui.md` against the existing
interface first if that hasn't already happened. Then triage findings
into must-preserve / should-fix / could-improve before touching
anything, per that workflow's sequencing rules (don't bundle structural
and cosmetic changes in one pass).

### CRITIQUE
Follow `workflows/critique-ui.md` directly. Load
`knowledge/anti-patterns.md` (already loaded in Step 1) as the primary
lookup table for dimension 15 (visual originality). If you don't have
enough context about the intended product/user to judge dimension 13
(product appropriateness), ask, or run a lightweight version of
`workflows/analyze-product.md` first rather than guessing.

### AUDIT
Run `workflows/responsive-audit.md` and/or `workflows/accessibility-audit.md`
depending on what was asked. Both lean on `knowledge/responsive-design.md`
§4 (the canonical Recomposition Framework) and `knowledge/accessibility.md`
respectively — load those two knowledge files, not the whole `knowledge/`
directory.

---

## Step 3 — Load knowledge files on demand, by what the product actually is

Do not load all 22 `knowledge/` files for every task. Once ANALYZE (or a
quick read of the request) tells you the product type and the
components involved, pull only what's relevant:

**By product category** (`knowledge/`):
- Dashboard / admin / analytics → `dashboard-patterns.md`,
  `data-visualization.md`, `content-density.md`
- Ecommerce / marketplace → `ecommerce-patterns.md`, `forms-inputs.md`
  (checkout), `navigation-patterns.md` (filters/search)
- Mobile app (native or responsive-critical, i.e. mobile is the
  *primary* device) → `mobile-patterns.md`, `responsive-design.md`
- Dashboard/admin/desktop-primary product with mobile as a *secondary*,
  occasional-check use case (not the primary workflow) → stay on the
  dashboard/desktop row above and pull `responsive-design.md` only —
  do NOT pull `mobile-patterns.md`, which is written for mobile-first
  products and will steer composition wrong (e.g. bottom-sheet/tab-bar
  patterns that don't fit an occasional secondary glance)
- Desktop-first / professional tool (IDE-like, dense) →
  `desktop-patterns.md`, `content-density.md`
- Marketing/landing page → `visual-hierarchy.md`, `typography.md`,
  and specifically `anti-patterns.md` #8, #10, #22, #23 (the
  generic-landing-page cluster)
- Forms-heavy (signup, checkout, settings, surveys) →
  `forms-inputs.md`, `states-feedback.md`, `accessibility.md`
- Fleet/delivery/field-service/any "where is X right now" component
  (live-commerce or delivery ops dashboards, courier/vehicle tracking,
  map+list pairing) → `geospatial-patterns.md` (also pull
  `dashboard-patterns.md` if it's an ops dashboard, and
  `data-visualization.md` for the shared "does this need a map/chart at
  all" test)

**By concern that spans product types**, load whichever apply:
- Layout/composition decisions → `visual-hierarchy.md`,
  `spacing-layout.md`, `responsive-design.md`
- Type-heavy screens (editorial, dashboards with dense labels) →
  `typography.md`
- Palette/theming/dark-mode work → `color-system.md`
- Anything with transitions, drag, loading, or real-time updates →
  `motion-interaction.md`, `states-feedback.md`
- Navigation structure of any kind → `navigation-patterns.md`
- Any component selection → `design-systems/components.md` (has
  per-component PATTERN → IMPLEMENTATION NOTES treatment; check here
  before inventing a component's behavior from scratch)
- Any token/theming system work → `design-systems/design-tokens.md`,
  `design-systems/composition.md`
- Wanting inspiration or "how would a good product handle this" →
  `references/*.md` — but read the "when NOT to copy it" field on
  every entry before applying; these are principle extractions, not
  templates (Constitution #20).
- Accessibility questions of any kind → `knowledge/accessibility.md`
  first; it's written to shape decisions early, not just check them
  at the end.

If you're unsure whether a file is relevant, its one-line description
is in its YAML frontmatter — that's cheap to check before deciding to
read the whole file.

---

## Step 4 — Keep the rationale trace

Per `workflows/generate-ui.md` stage 13 (DELIVER): don't hand off a
finished interface with no explanation of why it looks the way it does.
At minimum, be able to state: the Design Strategy (from ANALYZE), the
one or two most consequential IA/composition decisions and why, what
CRITIQUE found and how it was resolved, and anything deferred. This is
what makes the work auditable and revisable later — by you in a future
session, or by the person who receives it.

---

## Known limitations (be honest about these, don't paper over them)

See `research/research-log.md` for the full dated history. As of the
last knowledge-base update:
- Apple HIG's Dynamic Type exact point sizes and its motion philosophy
  page are sourced from secondary references, not verified live
  (their docs are JS-gated against the fetch tools available during
  research) — treat specific Apple point-size numbers in
  `knowledge/typography.md` as ESTABLISHED, not FOUNDATIONAL.
- `design-systems/components.md`'s coverage is deep for the
  highest-misuse components (buttons, dialogs/drawers/sheets, tables,
  empty/loading/error states, command palettes, navigation,
  breadcrumbs, pagination, filters, search, notifications, onboarding,
  lists, menus) — if a component isn't in that file, don't assume it
  was deliberately omitted as low-priority; check
  `research/research-log.md` first, and reason from Constitution
  principles plus `design-systems/composition.md` if it's genuinely
  uncovered.
- This skill has not yet been run end-to-end against a large volume of
  real briefs — treat friction you hit while using it as a signal to
  update the relevant `workflows/` or `knowledge/` file, not just a one-off
  workaround. If you find a gap, fix it in the knowledge base in the
  same session rather than silently compensating for it every time.

## Updating this knowledge base

If, in the course of using this skill, you find a claim that's wrong, a
gap that matters, or a new pattern worth capturing: fix the relevant
`knowledge/`/`design-systems/`/`references/` file directly, and log the
change in `research/research-log.md` using its documented row format.
This keeps the base self-improving rather than static — see the
project's original goal of "continuously updating its UI knowledge as
design practices evolve."
