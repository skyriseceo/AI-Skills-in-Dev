## Enhanced prompt

```
This is a React e-commerce app. I need to improve checkout conversion before
Black Friday, so the priority is fixing anything that blocks or discourages
a customer from completing a purchase — not a visual redesign.

Start with the known bug: the coupon code field crashes the page when
submitted empty. Reproduce it, fix the root cause, and make the field handle
empty/whitespace input gracefully (ignore it or show an inline "enter a code"
message — don't block checkout either way).

Then audit the rest of the checkout flow for similar conversion killers:
crashes or unhandled errors, dead-end states (e.g., a failed payment or
invalid coupon with no way forward), confusing or missing validation
messages, and anything that makes the user re-enter data. Fix what you find,
prioritizing by how likely it is to lose a sale.

Keep changes low-risk and scoped to the checkout flow — Black Friday traffic
is coming, so no refactors of shared code or new dependencies unless a fix
truly requires it. Add or update tests for each bug you fix (including the
empty-coupon case).

When done, give me a short list of what you fixed and why each one mattered
for conversion, plus anything you found but deliberately didn't touch.
```

## What changed and why

- **Named the goal (conversion before Black Friday)** — "better" was doing the work of an unstated objective; with the why stated, the model can judge for itself what counts as an improvement and what's a distraction.
- **Pinned the one known bug with expected behavior** — "fix the bugs" gave the model nothing to find; the empty-coupon crash is now a concrete, verifiable first task with a defined graceful outcome.
- **Defined "better" as removing purchase friction** — turns an open-ended aesthetic invitation into a targeted audit (crashes, dead ends, validation, re-entry), so the model selects fixes rather than inventing a redesign.
- **Added scope boundaries tied to the deadline** — checkout-only, low-risk, no refactors or new dependencies; prevents a well-meaning rewrite right before peak traffic.
- **Specified the deliverable** — a fix list with conversion rationale plus a "found but didn't touch" section, so you can review risk and report progress to your boss.

## Optional additions

- If you have analytics or error monitoring (Sentry, GA funnel data), add: `Check [tool] for checkout errors and drop-off points and use them to prioritize.` — real data beats the model's guesses about what loses sales.
- If you have an existing test suite/CI, add: `Run the full checkout test suite before and after; all existing tests must still pass.` — cheap regression insurance before a high-traffic event.

---

**Assumptions I made** (since you asked me not to ask questions): the agent has access to the codebase and can run it; "bugs" plural means the coupon crash plus whatever a checkout audit surfaces; and speed-to-safe-deploy matters more than a UX overhaul given the Black Friday deadline. If any of these are off — e.g., you *do* want visual/UX redesign work — tell me and I'll adjust.

Want it tighter, more or less prescriptive, or tuned for a specific coding assistant's style? I can also A/B test the original vs enhanced version via `/prompt-enhancer:run-evals`.
