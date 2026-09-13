---
title: Interaction Examples — Micro-interactions & Motion Decisions
confidence-note: Grounded in observed product behavior and published write-ups (see Sources). Interaction details change between product releases — treat specifics as illustrative of a durable principle, not a guaranteed current pixel/timing spec.
last_updated: 2026-09-12
---

# Interaction Examples (Specific Micro-interaction/Motion Decisions)

Every entry ties to Constitution #12 (motion must communicate state,
causality, continuity, or spatial relationship) or #19 (deviating from
convention only when it measurably pays off). None of these are "add this
animation because it looks nice" — read the WHY before reusing anything.

---

## Linear's contextual keyboard-shortcut hints

**OBSERVATION.** Hovering over most interactive elements in Linear for a
short pause surfaces a small tooltip showing that element's keyboard
shortcut, without requiring the user to open a help menu or shortcuts
reference sheet.

**WHY IT WORKS.** This solves the standard tradeoff between power-user
efficiency (keyboard shortcuts) and discoverability (a new user doesn't
know they exist) without forcing a choice between the two — the shortcut
is taught exactly at the moment of natural curiosity (hovering the mouse
version of the action), which is a near-zero-cost teaching moment.

**UNDERLYING PRINCIPLE.** Teach an efficient/expert interaction path at
the point where a user is already performing the slower path, rather
than in a separate onboarding tour or reference document that must be
independently sought out.

**WHEN TO REUSE THE PRINCIPLE.** Any tool with a meaningful keyboard or
gesture shortcut layer sitting alongside a mouse/touch path — productivity
software, creative tools, developer tools.

**WHEN NOT TO COPY IT.** On touch-only interfaces there's no hover state
to attach this to, so the pattern has no direct mobile equivalent — don't
force a hover-triggered tooltip onto touch UI; use a first-use coach mark
or a discoverable shortcuts reference instead.

---

## Linear's soft, fast state-change transitions

**OBSERVATION.** When an issue's status or priority changes, or a filter
is applied, Linear animates the resulting change (an item moving between
columns, a list re-filtering) with a short, easing transition rather than
an instant cut or a lingering, elaborate animation.

**WHY IT WORKS.** The motion answers "what changed and where did it go"
(Constitution #12's continuity job) — when an issue moves from "In
Progress" to "Done," seeing it animate to its new position preserves the
user's mental model of what just happened, versus an instant jump that
would require the user to re-scan and infer the change themselves. Being
fast (not lingering) keeps it from adding perceived latency.

**UNDERLYING PRINCIPLE.** Animate a state change specifically when the
*before* and *after* positions differ and the user would otherwise have
to re-locate the item — keep the duration short enough that it never
reads as the interface being slow.

**WHEN TO REUSE THE PRINCIPLE.** Any reorderable/filterable list, kanban
board, or sortable table where items change position as a direct result
of a user action.

**WHEN NOT TO COPY IT.** For a change that doesn't involve a spatial
move (a value updating in place, a toggle flipping) an accompanying
"item flies across the screen" animation would be inventing motion with
no continuity job to do — a simple, instant or fade-based state change is
more honest there.

---

## Stripe Checkout's inline, real-time field validation

**OBSERVATION.** Stripe's checkout fields (card number, expiry, CVC)
validate and format as the user types — card number auto-detects the
network and shows its icon, invalid states appear immediately next to
the specific field rather than only on submit.

**WHY IT WORKS.** Financial form entry is exactly the high-stakes-if-
misread context where Constitution #16 says decision-critical
information must not be deferred — an error discovered only after
submitting the whole form (and after typing several more fields) costs
far more than one discovered the moment it occurs, both in time and in
the anxiety of a payment failing.

**UNDERLYING PRINCIPLE.** Validate and give feedback at the moment an
error becomes knowable, not at the moment of submission, whenever the
cost of a late-discovered error is high (time, money, trust).

**WHEN TO REUSE THE PRINCIPLE.** Any form collecting payment, account
credentials, or other data where a submit-time-only error is costly —
signup forms, password fields, payment forms.

**WHEN NOT TO COPY IT.** For low-stakes, exploratory input (a search box,
a comment draft, most single-field forms) instant inline validation can
feel naggy or premature (validating an email field before the user has
finished typing the domain) — validate on blur or submit instead when the
cost of an early false-positive error outweighs the benefit of catching
it a few keystrokes sooner.

---

## Superhuman's "command" confirmation via keystroke, not dialog

**OBSERVATION.** Actions like archive or snooze in Superhuman execute
immediately on keypress with a brief, non-blocking visual confirmation
(the message animates out of the list) rather than a confirmation dialog
requiring a click to proceed.

**WHY IT WORKS.** Constitution #19 says deviating from a safer convention
(most apps confirm destructive-ish actions with a dialog) is worth it
only when the payoff is measurable — here the payoff is real: at
Superhuman's target throughput (processing dozens/hundreds of emails
per session), a confirmation dialog on every archive would multiply
total interaction time enormously, and archive/snooze are non-destructive
and typically reversible (undo), so the risk a dialog would guard
against is low relative to the throughput cost of adding one.

**UNDERLYING PRINCIPLE.** Skip confirmation dialogs for high-frequency
actions that are low-risk and reversible; reserve confirmation for
actions that are genuinely destructive or hard to reverse — the decision
should be made per action based on frequency × reversibility, not
applied as a blanket policy.

**WHEN TO REUSE THE PRINCIPLE.** Any high-frequency, reversible action in
an expert tool (archiving, marking read, dismissing a notification,
completing a checklist item) where an undo affordance exists.

**WHEN NOT TO COPY IT.** For low-frequency, hard-to-reverse actions
(deleting an account, permanently removing data, sending a message that
can't be recalled) skipping confirmation removes a safety net users need
— this pattern is specifically for high-frequency + reversible actions,
not a general argument against confirmation dialogs.

---

## Raycast's live-filtering, zero-submit search

**OBSERVATION.** Typing into Raycast's command bar filters and reorders
results on every keystroke with no separate "search" submission step;
results are ranked and the top one is pre-selected so pressing Enter
immediately after typing usually does the right thing.

**WHY IT WORKS.** This removes an entire interaction step (submit) from
the most frequent action in the entire product, and pre-selecting the
likely-correct top result means the fast path (type, then Enter) works
without requiring the user to also navigate to a result — directly
serving the throughput goal for Raycast's expert, high-frequency users
(Constitution #11).

**UNDERLYING PRINCIPLE.** For a search/filter interaction performed very
frequently, eliminate the explicit submit step and rank results so the
default (first) choice is very likely to be the correct one — optimize
the modal (most common) path even at some cost to less common paths.

**WHEN TO REUSE THE PRINCIPLE.** Any frequently-used search/filter/
autocomplete surface — omnisearch bars, command palettes, autocomplete
in forms.

**WHEN NOT TO COPY IT.** For a search whose results carry real
consequence if acted on too quickly (a destructive command, a financial
transaction search where the wrong pre-selected result could be
executed), auto-selecting a top result and allowing instant Enter-to-
execute is dangerous — that context needs an explicit confirm step even
at some throughput cost.

---

## Figma's live multiplayer cursor and selection presence

**OBSERVATION.** Other users' cursors, current tool, and object selections
render live on the canvas with a name label and consistent per-user
color, updating continuously rather than only on a save/sync event.

**WHY IT WORKS.** This directly satisfies Constitution #12's "spatial
relationship / what's happening right now" job for motion — a
collaborator's cursor moving toward an object is itself information
(they're about to select/edit it), letting users implicitly coordinate
(avoid the same element) without any explicit chat message, which a
turn-based or lock-based system couldn't communicate at all.

**UNDERLYING PRINCIPLE.** In real-time multi-user work on a shared
spatial surface, render other users' intent (cursor position, active
tool) continuously and live, because the motion itself carries
coordination information that no static state summary could replace.

**WHEN TO REUSE THE PRINCIPLE.** Any simultaneous multi-user spatial
tool — whiteboards, collaborative diagramming, live collaborative
editing of visual layouts.

**WHEN NOT TO COPY IT.** For asynchronous or turn-based collaboration
(most commenting, most document review, most task-management handoffs)
live cursor presence is unneeded complexity with real engineering cost
— reserve it for genuinely simultaneous, spatial collaboration, not as a
default "collaboration feature" checkbox.

---

## GitHub's optimistic UI on reactions and quick actions

**OBSERVATION.** Adding an emoji reaction or a quick label to an issue/PR
updates the UI immediately (optimistically), before the server
confirms the write, with a fast, low-key visual pop rather than a
loading spinner blocking the action.

**WHY IT WORKS.** These are low-risk, easily-reversible, high-frequency
social/organizational actions where the overwhelming majority of the
time the write will succeed — showing a spinner and waiting for
round-trip confirmation on every one would add perceived latency
(Constitution #12 explicitly calls out that motion/waiting states are a
cost) with no real benefit, since a rare failure can be quietly
reconciled or rolled back after the fact.

**UNDERLYING PRINCIPLE.** For frequent, low-risk, high-success-rate
actions, update the UI optimistically and reconcile failures afterward
rather than blocking on server confirmation — reserve blocking
loading states for actions with a meaningfully high failure rate or
real consequence if the optimistic assumption is wrong.

**WHEN TO REUSE THE PRINCIPLE.** Likes/reactions, labels/tags, toggling
a checkbox, starring/favoriting — any frequent, low-stakes, high-success
write.

**WHEN NOT TO COPY IT.** For payments, irreversible deletions, or any
action with a meaningful failure rate or high cost if wrong, optimistic
UI can mislead the user into believing something succeeded when it
didn't — those need real confirmation feedback tied to the actual server
response, not an optimistic guess.

---

## Sources

- [The Elegant Design of Linear.app](https://telablog.com/the-elegant-design-of-linear-app/) — Tela Blog, accessed 2026-09-12
- [Checkout UI design strategies for faster transactions](https://stripe.com/resources/more/checkout-ui-strategies-for-faster-and-more-intuitive-transactions) — Stripe, accessed 2026-09-12
- [Checkout page design optimisation for e-commerce](https://stripe.com/gb/resources/more/how-to-design-an-ecommerce-checkout-page-that-converts-more-sales) — Stripe, accessed 2026-09-12
- [Superhuman's Secret 1-on-1 Onboarding Revealed](https://growth.design/case-studies/superhuman-user-onboarding) — Growth.Design, accessed 2026-09-12
- [The need for speed: Keyboard first interaction and the user](https://medium.com/apsi/the-need-for-speed-b13f2eef3938) — Medium/APSI, accessed 2026-09-12
- [Raycast showed me what Command Palette should have been all along](https://daily.dev/posts/raycast-showed-me-what-command-palette-should-have-been-all-along-xil92k8bx) — daily.dev, accessed 2026-09-12
- [Multiplayer Editing in Figma](https://www.figma.com/blog/multiplayer-editing-in-figma/) — Figma Blog, accessed 2026-09-12
- [Building Figma Multiplayer Cursors](https://mskelton.dev/blog/building-figma-multiplayer-cursors) — Mark Skelton, accessed 2026-09-12
- [Reviewing proposed changes in a pull request](https://docs.github.com/en/pull-requests/how-tos/review-pull-requests/reviewing-proposed-changes-in-a-pull-request) — GitHub Docs, accessed 2026-09-12
- Internal: `knowledge/ui-principles-2026.md` (Constitution #12, #16, #19) — this project, accessed 2026-09-12
