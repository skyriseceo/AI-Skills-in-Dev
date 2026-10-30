---
name: prompt-enhancer
description: Analyze and rewrite prompts to get better results from LLMs — task prompts, system prompts, agent scaffolding, and skill/slash-command instructions. Use whenever the user asks to improve, enhance, refine, optimize, or "make better" a prompt, wants help writing a prompt or system prompt, pastes a prompt and asks why it isn't working, or wants their agent/skill instructions reviewed. Also use when the user says "prompt engineer this", "enhance this prompt", or invokes /prompt-enhancer. Supports an optional eval mode via "/prompt-enhancer:run-evals" to A/B test the original vs enhanced prompt.
---

# Prompt Enhancer

Turn a rough prompt into one that reliably gets the intended behavior from a capable LLM. The workflow is: classify the prompt, analyze it against a pattern library, ask the user a few targeted questions with options, then deliver an enhanced prompt with change notes.

Modern frontier models follow instructions well and reason deeply. Most prompt failures are not about "magic words" — they come from missing intent, ambiguous scope, over-prescription, or unstated boundaries. Your job is to diagnose which of those is happening and fix it, not to pad the prompt with boilerplate.

## Out of scope

This skill is for instructions given to text-based LLMs. Not covered: image/video-generation prompts (different craft — composition and style keywords, not instruction-following), defending against prompt injection (a security problem, not a quality problem), building prompt-management tooling or template libraries (a coding task), translating prompts between languages, and performing the prompt's task itself. If a request lands here anyway, say the skill doesn't cover it and handle the request normally without it.

## Step 1: Get the prompt and classify it

If the user didn't include the prompt to enhance, ask for it (and where it runs — chat, API system prompt, agent harness, skill file).

Classify into one of four types, since the relevant patterns differ (the letters are how `references/patterns.md` tags applicability):

1. **Task prompt (T)** — a one-off request typed into a chat or coding agent ("fix this bug", "write a landing page").
2. **System prompt / agent scaffolding (S)** — persistent instructions for an app or long-running agent built on an LLM API.
3. **Skill / command instructions (K)** — reusable instruction files (SKILL.md, slash commands, custom instructions) that a model loads on demand.
4. **Autonomous pipeline prompt (A)** — instructions for unattended, long-horizon runs where no human can answer questions mid-task.

If the type is ambiguous, ask — the right enhancements for a system prompt can be wrong for a task prompt.

## Step 2: Analyze against the pattern library

Read `references/patterns.md` and evaluate the prompt against each applicable pattern. Start with the patterns most load-bearing for the prompt's type, then scan the rest quickly:

| Type | Check first |
|---|---|
| T — task prompt | 1 (intent), 2 (success criteria), 4 (scope), 16 (de-prescription) |
| S — system prompt | 1–6, 8 (boundaries), 13 (summaries), 16, 17 (reasoning echo) |
| K — skill/command | 2, 5 (brevity), 16, 17 |
| A — autonomous pipeline | 2, 3, 6, 7 (grounded claims), 10 (memory), 11 (early stopping), 15 (verification) |

For each pattern note: applies / doesn't apply / already handled. You're looking for:

- **Missing intent** — the request without the why. The single highest-leverage fix on capable models.
- **Ambiguity the model will resolve wrong** — undefined scope, success criteria, output format, or audience.
- **Over-prescription** — enumerated micro-instructions, ALL-CAPS MUSTs, rigid step lists that a capable model would do better without. Prompts written for older, weaker models often degrade output on newer ones.
- **Missing boundaries** — no statement of what NOT to do, when to stop, when to ask.
- **Long-run hazards** (types S and A only) — no verification instruction, no grounding-of-progress-claims instruction, no guidance on when pausing is appropriate, no memory/notes mechanism.
- **Reasoning-echo instructions** — prompts telling the model to transcribe or explain its internal reasoning in the response; replace with structured output or a progress-reporting mechanism.

Don't force patterns onto a prompt that doesn't need them. A two-line task prompt usually needs intent and success criteria, not a verification harness. And if the analysis finds the prompt is already solid, say so — deliver a verdict with at most one or two marginal suggestions rather than manufacturing a rewrite. An enhancement that changes nothing important is worse than none, because it costs the user a re-review of their own prompt.

## Step 3: Ask targeted questions

This step is the skill's core value — any capable model can rewrite a prompt from assumptions; surfacing the right unknowns before rewriting is what makes the enhancement fit. Ask one round of 2–4 questions, then rewrite. Don't loop unless the answers reveal the prompt needs restructuring the user should weigh in on (e.g., it should be split into a system prompt plus per-task prompts).

Question craft:

- **Ask only what analysis surfaced as genuinely unknown** — things you cannot infer from the prompt, the conversation, or common sense. Never ask to confirm what you can safely assume.
- **Every question must change the rewrite.** If all its answers lead to the same enhanced prompt, drop the question.
- **Be opinionated.** Give each question 2–4 concrete options, mark one as recommended, and say in a sentence why. Options should name real trade-offs ("halt and wait vs skip and log"), not abstractions ("more robust vs simpler").

Typical high-value questions by type:

- **T:** Who consumes the output and what do they do with it? What would make you say "yes, exactly this"? What's explicitly out of bounds?
- **S:** What does the agent do when a request falls outside its role? Which behaviors have users actually complained about?
- **K:** Which of these rules are real requirements today vs habits from working with weaker models? What does the model get wrong *without* the instructions?
- **A:** When the agent hits a blocker unattended, should it halt, or skip-and-log and continue? What evidence counts as "done"?

Skip this step when the user explicitly asks for a one-shot rewrite, when the prompt plus conversation context already answers everything, or when you're running non-interactively and can't ask (e.g., as a subagent or in a pipeline). In all three cases, state the assumptions you inferred alongside the result so the user can correct them.

## Step 4: Rewrite and deliver

Produce the enhanced prompt using the answers. Rewriting principles:

- **Explain why, not just what.** Give the model the reason behind each constraint so it can generalize. "Lead with the outcome, because the reader wants the TLDR first" beats "BE CONCISE."
- **Prefer one clear principle over ten enumerated cases.** Capable models generalize from a well-stated rule; enumerations invite loophole-hunting and go stale.
- **State boundaries positively where possible** — what the deliverable is, when to stop, when to ask — rather than a wall of prohibitions.
- **Cut what isn't pulling weight.** If a line wouldn't change the model's behavior, delete it. Shorter prompts with high signal beat long defensive ones.
- **Keep the enhancement proportional.** A three-line task prompt should come back as a focused paragraph or two, not a multi-page specification — inventing detailed requirements the user never stated is speculation dressed as thoroughness. System prompts and pipeline prompts can justifiably grow more, but every addition must trace back to a real failure mode or a stated need.
- **Keep the user's voice and domain terms.** You're enhancing their prompt, not replacing it with yours.
- Never name a specific model or vendor in the enhanced prompt unless the user's original did — keep it portable across LLMs.

### Output format

Deliver in this structure — it's fixed so users always know where the copy-paste block is and can diff iterations against each other:

````markdown
## Enhanced prompt

```
[the full enhanced prompt, ready to copy-paste]
```

## What changed and why

- **[Change]** — [one sentence: the problem it fixes and the principle behind it]
- ...

## Optional additions

[Only if applicable: 1–3 snippets the user might add depending on their setup — e.g., a verification instruction for long runs, an output-format block — each with one line on when to include it.]
````

If the enhanced prompt itself contains triple-backtick code fences, wrap it in a longer fence (four or more backticks) so it renders intact.

Keep change notes selective: list the changes that alter behavior, not every rewording. After delivering, offer to iterate ("want it more/less prescriptive, different tone, tighter?") and mention that `/prompt-enhancer:run-evals` can A/B test it.

### Worked example

Original (task prompt, user said the report is for their weekly stakeholder email): `summarize this sales data and make it look professional`

## Enhanced prompt

```
I send a weekly sales summary to non-technical stakeholders who skim it in
under a minute. From the attached data, write a summary that leads with the
one number they'd care about most this week, then 3-4 bullets covering
notable changes vs last week. Plain language, no tables. Flag anything that
needs a decision from them. Keep it under 150 words.
```

## What changed and why

- **Added the audience and how they read it** — "professional" was doing the work of an unstated audience; naming skimming stakeholders lets the model pick length, tone, and structure itself.
- **Defined what "summarize" means here** — lead metric + week-over-week changes + decisions needed, so the model selects rather than compresses everything.
- **Replaced "look professional" with checkable constraints** — word cap, no tables, plain language.

Note the scale: a one-line prompt became six lines, not a specification document.

## Eval mode: /prompt-enhancer:run-evals

When invoked with `run-evals` (or the user asks to test/compare the prompts), A/B test the original against the enhanced version:

1. If no enhanced prompt exists yet in this conversation, run the enhancement workflow first.
2. Agree with the user on 1–3 representative test inputs, chosen by prompt type: for a task prompt, the prompt itself is the test; for a system prompt or skill/command file, sample user messages or tasks it must handle (include at least one edge case, not just the happy path).
3. Spawn parallel generation subagents: for each test input, one subagent operates under the original prompt and one under the enhanced prompt (for system/skill prompts, the prompt becomes the subagent's instructions and the test input is the user message it handles). Give each subagent only its own prompt and test input — no knowledge that a comparison is happening or what the other version says.
4. Judge blind. First apply any objective checks yourself (does the output match the requested format, does code run, are constraints respected). For qualitative comparison, pass both outputs to a fresh judge subagent labeled only "A" and "B" — randomize which is which and don't reveal origins — with the success criteria from Step 3 and ask for a per-criterion verdict. Blind judging matters because knowing which output came from the "enhanced" prompt biases the judgment toward it.
5. Report: verdict first (which version won and on what), then a short table of criteria × versions, then concrete revisions if the enhanced prompt lost on anything.

Keep the cost bounded: default to at most 3 test inputs, one generation run per version per input, and one judge per input (≤9 subagents total). System-prompt and pipeline tests are the expensive ones — the subagent executes real work under the prompt, not just a rewrite. Before going beyond the default scale (more inputs, repeat runs for variance), tell the user the rough size of the run and get their go-ahead.

If subagents aren't available in the current environment, run the comparisons sequentially yourself and state clearly that the judgment isn't blind.

## Reference files

- `references/patterns.md` — the pattern library used in Step 2. Read it during every analysis; don't work from memory of it.
