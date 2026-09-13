---
title: Generate UI
last_updated: 2026-09-12
---

# Workflow: Generate UI

**Purpose.** The full pipeline for taking a product description to
production-quality interface code. This is the top-level orchestration
workflow — most other workflows in this directory are sub-steps it
calls into.

**Hard rule (Constitution #22):** this pipeline must never be entered
partway through. Specifically, it must never jump straight from a
product description to code — every stage below that precedes
IMPLEMENT exists because skipping it is the most common cause of
generic or wrong output. If asked to "just build X" with no product
context, run UNDERSTAND and ANALYZE anyway (briefly, stating
assumptions) rather than skipping to COMPOSE/IMPLEMENT.

---

## The pipeline

```
UNDERSTAND → ANALYZE → PRIORITIZE → ARCHITECT → SELECT DESIGN DIRECTION
→ DEFINE SYSTEM → COMPOSE → IMPLEMENT → RENDER → CRITIQUE → REVISE
→ VALIDATE → DELIVER
```

### 1. UNDERSTAND
**What happens:** Read the request as given. Identify what's explicit
vs. implied vs. missing. Do not fill gaps yet — just inventory them.
**Draws on:** nothing yet — this is raw intake.
**Done when:** you can state, in one sentence, what was asked for and
list every open question it leaves (these feed ANALYZE).

### 2. ANALYZE
**What happens:** Run `workflows/analyze-product.md` in full — the 15
questions, resolved to a Design Strategy. If information is missing,
ask the user or state the assumption explicitly (never assume
silently).
**Draws on:** `workflows/analyze-product.md`.
**Done when:** a Design Strategy exists with information priority,
design character, primary action model, and constraints stated. Per
Constitution #22, nothing past this point may proceed without it.

### 3. PRIORITIZE
**What happens:** Take the Information Priority ranking from ANALYZE
and stress-test it — does the ranking actually reflect what the user
needs *first* versus what's easiest to build or most visually
interesting? Resolve any tension in favor of user need (Constitution
#1, #4).
**Draws on:** the Design Strategy from step 2.
**Done when:** there is one unambiguous ranked list of what matters,
per screen if the product has more than one.

### 4. ARCHITECT
**What happens:** Run `workflows/information-architecture.md` — Goals
→ Tasks → Information Groups → Navigation → Screens → Sections.
Explicitly do not select components yet.
**Draws on:** `workflows/information-architecture.md`,
`knowledge/navigation-patterns.md`.
**Done when:** the IA validation checklist in that workflow passes —
nav structure, screens, and sections are settled and traceable to user
goals.

### 5. SELECT DESIGN DIRECTION
**What happens:** Choose the visual/interaction character that
expresses the Design Character from the Strategy (step 2) — density
posture, typographic voice, color role strategy, motion posture,
degree of visual novelty (Constitution #2, #19, #20). This is a
deliberate choice with a rationale, not a default template.
**Draws on:** `knowledge/anti-patterns.md` (to actively avoid generic
defaults), `research/emerging-patterns.md` and
`research/deprecated-patterns.md` (to calibrate what's current versus
stale), `design-systems/` references for existing token/style
conventions if constrained by step 2's technical constraints.
**Done when:** the direction can be stated as a short set of concrete,
falsifiable choices (e.g. "high-contrast neutral base + one accent hue
reserved for the primary action only; no shadows below the modal
layer; type scale biased toward compact for throughput") — not mood
words alone.

### 6. DEFINE SYSTEM
**What happens:** Establish or confirm the concrete tokens this build
will use — spacing scale, type scale, color roles, radius, elevation,
component states — consistent with Constitution #13 and #15.
**Draws on:** `design-systems/composition.md` and any existing
project/brand tokens named in the Design Strategy's technical
constraints.
**Done when:** every value COMPOSE will need (spacing unit, heading
sizes, color role names, etc.) is defined once, not invented ad hoc
per component.

### 7. COMPOSE
**What happens:** For each section from ARCHITECT, select the specific
component(s) that render it, applying the system from step 6.
Deliberately check each candidate card/container against Constitution
#5/#6 (don't card everything) and each emphasis choice against #4/#7
(hierarchy via type/space/position before color).
**Draws on:** `design-systems/composition.md`, relevant
`knowledge/` pattern files (layout, typography, spacing) for the
component types involved.
**Done when:** every section has a named component and layout, and a
one-line justification exists for any non-default choice (e.g. why a
list instead of a table, why a sheet instead of a modal).

### 8. IMPLEMENT
**What happens:** Write the actual production code (HTML/CSS/JS or
the target framework) realizing the composition from step 7, following
the technical constraints from ANALYZE.
**Draws on:** the outputs of steps 4-7 directly; no new design
decisions should be invented at this stage — if one is needed, that's
a sign step 7 was incomplete and should be revisited.
**Done when:** the interface renders and functions per the composed
spec; no placeholder/lorem content where real information-hierarchy
content was specified.

### 9. RENDER
**What happens:** Produce the actual visible output (preview, screenshot,
running artifact) so it can be evaluated as seen, not as described.
**Draws on:** nothing new — this is execution of step 8's output.
**Done when:** the interface can be looked at, clicked through, and
resized, not just read as code.

### 10. CRITIQUE
**What happens:** Run `workflows/critique-ui.md` in full against the
rendered output. This is a real evaluation, not a rubber stamp — see
that workflow's explicit anti-praise requirement.
**Draws on:** `workflows/critique-ui.md`, `knowledge/anti-patterns.md`.
**Done when:** every weakness found has PROBLEM / WHY IT MATTERS /
SEVERITY / RECOMMENDED CHANGE / EXPECTED IMPROVEMENT, and dimension
scores are recorded.

### 11. REVISE
**What happens:** Apply the recommended changes from CRITIQUE,
highest severity first. Re-run COMPOSE/IMPLEMENT only for the affected
sections, not a full rebuild, unless the critique found a structural
(IA-level) problem — in which case loop back to ARCHITECT, not just
COMPOSE.
**Draws on:** the critique output from step 10.
**Done when:** every "must fix" (high severity) item is resolved and
re-checked; "should/could" items are resolved or explicitly deferred
with a reason.

### 12. VALIDATE
**What happens:** Run the audits that CRITIQUE's general pass doesn't
cover in depth: `workflows/responsive-audit.md` and
`workflows/accessibility-audit.md`. Confirm the build still matches the
Design Strategy from step 2 (no drift from the original product
understanding).
**Draws on:** `workflows/responsive-audit.md`,
`workflows/accessibility-audit.md`.
**Done when:** both audits pass or have explicitly tracked follow-ups,
and the result is re-checked against the original Design Strategy.

### 13. DELIVER
**What happens:** Hand off the final implementation along with a short
rationale trace — what the Design Strategy was, what IA decisions were
made, what critique found and how it was resolved. This trace is what
lets a human (or a future revision pass) understand *why* the design
looks the way it does, not just what it looks like.
**Draws on:** the accumulated outputs of steps 2, 4, 5, 10, 11.
**Done when:** the interface, its rationale, and any open/deferred
items are all handed off together.

---

## Failure mode this workflow exists to prevent

Jumping from "build me a dashboard" directly to IMPLEMENT (or worse,
straight to a component library's default template) skips UNDERSTAND
through COMPOSE entirely. The output of that shortcut is exactly the
generic, context-free interface Constitution #21 describes — locally
plausible, individually defensible, and wrong for the specific
product. Every stage above exists to prevent one specific version of
that failure; do not remove a stage to save time on a "simple" request
— simple requests still deserve a fast, honest pass through all of
them, just a briefer one.
