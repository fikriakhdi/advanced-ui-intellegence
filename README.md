# Advanced UI Intelligence

A research-driven UI/UX knowledge base and AI agent skill for generating,
critiquing, and auditing interfaces with senior-designer judgment —
instead of defaulting to generic, templated "AI-looking" UI.

Works as a project-level skill for both **Claude** (Claude Code /
Cowork) and **Codex** (or any coding agent that reads `AGENTS.md`
automatically). See [Usage](#usage) below.

## What this is

Most AI-generated UI looks the same: centered hero sections, purple
gradients, everything wrapped in a card, generic stat tiles, and a
component chosen because it's the default rather than because it fits
the product. This repo is an attempt to fix that at the source — not
with more visual polish, but with actual design *reasoning*: why a
decision works, when a pattern applies, and when it should be avoided.

It's built around:
- A **22-principle design constitution** (`knowledge/ui-principles-2026.md`)
  that every other file cites back to by number.
- **23 named anti-patterns** (`knowledge/anti-patterns.md`) — specifically
  the failure modes that make interfaces look AI-generated, each with
  *why AI generates it*, *why it's weak*, *when it's actually fine*, and
  *how to fix it*.
- A **routing skill** (`SKILL.md`) that reads a request, decides whether
  it's a generate/redesign/critique/audit task, and loads only the
  knowledge files relevant to that specific product — not the whole
  base every time.
- A **13-stage generation pipeline** (`workflows/generate-ui.md`):
  `UNDERSTAND → ANALYZE → PRIORITIZE → ARCHITECT → SELECT DESIGN
  DIRECTION → DEFINE SYSTEM → COMPOSE → IMPLEMENT → RENDER → CRITIQUE →
  REVISE → VALIDATE → DELIVER` — with an explicit rule against skipping
  straight from a product description to code.
- A **sourced, dated research log** (`research/research-log.md`) where
  every claim is confidence-tagged (`FOUNDATIONAL → VISUAL TREND`) and
  traceable to a real source (WCAG 2.2, ARIA APG, Baymard, Material
  Design, Apple HIG, GOV.UK, and others) — gaps are logged honestly
  rather than papered over.

## Repo structure

```
SKILL.md                    ← entry point / router — read this first
AGENTS.md                   ← cross-tool agent instructions (Codex, etc.)
CLAUDE.md                   ← pointer to AGENTS.md / SKILL.md for Claude Code

knowledge/                  ← durable design reasoning by domain (19 files)
  ui-principles-2026.md       the constitution (22 numbered principles)
  anti-patterns.md             23 AI-UI failure modes
  visual-hierarchy.md, typography.md, color-system.md, spacing-layout.md
  responsive-design.md, accessibility.md, motion-interaction.md
  mobile-patterns.md, desktop-patterns.md, dashboard-patterns.md
  ecommerce-patterns.md, forms-inputs.md, navigation-patterns.md
  data-visualization.md, geospatial-patterns.md, states-feedback.md
  content-density.md

design-systems/             ← tokens, components, composition, patterns
  design-tokens.md, components.md, composition.md, patterns.md

references/                 ← real-product principle extraction
  best-ui-examples.md, layout-examples.md, interaction-examples.md
  product-patterns.md
  (never "make it look like X" — every entry extracts a transferable
  principle and states when NOT to copy it)

workflows/                  ← the process layer
  analyze-product.md, information-architecture.md, generate-ui.md
  critique-ui.md, redesign-ui.md, responsive-audit.md
  accessibility-audit.md

research/                   ← how this knowledge base stays honest
  research-log.md              dated, sourced, append-only log
  emerging-patterns.md         trending but not yet established
  deprecated-patterns.md       once-standard, now poor practice
```

## Usage

### With Claude Code / Claude (Cowork)
Open this repo as a working directory, or install it as a skill. Claude
reads `CLAUDE.md` → `AGENTS.md` → `SKILL.md` and follows the router from
there. If installed as a proper Claude Skill, its name is
`advanced-ui-intelligence` and it triggers on requests to design, build,
redesign, or critique a UI.

### With Codex (or other AGENTS.md-aware agents)
Open this repo as the working directory. `AGENTS.md` is read
automatically at session start and points to `SKILL.md` as the real
entry point — the routing logic and workflows are tool-agnostic and
don't depend on any Claude-specific tool calls.

### The one rule to know before using either
Never let the agent jump straight from "build me a UI" to code. Every
mode (generate, redesign, critique, audit) starts by understanding the
product and its users first — see `knowledge/ui-principles-2026.md`
Constitution #22. A one-component request still gets a fast, explicit
pass through this, not a skip.

## Status

The knowledge base and `SKILL.md` router have both been written and
passed one real end-to-end dry run (a dashboard/ops-style brief, run
through `analyze-product.md` to a full Design Strategy, with the
resulting file routing verified). Two real gaps found during that test
were fixed in place, including a previously entirely-missing
`geospatial-patterns.md` file. The `IMPLEMENT → RENDER → REVISE` stages
of the pipeline have not yet been exercised end-to-end on a real build —
see `research/research-log.md` for the full, dated history of what's
verified, what's sourced-but-unverified, and what's still an open gap.

## Contributing / extending

If you find a claim that's wrong, outdated, or a gap that matters while
using this: fix the relevant file directly and add a row to
`research/research-log.md` in its documented format. This is meant to
stay a living knowledge base, not a static snapshot — see
`research/emerging-patterns.md` and `deprecated-patterns.md` for where
ongoing verification is expected.

## License

Not yet specified by the author.
