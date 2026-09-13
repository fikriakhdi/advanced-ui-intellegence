# AGENTS.md — Advanced UI Intelligence

This repo is a design-reasoning knowledge base for building, redesigning,
critiquing, or auditing user interfaces with senior-designer judgment —
not templated, generic-looking output. It works the same way whether
you're Codex, Claude, or another coding agent that reads this file
automatically at session start.

**If the user asks you to design, build, redesign, or critique a UI (a
screen, flow, component, dashboard, landing page, or design system), or
to audit one for responsiveness/accessibility/genericness — read
`SKILL.md` in full before doing anything else.** It is the router: it
tells you which mode you're in (generate / redesign / critique / audit)
and which files under `knowledge/`, `design-systems/`, `references/`,
and `workflows/` to load for the specific product type at hand. This
file (`AGENTS.md`) is intentionally short — `SKILL.md` has the real
instructions, kept in one place so they don't drift out of sync across
tools.

## The one non-negotiable rule

**Never go straight from a UI request to code.** Read
`knowledge/ui-principles-2026.md` (Constitution #22 and its amendment
#21) for why: skipping product understanding and jumping to
implementation is the single most common cause of generic, wrong, or
"looks AI-generated" output. `SKILL.md` and `workflows/generate-ui.md`
walk through the stages that prevent this — follow them, including for
small requests ("just make me a button" still gets a fast, explicit
pass through UNDERSTAND/ANALYZE, not a skip).

## Repo map

```
SKILL.md                 ← START HERE — router + orchestration
knowledge/                ← durable design reasoning by domain (23 files)
design-systems/           ← tokens, components, composition, patterns
references/                ← real-product principle extraction (not style templates)
workflows/                 ← analyze → architect → generate → critique → audit
research/                  ← research-log.md (sourced, dated, confidence-tagged),
                              emerging-patterns.md, deprecated-patterns.md
```

## Keeping this base honest

Every claim in `knowledge/` and `design-systems/` is tagged with a
confidence level (FOUNDATIONAL / ESTABLISHED / CONTEXTUAL / EMERGING /
EXPERIMENTAL / VISUAL TREND — defined in `knowledge/ui-principles-2026.md`).
If you find something wrong, outdated, or missing while using this repo,
fix the file in place and log the change in `research/research-log.md`
using its documented row format — don't silently work around a gap.
`research/research-log.md` also lists currently known gaps; check it
before assuming something isn't covered on purpose.
