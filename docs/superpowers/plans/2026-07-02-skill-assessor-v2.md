# skill-assessor v2 Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Apply the four approved v2 changes to the skill-assessor skill: tiebreak priority ladder, bounded rejected list, capabilities (skills + agent types), and phrasing-based execution stance.

**Architecture:** The skill is two markdown files (SKILL.md + references/routing-heuristics.md) plus an eval definition file. Because three of the four changes touch overlapping sections, each file is rewritten whole via Write with its complete v2 content rather than chained partial edits. Verification is an eval rerun (iteration 3), not unit tests.

**Tech Stack:** Markdown skill files, JSON evals, skill-creator eval harness (subagent runs + `scripts.aggregate_benchmark` + `eval-viewer/generate_review.py`).

**Spec:** `/home/ah/Projects/SkySkills/docs/superpowers/specs/2026-07-02-skill-assessor-v2-design.md`

## Global Constraints

- `/home/ah/Projects/SkySkills` is NOT a git repository — all commit steps are omitted. Do not run `git init` unless the user asks.
- Skill name stays `skill-assessor`; directory stays `/home/ah/Projects/SkySkills/skill-assessor/`.
- SKILL.md must stay well under 500 lines (v2 target ≈ 110 lines).
- Frontmatter `name` and `description` are the only YAML keys — do not add others.
- Mechanism prefixes in plan output are exactly `skill:` and `agent:` (lowercase, colon, space before the name).
- The no-fit verdict wording changes from "no skill — handle directly" to "no capability — handle directly" everywhere (SKILL.md, heuristics, evals).

---

### Task 1: Rewrite SKILL.md to v2

**Files:**
- Modify (full rewrite): `/home/ah/Projects/SkySkills/skill-assessor/SKILL.md`

**Interfaces:**
- Produces: the v2 skill contract that Tasks 2–4 reference — mechanism prefixes `skill:`/`agent:`, "no capability" wording, bounded rejected list (≤4 full entries + `Not close:` bucket), phrasing-based Step 5.

- [ ] **Step 1: Write the complete v2 SKILL.md**

Write this exact content (full file replacement):

````markdown
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
````

- [ ] **Step 2: Verify the file**

Run: `head -5 /home/ah/Projects/SkySkills/skill-assessor/SKILL.md && grep -c "no capability" /home/ah/Projects/SkySkills/skill-assessor/SKILL.md && wc -l /home/ah/Projects/SkySkills/skill-assessor/SKILL.md`
Expected: frontmatter starts with `---` / `name: skill-assessor`; "no capability" count ≥ 4; line count between 90 and 130.

---

### Task 2: Rewrite routing-heuristics.md to v2

**Files:**
- Modify (full rewrite): `/home/ah/Projects/SkySkills/skill-assessor/references/routing-heuristics.md`

**Interfaces:**
- Consumes: Task 1's SKILL.md references "the tiebreak ladder", "skills vs. agents", and "no capability" — the section headings below must exist under those concepts.
- Produces: the ladder ordering that Task 3's eval-0 assertion depends on (explicit claim outranks specificity ⇒ design-master wins eval-0).

- [ ] **Step 1: Write the complete v2 routing-heuristics.md**

Write this exact content (full file replacement):

````markdown
# Routing Heuristics

Judgment calls that recur when routing tasks to capabilities. These are heuristics, not rules — the task's specifics always win.

## Process before implementation

Process skills (brainstorming, planning, debugging methodology, TDD, verification) set the frame; implementation capabilities (design, code generation, media editing, delegated edits) fill it. When both apply, the process skill goes first in the plan. Exception: if the user has already done the process step themselves ("here's my spec, build it"), skip straight to implementation — re-running brainstorming on a settled decision is friction, not rigor.

## The tiebreak ladder

When several capabilities in the same domain overlap (multiple design skills, multiple review paths), settle the tie with this ladder. A higher rung always decides before a lower rung is consulted:

1. **User's pick.** A capability the user named is an instruction. Flag a contract miss if one exists; otherwise route as told.
2. **Mode match.** The output type must match what the user needs — comps vs. working code vs. critique, report vs. action. This is a disqualifying filter, not a comparison: a candidate producing the wrong artifact loses no matter how well it scores below.
3. **Explicit claim.** A description that names the user's exact situation or trigger phrase wins over one where you'd have to infer the fit — the explicit claim means the author designed for exactly this case.
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
````

- [ ] **Step 2: Verify the file**

Run: `grep -n "^## " /home/ah/Projects/SkySkills/skill-assessor/references/routing-heuristics.md`
Expected: seven sections in this order — Process before implementation / The tiebreak ladder / Skills vs. agents / When a mix beats a single capability / Meta-skills and self-reference / Communication\/style skills / "No capability" is a real answer.

---

### Task 3: Update evals.json for v2 assertions

**Files:**
- Modify: `/home/ah/Projects/SkySkills/skill-assessor/evals/evals.json`

**Interfaces:**
- Consumes: v2 wording from Tasks 1–2 ("no capability — handle directly", `agent:` prefix, ladder ⇒ design-master wins eval-0, ≤4 full rejection entries + `Not close:` bucket).
- Produces: the assertion set Task 4's iteration-3 grading uses verbatim.

- [ ] **Step 1: Apply the eval changes with this script**

```bash
python3 - <<'EOF'
import json
p = "/home/ah/Projects/SkySkills/skill-assessor/evals/evals.json"
e = json.load(open(p))
for ev in e["evals"]:
    if ev["id"] == 0:
        ev["assertions"].append(
            "Primary pick is design-master — the explicit-claim winner under the tiebreak ladder "
            "(its description names 'AI-generated'/premium and dashboards verbatim; specificity-based "
            "redesign-skill must appear only as the rejected runner-up)")
    if ev["id"] == 1:
        ev["assertions"] = [
            "Verdict is 'no capability — handle directly' (or equivalent), OR routes to agent: cavecrew-builder "
            "with an explicitly delegation-shaped justification (bounded 1-2 file mechanical edit / context "
            "conservation); does not route to any skill",
            "Does not recommend process or meta skills (brainstorming, TDD, planning, skill-creator) for the rename",
            "Does not attempt to perform the rename itself"]
        ev["expected_output"] = (
            "Either 'no capability — handle directly' or agent: cavecrew-builder with a delegation-shaped "
            "justification. No skill routed; no process/meta skills; rename not performed.")
    if ev["id"] == 2:
        ev["assertions"].append(
            "Rejected list is bounded: at most 4 full rejection entries; remaining discarded candidates "
            "collapsed into one or more 'Not close:' bucket lines with a shared reason")
json.dump(e, open(p, "w"), indent=2)
print("evals updated:", [len(ev["assertions"]) for ev in e["evals"]])
EOF
```

Expected output: `evals updated: [6, 3, 6, 4]`

- [ ] **Step 2: Verify JSON validity**

Run: `python3 -m json.tool /home/ah/Projects/SkySkills/skill-assessor/evals/evals.json > /dev/null && echo VALID`
Expected: `VALID`

---

### Task 4: Iteration-3 verification run

Performed in the main session (spawns subagents), not delegated to a plan-executor.

**Files:**
- Create: `/home/ah/Projects/SkySkills/skill-assessor-workspace/iteration-3/` (eval dirs, `run-1` layout, `eval_metadata.json` per eval, `grading.json`, `timing.json`, `benchmark.json`)

**Interfaces:**
- Consumes: v2 skill files (Tasks 1–2) and updated assertions (Task 3).
- Produces: benchmark + viewer for user review; pass/fail evidence that all four v2 changes behave.

- [ ] **Step 1: Create iteration-3 directories and metadata** — same layout as iteration-2 (`eval-N-<name>/{with_skill,without_skill}/run-1/outputs/`), `eval_metadata.json` copied from evals.json with the new assertions.
- [ ] **Step 2: Spawn all 8 runs in one turn** — 4 evals × (with-skill pointing at `/home/ah/Projects/SkySkills/skill-assessor/SKILL.md`, baseline without). Same prompts as iteration 2, unchanged.
- [ ] **Step 3: Capture `timing.json` per run** from each task notification as it arrives.
- [ ] **Step 4: Grade all runs** against the Task-3 assertions into `grading.json` (fields: `text`, `passed`, `evidence`), checking specifically: eval-0 primary is design-master; eval-1 verdict is no-capability or `agent: cavecrew-builder`; eval-2 rejected list ≤ 4 full entries with `Not close:` bucket; all with-skill plans use `skill:`/`agent:` prefixes.
- [ ] **Step 5: Aggregate and review** —

```bash
cd /home/ah/.claude/plugins/cache/claude-plugins-official/skill-creator/unknown/skills/skill-creator && \
python3 -m scripts.aggregate_benchmark /home/ah/Projects/SkySkills/skill-assessor-workspace/iteration-3 --skill-name skill-assessor
```

then add analyst notes to `benchmark.json` and launch the viewer with `--previous-workspace .../iteration-2`.
Expected: with-skill pass rate ≥ iteration 2 (100%); eval-0 pick stable on design-master.
