Since you asked for a one-shot rewrite, here's what I inferred from the original (correct me if any of this is off): this is a slash-command file for coding tasks, and the real intents behind the eight rules were — think before coding, don't make unilateral risky changes, keep code readable and well-handled, and keep responses digestible. The enumerated MUST/NEVER framing was compensation for an older model's weaker judgment; on current models those rigid rules suppress judgment and several of them actively conflict (be concise vs. explain every step vs. comment every line).

## Enhanced prompt

```
Before writing code, make sure you understand the task. If there are meaningfully
different ways to approach it, briefly weigh them and say which you're taking and
why; for routine changes, just proceed — planning overhead should match the size
of the decision.

Check with me before doing anything risky or hard to reverse: large refactors,
deleting or overwriting significant code, or changes beyond what the task asked
for. Ordinary edits that follow directly from the task don't need confirmation.

Code style: prefer small, focused functions, but let the logic decide the size —
don't fragment a coherent function to hit a line count. Handle errors where
failure is actually possible (user input, I/O, external calls), not defensively
around everything. Comment the non-obvious "why"; skip comments that restate
what the code plainly does.

In your responses, lead with what you did or found, then the key decisions and
their rationale. Keep it short by leaving out what doesn't change my next step,
not by compressing into fragments.
```

## What changed and why

- **Replaced the eight ALWAYS/NEVER rules with four principles, each carrying its reason** — capable models generalize from a well-stated rule; rigid enumerated absolutes written for weaker models suppress the judgment that makes current models good, which is exactly why the file was making outputs worse.
- **"List at least 5 approaches" → weigh alternatives only when they meaningfully differ** — forced option-listing produces padded surveys of approaches the model won't take; the intent (don't code blindly) is kept, scaled to the decision.
- **"Confirm before every file edit" → confirm before risky/irreversible/out-of-scope changes** — per-edit confirmation makes an agent unusable and trains you to rubber-stamp; the boundary is now the thing you actually cared about.
- **Dropped "explain your reasoning step by step"** — reasoning models already reason internally; asking for a transcript bloats output and can degrade it. Replaced with a decision-rationale ask (a product artifact, not a reasoning echo).
- **10-line limit and blanket try/catch → readability and boundary-only error handling** — hard numeric limits fragment coherent logic, and universal try/catch swallows errors; both rules now state the goal instead of a proxy for it.
- **Resolved the concision contradiction** — "be concise" fought with "explain step by step" and "comment every line"; the rewrite defines concision as selectivity (drop what doesn't matter) rather than compression, and scopes comments to the non-obvious "why."

## Optional additions

- If you use this on long autonomous runs where you can't answer mid-task, add: *"You're running unattended — for reversible actions that follow from the request, proceed without asking; end your turn only when done or blocked on input only I can provide."* (Otherwise the confirm-before-risky-changes rule will stall the run.)
- If you want verified status reports on bigger tasks, add: *"Before saying something works, run it (or its tests) and report the actual result; say explicitly what you haven't verified."*

Want it more or less prescriptive, or tuned to a specific harness? I can also A/B test the original against this version with `/prompt-enhancer:run-evals`.
