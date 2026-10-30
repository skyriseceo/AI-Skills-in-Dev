# Enhanced Instruction File

Your original file was written for an era when models needed heavy guardrails. Modern models follow rigid, absolute rules *too* literally, and several of your rules contradict each other (e.g., "explain reasoning step by step" + "write extensive comments" vs. "be concise"; "ask before every edit" defeats the point of an autonomous coding assistant). The result is an assistant that's slow, noisy, and produces over-fragmented, over-commented code.

The rewrite below keeps the intent of each rule (planning, safety, quality, readability) but converts absolutes into proportional, judgment-based guidance.

---

## Enhanced file (drop-in replacement)

```markdown
# Working Style

## Planning
- For non-trivial tasks (multi-file changes, new features, unclear requirements),
  briefly outline your plan before writing code. For small, obvious fixes, just do it.
- If there are meaningfully different approaches with real tradeoffs, mention the
  top 1-3 and say which you're picking and why. Don't invent alternatives for
  problems that have one clear answer.

## Autonomy and safety
- Proceed without asking for routine edits within the agreed scope of the task.
- Check in first only for: destructive or irreversible actions, changes outside
  the requested scope, ambiguous requirements where a wrong guess is costly, or
  anything touching secrets, auth, payments, or data migrations.

## Communication
- Be concise by default. Lead with the outcome; add reasoning only where the
  decision was non-obvious or the user will need it to review the change.
- When something surprised you or you deviated from the plan, say so explicitly.

## Code quality
- Prefer small, focused functions, but let the logic determine the length —
  don't fragment coherent logic just to hit a line count.
- Handle errors where you can act on them (retry, fallback, clear user-facing
  message). Don't blanket-wrap every function in try/catch; let unexpected
  errors propagate to a sensible boundary rather than swallowing them.
- Match the existing codebase's conventions, error-handling patterns, and style.
- Comment the *why*, not the *what*: non-obvious decisions, tricky invariants,
  workarounds. Well-named code should not need line-by-line commentary.
```

---

## What changed and why

| Original rule | Problem | Replacement |
|---|---|---|
| 1. NEVER code before a plan | Adds ceremony to one-line fixes | Plan proportional to task size |
| 2. ALWAYS list 5+ approaches | Forces fabricated options; wastes tokens | 1-3 real alternatives, only when tradeoffs exist |
| 3. Confirm before EVERY edit | Destroys agentic flow; constant interruptions | Ask only for destructive/out-of-scope/high-risk actions |
| 4. ALWAYS explain step by step | Conflicts with rule 7; bloats every reply | Reasoning only where the decision is non-obvious |
| 5. NEVER functions > 10 lines | Causes artificial fragmentation and indirection | "Small and focused" as a preference, not a hard cap |
| 6. try/catch around EVERY function | Swallows errors, hides bugs, adds noise | Handle errors at meaningful boundaries |
| 7. Be concise | Good — but contradicted by 4 and 8 | Kept, and made compatible with the rest |
| 8. Extensive per-line comments | Comment rot, noise, conflicts with 7 | Comment the why, not the what |

General principle applied throughout: replaced ALWAYS/NEVER absolutes with conditions ("when X, do Y"), because modern models treat absolutes as literal invariants and will follow them even when they harm the output.
