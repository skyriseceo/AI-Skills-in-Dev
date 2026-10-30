Your diagnosis is right: this file was written to compensate for a weaker model's failure modes, and on a modern model those compensations actively degrade output. This is a skill/command-instruction prompt, and the analysis found four problems:

1. **Over-prescription throughout** — eight ALL-CAPS ALWAYS/NEVER/MUST rules with no reasons. Capable models follow a stated principle better than a list of absolutes, and rigid micro-rules suppress the judgment that makes them good.
2. **Direct contradictions** — "be concise" vs "explain your reasoning step by step" vs "write extensive comments explaining each line." The model resolves the conflict unpredictably, which is exactly the inconsistency you're seeing.
3. **A reasoning-echo instruction** — "explain your reasoning step by step in your response" is redundant on reasoning models (the reasoning already happens internally) and bloats every reply.
4. **Blanket code-style mandates that misfire** — hard 10-line function caps force artificial fragmentation; try/catch around every function is the defensive-error-handling anti-pattern (swallows bugs, adds noise); line-by-line comments produce the noisiest possible code.

Since you asked me not to ask questions, I made these assumptions — correct me if any are wrong:

- The original intent was: plan before coding on nontrivial work, don't make risky edits silently, and keep code readable and robust.
- "Confirm before every file edit" was a trust guardrail, not a workflow you actually want (it makes the agent nearly useless for multi-file tasks). I've narrowed it to genuinely risky actions.
- This runs as a general-purpose coding slash command, not in a domain with real invariants (safety/legal) that would justify hard absolutes.

## Enhanced prompt

```
Before writing code for anything nontrivial, form a short plan: what you'll
change, where, and the main trade-off behind your approach. If two approaches
are genuinely competitive, name them and pick one with a one-line reason —
don't survey options for their own sake. For small, obvious changes, just
make them.

Proceed with routine, reversible edits without asking. Check with me first
only for destructive or hard-to-reverse actions (deleting files, migrations,
rewriting large sections) or when the task turns out to be ambiguous in a way
that changes what you'd build.

Code style: keep functions small enough that each does one clear thing, but
don't fragment logic just to hit a line count. Handle errors where something
can actually fail — user input, I/O, external calls — and let internal code
trust its own invariants rather than wrapping everything defensively. Comment
the why (intent, non-obvious decisions, gotchas), not the what; code that
needs a comment per line usually needs a rewrite instead.

In your replies, lead with what you did or found and the one decision worth
knowing about. Keep detail selective — include what changes my next step,
drop the rest.
```

## What changed and why

- **Collapsed eight absolutes into four principles with reasons** — a capable model generalizes from "why the rule exists" better than from ALL-CAPS MUSTs, which were written to constrain a weaker model and now suppress judgment.
- **Resolved the concise/step-by-step/extensive-comments contradiction** — replaced with one selection principle ("include what changes my next step"); the model no longer has to guess which of three conflicting rules wins.
- **Deleted "explain your reasoning step by step"** — reasoning models already reason internally; asking for a transcript just bloats responses. What you actually want — the key decision — is now a named part of the reply.
- **Replaced "confirm before every edit" with a risk boundary** — per-edit confirmation makes an agent unusable for real tasks; the guardrail now sits where it matters (destructive/irreversible actions, genuine ambiguity).
- **Dropped "at least 5 approaches" and "never code before a plan"** — forced option-enumeration produces padding, and mandatory planning for trivial changes is pure overhead; both are now proportional to task size.
- **Reframed the 10-line cap, blanket try/catch, and line-by-line comments as their underlying intents** — cohesion, error handling at real failure boundaries, and why-comments; the literal rules produced fragmented, noisy, bug-swallowing code.

## Optional additions

- If you use this for long unattended runs, add: "Before reporting progress, check each claim against an actual tool result from this session; if something isn't verified, say so." (Grounds status reports in evidence.)
- If your project has real hard invariants (e.g., "never commit directly to main"), state those separately as the few genuine absolutes — they carry more weight when they're not diluted by eight stylistic ones.

Want it more or less prescriptive, or tuned to a specific language/stack? I can also A/B test the original vs enhanced version with `/prompt-enhancer:run-evals`.
