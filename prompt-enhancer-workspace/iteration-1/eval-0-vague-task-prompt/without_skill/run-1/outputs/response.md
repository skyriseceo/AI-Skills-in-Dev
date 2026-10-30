# Enhanced Prompt

Here's an enhanced version of your prompt, ready to paste into an AI coding assistant:

---

## Improve checkout page: fix coupon crash + optimize for conversion

**Context:** This is a React e-commerce app. We need the checkout page fixed and improved before Black Friday, with the primary business goal of increasing checkout conversion rate.

### Part 1 — Bug fixes (do these first)

1. **Critical: coupon field crashes on empty input.** Reproduce and fix the crash that occurs when the coupon code field is submitted (or applied) while empty. Likely causes to check: calling `.trim()`/`.toUpperCase()` on `undefined`/`null`, unguarded array/object access on an empty API response, or an unhandled rejection from the coupon-validation request. Add a guard so that:
   - Empty/whitespace-only input either disables the "Apply" button or shows an inline validation message ("Please enter a coupon code") — it must never throw.
   - Invalid coupon codes show a friendly inline error, not a crash or a raw API error.
   - Wrap the coupon component in an error boundary so a future coupon failure can never take down the whole checkout page.
2. **Audit the rest of the checkout flow for similar defects:** unhandled promise rejections, missing loading/disabled states on the submit button (double-submit risk), stale totals after coupon apply/remove, and form validation that fires at the wrong time.
3. **Add tests** for the coupon field: empty input, whitespace input, invalid code, valid code, and removing an applied coupon. Add a regression test that reproduces the original crash.

### Part 2 — Conversion-focused improvements

Prioritize changes with proven impact on checkout conversion:

1. **Reduce friction:**
   - Minimize required fields; use autocomplete attributes (`autocomplete="email"`, `postal-code`, etc.) and appropriate input types/inputmodes for mobile keyboards.
   - Inline, real-time field validation with clear, specific error messages next to the offending field — never a generic top-of-page error after submit.
   - Preserve all entered form state if anything fails (a failed coupon or payment error must not clear the form).
2. **Build trust and clarity:**
   - Always-visible order summary with itemized subtotal, discount (when a coupon applies), shipping, tax, and total; update totals immediately when a coupon is applied or removed, with a brief loading state.
   - Clear progress indication if checkout is multi-step; a single prominent primary CTA with explicit copy (e.g., "Pay $XX.XX").
3. **Handle failure gracefully:** payment or network errors should show actionable recovery messaging and allow retry without data loss.
4. **Performance:** check for unnecessary re-renders and heavy bundles on the checkout route (code-split if needed); slow checkouts kill conversion, especially on mobile under Black Friday load.
5. **Mobile:** verify the entire flow works well at small viewport widths — tap targets, keyboard behavior, and the order summary placement.

### Constraints & deliverables

- Don't redesign the visual brand; improve within the existing design system/components.
- Don't change pricing, tax, or payment-provider logic beyond what's needed for the fixes.
- Make changes in small, reviewable commits: bug fixes first, then UX/conversion improvements.
- Summarize at the end: root cause of the coupon crash, every bug found and fixed, each conversion improvement made and why it should help, and anything you recommend but deliberately didn't do.

Before starting, explore the codebase to locate the checkout page and coupon components, and state your plan briefly. If the coupon crash's root cause turns out to be in shared code (e.g., an API client), fix it at the source, not with a local band-aid.

---

## Notes on what I changed and why

- **Split "better" into two concrete workstreams** — bug fixes and conversion improvements — and ordered them (crash first, since a crashing checkout is the biggest conversion killer of all).
- **Turned the known bug into a spec:** described the symptom, likely root causes, expected behavior after the fix, and asked for a regression test plus an error boundary so the coupon can never crash checkout again.
- **Anchored "better" to the business goal** (conversion before Black Friday) with specific, high-leverage tactics: friction reduction, trust signals, error recovery, performance, and mobile.
- **Added guardrails** (no redesign, no pricing-logic changes, small commits) so the assistant doesn't over-engineer or make risky changes right before your peak sales event.
- **Required a summary of root cause and remaining recommendations**, so you get an audit trail you can show your boss.

Feel free to trim Part 2 if you want a smaller, safer change set before Black Friday — Part 1 alone is worth shipping immediately.
