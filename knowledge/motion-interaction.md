---
title: Motion and Interaction
confidence-note: Framed entirely around Constitution #12 (motion must justify itself as communication, not decoration). Material Design 3's exact duration/easing token values are now FOUNDATIONAL, primary-source verified 2026-09-12 by fetching Google's open-source `material-web` token source directly (see §2/§3 and Sources) — the m3.material.io marketing site itself is still JS-gated, but the underlying generated token files are plain text and were fetched successfully. Apple HIG's motion guidance remains ESTABLISHED from training knowledge only — a second attempt this session (direct URL and `.md` variant) still could not get past client-side JS rendering; treat Apple-specific figures as unverified.
last_updated: 2026-09-12
---

# Motion and Interaction

Governed entirely by **Constitution #12**: motion is justified only when it
communicates *state* (what changed), *causality* (what caused what),
*continuity* (where did this come from/go to), or *what's happening right
now* (processing/loading). Motion with none of these jobs is decorative,
and because animation adds literal time between an action and its visible
result, decorative motion is a **latency cost paid on every single
interaction**, not a free aesthetic add-on. This file is the operational
companion: it tells you which interactions need which kind of motion, and
gives real numbers so "give it some motion" becomes a specific, reviewable
decision.

## 1. The four justifications, applied

For every motion you're about to add, name which of the four jobs it does.
If you can't name one, cut it.

- **State change**: a toggle flipping, a value updating, a checkbox
  checking, an item being added/removed from a list. The motion's job is
  to make the *before → after* legible as one continuous change rather
  than a jump-cut the eye has to reconcile after the fact.
- **Causality**: a dependent field appearing because a checkbox was
  ticked, a form section expanding because an option was selected. The
  motion's job is to bind the effect visually to its cause so the user
  doesn't have to infer the connection.
- **Spatial continuity**: a modal growing outward from the button that
  opened it, a detail view sliding in from the card that was tapped, a
  dragged item following the cursor. The motion's job is to preserve the
  user's mental model of *where things live* across a transition, so nav
  doesn't feel like teleportation.
- **Processing feedback**: a spinner, a progress bar, a skeleton, a
  pressed-state ripple. The motion's job is to communicate "your input was
  received and something is happening" during the gap where nothing else
  on screen would tell you that.

Motion that serves none of these — an entrance animation on every card in
a list purely because it looks lively, a bounce on a button that does
nothing but decorate the click, a page transition that exists because it's
"delightful" rather than because it preserves any spatial relationship —
is a tax on every user, every time, forever. Reserve genuinely decorative,
non-functional motion for the rare, deliberate delight moments Constitution
#3 permits (a first-run empty state, a completed-onboarding celebration) —
never for high-frequency interaction paths.

## 2. Duration: match to the mass and frequency of what's moving

**FOUNDATIONAL for the Material 3 numbers (primary-source verified
2026-09-12); ESTABLISHED/training-knowledge for the Apple HIG numbers
(unverified — see below).**

General principle, true across every motion system referenced below: the
farther something travels or the more it changes, the longer the
animation may run — but the *more frequently* an interaction happens, the
*shorter* it needs to be, because frequency compounds latency cost.

- **Material Design 3 motion tokens — exact values, fetched directly from
  `material-components/material-web`'s generated token source
  (`tokens/versions/v0_192/_md-sys-motion.scss`, design system version
  v0.192) rather than reconstructed:**
  - `duration-short1..4`: **50 / 100 / 150 / 200ms**
  - `duration-medium1..4`: **250 / 300 / 350 / 400ms**
  - `duration-long1..4`: **450 / 500 / 550 / 600ms**
  - `duration-extra-long1..4`: **700 / 800 / 900 / 1000ms**
  - `easing-standard`: `cubic-bezier(0.2, 0, 0, 1)` (same curve as
    `easing-emphasized`, i.e. the current spec uses one shared curve for
    both by default); `easing-standard-accelerate`:
    `cubic-bezier(0.3, 0, 1, 1)`; `easing-standard-decelerate`:
    `cubic-bezier(0, 0, 0, 1)`.
  - `easing-emphasized-accelerate`: `cubic-bezier(0.3, 0, 0.8, 0.15)`;
    `easing-emphasized-decelerate`: `cubic-bezier(0.05, 0.7, 0.1, 1)`.
  - `easing-legacy` (the older M2-era curve, kept for migration):
    `cubic-bezier(0.4, 0, 0.2, 1)`, with its own accelerate/decelerate
    variants.
  - `easing-linear`: `cubic-bezier(0, 0, 1, 1)` — i.e. true linear,
    provided as an explicit token rather than left as a CSS default.
  - The previously-cited round-number bands (short ~50–200ms, medium
    ~250–400ms, long ~450–600ms, extra-long 700ms+) turn out to match the
    verified per-step values closely — the earlier training-knowledge
    figures were directionally accurate, and are now confirmed exact.
- **Apple HIG** guidance (still ESTABLISHED, general principle from
  training knowledge only — a second live-fetch attempt this session,
  both the direct URL and the `.md` documentation-source variant that
  worked for Apple's API reference pages, still returned only
  navigation/metadata for the HIG Motion page; it remains a fully
  JS-rendered page with no static fallback found): favor fluid,
  physically-plausible motion — spring-based animations that reflect
  real-world momentum and settle naturally, rather than fixed-duration
  ease curves — for anything the user's touch is directly driving (drag,
  swipe-to-dismiss, rubber-banding at scroll bounds). For non-gesture-driven
  transitions, short and unobtrusive is preferred; iOS system transitions
  are typically in the 200–400ms range. **Treat these Apple-specific
  figures as unverified** until a rendered-browser fetch or an Apple WWDC
  transcript/sample-code source is checked.
- **Practical duration bands to use as defaults** (cross-referencing the
  above, ESTABLISHED as a synthesis):
  - **~100–150ms**: micro-interactions — hover, press/active state,
    focus ring appearing, a small icon toggling.
  - **~150–250ms**: local UI changes — a dropdown/menu opening, a tooltip
    appearing, an accordion section expanding, a toast entering.
  - **~250–400ms**: component-level or moderate-distance transitions — a
    modal/dialog entering, a drawer sliding in, a card expanding to a
    detail view.
  - **~400–600ms**: large-scale or full-screen transitions — page-level
    navigation with spatial continuity, a bottom sheet rising to cover
    most of the screen.
  - Anything longer than ~600ms should be justified by genuine scale
    (a full page transition) or gated behind explicit user awareness
    (an onboarding celebration) — long durations on frequent interactions
    are almost always a mistake.

## 3. Easing: describe acceleration, not just duration

**ESTABLISHED.** A duration without an easing curve is incomplete — linear
motion (constant velocity) reads as mechanical and cheap; almost nothing
in a well-designed interface should use `linear` easing except a
continuously-running indeterminate progress indicator.

- **Standard/ease-in-out curves** (Material's `easing-standard`, verified
  exact value `cubic-bezier(0.2, 0, 0, 1)`): fast start, fast
  finish, brief hang in the middle — appropriate for most UI transitions
  where something appears and settles.
- **Decelerate curves** (fast start, slow finish — an object "arriving"):
  appropriate for elements entering the screen (a menu opening, a toast
  sliding in) — motion that ends by gently settling reads as more
  polished than one that stops abruptly.
- **Accelerate curves** (slow start, fast finish — an object "departing"):
  appropriate for elements exiting (a dismissed toast, a closed menu) —
  exits can leave faster than entrances arrive, since the user's attention
  has already moved on.
- **Spring/physics-based easing** (used heavily in iOS and increasingly in
  web via libraries): appropriate for anything directly following a
  user's continuous gesture (drag, swipe) because it can naturally
  incorporate velocity and produce responsive-feeling overshoot/settle
  rather than a fixed timeline that ignores how fast the user was moving.
- **Never use `ease` (browser default) or `linear` as a considered
  choice** — both are defaults nobody actually picked; a real motion
  system specifies its curves as tokens (see
  `design-systems/design-tokens.md` §motion tokens) the same way it
  specifies color and spacing.

## 4. Interaction states as motion's most common job

Most motion in a mature product isn't a "transition" at all — it's state
feedback on ordinary controls. Every interactive element needs a
specified behavior across its full state set:

- **Hover** (pointer-capable input only, per `responsive-design.md` §5):
  a subtle, fast (~100–150ms) property change — background tint,
  elevation, border — signaling "this is interactive and your pointer is
  over it." Must never be the *only* way a control's interactivity is
  communicated (touch has no hover).
- **Focus**: a visible, non-hover-dependent indicator (see
  `accessibility.md` §3, WCAG SC 2.4.7) — should appear instantly or
  near-instantly (no slow fade-in on a focus ring; a user tabbing quickly
  needs to see immediately where focus landed).
- **Pressed/active**: immediate, distinct feedback the instant a pointer
  or touch makes contact (a slight scale-down, an opacity/color shift, a
  ripple) — this is the single fastest state change in the system (near
  0–80ms) because perceived responsiveness to a physical press is one of
  the most latency-sensitive things a user notices.
- **Disabled**: no motion at all — a disabled control should not react to
  hover/press, which is itself part of communicating "not currently
  actionable."
- **Loading (as a button state)**: swapping label for a spinner, or
  showing a spinner alongside a dimmed label, should transition in fast
  (~100–150ms) since the user is waiting on their action's result and any
  extra delay reads as sluggishness, not polish.

## 5. Loading feedback and optimistic UI

**ESTABLISHED, ties directly to Constitution #12's "what's happening right
now" justification.**

- **Under ~300–400ms**: no loading indicator at all — a spinner that
  flashes for a fraction of a second reads as a glitch, not feedback (this
  is why "minimum spinner display time" logic exists in many production
  systems — but the better fix is usually to make the operation fast
  enough that this doesn't come up, or to use optimistic UI, below).
- **~400ms – a few seconds**: a skeleton screen (a low-detail placeholder
  matching the eventual content's layout) communicates *what kind of
  content is coming* and *roughly where it will appear*, reducing the
  perceived wait and preventing layout shift when real content arrives.
  Skeletons should mirror actual layout (a list skeleton shows list-shaped
  bars, not a generic centered spinner) — this is the "what's happening"
  justification made concrete.
- **Longer, indeterminate waits**: an indeterminate progress
  indicator (spinner or animated bar) signals "still working," not stuck.
- **Longer, determinate waits with a known duration/size** (a file
  upload, a multi-step import): a determinate progress bar with real
  percentage — this additionally answers "how much longer," which an
  indeterminate spinner cannot.
- **Optimistic UI**: for actions that will very likely succeed (liking a
  post, reordering a list, toggling a setting), update the UI
  *immediately* as if the action already succeeded, and only show
  feedback if it actually fails (revert + explain). This eliminates
  loading-state motion entirely for the common case — it is the strongest
  possible response to "loading feedback," because the best loading state
  is no loading state. Reserve it for actions that are reversible or
  low-stakes; never apply it to destructive or hard-to-reverse actions
  (a payment, a delete) where the user needs a real confirmation of actual
  success.

## 6. Drag and gesture feedback

**ESTABLISHED.**
- An element being dragged should visually detach from its origin (a
  slight scale-up, elevation/shadow increase, reduced opacity of the
  original position) so it's unambiguous that it's now "picked up" and
  moving independently of the layout underneath it.
- Valid drop targets should highlight as the dragged item passes over
  them (causality: this space will accept the drop). Invalid targets
  should give a distinct negative signal (not just "no highlight" — that's
  ambiguous with "haven't checked yet").
- On drop, the item should animate into its final resting position rather
  than snapping instantly, closing the spatial-continuity loop opened when
  the drag began.
- Swipe-to-reveal/dismiss gestures should track the gesture 1:1 while the
  finger is down (not a fixed-duration animation ignoring the user's
  actual finger position) and only switch to an eased/spring animation
  once the finger lifts, animating to either the completed or the
  reverted state depending on velocity/distance thresholds crossed.

## 7. Page and view transitions

**ESTABLISHED, spatial-continuity-first.**
- A transition between two views should reflect their actual spatial or
  hierarchical relationship: drilling into detail (list → item) slides
  the new view in from the direction implied by hierarchy (typically
  right-to-left in LTR locales) and reveals a way back (slide back
  right); switching between sibling views (tabs) should not use a
  "drilling" transition, since neither is more hierarchically inside the
  other, — a simple cross-fade or a directional slide tied to tab order
  is more honest.
- A modal/dialog that was invoked from a specific control benefits from
  growing from that control's position/size rather than fading in
  centered from nowhere — this is what makes "spatial continuity" concrete
  for modals specifically, and is the canonical example used in
  Constitution #12 itself.
- Full page navigations on the web historically had no native transition
  primitive; the View Transitions API (broadly available in modern
  browsers by 2026) allows genuine cross-document and SPA transitions with
  spatial continuity where previously only client-heavy libraries could —
  worth reaching for specifically where it replaces a jarring hard cut
  with a continuity-preserving morph, not merely to add movement.

## 8. Reduced motion

**FOUNDATIONAL — this is an accessibility requirement, not a nice-to-have
(see `accessibility.md` §7 and WCAG's broader intent around vestibular
disorders).** Respect `prefers-reduced-motion: reduce` by:
- Removing large-scale translation/parallax/zoom effects and any
  auto-playing decorative animation.
- **Keeping** the state-communication function of essential motion, but
  substituting a lower-motion equivalent — a cross-fade or instant swap
  instead of a slide/bounce/scale — so the *information* the motion
  carried (something changed, something appeared) isn't silently lost,
  only its physical translation/rotation intensity.
- Every motion token/animation utility in a design system should have a
  `prefers-reduced-motion` fallback defined as part of the token, not
  hand-added by whichever engineer happens to remember.

## 9. Common failure modes
- Animating every list item's entrance on every render (including
  re-renders that didn't actually add new items) — motion fatigue that
  actively slows down frequent, repetitive tasks.
- Long (600ms+) transitions on high-frequency interactions (opening a
  menu that's opened dozens of times a session).
- A spinner with no minimum-perceptible-content strategy, flashing for
  80ms and vanishing.
- Modals that fade in from screen-center regardless of what invoked them,
  discarding a free opportunity for spatial continuity.
- Motion added post-hoc for "delight" on a screen where nothing in
  Constitution #12's four justifications applies.
- No `prefers-reduced-motion` handling anywhere in the codebase.
- Using drag feedback and drop-target highlighting inconsistently across
  similar list/reorder components in the same product (breaks
  Constitution #13's behavioral consistency).

## Sources
- [`material-components/material-web` — `tokens/versions/v0_192/_md-sys-motion.scss`](https://raw.githubusercontent.com/material-components/material-web/main/tokens/versions/v0_192/_md-sys-motion.scss) (raw source, fetched directly, auto-generated from "Google Material 3" design-system version v0.192) — exact duration (ms) and easing (cubic-bezier) token values. **Primary source, fetched successfully 2026-09-12** — this is the actual generated-token file the `m3.material.io` site's own token specs page compiles from; it is plain text and was not blocked, unlike the marketing site.
- Apple Human Interface Guidelines — Motion (spring-based, gesture-driven animation; preference for fluid, physically plausible motion) — training knowledge; re-attempted 2026-09-12 via direct URL and via the `.md` documentation-source suffix (which successfully worked for Apple's API-reference pages elsewhere in this research pass) — both still blocked by client-side JS rendering with no static fallback found. Genuinely unverified after two independent attempts across two sessions.
- [MDN — prefers-reduced-motion](https://developer.mozilla.org/en-US/docs/Web/CSS/@media/prefers-reduced-motion) — background knowledge
- [MDN — View Transitions API](https://developer.mozilla.org/en-US/docs/Web/API/View_Transitions_API) — background knowledge
- Constitution principle #12 (and #2, #3, #13, #19) — `/home/claude/advanced-ui-skill/knowledge/ui-principles-2026.md`
