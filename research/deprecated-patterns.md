# Deprecated Patterns

**Purpose.** Track patterns that were once standard, trendy, or
widely recommended and are now considered poor practice — the mirror
image of `research/emerging-patterns.md`. Recognizing a deprecated
pattern matters for two reasons: it prevents this skill from
reproducing yesterday's best practice as if it were current (a subtler
version of the genericness failure in Constitution #21), and it's
often exactly what a `workflows/redesign-ui.md` pass finds sitting
inside an older interface that "must preserve vs. should fix" needs to
sort correctly — a deprecated pattern is usually a should-fix, not a
must-preserve, even if users are used to it.

Entries below reflect general knowledge as of this session (through
Jan 2026), not a fresh web-verification pass — treat dates/specifics as
approximate and worth confirming if precision matters for a specific
claim.

---

### Hamburger-menu-only navigation on desktop
Hiding all primary navigation behind a hamburger icon on wide desktop
viewports, a pattern borrowed wholesale from mobile constraints where
it made sense (limited width) and applied somewhere that constraint
doesn't exist. **Why deprecated:** desktop has ample width for visible
navigation; hiding it costs a click and hurts discoverability with no
corresponding space benefit — a direct instance of Constitution #19
(deviating from convention, here "always-visible nav on wide screens,"
without the alternative being better). Usability research going back
over a decade has consistently found hidden desktop nav measurably
hurts feature discovery. Mobile/narrow-viewport hamburger nav is not
deprecated — the deprecation is specifically "-only," applying it where
space isn't actually constrained.

### Carousel-heavy homepages / hero sliders
Auto-rotating image/content carousels as the primary above-the-fold
element on marketing and e-commerce homepages, common through the
2010s. **Why deprecated:** well-documented banner blindness means
users rarely see or interact with anything past the first slide;
auto-advancing content competing with a user's own reading pace
violates Constitution #12 (motion must communicate something, not just
decorate) and #4 (multiple slides claiming primary visual weight in
sequence dilutes the actual primary message). Largely replaced by a
single, well-prioritized hero message plus a real information
hierarchy below the fold.

### Aggressive skeuomorphism
Heavy real-world-material mimicry (leather textures, stitched seams,
glossy reflective buttons, page-turn animations) as a default UI
style, common in early smartphone-era interfaces (roughly 2007-2013).
**Why deprecated:** the visual detail added rendering/maintenance cost
and scanning noise (Constitution #9) without adding comprehension once
users no longer needed the metaphor bridge from physical objects to
digital ones; flat/material design movements that followed were a
direct correction. (Note: tasteful, selective skeuomorphic/tactile
detail is not itself banned — see Constitution #17 on personality — the
deprecation is specifically the *aggressive, wall-to-wall* application
as a default style.)

### Autoplay-everything (video/audio) on load
Automatically playing video or audio content — especially with sound —
the instant a page loads, common on media and marketing sites.
**Why deprecated:** actively harmful to task focus and accessibility
(startles users, conflicts with screen readers, wastes bandwidth/
battery, and is disorienting in shared/quiet environments); browser
vendors have shipped autoplay-blocking-by-default specifically in
response, and platform accessibility guidelines increasingly treat
unexpected autoplaying motion/audio as a hard violation, not a style
choice. Also a direct instance of the reduced-motion consideration in
`workflows/accessibility-audit.md`.

### Manipulative "dark patterns" (confirmshaming, forced continuity, hidden costs, roach-motel cancellation)
UI patterns deliberately engineered to trick or pressure users into
choices they wouldn't otherwise make — pre-checked add-ons, guilt-
worded decline buttons ("No thanks, I don't want to save money"),
subscriptions that are trivial to start and deliberately difficult to
cancel, costs revealed only at the final step. **Why deprecated:**
beyond being a direct ethical failure, this class of pattern is
increasingly subject to actual regulation (e.g. rules requiring
cancellation to be as easy as sign-up, restrictions on default opt-ins
for data/marketing consent) — meaning what was once merely bad practice
is trending toward being legally risky as well as reputation-damaging.
Directly contradicts the honest-comprehension spirit running through
this knowledge base's constitution (Constitution #2, #3: clarity and
function should never be subordinated to a manufactured outcome).
