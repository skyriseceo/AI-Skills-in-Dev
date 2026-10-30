# Routing Heuristics

Judgment calls that recur when routing tasks to capabilities. These are heuristics, not rules — the task's specifics always win.

## Process before implementation

Process skills (brainstorming, planning, debugging methodology, TDD, verification) set the frame; implementation capabilities (design, code generation, media editing, delegated edits) fill it. When both apply, the process skill goes first in the plan. Exception: if the user has already done the process step themselves ("here's my spec, build it"), skip straight to implementation — re-running brainstorming on a settled decision is friction, not rigor.

## The tiebreak ladder

When several capabilities in the same domain overlap (multiple design skills, multiple review paths), settle the tie with this ladder. A higher rung always decides before a lower rung is consulted:

1. **User's pick.** A capability the user named is an instruction. Flag a contract miss if one exists; otherwise route as told.
2. **Mode match.** The output type must match what the user needs — comps vs. working code vs. critique, report vs. action. This is a disqualifying filter, not a comparison: a candidate producing the wrong artifact loses no matter how well it scores below.
3. **Explicit claim.** A description that names the user's exact situation or trigger phrase wins over one where you'd have to infer the fit — the explicit claim means the author designed for exactly this case. When *several* candidates make an explicit claim, don't call it a tie and fall through: score how much of the user's own framing each claim covers — the stated problem, the artifact, and the desired outcome — and the fuller match wins. A description that names the user's problem, their artifact type, *and* their goal has claimed the case more completely than one that names the general activity. Only drop to rung 4 when the claims genuinely cover the same ground.
4. **Specificity.** A capability scoped to the exact artifact (e.g., "mobile app screens", "brand kits") beats a general one when the task matches that scope. The general one wins only when the task spans the family.
5. **Coverage.** Spans more of the task with fewer seams.

However close the tie, break it. Naming two winners re-creates the problem the assessment was asked to solve. Same inputs should give the same pick — the ladder exists so routing is deterministic, not a mood.

## Skills vs. agents

Both are routing targets. Pick by the shape of the job:

- **An agent type wins when the job is delegation-shaped:** a bounded, well-specified unit of work (a 1–2 file mechanical edit, a read-only locate/search sweep, a diff review) — especially when the main conversation is long and offloading conserves context. The agent needs no methodology; it needs a clear brief.
- **A skill wins when the job needs a methodology or domain guidance loaded into the main thread:** a design system to follow, a debugging discipline, a multi-step workflow the model must internalize while working.
- When neither delegation nor methodology applies, "no capability — handle directly" stands.

## When a mix beats a single capability

A mix is justified only when the task has genuinely distinct phases with different output types — e.g., research → build, design comps → implementation, build → review. Chain them in phase order and say what each hands to the next. If the "phases" are really one continuous activity, pick the one capability that covers the whole span.

## Meta-skills and self-reference

Skill-management skills (skill creation, skill assessment itself, config/harness skills) route only when the task is *about* skills or the harness — creating one, improving one, configuring behavior. A task that merely *uses* skills never routes to a meta-skill. In particular, never route to skill-assessor itself — recursion isn't a plan.

## Communication/style skills

Skills that shape *how* output is written (tone, compression, formatting) are orthogonal to task routing: they can layer on top of any plan but never substitute for a task capability. Mention them only if the user's request is specifically about output style.

## "No capability" is a real answer

Route to nothing when:
- The task is simple enough that loading a skill or briefing an agent adds overhead without changing the outcome (one-line fix, factual question).
- Every candidate is a keyword-level match but a contract-level miss (the description's "use when" conditions don't actually hold).
- The task needs something no installed capability provides — say what's missing instead of stretching a neighbor.
