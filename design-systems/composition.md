---
title: Composition — Slots, Variants, and Props
confidence-note: ESTABLISHED synthesis of common design-system engineering practice (compound component patterns, slot-based APIs, variant/props systems as used across Radix, shadcn/ui-style headless component conventions, Spectrum, and Carbon), framed directly around Constitution #15. Not tied to one vendor's exact API — the point of this file is the underlying composition logic, which is convergent across systems.
last_updated: 2026-09-12
---

# Composition

Directly implements **Constitution #15**: a design system's job is to make
*common* decisions once so creative effort concentrates on what's actually
specific to a product — and a system fails when it's harder to build the
right thing than the wrong thing. This file is about the mechanism that
keeps a system open (composable) rather than closed (a fixed catalog of
finished, rigid components).

## 1. The closed-system failure mode, precisely

A closed component library gives you `<Card variant="product">`,
`<Card variant="user">`, `<Card variant="stat">` — a fixed enumeration of
every shape someone anticipated. The moment a real product needs a card
that's 90% like `variant="product"` but needs one extra piece of content
in a slightly different place, there are exactly two bad options: fork the
component (now there are two cards to maintain, silently diverging) or
force the new content into a slot that wasn't designed for it (now the
component's internal layout logic fights the content). Both outcomes are
Constitution #15's failure mode made concrete — the system made the wrong
thing (forking or fighting the component) easier than the right thing
(composing existing primitives to build exactly what's needed).

## 2. Slots: composition over configuration

**ESTABLISHED.** A **slot-based** component exposes named regions where
arbitrary content can be placed, instead of a fixed set of props for
"the icon," "the title," "the description" that only accept plain
strings/known types.

```
// Configuration-only (closed) — brittle the moment a need doesn't fit:
<Card title="..." description="..." icon="star" badge="New" />

// Slot-based (composable) — same default look, unlimited actual content:
<Card>
  <Card.Media><Thumbnail ... /></Card.Media>
  <Card.Header>
    <Card.Title>...</Card.Title>
    <Badge>New</Badge>
  </Card.Header>
  <Card.Body>...</Card.Body>
  <Card.Footer>
    <Button variant="tertiary">Dismiss</Button>
    <Button variant="primary">Open</Button>
  </Card.Footer>
</Card>
```

The slot version still enforces the *structural* rules that matter
(padding, spacing between regions, one primary action per footer — per
Constitution #8) while placing no ceiling on what content or how many
extra elements a specific instance needs. This is the practical form of
Constitution #13's "consistent behavior, not identical look": the card's
*layout grammar* (media above header above body above footer, consistent
internal spacing) is fixed; its *content* is unconstrained.

**When configuration-only is actually fine**: a component with a
genuinely small, stable, unlikely-to-grow set of variations (a `Switch`
component realistically only ever needs `checked`, `disabled`, `label`)
doesn't need slot machinery — adding it there is needless complexity.
Reach for slots specifically where content shape is heterogeneous or
where product teams have historically needed "just one more field" from a
configuration-only version.

## 3. Variants: a controlled vocabulary of legitimate differences

**ESTABLISHED.** A variant prop (`variant="primary" | "secondary" |
"tertiary" | "destructive"` on Button, `size="sm" | "md" | "lg"`) encodes
the *legitimate, pre-agreed* set of differences a component may express —
this is the difference between "flexibility" and "chaos." Composition
should be open along the *content* axis (slots, children) but
intentionally closed along the *visual-treatment* axis (variants) —
because uncontrolled visual variation is exactly what erodes Constitution
#13's consistency, while uncontrolled content flexibility is what
Constitution #15 demands.

Practical rule: if a one-off screen needs a visual treatment that isn't
one of the existing variants, that's a signal to have the *system*
conversation (should this become a new variant, used more than once?) —
not to silently override styles on one instance with inline overrides
that no one else can find or reuse. A variant added because two unrelated
teams independently needed it is a healthy system growing; a variant
added because one designer wanted a slightly different shadow on one
screen is scope creep the token/variant layer should have prevented.

## 4. Props for behavior, not just appearance

**ESTABLISHED.** Composable components expose props that change
*behavior*, not only look: a `Dialog` should accept
`initialFocusRef`/`closeOnOutsideClick`/`preventClose` type behavioral
props precisely because the accessibility contract detailed in
`design-systems/components.md` has legitimate per-instance variation
(a destructive-confirm dialog needs different initial-focus behavior than
a simple info dialog) — encoding this as a supported prop keeps every
consuming team following the correct focus-management pattern instead of
each one reimplementing (and likely getting wrong) the ARIA dialog
contract from scratch. This is the composition-layer version of "make the
right thing easy": the *hard*, error-prone part (focus trap, return
focus, escape handling) is solved once inside the component; the
*product-specific* part (which element gets initial focus) is exposed as
a prop.

## 5. Contextual behavior: components that adapt to where they're placed

**ESTABLISHED, ties directly to `knowledge/responsive-design.md` §2.** A
composable component should be able to read its *context* — the space
available (via container queries, per `responsive-design.md` §2), a
density mode (via tokens, per `design-systems/design-tokens.md` §3), a
surface/elevation context (a button rendered on a colored surface may
need an automatically adjusted contrast-safe variant) — and adjust without
the consumer having to manually pass every contextual fact down as a
prop. This is what lets one `Card` component correctly compress from a
horizontal media+text layout to a stacked layout when placed in a narrow
sidebar, without whoever places it in a sidebar needing to remember to
also pass `layout="compact"`.

Where full automatic context-adaptation isn't practical, an explicit
context-setting prop (`density="compact"`, a `Card` rendered inside a
`<CardGrid columns={4}>` that broadcasts an implied max-width to children)
is an acceptable middle ground — the key requirement is that the
*consuming team* shouldn't have to re-derive layout logic the system
already knows.

## 6. Composition rules over component catalogs

**FOUNDATIONAL, restates Constitution #15's own resolution.** A design
system's documentation should teach *how components combine* (spacing
rules between a Card and the grid it sits in; which components may nest
inside a Dialog's body vs. which shouldn't; how a Button's size prop
should be chosen relative to the Input it sits beside in a form row) at
least as much as it documents each component in isolation. A catalog of
beautifully documented individual components with no composition guidance
still produces inconsistent products, because most real UI problems are
about *how things combine*, not about any single component's own API —
which is precisely why `design-systems/patterns.md` exists as a separate,
system-level document from `design-systems/components.md`.

## 7. Practical composition checklist

Before finalizing a new shared component, verify:
- Can a consumer add content this component's original designer didn't
  anticipate, without forking it? (slots)
- Does every supported visual difference correspond to a real, named,
  documented variant — none achieved by ad hoc style overrides?
- Are the component's non-obvious behavioral contracts (focus management,
  keyboard handling, async loading behavior) exposed as props rather than
  hardcoded, wherever legitimate per-instance variation exists?
- Does the component behave correctly without being told everything about
  its context, wherever that context (space, density, surface) is
  reasonably detectable?
- Is there a documented composition rule (not just a component doc) for
  the 2–3 most common ways this component is expected to combine with
  others?

## Sources
- Constitution principles #8, #13, #15 — `/home/claude/advanced-ui-skill/knowledge/ui-principles-2026.md`
- General compound-component / slot-API conventions as practiced in modern component libraries (Radix UI primitives, shadcn/ui composition style, Adobe Spectrum, IBM Carbon) — training knowledge synthesis, not tied to a single fetched source this session
