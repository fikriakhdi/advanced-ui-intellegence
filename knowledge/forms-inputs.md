---
title: Forms and Inputs
confidence-note: Grounded primarily in GOV.UK Design System (accessibility-first, empirically tested at large scale in a government-services context) and Baymard checkout-field findings; validation-timing guidance is ESTABLISHED with an explicit caveat from GOV.UK's own research that context should override the default.
last_updated: 2026-09-12
---

# Forms and Inputs

Forms are where accessibility (Constitution #14) is least optional: a form
a user cannot complete is a task the product has entirely failed at, not a
degraded experience. This file treats accessibility as load-bearing
throughout rather than as a separate closing section, per Constitution #14
("a design constraint, not a final checklist").

## Field Grouping and Order

**FOUNDATIONAL**, application of Constitution #5 (spacing/whitespace as a
grouping tool) and #1 (hierarchy) to forms specifically.

**DESIGN RULES**
- Group fields by the mental model of the *task*, not the database schema —
  "Shipping address" as one visual group, "Payment" as another, even if the
  backend stores them as one flat record.
- Order fields in the sequence a user would naturally produce the
  information (name → contact → address, not alphabetical or arbitrary
  database-column order).
- Use fieldset/legend grouping (or an equivalent heading) for related
  radio/checkbox sets so the group's shared question is announced once,
  not repeated per option, and so assistive technology can convey the
  grouping.
- Long forms should be broken into logical sections with clear headings
  even before considering a multi-step wizard — visual/semantic grouping
  is the first tool, pagination into steps is a bigger, costlier
  intervention reserved for genuinely long or branching forms (see below).

## Label Placement

**ESTABLISHED.**

**DESIGN RULES**
- Labels sit above their field as the default, robust choice — this
  reads correctly at any viewport width (it doesn't break when a label-left
  layout runs out of horizontal room, per Constitution #10/#18) and keeps
  label and value close (Constitution #5 proximity) regardless of field
  width.
- Label-left-of-field (common in dense enterprise/desktop forms) is
  acceptable for short, uniform-width fields in a high-density, expert
  context (Constitution #11) — a settings table, a professional data-entry
  screen — but must be abandoned at narrow viewports (recomposition: label
  above field on mobile, per `knowledge/mobile-patterns.md`).
- Never use a placeholder as a substitute for a label — placeholder text
  disappears the moment the user starts typing, so if it was carrying the
  only label information, the user loses their orientation about what
  they're filling in and assistive technology support for placeholder-as-
  label is inconsistent at best. Placeholders are appropriate only for a
  *format example* ("MM/DD/YYYY") shown alongside a real, persistent label.
- Keep labels visible at all times the field is in view (a label that
  disappears into the field, "floating label" patterns notwithstanding,
  must animate to a persistent position above the field on focus/fill, not
  vanish).

## Inline vs. Summary Validation Timing

**ESTABLISHED, with an explicit caveat.** GOV.UK Design System's guidance —
built on testing across a very large, diverse user base — is that
validation should fire on submission attempt (not on blur/per-field) by
default, surfaced as an error summary at the top of the page with focus
moved to it, *and* that real-time (inline, on-blur) validation should only
be added where user research specifically shows it solves more problems
than it introduces. This directly favors the boring, well-tested default
over a fashionable inline-validation pattern unless it's earned
(Constitution #19).

**DESIGN RULES**
- **Default:** validate on submit; on failure, show an error summary at the
  top of the form ("There is a problem" + a list of errors, each linking to
  its field), move keyboard focus to that summary, and prefix the page
  title with "Error:" so a screen reader announces the failure state
  immediately.
- **Earned exception — inline/on-blur validation** is justified when the
  cost of a late-discovered error is unusually high (e.g., a username-
  availability check that would otherwise require a full round trip, or
  password-confirmation mismatch on a long signup form) — validate as the
  user finishes that field, not on every keystroke.
- **Never validate on every keystroke** for anything beyond a live
  character-count or format mask — keystroke-level validation shows "error"
  states on fields the user hasn't finished typing yet, which is simply
  wrong information, not helpful-early information.
- Preserve all previously entered, valid field values when returning the
  user to fix errors — never clear the form on a failed submission.
- Server-side validation is mandatory regardless of client-side validation
  present, since client-side validation can be bypassed or fail to load —
  never trust the client alone.

## Error Message Quality

**ESTABLISHED**, cross-referenced against both GOV.UK guidance and Baymard
checkout field-error findings (94% of ecommerce sites still use unhelpful
generic messages like "Invalid card" instead of "Your card number is
incomplete" — see `knowledge/ecommerce-patterns.md`).

**DESIGN RULES**
- State **what's wrong** and, where possible, **how to fix it** — "Enter a
  valid email address" is better than "Invalid input"; "Enter a password
  that's at least 8 characters" is better than "Password too short" without
  the number.
- Attach the error message directly adjacent to its field (not only in a
  top summary) — the summary aids overview and focus management, the
  inline message aids the moment of fixing that specific field. Use both
  together, not one or the other.
- Never rely on color alone to indicate an error field (Constitution #7) —
  pair red styling with an icon and/or text; a colorblind user or a user on
  a poor display must be able to identify the errored field without color.
- Write error copy in plain language, not internal system/validation-
  library jargon ("Value does not match pattern" is a developer message
  leaking into user-facing copy, not a real error message).

## Required vs. Optional Field Marking

**ESTABLISHED**, directly citing Baymard's checkout finding: marking only
*one* state (only required, or only optional) causes real errors — 32% of
tested users skipped a required field when the form only visually marked
*optional* fields, presumably because the unmarked majority read as "the
normal/default state," inverting the intended signal.

**DESIGN RULES**
- Mark both states explicitly: an asterisk (with a legend explaining what
  it means) or the word "(required)" on required fields, and "(optional)"
  on optional ones — don't make the user infer one from the absence of the
  other.
- Where a form is overwhelmingly one or the other (nearly all required,
  with 1-2 optional fields), it's acceptable to mark only the minority
  case explicitly (label the few optional fields "(optional)" and leave
  the rest unmarked) since the ambiguity risk is much lower with only one
  or two exceptions — but this is a judgment call to make deliberately, not
  a default to reach for.
- Prefer removing optional fields from the primary flow entirely
  (Constitution #9 — does this field justify its presence right now) over
  marking many fields optional; a shorter form of all-required fields often
  out-converts a longer form padded with skippable ones.

## Multi-Step Forms and Wizards

**CONTEXTUAL** — the right tool for long or branching forms, not a default
for every form.

**WHEN TO USE.** The form is long enough that a single page would be
overwhelming or scroll-heavy for its likely user (Constitution #11 —
consider whether this user is a frequent expert who'd prefer one dense
page, or an infrequent user who benefits from smaller, sequential steps),
or later steps genuinely depend on earlier answers (branching logic that a
single flat page can't represent cleanly).

**WHEN NOT TO USE.** Splitting a short form (under ~7-8 fields) into
multiple steps purely for aesthetic minimalism adds navigation overhead
(more clicks, more round trips, more chances to abandon between steps)
without a real complexity reason to justify it — see Constitution #19: the
familiar single-page form is the default, and pagination is a deviation
that needs to earn its cost.

**DESIGN RULES**
- Always show progress (a step indicator: "Step 2 of 4," or a progress bar)
  so the user can gauge remaining effort — an un-numbered wizard with no
  visible end is a common source of abandonment.
- Let users go back to previous steps without losing entered data.
- Validate each step's own fields before advancing (an application of the
  submit-time validation default, scoped per-step rather than only at the
  very end) — discovering an error on step 1 after completing step 4 is a
  severe, avoidable frustration.
- Persist partial progress (autosave or explicit "save and continue later")
  for long or high-effort forms, since abandonment-and-return is common.

## Autofill and Autocomplete

**FOUNDATIONAL**, especially load-bearing on mobile (see
`knowledge/mobile-patterns.md`).

**DESIGN RULES**
- Use correct `autocomplete` attribute values (`name`, `email`,
  `street-address`, `cc-number`, `one-time-code`, etc.) and appropriate
  `type`/`inputmode` so browsers and password managers can autofill
  correctly — a form that looks fine but omits these silently loses this
  entire class of friction reduction.
- Don't fight autofill by disabling it "for security" on ordinary contact/
  address fields — this is a well-documented anti-pattern that increases
  user effort without a real security benefit for that field class (it's
  legitimately more defensible on password fields on a shared/kiosk device,
  a narrow exception, not a blanket rule).
- Support platform-level OTP autofill (SMS-delivered one-time codes) via
  `autocomplete="one-time-code"` for verification steps.

## Input Types Matched to Data Type

**FOUNDATIONAL.**

**DESIGN RULES**
- Use the semantic HTML input type/mode for the data being collected:
  `email`, `tel`, `number`/`inputmode="numeric"`, `date` (or a well-tested
  custom date picker where the native control's UX is inconsistent across
  browsers), `url` — this drives correct mobile keyboards (see mobile
  patterns) and enables native browser validation hints as a supplement to
  (never a replacement for) real server-side validation.
- Use a picker (date picker, dropdown, radio group) instead of free text
  wherever the answer set is small and known — free text invites
  formatting inconsistency that then requires defensive parsing.
- Conversely, don't force a rigid picker (e.g., a country-style phone
  format mask) onto genuinely variable free-form data — that produces the
  "wrong interface type" failure Baymard documented in checkout (roughly a
  third of tested sites choose an ill-fitting control type for an optional
  or variable field).

## Accessibility of Forms

Ties directly to `knowledge/accessibility.md` — the summary here is
form-specific.

**DESIGN RULES**
- Every input has a programmatically associated `<label>` (not just visual
  proximity) — screen reader users rely on this association, not layout.
- Errors are announced to assistive technology (e.g., via `aria-live` on
  the error summary, `aria-describedby` linking the field to its inline
  error, `aria-invalid="true"` on the errored field).
- Focus order follows visual/logical order; focus moves to the error
  summary on failed submission (see validation section) and back to a
  sensible place on success.
- Sufficient color contrast for label text, input borders, and error text
  (Constitution #7/#14) — an input with only a 1px light-gray border and no
  other affordance is hard to perceive as an input at all for low-vision
  users.
- Never disable browser-native `required`/pattern validation UI in a way
  that leaves no accessible substitute — GOV.UK's own approach disables
  HTML5's inconsistent native messaging specifically because it replaces it
  with a more robust, tested custom pattern (error summary + inline
  message), not because native validation should simply be removed.

## Sources

- [Recover from validation errors – GOV.UK Design System](https://design-system.service.gov.uk/patterns/validation/)
- [Error message component – GOV.UK Design System](https://design-system.service.gov.uk/components/error-message)
- [Checkout UX Best Practices 2025 – Baymard Institute](https://baymard.com/blog/current-state-of-checkout-ux)
