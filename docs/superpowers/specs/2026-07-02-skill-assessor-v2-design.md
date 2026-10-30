# skill-assessor v2 — Design

Date: 2026-07-02
Status: approved (user), pending implementation
Target: `/home/ah/Projects/SkySkills/skill-assessor/` (SKILL.md, references/routing-heuristics.md, evals/evals.json)

## Motivation

Two eval iterations surfaced four improvement areas:

- Routing flipped between iterations on the same prompt (design-master vs taste-skill:redesign-skill) because two tiebreak heuristics conflict with no precedence.
- Rejected lists ran 8–11 entries, mostly trivially irrelevant — token bloat that dilutes the near-miss signal.
- The best candidate for a bounded mechanical edit was an agent type (cavecrew-builder), which the skill could only reject awkwardly because it routes to skills exclusively.
- The "execute unless told otherwise" default forced every realistic prompt to append "assess only".

## Change 1: Tiebreak priority ladder

File: `references/routing-heuristics.md`, section "Overlapping skill families".

Replace the flat bullets with an ordered ladder; a higher rung always settles the tie before lower rungs are consulted:

1. **User's pick** — a skill the user named is an instruction; flag a contract miss if one exists, otherwise route as told.
2. **Mode match** — output type must match what the user needs (comps vs. working code vs. critique). Acts as a disqualifying filter, applied before any comparative rule.
3. **Explicit claim** — a description that names the user's exact situation or trigger phrase wins over one where fit is inferred.
4. **Specificity** — narrower artifact scope beats the general family skill when the task fits that scope.
5. **Coverage** — spans more of the task with fewer seams.

Keep the existing "break the tie, always" rule. Expected effect: eval-0 routes stably to design-master (explicit claim beats specificity).

## Change 2: Bounded rejected list

File: `SKILL.md` Step 4 + output template.

- Full rejection entries only for genuine contenders: 2–4 max, each with a specific reason and a flip condition.
- All other shortlisted-then-discarded candidates collapse to a single bucket line:
  `Not close: <group> — <shared reason>` (e.g. "design family, dataviz, meta skills — wrong domain for a video deliverable").
- Written rationale in the skill: near-misses build trust; padding with obvious non-fits dilutes the signal and burns tokens.

## Change 3: Capabilities, not just skills

Files: `SKILL.md` Steps 2–5, `references/routing-heuristics.md`.

- Candidate pool = installed skills (available-skills list) **plus available agent types** (Agent tool list injected into context). Both are session-authoritative sources; the filesystem fallback remains skills-only.
- Plan and rejected entries prefix the mechanism: `skill: <exact-name>` or `agent: <exact-type>`.
- Step 5 executes each plan entry via its mechanism: Skill tool for skills, Agent tool for agent types.
- New heuristics rule ("Skills vs. agents"): an agent type wins when the job is delegation-shaped — bounded edit, read-only search/locate, or context conservation in a long session; a skill wins when the job needs a methodology or domain guidance loaded into the main thread. When neither delegation nor methodology applies, "no capability — handle directly" stands.
- Skill name stays `skill-assessor`. Frontmatter description updated to mention agent types.

## Change 4: Execution stance

File: `SKILL.md` Step 5.

Phrasing of the original request decides what happens after the plan is presented:

- **Question form** ("which skill should I use…?", "what applies here?") — the plan is the deliverable. Stop after presenting it.
- **Imperative with bundled intent** ("fix this using whatever fits", "route and run it") — present the plan, then execute immediately. No confirmation round, so autonomous runs stay unblocked.
- **Explicit assess-only** — unchanged.

Blockers still hold execution regardless of stance. The mid-execution reroute rule is unchanged.

## Eval updates

File: `evals/evals.json` (+ iteration-3 metadata when run).

- eval-0: add assertion that the primary pick is the explicit-claim winner (design-master), testing ladder determinism.
- eval-1: reword the no-skill assertions — acceptable outcomes are "no capability — handle directly" **or** `agent: cavecrew-builder` with a delegation-shaped justification; still must not route to any skill.
- eval-2: add assertion that non-contender rejections are bucketed (rejected list ≤ 4 full entries).
- eval-3: unchanged (user's pick is now ladder rung 1 — same expected behavior).
- All prompts keep their "assess only" phrasing except one variant if desired later; Change 4 makes question-form prompts assess-only by default, so assertions on "does not execute" remain valid.

## Out of scope

- Description trigger optimization (skipped by user decision).
- Renaming the skill.
- Any change to the no-skill verdict form or Blockers line (both validated in iteration 2).

## Note

Project directory is not a git repository, so the spec is saved without a commit. Run `git init` first if version control is wanted.
