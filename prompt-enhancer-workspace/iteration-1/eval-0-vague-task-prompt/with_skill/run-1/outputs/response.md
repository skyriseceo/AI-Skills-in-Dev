Since you asked for a one-shot rewrite, here's what I inferred rather than asked: the goal is higher checkout conversion before Black Friday, the one confirmed bug is the coupon-field crash on empty input, "better" means fewer abandoned checkouts (not a visual redesign), and you'd rather get a stable, focused improvement shipped than a sweeping rewrite this close to the holiday. Correct me on any of these and I'll adjust.

## Enhanced prompt

```
I'm working on the checkout page of our React e-commerce app. The business goal: checkout
conversion needs to go up before Black Friday, so the priority is removing anything that
makes shoppers abandon or fail to complete a purchase — not a visual redesign.

Two parts to this task:

1. Fix the known bug: the coupon code field crashes when submitted with empty input.
   Find the root cause, fix it, and make sure invalid or expired coupon codes also fail
   gracefully with an inline message instead of crashing or clearing the customer's cart.

2. Audit the rest of the checkout flow for anything else that could break or block a
   purchase — unhandled errors, form validation that rejects valid input, broken states
   on the payment/submit step, dead ends after a failed action. Fix what you find, and
   for each fix explain what was failing and how a customer would have hit it.

While you're in there, apply low-risk conversion improvements to the flow you touch:
clear inline validation messages instead of silent failures or alerts, a disabled +
loading state on the submit button so double-clicks can't fire duplicate orders, and
preserving the customer's entered data when an error occurs. Skip anything that
requires backend changes or new dependencies unless a fix genuinely can't be done
without them — flag those cases instead.

Boundaries: don't redesign the layout, restructure components beyond what the fixes
require, or add new features (saved carts, new payment methods, etc.). We're close to
Black Friday, so small verifiable changes beat a big refactor.

Done means: the coupon field handles empty, invalid, and valid input without crashing;
every bug you fixed has a test or a manual repro step showing it's fixed; and you finish
with a short summary listing each change, the customer-facing problem it solves, and
anything you found but deliberately left alone.
```

## What changed and why

- **Stated the intent (conversion before Black Friday, not aesthetics)** — "make it better" would otherwise get resolved as a visual polish pass; the goal statement lets the model make the right judgment calls everywhere the prompt is silent.
- **Named the known bug with expected behavior** — "fix the bugs" gave the model nothing to find; specifying the coupon crash plus the graceful-failure requirement turns a guess into a checkable task, and extends it to the adjacent cases (invalid/expired codes) a shopper will actually hit.
- **Turned "better" into a bounded audit plus specific low-risk improvements** — gives the model a concrete definition of improvement (blockers and abandonment causes) instead of an open invitation, and pre-approves the highest-value cheap wins (double-submit protection, preserved form data).
- **Added scope boundaries and the reason for them** — without "no redesign, no refactor, no new features," a capable coding agent given "make it better" will happily restructure the whole page right before your peak traffic event; the timing rationale helps it generalize to cases the boundary list doesn't cover.
- **Defined done with verifiable criteria** — tests or repro steps per fix, plus a summary of what was changed vs. deliberately left, so you get evidence rather than "fixed some bugs" and a list of leftovers you can triage yourself.

## Optional additions

- **If the agent has access to your test suite / dev server**, add: `Run the existing test suite before and after your changes, and exercise the full checkout flow in the browser (add item → apply coupon → pay) to confirm nothing regressed.` — include this when the agent can actually execute things, so its "done" claims are grounded.
- **If you know your checkout's file locations**, add a line like `The checkout lives under src/pages/checkout/ and the coupon component is src/components/CouponField.jsx` — saves the agent an exploration pass and prevents it fixing a lookalike component.
- **If you want analytics-informed priorities**, add: `We see the biggest drop-off at [step] per our funnel data — weight your audit toward that step.` — only if you have the data; otherwise let the agent audit evenly.

Want me to iterate — tighter, more or less prescriptive, or adjusted for a specific coding agent? You can also run `/prompt-enhancer:run-evals` to A/B test the original against this version.
