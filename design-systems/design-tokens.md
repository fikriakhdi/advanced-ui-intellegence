---
title: Design Tokens — Three-Tier Architecture
confidence-note: The three-tier (primitive/global → semantic/alias → component) model is ESTABLISHED and convergent across every major system researched (Material 3, Spectrum, Carbon, and independent design-token-naming guidance) — treat this convergence itself as strong evidence, though exact tier names differ per vendor (documented below). Adobe Spectrum's specific token naming is now FOUNDATIONAL, primary-source verified 2026-09-12 — fetched real token JSON directly from `adobe/spectrum-design-data` (the repo `adobe/spectrum-tokens` was renamed to). Material 3's design-tokens overview page and Spectrum's spacing/motion token files specifically were not located this pass (see §Sources) — those two remain unverified.
last_updated: 2026-09-12
---

# Design Tokens

A token layer is what makes **Constitution #13** (consistent behavior
without identical look) and **Constitution #15** (composition, not a
closed component set) actually implementable: components reference
*roles*, not raw values, so re-skinning a product — or supporting dark
mode, high contrast, or a white-label brand — is a token-mapping exercise,
not a find-and-replace across every component.

## 1. The three tiers

**ESTABLISHED — convergent across Material 3, Spectrum, Carbon, Polaris,
Primer despite differing terminology.**

### Tier 1 — Primitive / Global / Reference tokens
Raw values with no meaning attached — a color, a number, a font family.
They are the *palette*, not a decision about what anything is used for.

```
color-blue-60:     #0B5FFF
color-neutral-10:  #1A1A1A
spacing-4:         4px
spacing-8:         8px
font-family-sans:  "Inter", system-ui, sans-serif
radius-2:          4px
```

Naming pattern: `{category}-{hue-or-family}-{step}`. Steps are usually a
numeric scale (Material and Carbon both use a 0–100 or 10–90 lightness
scale per hue: `blue-10` lightest → `blue-90` darkest, or the inverse
depending on system). Primitive tokens are almost never referenced
directly by a component or a screen — they exist so Tier 2 has a
consistent, exhaustive palette to draw from.

### Tier 2 — Semantic / Alias / System tokens
Map a primitive to a *role* — what the value means in the UI, independent
of which literal color/size it happens to resolve to today.

```
color-action-primary:      color-blue-60
color-action-primary-hover: color-blue-70
color-text-primary:        color-neutral-10
color-text-secondary:      color-neutral-40
color-border-default:      color-neutral-20
color-surface-critical:    color-red-10
spacing-section-gap:       spacing-8   /* i.e. a multiple, or a named step */
```

Naming pattern: `{category}-{role}-{variant?}`, e.g. `color-action-primary`,
`color-feedback-danger`, `color-surface-raised`. This is the tier that
makes **dark mode, high contrast, and theming tractable**: `color-blue-60`
never changes, but `color-action-primary` can point at `color-blue-60` in
light mode and `color-blue-40` in dark mode — every component using the
semantic token gets the right value automatically, with zero
component-level code change. This is also the tier a rebrand or
white-label operates on: repoint `color-action-primary` to a different
hue family and every button, link, and focus ring updates coherently.

### Tier 3 — Component tokens
Map a semantic token to one specific part of one specific component. This
is the tier that lets an individual component deviate slightly from the
system default when there's a real reason to, without breaking the chain
back to semantic meaning.

```
button-primary-background:        color-action-primary
button-primary-background-hover:  color-action-primary-hover
button-primary-text:              color-text-on-action
button-primary-radius:            radius-2
input-border-default:             color-border-default
input-border-focus:               color-action-primary
card-surface-background:          color-surface-raised
card-surface-border:              color-border-default
```

Naming pattern: `{component}-{part}-{property}-{state?}`, e.g.
`button-primary-background-hover`, `dialog-surface-shadow`,
`tab-indicator-color-selected`. Component tokens are what a component's
implementation actually consumes — never a raw primitive, and ideally
never a bare semantic token either (going through the component tier
means a future "make all dialogs slightly more elevated" change is a
one-line edit at Tier 3, not a hunt through every dialog instance).

**Why three tiers and not two or four**: two tiers (primitive → component)
loses the theming leverage point — you'd have to touch every component
token to reskin. Four+ tiers (adding, say, a "category" tier between
semantic and component) is occasionally used in very large multi-product
systems (Carbon has explored an additional "layer" concept for
light/dark/contrast token layering — see Sources) but adds a real
maintenance and comprehension cost; three is the right default and should
only be exceeded with a specific, demonstrated need.

## 2. Token categories to define

**ESTABLISHED.** A complete token set covers, at minimum:

- **Color** — the largest category; needs primitive ramps for neutral +
  each semantic hue (brand, success, warning, danger, info at minimum),
  and semantic roles for text (primary/secondary/disabled/on-color),
  surface (default/raised/sunken/overlay), border, and interactive states
  (default/hover/active/focus/disabled) per action type.
- **Spacing** — a single numeric scale (commonly a 4px or 8px base:
  4/8/12/16/24/32/48/64/96) used for padding, gaps, and margins alike, so
  every space-related decision in the product draws from one visual
  rhythm. Semantic spacing tokens (`spacing-component-padding-sm`,
  `spacing-section-gap`) sit on top for named contexts.
- **Typography** — font family, a modular type scale (size + line-height
  + weight bundled as a "type style" token, e.g. `type-heading-lg`,
  `type-body-md`, `type-label-sm`), letter-spacing where relevant.
- **Radius** — a small scale (e.g. `radius-none/sm/md/lg/full`) — full
  usually reserved for pills/avatars, not applied to large containers.
- **Border** (width + style) — usually a tiny scale (`border-width-thin`
  /`-thick`) separate from color, so width and color can vary
  independently.
- **Shadow / elevation** — a small ordered scale representing z-axis
  layering (`elevation-0` through `elevation-4` or similar), each bundling
  blur/spread/color/opacity — used to signal stacking order (a popover
  above a card above the page), not decoration.
- **Motion** — duration and easing tokens (see
  `knowledge/motion-interaction.md` §2–3 for the actual values) —
  `motion-duration-short`, `motion-easing-standard`, etc. — so a
  product-wide "make transitions snappier" change is one token edit.
- **Breakpoint / container-size** — named width thresholds
  (`breakpoint-sm/md/lg/xl`) used consistently rather than inline magic
  numbers per component (see `knowledge/responsive-design.md` §1).
- **Sizing** — icon sizes, avatar sizes, control heights (`size-control-sm
  /md/lg`) — critically, control-height tokens should be shared across
  button, input, and select so that a form row of mixed controls aligns
  by default.
- **Z-index** — a small, named, exhaustive scale (`z-dropdown`,
  `z-sticky`, `z-overlay`, `z-modal`, `z-toast`, `z-tooltip`) — z-index
  chosen ad hoc per component is one of the most common sources of
  stacking bugs (a tooltip trapped behind a modal) in real codebases;
  centralizing it in tokens is cheap insurance.

## 3. Mode support: how tokens make modes tractable

**ESTABLISHED.** A "mode" (light, dark, high-contrast, compact, comfortable)
is implemented as an alternate mapping at the semantic tier — the
primitive palette and the component token names stay identical; only what
each semantic token *resolves to* changes per mode.

- **Light / dark**: `color-surface-default` → a near-white primitive in
  light mode, a near-black (not pure black — pure black causes excessive
  contrast/glow on OLED and reads harshly) primitive in dark mode.
  Semantic *pairs* often need independent definition rather than simple
  inversion — a shadow that reads as "elevation" in light mode typically
  needs to become a lighter border or subtle glow in dark mode, since
  shadows read poorly against dark surfaces; don't assume dark mode is
  "invert the light mode values."
- **High contrast**: a mode that widens gaps between adjacent semantic
  colors specifically at boundaries covered by WCAG 1.4.11 (borders,
  focus rings, disabled-vs-enabled) — implemented as yet another semantic
  mapping layer, so components need zero awareness that high-contrast
  mode exists.
- **Compact / comfortable (density)**: implemented primarily through the
  spacing and sizing tiers — a "compact" mode remaps
  `spacing-component-padding-*` and `size-control-*` tokens to smaller
  values system-wide, giving a genuine density switch (relevant to
  Constitution #11 — expert/high-frequency users benefiting from density)
  without touching component code or duplicating components.
- Implementation mechanism on the web is typically CSS custom properties
  scoped per mode (`:root`, `[data-theme="dark"]`,
  `[data-density="compact"]`) — the same mechanism this project's own
  Artifact-authoring rules for theme-aware pages rely on, which is a
  useful concrete example of tokens-as-CSS-custom-properties in practice.

## 4. Practical rules

- **Components must never reference primitive tokens directly** — this is
  the rule that keeps theming/mode-switching working; a component that
  hardcodes `color-blue-60` instead of `button-primary-background` breaks
  the moment the brand or mode changes.
- **Every new component token should map to an existing semantic token**
  where possible; only add a new semantic token when a genuinely new *role*
  is needed, not merely a new component.
- **Token names should describe role, not appearance** — `color-danger`,
  not `color-red`, at the semantic tier — because the whole point is that
  the mapped value can change without the name becoming a lie
  (`color-red` mapped to an orange in a rebrand is confusing; a role name
  never becomes false).
- **Keep the primitive palette exhaustive but the semantic layer
  disciplined** — it's fine (expected) to have 10 steps per hue at Tier 1;
  it's a smell to have 40 near-duplicate semantic tokens at Tier 2, since
  that tier is supposed to be the small, curated set of *decisions*, not a
  repackaging of the whole palette.

## 5. Adobe Spectrum 2 — real token naming, verified 2026-09-12

**FOUNDATIONAL, primary-source verified.** The repo previously cited here
(`adobe/spectrum-tokens`) has been renamed to `adobe/spectrum-design-data`,
and its `main` branch now holds Spectrum **2** token data directly (S1 data
moved to an `s1-legacy` branch). Fetching the actual source JSON —
`packages/tokens/src/color-palette.json` (primitives) and
`packages/tokens/src/color-aliases.json` (semantic roles) — confirms the
three-tier model with real, current token strings rather than
reconstructed ones:

- **Primitives** (`color-palette.json`): hue-and-step names like
  `blue-100` through `blue-1300` (13 steps per hue, not the 10-step
  convention some other systems use), each resolved as a **set** of three
  literal values keyed `light` / `dark` / `wireframe` rather than one
  value per token — e.g. `blue-1000` → `rgb(39, 77, 234)` in light,
  `rgb(105, 149, 254)` in dark, `rgb(61, 94, 165)` in wireframe mode. This
  three-way (not just light/dark) mode split at the *primitive* tier is a
  genuine Spectrum-specific detail — most systems only branch modes at the
  semantic tier.
- **Semantic/alias tokens** (`color-aliases.json`): real examples —
  `accent-background-color-default`, `accent-background-color-hover`,
  `accent-background-color-down`, `accent-background-color-key-focus`
  (state suffixes: `-default`/`-hover`/`-down`/`-key-focus`, where "down"
  is Spectrum's term for the pressed/active state), `background-base-color`,
  `background-elevated-color`, `background-layer-1-color`,
  `background-layer-2-color` (layer-numbered surface roles, an alternative
  to named surface roles like "raised"/"sunken"), and opacity-only tokens
  like `background-opacity-hover` (value `0.1`) used as a separate
  compositing layer rather than baked into a solid color. Each alias
  token's value is a brace-reference to a primitive or another alias,
  e.g. `"value": "{accent-color-900}"` — confirming the alias tier
  literally points at the primitive tier by name, the same mechanism this
  file's Tier 1→2 example uses.
- **Not located this pass**: the equivalent spacing and motion/animation
  token source files. `color-palette.json` and `color-aliases.json` both
  resolved (HTTP 200 from raw.githubusercontent.com); guessed filenames
  for spacing (`spacing.json`, `dimensions.json`, `sizing.json`) and
  motion (`animation.json`, `motion.json`, `durations.json`, `easing.json`)
  all 404'd, and this session's GitHub API access is restricted enough
  that a full repository tree listing wasn't available to find the real
  names. **Honestly unresolved** — the color-token naming above is
  primary-verified; Spectrum's spacing-scale and motion-token naming in
  the rest of this document (if any) should still be treated as
  unverified until a follow-up pass finds the actual file.

## Sources
- [Design Token Naming Best Practices — Netguru](https://www.netguru.com/blog/design-token-naming-best-practices) — accessed 2026-09-12 (confirms three-tier global/alias/component structure with examples)
- Material Design 3 — Design tokens overview (reference/system/component token model) — training knowledge; live fetch of https://m3.material.io/foundations/design-tokens/overview blocked by client-side JS rendering this session
- [`adobe/spectrum-design-data` — `packages/tokens/src/color-palette.json`](https://raw.githubusercontent.com/adobe/spectrum-design-data/main/packages/tokens/src/color-palette.json) and [`color-aliases.json`](https://raw.githubusercontent.com/adobe/spectrum-design-data/main/packages/tokens/src/color-aliases.json) — raw JSON, fetched and parsed directly. **Primary source, verified 2026-09-12** (repo renamed from `spectrum-tokens`; `main` branch now holds Spectrum 2 data). See §5 above.
- [Carbon Design System — new color token layering models discussion](https://github.com/carbon-design-system/carbon/discussions/7743) — link surfaced this session, not fully fetched; cited as evidence of ongoing multi-layer token exploration in a major system
- Constitution principles #11, #13, #15 — `/home/claude/advanced-ui-skill/knowledge/ui-principles-2026.md`
