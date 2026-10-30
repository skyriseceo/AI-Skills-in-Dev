---
name: skill-assessor
description: Assess which installed capability — a skill, a subagent type, or an ordered mix — best fits a given task, then produce a ranked routing plan (and execute it when the request carries do-it intent). Use whenever the user asks "which skill should I use for this", "what skills apply here", "route this task", "assess this prompt", invokes /skill-assessor, or hands over a complex or multi-domain task where several skills or agents could plausibly apply and picking the wrong one (or wrong order) would waste work. Also use when the user seems unsure whether a skill exists for their need.
---

# Skill Assessor

Given a task, decide which of the currently available capabilities should handle it — one skill, one agent type, a mix in a specific order, or none — and explain why. The assessment is the core deliverable; whether execution follows depends on how the user asked (Step 5).

Capabilities are only useful if the right one triggers at the right time. Models tend to undertrigger (skip a skill that would help) and occasionally overtrigger (force a skill onto a task it doesn't fit). This skill exists to make that routing decision explicit, inspectable, and correct before any work starts.

## Step 1: Get the task

The task to assess is whatever the user passed as arguments or described in their message. If no task was given, ask for it — a routing plan without a task is meaningless.

Restate the task in one line to anchor the assessment: what is the user actually trying to produce or learn?

If the routing decision genuinely hinges on something unstated — most often the output type (design comps vs. working code, report vs. action) — ask one targeted question rather than guessing. Don't ask about details that only matter during execution; the chosen capability will handle those itself.

If the user already named a specific skill ("use design-master for this"), that's an instruction, not a routing question: honor it. Assess only whether it's a clear contract miss, and if so, say why and name the better fit before proceeding — don't silently override.

## Step 2: Enumerate candidates

Build the candidate pool from what is actually available **in this session** — do not assess against skills or agents you remember from other installs. Two authoritative sources, both injected into your context:

- **Skills:** the available-skills list (the system-reminder listing skills usable with the Skill tool).
- **Agent types:** the available-agent-types list for the Agent tool (e.g. Explore, general-purpose, plugin-provided agents).

If for some reason the skills list is absent, fall back to scanning the filesystem — personal skills in `~/.claude/skills/`, project skills in `.claude/skills/`, and plugin skills nested under `~/.claude/plugins/cache/` (layout varies by marketplace and version, so search rather than glob a fixed depth):

```bash
find ~/.claude/skills .claude/skills ~/.claude/plugins/cache -name SKILL.md 2>/dev/null
```

Read only the frontmatter (name + description) of each hit. There is no filesystem fallback for agent types; if their list is absent, assess skills only and say so.

Record each candidate under its exact invocable name with a mechanism prefix — `skill: taste-skill:redesign-skill`, `agent: cavecrew-builder` — exactly as the name appears in its list. The plan's names must be directly usable with the Skill or Agent tool; a paraphrased name breaks execution in Step 5.

From the pool, shortlist every capability whose description plausibly overlaps the task. Be generous at this stage — a candidate rejected with a stated reason is more valuable to the user than one silently ignored. Exclude skill-assessor itself: a task routed to the router is a loop, never an answer.

## Step 3: Assess

For each shortlisted capability, judge fit on three axes:

1. **Trigger match** — does the task fall inside the capability's stated "use when" conditions? A description is a contract; a capability used outside its contract usually performs worse than none.
2. **Coverage** — how much of the task does it actually handle? A capability covering 90% of the work beats two covering 50% each with an awkward seam.
3. **Ordering role** — is it a *process* skill (shapes how the work happens: brainstorming, debugging methodology, planning, TDD) or an *implementation* capability (does the work: design, video editing, code generation, delegated edits)? Process skills run first; implementation capabilities execute inside the frame they set. Getting this order wrong is the most common routing mistake.

Read `references/routing-heuristics.md` for judgment calls that come up repeatedly: the tiebreak ladder for overlapping candidates, skills vs. agents, when a mix beats a single capability, meta-skills, and when the right answer is "no capability".

Two honest outcomes to stay open to:

- **Nothing fits.** If the best candidate is a stretch, say so. "Handle directly, no capability" is a valid and often correct verdict — forcing a near-miss produces worse output than plain competence.
- **One capability dominates.** Don't manufacture a mix to look thorough. Most tasks route to a single capability.

Then commit. The assessment must end with exactly one plan — a single primary (plus supporting capabilities only where phases genuinely differ), not a menu. "Co-primary", "either X or Y", or "minimum vs. full combo" verdicts push the routing decision back onto the user, which is the job this skill exists to do. When two candidates are genuinely close, break the tie with the ladder in the heuristics file, pick one, and let the rejection entry for the runner-up say how close it was and what would flip the call.

## Step 4: Deliver the routing plan

ALWAYS use this exact structure:

```
## Skill Assessment

**Task:** <one-line restatement>

**Plan:**
1. skill: <exact-name> — <role in the task> — <why it wins>
2. agent: <exact-type> — <role> — <why>   (only if a mix is genuinely needed)

**Considered and rejected:**
- <mechanism: exact-name> — <specific reason it loses to the pick, plus what would flip the call>
- Not close: <group of remaining candidates> — <the one shared reason>

**Blockers:** <missing inputs or preconditions that would stall execution — omit this line if none>

**Verdict:** <one sentence: what runs, in what order, or "no capability — handle directly">
```

When the verdict is "no capability", the Plan section becomes a single line: `1. No capability — handle directly — <the one-or-two-tool approach>`.

Keep the rejected list bounded: full entries only for genuine contenders — the 2–4 candidates that could plausibly have won — each with a specific, task-grounded reason and a flip condition. Collapse everything else into one `Not close:` bucket line per group. Near-misses are how the user learns to trust the routing; padding the list with obvious non-fits dilutes that signal and burns tokens.

Blockers are things the plan can see are missing before any capability runs — a referenced input file that doesn't exist, an unspecified target that changes the deliverable (platform, aspect ratio, framework). Flag them; don't resolve them during assessment.

## Step 5: Deliver or execute

How the user phrased the original request decides what happens after the plan is presented:

- **Question form** ("which skill should I use for X?", "what applies here?") — the plan *is* the deliverable. Present it and stop; the user asked for routing, not the work.
- **Imperative with bundled intent** ("fix this using whatever skill fits", "route it and run it") — present the plan, then execute immediately: invoke each plan entry in order via its mechanism (Skill tool for `skill:`, Agent tool for `agent:`), or do the task directly with core tools on a "no capability" verdict. No confirmation round — pausing to ask blocks autonomous runs and the user already said go.
- **Explicit assess-only** ("assess only", "don't run it yet") — present the plan and stop, regardless of phrasing.

Blockers hold execution in every case — a missing input file can't be routed around; tell the user what's needed.

If mid-execution the plan turns out wrong (the chosen capability doesn't fit once its full instructions load), say so, update the verdict, and reroute rather than pushing through.
