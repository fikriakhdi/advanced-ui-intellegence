---
title: Information Architecture
last_updated: 2026-09-12
---

# Workflow: Information Architecture

**Purpose.** Convert a Design Strategy (output of
`workflows/analyze-product.md`) into structure — navigation, screens,
sections — *before* any component is chosen. This workflow is the
bridge between "what the product is for" and "what gets drawn."

**Hard rule:** IA work must never start from components. Starting from
"we'll need a sidebar, some cards, and a table" before user goals and
information groups are settled inverts the correct order and is a
direct violation of Constitution #1 (hierarchy before decoration) and
#9 (every element must justify its existence against a goal, not the
reverse). If you catch yourself naming a component before naming a
goal, stop and back up.

---

## The chain

Work strictly in this order. Do not skip a link to save time — each
link constrains the next, and skipping one means the next link is
decided arbitrarily instead of derived.

```
User Goals → Tasks → Information Groups → Navigation → Screens → Sections → Components
```

### 1. User Goals
Pull directly from `workflows/analyze-product.md` Q3/Q4 (primary +
secondary goals). State each as an outcome, not an activity: "know
whether today's revenue is on track" not "view revenue chart."

### 2. Tasks
For each goal, list the concrete tasks that accomplish it. A goal
usually decomposes into 2-5 tasks. Tasks are still technology-agnostic:
"compare this week's revenue to last week's," "find which product is
underperforming," "message an affiliate about a payout." No screens or
components yet.

### 3. Information Groups (card-sorting logic)
Cluster tasks and the data/content they touch by **what question they
answer together**, not by superficial similarity. This is the same
judgment a card-sorting exercise externalizes with real users — apply
it analytically when you don't have participants to sort with:

- Group by **user question**, not by data source. "Revenue today,"
  "revenue this month," and "revenue by channel" belong together
  because they all answer "how is revenue doing," even if they come
  from different backend tables. Conversely, "orders" and "shipping
  labels" may come from the same table but answer different questions
  and can belong in different groups.
- Group by **decision the user makes with it**, not by object type. If
  "affiliate leaderboard" and "affiliate payout approval" both feed the
  decision "should I keep working with this affiliate," they group
  together even though one is read-only data and one is an action.
- A group that only makes sense to *you* (the designer) and not to the
  user's mental model of their own job is wrong — re-cluster around
  their language, not the schema.
- Cross-check every group against the Information Priority ranking from
  `analyze-product.md` Q8 — the highest-ranked items should map to the
  most prominent groups, not get buried inside a lower group for
  structural convenience.

### 4. Navigation
Decide structure from the shape of the groups, not preference:

- **Flat navigation** (all groups as top-level, equal-weight nav items)
  when: there are ≤7 groups, they're used with roughly similar
  frequency, and none is a natural "detail" of another. Most dashboards
  and admin tools land here.
- **Nested/hierarchical navigation** when: some groups are clearly
  parent/child (e.g. "Products" → "Product detail" → "Variants"), or
  there are more groups than can be scanned flat (>7-9), or usage
  frequency is very uneven (a few used constantly, many used rarely —
  bury the rare ones under a parent or an overflow, per Constitution
  #16's progressive-disclosure test: hiding them must not change a
  decision the user needs to make up front).
- **Test before finalizing:** for each nav item, can a first-time user
  predict what's behind it from its label alone? If not, the grouping
  logic (step 3) is probably off, not the labeling.

### 5. Screens
Each top-level nav destination becomes at least one screen. Detail
views (a single order, a single product) become their own screens only
when they need enough content/actions to not fit as an expansion of
the list view — otherwise prefer inline expansion or a side panel
(cheaper navigationally, see Constitution #19 on not inventing new
patterns without payoff).

### 6. Sections
Within a screen, sections are the information groups from step 3 laid
out in the priority order from `analyze-product.md` Q8. The
highest-priority group should be the first thing visible without
scrolling on the primary device (Q7). A section is a candidate for a
card wrapper only if it's a genuinely comparable/repeatable unit
(Constitution #6) — most sections should be typographic/spatial, not
card-boxed by default.

### 7. Components
Only now: what component renders each section's content given its
data shape and the interactions it needs (table vs. list vs. stat row
vs. chart). This is where `workflows/generate-ui.md`'s COMPOSE stage
picks up.

---

## Validating IA against user goals before screens are drawn

Before moving to visual/component work, run this checklist:

- [ ] Every user goal (step 1) is reachable in ≤2 navigation actions
      from the primary entry point.
- [ ] Every information group maps to at least one goal or task — a
      group with no goal behind it is probably vanity content
      (Constitution #9).
- [ ] The nav's item order and prominence match the Information
      Priority ranking, not alphabetical or "however they were listed
      in the brief."
- [ ] No screen requires the user to visit another screen just to
      understand what a number means (e.g. a KPI with no way to see
      what's driving it) — that's a hierarchy/grouping failure, not a
      later polish item.
- [ ] Secondary/tertiary tasks (step 2) have a place, even if it's
      progressive disclosure — nothing was silently dropped.
- [ ] If a real user (or a stand-in stakeholder) were handed just the
      nav labels and screen titles, they could describe what each
      screen is for. If they can't, the grouping or labeling is wrong —
      fix it here, before it's expensive to fix in components.

Only after this checklist passes does `workflows/generate-ui.md`'s
ARCHITECT stage hand off to SELECT DESIGN DIRECTION and COMPOSE.
