---
title: States and Feedback
confidence-note: Grounded in NN/g's skeleton-screen/loading-indicator research and Carbon Design System's empty-state taxonomy; timing thresholds (1s/10s) are ESTABLISHED NN/g findings, notification/toast guidance is ESTABLISHED cross-industry convention.
last_updated: 2026-09-12
---

# States and Feedback

Every interface has more states than its "happy path" screenshot shows:
empty, loading, error, success, and the transitions between them. Treating
these as afterthoughts is one of the most common sources of a product
feeling unfinished or untrustworthy, independent of how polished the
primary state looks (Constitution #9 — these states are not optional
extras, they're required elements of the actual interface).

## Empty States: Four Distinct Situations

**FOUNDATIONAL.** "Empty state" is not one pattern — it's at least four
different situations that need different treatment, cross-referenced
against Carbon Design System's empty-state taxonomy:

1. **First-use (nothing created yet).** The user hasn't created any
   content yet, not because of an error or a filter — a brand-new
   workspace, an empty inbox on day one. This is the highest-opportunity
   empty state: it should explain what this space is *for* and provide a
   clear, prominent primary action to create the first item. Tone can
   afford personality/illustration here more than any other empty state
   (Constitution #3: low-frequency, high-delight context where
   cleverness is acceptable) since the user will see it once.
2. **Cleared/completed (was full, now empty by the user's own action).**
   An inbox at zero, a completed task list. This deserves a *different*
   message than first-use — congratulatory or confirming ("All caught
   up," "Inbox zero") rather than an onboarding pitch to create something,
   since the user already knows how the feature works.
3. **No-results (a search or filter produced nothing).** This is not
   "empty," it's "your query matched nothing" — the message must reflect
   the applied filter/search term back to the user ("No results for
   'blue jacket under $50'") and offer a way to broaden or clear the
   query, not a generic "nothing here" message that leaves the user
   unsure whether the feature is broken or their search was just too
   narrow.
4. **Error-caused empty (data failed to load).** This is a failure state
   mis-filed as "empty" if left undifferentiated — it needs error
   framing (see Error States below: what happened, and a retry action),
   not an empty-state illustration implying there's simply nothing there.

**DESIGN RULE.** Never reuse one generic "Nothing to show" empty-state
component across all four situations — misidentifying which one is
occurring (especially conflating error with empty, or no-results with
first-use) actively misleads the user about what's actually happening and
what action would help.

## Loading States: Skeleton vs. Spinner vs. Progress Bar

**ESTABLISHED**, direct NN/g findings on wait-time thresholds.

| Expected wait | Recommended pattern | Why |
|---|---|---|
| < 1 second | No loading indicator at all | A skeleton/spinner shown for under a second causes flicker/flash that reads as noise, not information — the delay itself is below the threshold where users perceive a wait. |
| ~2–10 seconds | Skeleton screen for full-page/section loads; spinner for a small, isolated component | A skeleton communicates the *shape* of what's coming, reducing perceived wait and cognitive load, per NN/g's research — it works because it sets structural expectations, not just "something is happening." A bare spinner is adequate for a small, isolated widget where there's no layout to preview. |
| > 10 seconds | A determinate progress bar with an estimate | Beyond ~10 seconds, users need a sense of *how much longer*, not just that the system is alive — an indeterminate spinner or skeleton left running past this point reads as possibly stuck. |

**DESIGN RULES**
- A skeleton screen must approximate the real layout (matching block sizes
  and positions) — a "frame-only" skeleton showing just a header/footer
  with a blank body is explicitly called out by NN/g as a poor
  implementation, since it gives the structural-preview benefit up while
  keeping most of the blank-screen problem.
- Avoid distracting shimmer/pulse animation intensity on skeletons — subtle
  is sufficient to signal "loading," and overly energetic animation can
  itself become a distraction or, for some users, a vestibular/motion-
  sensitivity concern (Constitution #12 — motion must communicate state,
  not just decorate the wait).
- For actions the user just triggered (not a page load), prefer optimistic
  UI (see below) over any loading indicator at all where the action is
  very likely to succeed.

## Error States: Recoverable vs. Blocking

**ESTABLISHED.**

**DESIGN RULES**
- **Recoverable errors** (a failed save that can be retried, a form
  validation error) should keep the user's context and work intact, state
  plainly what went wrong, and offer a direct retry/fix action in place —
  never force the user to restart the whole flow for a locally recoverable
  problem.
- **Blocking errors** (the page/feature is fundamentally unavailable — a
  403, a 500, a required dependency down) should be honest about the
  severity, avoid vague reassurance ("Oops! Something went a little
  wrong~") that undersells a real outage, and provide a genuinely useful
  next step (retry, go back, contact support) rather than a dead end.
- State what happened in plain language and, if actionable, how to fix it
  — this is the same standard as `knowledge/forms-inputs.md`'s error-
  message-quality rules, generalized to system-level errors.
- Preserve user input/work across an error whenever technically possible —
  losing a half-written form or document to an error is a severity
  multiplier on top of the original problem.

## Success and Confirmation Feedback

**FOUNDATIONAL.**

**DESIGN RULES**
- Match the feedback's prominence to the action's significance
  (Constitution #4 applied to feedback): a routine autosave needs at most
  a subtle, peripheral confirmation (a small "Saved" indicator); a
  significant, hard-to-reverse action (a payment, a publish, a delete)
  warrants a clear, explicit confirmation the user can't miss.
- Don't over-notify for high-frequency actions — a toast on every single
  autosave in a document that saves every few seconds trains the user to
  ignore all toasts (the same alert-fatigue mechanism as
  `knowledge/dashboard-patterns.md`'s alert-threshold guidance).
- Where an action changes visible state (an item added to a list, a value
  updated), let the state change itself be the primary confirmation
  (Constitution #12: motion/change communicating what happened) rather
  than *only* a transient toast the user might miss — the toast should
  supplement a visible state change, not be the sole record of what
  happened.

## Optimistic UI Feedback

**ESTABLISHED**, common in modern high-frequency-interaction products
(likes, reactions, drag-reorder, chat sends).

**PATTERN.** The UI updates immediately as if the action succeeded, before
the server has confirmed it, then quietly reconciles or rolls back if it
actually failed.

**WHEN TO USE.** High-frequency, low-risk, very-likely-to-succeed actions
where waiting for a server round-trip before showing feedback would make
the interface feel sluggish relative to the action's low stakes (a like
button, reordering a list, sending a chat message).

**WHEN NOT TO USE.** High-stakes or failure-prone actions (a payment, a
destructive action, anything with a meaningful failure rate or where a
silent rollback would be confusing/costly) — for these, an honest loading
state followed by real confirmation is more trustworthy than a premature
"success."

**DESIGN RULE.** Always have a real rollback/error path for the rare
failure case — optimistic UI without a graceful failure treatment leaves
the interface lying to the user when the optimistic update doesn't
actually stick.

## Notification / Toast Patterns and When They're the Wrong Tool

**ESTABLISHED.**

**WHEN A TOAST IS RIGHT.** Transient, non-critical, informational feedback
about something that just happened, where the user doesn't need to act
immediately and the information isn't needed later (a background action
completed, a passive confirmation).

**WHEN A TOAST IS THE WRONG TOOL**
- **Anything the user needs to act on.** A toast disappears on its own
  timer — using one to deliver an error that requires a decision (Constitution
  #16: information that changes the decision must not be easy to miss)
  risks the user never seeing it at all if they looked away.
- **Anything the user might need to reference later.** A toast is
  ephemeral by design; persistent information (a saved record, an ongoing
  status) belongs in the actual UI state, not a vanishing notification.
- **High-frequency events.** Stacking many toasts for routine, frequent
  events causes exactly the alert-fatigue problem described above and
  visually clutters the screen.
- **Anything better shown as an inline state change.** If the action's
  result is visible in the interface itself (an item now shows as
  archived, a field now shows the saved value), a toast on top of that is
  often redundant — reserve toasts for actions whose result isn't
  otherwise visible in the current view (e.g., "Email sent" when the
  compose window has already closed).

**DESIGN RULE.** Before reaching for a toast, ask whether the feedback
belongs in-place (an inline confirmation near the action), in the page's
persistent state (a status badge, an updated value), or truly is a
fire-and-forget notification — toast is a residual category for the last
one, not a default feedback mechanism for everything.

## Sources

- [Skeleton Screens 101 – NN/g](https://www.nngroup.com/articles/skeleton-screens/)
- [Skeleton Screens vs. Progress Bars vs. Spinners (Video) – NN/g](https://www.nngroup.com/videos/skeleton-screens-vs-progress-bars-vs-spinners/)
- [Empty states – Carbon Design System](https://carbondesignsystem.com/patterns/empty-states-pattern/)
- [Empty States in Application Design: 3 Guidelines (Video) – NN/g](https://www.nngroup.com/videos/empty-states-in-application-design-guidelines/)
