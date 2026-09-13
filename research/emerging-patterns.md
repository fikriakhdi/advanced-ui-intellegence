# Emerging Patterns

**Purpose.** Track UI/UX patterns that are gaining real traction but
are not yet universal or safely default-able — the EMERGING/
EXPERIMENTAL confidence tier from `knowledge/ui-principles-2026.md`.
Per Constitution #20, trends are tools, not objectives: an entry here
is not a recommendation to use the pattern, it's a note that the
pattern exists, how confident we are it's real (versus a fad or a
narrow niche habit), and what evidence would move it up to
ESTABLISHED status in the main knowledge base (at which point it
should graduate out of this file and into the relevant `knowledge/`
file, with a research-log entry noting the move).

Entries below reflect the state of knowledge as of this session
(model knowledge through Jan 2026); they have not all been freshly
verified via web search this session and should be spot-checked
periodically, especially adoption-rate claims which age quickly.

---

### Bento-grid layouts
Modular, asymmetric grids of variable-sized rectangular tiles (popularized
by Apple's marketing/product pages, widely copied since) used for
dashboards, feature showcases, and portfolio/landing sections.
**Confidence:** EMERGING, trending toward CONTEXTUAL for landing/
marketing pages specifically. **To graduate to ESTABLISHED:** evidence
it holds up in *functional* (not just marketing) UI over time without
becoming a decorative default applied regardless of content shape —
watch for it becoming a Constitution #21-style generic default
("bento grid because that's what a modern dashboard looks like" rather
than because the content genuinely has variable-importance modular
chunks).

### AI-chat-as-interface / conversational-command surfaces
Chat or command-palette-style input as a primary interaction mode
layered onto (or replacing parts of) traditional GUI navigation —
seen across productivity, search, and increasingly enterprise/B2B
tools. **Confidence:** EMERGING for chat-as-primary-interface, closer
to CONTEXTUAL for command-palette-as-secondary-accelerator (the latter
has been stable for years in developer tools). **To graduate:**
chat/command interfaces need to demonstrate they don't just add a
second, less discoverable way to do things GUI already does well —
graduation criterion is evidence of tasks that are *genuinely* better
served by natural-language input (ambiguous/exploratory queries,
power-user shortcuts) versus tasks forced into chat for novelty.

### Container queries in production responsive design
CSS container queries (`@container`) letting a component respond to
its own containing element's size rather than the viewport — directly
operationalizes Constitution #18's "respond to available space, not
device labels." Browser support matured through 2023-2024 and adoption
in production design systems has been climbing since.
**Confidence:** EMERGING moving toward ESTABLISHED for component-level
responsive design specifically — this one is closer to graduating than
the others. **To graduate:** default assumption in
`knowledge/responsive-design.md` shifts from "viewport media queries
with container-query as an enhancement" to "container queries as the
default tool, viewport queries as the fallback/outer-shell case" once
support and tooling friction are no longer a practical concern for the
target audience.

### Variable fonts as default (not optimization-only)
Single variable-font files exposing a continuous range of weight/width/
optical-size axes, used not just for performance (fewer font file
requests) but as a design tool — fine-grained weight tuning for
hierarchy, optical sizing for small vs. large text. **Confidence:**
EMERGING for "used as a deliberate hierarchy tool," ESTABLISHED already
for "used for performance reasons alone." **To graduate (design-tool
use):** wider default tooling support in design-to-code handoff for
picking arbitrary axis values, not just a handful of named weights.

### Spatial / glanceable widgets (OS-level and in-app)
Small, always-visible summary surfaces (home-screen widgets, desktop
widgets, in-app "glanceable" cards) optimized for a sub-second glance
rather than a full session — distinct from a dashboard, which still
assumes some dwell time. Growing as OS platforms (mobile and desktop)
invest in widget surfaces and as in-app "at a glance" cards proliferate
in dashboards. **Confidence:** EMERGING. **To graduate:** clearer
shared vocabulary/pattern conventions for what makes a good glanceable
widget (information density ceiling, update-frequency conventions,
tap-through behavior) rather than every platform/product inventing its
own — right now this is more "a growing category" than "a settled
pattern."

---

**Reminder:** an EMERGING entry is a hypothesis to test against a
specific product's actual needs (per `workflows/analyze-product.md`),
never a default to reach for because it's current.
