# Prompt Pattern Library

Patterns for getting reliable behavior from capable, instruction-following LLMs. Each entry: when it applies, the failure it prevents, and reusable snippet language. Snippets are starting points — adapt wording to the user's prompt and voice; don't paste verbatim if it clashes.

Applicability key: **T** = task prompt, **S** = system prompt/agent scaffolding, **K** = skill/command instructions, **A** = autonomous pipeline.

## Contents

1. [Give the reason, not only the request](#1-give-the-reason-not-only-the-request) (T, S, K, A)
2. [Define success criteria and output format](#2-define-success-criteria-and-output-format) (T, S, K, A)
3. [Act when you have enough — no overplanning](#3-act-when-you-have-enough--no-overplanning) (S, A)
4. [Scope discipline — no unrequested extras](#4-scope-discipline--no-unrequested-extras) (T, S, K)
5. [Brevity through selection, not compression](#5-brevity-through-selection-not-compression) (S, K)
6. [Checkpoint behavior — when to pause for the user](#6-checkpoint-behavior--when-to-pause-for-the-user) (S, A)
7. [Ground progress claims in evidence](#7-ground-progress-claims-in-evidence) (S, A)
8. [State the boundaries — assessment vs action](#8-state-the-boundaries--assessment-vs-action) (S, K, A)
9. [Delegation to subagents](#9-delegation-to-subagents) (S, A)
10. [Memory and lessons across runs](#10-memory-and-lessons-across-runs) (S, A)
11. [Prevent early stopping in autonomous runs](#11-prevent-early-stopping-in-autonomous-runs) (A)
12. [Context-budget reassurance](#12-context-budget-reassurance) (S, A)
13. [Readable final summaries](#13-readable-final-summaries) (S, A)
14. [Verbatim message delivery for async agents](#14-verbatim-message-delivery-for-async-agents) (S, A)
15. [Self-verification with fresh eyes](#15-self-verification-with-fresh-eyes) (S, A)
16. [De-prescription — trust the model](#16-de-prescription--trust-the-model) (S, K)
17. [Don't ask the model to echo its reasoning](#17-dont-ask-the-model-to-echo-its-reasoning) (S, K, A)

---

## 1. Give the reason, not only the request

**Applies:** all types. The single highest-leverage enhancement.

**Failure prevented:** the model infers intent on its own and optimizes for the wrong thing — technically satisfying the words while missing the goal.

Capable models perform better when they understand why: context lets them connect the task to relevant information, make good judgment calls on unspecified details, and know which trade-offs matter.

**Template:**

```
I'm working on [the larger task] for [who it's for]. They need [what the output enables]. With that in mind: [request].
```

For system prompts, the equivalent is a role-and-purpose opening: what the agent is, who its users are, what a good outcome looks like for them.

## 2. Define success criteria and output format

**Applies:** all types. For autonomous pipelines this is the load-bearing pattern: an unattended agent can't ask what "done" means, so the criteria must be checkable by the agent itself (e.g., "all migrated tests pass against the recorded baseline"), not judgment calls.

**Failure prevented:** output that's correct but unusable — wrong format, wrong audience level, wrong length, missing the one thing the user needed. In unattended runs: an agent that declares victory against its own invented definition of done.

State what "done" looks like: the artifact, its format, its audience, and any hard constraints. For structured output, show the exact template rather than describing it. One concrete example of a good input→output pair is worth paragraphs of description; two examples that differ show the model what varies and what's fixed.

## 3. Act when you have enough — no overplanning

**Applies:** S, A. Especially valuable at high reasoning-effort settings where models can deliberate beyond what the task needs.

**Failure prevented:** the model re-derives established facts, re-litigates decided questions, or narrates option surveys instead of acting.

**Snippet:**

```
When you have enough information to act, act. Do not re-derive facts already established in the conversation, re-litigate a decision the user has already made, or narrate options you will not pursue. If you are weighing a choice, give a recommendation, not an exhaustive survey.
```

## 4. Scope discipline — no unrequested extras

**Applies:** T, S, K. Most needed for coding agents at high effort.

**Failure prevented:** unrequested refactors, defensive error handling for impossible cases, premature abstraction, feature creep around a simple fix.

**Snippet:**

```
Don't add features, refactor, or introduce abstractions beyond what the task requires. A bug fix doesn't need surrounding cleanup, and a one-shot operation usually doesn't need a helper. Do the simplest thing that works well. Don't add error handling or validation for scenarios that cannot happen — trust internal code and framework guarantees; validate only at system boundaries (user input, external APIs).
```

## 5. Brevity through selection, not compression

**Applies:** S, K.

**Failure prevented:** two opposite failures — bloated output (option surveys, heavily-structured reports, narrating comments) and over-compressed output (fragments, arrow chains, jargon) when the model is told simply "be concise."

One short principle outperforms enumerating every verbose behavior to ban:

```
Lead with the outcome. Your first sentence after finishing should answer "what happened" or "what did you find" — the thing the user would ask for if they said "just give me the TLDR." Supporting detail comes after. Keep output short by being selective about what you include (drop details that don't change what the reader would do next), not by compressing the writing into fragments, abbreviations, or jargon. Being readable and being concise are different things, and readability matters more.
```

## 6. Checkpoint behavior — when to pause for the user

**Applies:** S, A.

**Failure prevented:** either extreme — the agent asks permission for routine reversible steps, or barrels through decisions that were genuinely the user's to make.

No need to enumerate every case; state the principle:

```
Pause for the user only when the work genuinely requires them: a destructive or irreversible action, a real scope change, or input that only they can provide. If you hit one of these, ask and end the turn, rather than ending on a promise.
```

## 7. Ground progress claims in evidence

**Applies:** S, A. Essential for long autonomous runs.

**Failure prevented:** fabricated or optimistic status reports — "tests pass" without having run them, "done" for skipped steps. Grounding claims in tool results nearly eliminates this even on tasks designed to elicit it.

**Snippet:**

```
Before reporting progress, audit each claim against a tool result from this session. Only report work you can point to evidence for; if something is not yet verified, say so explicitly. Report outcomes faithfully: if tests fail, say so with the output; if a step was skipped, say that; when something is done and verified, state it plainly without hedging.
```

## 8. State the boundaries — assessment vs action

**Applies:** S, K, A.

**Failure prevented:** unrequested actions — applying a fix when the user was only describing a problem, drafting an email nobody asked for, "helpful" state changes based on pattern-matching.

**Snippet:**

```
When the user is describing a problem, asking a question, or thinking out loud rather than requesting a change, the deliverable is your assessment. Report your findings and stop; don't apply a fix until they ask for one. Before running a command that changes system state (restarts, deletes, config edits), check that the evidence actually supports that specific action — a signal that pattern-matches a known failure may have a different cause.
```

## 9. Delegation to subagents

**Applies:** S, A, in harnesses with subagent support.

**Failure prevented:** serial bottlenecks, and orchestrators that block on each subagent instead of working in parallel.

Give explicit guidance on when delegation is appropriate; prefer asynchronous communication over blocking; prefer long-lived subagents that keep context across subtasks (cheaper via cache reads, no bottleneck on the slowest one).

**Snippet:**

```
Delegate independent subtasks to subagents and keep working while they run. Intervene if a subagent goes off track or is missing relevant context.
```

## 10. Memory and lessons across runs

**Applies:** S, A, for agents that run repeatedly.

**Failure prevented:** repeating mistakes across sessions; re-learning the same environment quirks every run.

Provide a place to write notes — a Markdown directory is enough — plus curation rules:

```
Store one lesson per file with a one-line summary at the top. Record corrections and confirmed approaches alike, including why they mattered. Don't save what the repo or chat history already records; update an existing note rather than creating a duplicate; delete notes that turn out to be wrong.
```

Bootstrap from history: "Reflect on our previous sessions. Identify core themes and lessons and store them in [X]. Reference [X] in future runs."

## 11. Prevent early stopping in autonomous runs

**Applies:** A.

**Failure prevented:** deep into a long session, the model ends a turn with a statement of intent ("I'll now run X") without doing it, or pauses to ask permission it doesn't need.

**Snippet (as a system reminder for unattended pipelines):**

```
You are operating autonomously. The user is not watching in real time and cannot answer questions mid-task, so asking "Want me to…?" will block the work. For reversible actions that follow from the original request, proceed without asking. Before ending your turn, check your last paragraph: if it is a plan, a question, a list of next steps, or a promise about work you have not done ("I'll…"), do that work now. End your turn only when the task is complete or you are blocked on input only the user can provide.
```

Pair with pattern 6 so the model still stops where it genuinely should.

## 12. Context-budget reassurance

**Applies:** S, A, when the harness surfaces remaining-token counts.

**Failure prevented:** the model suggesting a new session, offering to summarize and hand off, or trimming its own work out of context anxiety.

Best fix: don't show explicit context-budget counts. If the harness must:

```
You have ample context remaining. Do not stop, summarize, or suggest a new session on account of context limits. Continue the work.
```

## 13. Readable final summaries

**Applies:** S, A — extended agentic conversations with many tool calls.

**Failure prevented:** final messages written as a continuation of the model's working thread — arrow-chain shorthand, invented labels, references to reasoning the user never saw.

**Snippet:**

```
Terse shorthand is fine between tool calls; your final summary is different — it's for a reader who didn't see any of that. Write it as a re-grounding: the outcome first, then the one or two things you need from them, each explained as if new. Drop working shorthand, write complete sentences, spell out terms, and don't use labels you made up while working. If you have to choose between short and clear, choose clear.
```

## 14. Verbatim message delivery for async agents

**Applies:** S, A — long-running agents whose UX must deliver content mid-task.

**Failure prevented:** deliverables and direct answers getting lost in summarization, or the agent ending its turn just to show the user something.

Provide a client-side `send_to_user`-style tool (input = the message; render it verbatim; return a simple ack). Defining the tool is not enough — pair it with elicitation in the system prompt:

```
Between tool calls, when you have content the user must read verbatim (a partial deliverable, a direct answer to their question), call the send_to_user tool with that content. Use it only for user-facing content, not for narration or reasoning.
```

## 15. Self-verification with fresh eyes

**Applies:** S, A — long builds.

**Failure prevented:** self-critique blindness; the author-model marking its own work correct.

Separate, fresh-context verifier subagents outperform self-critique. For long runs:

```
Establish a method for checking your own work as you build. Every [interval], verify your work against the specification using fresh-context subagents that have not seen your implementation process.
```

If the harness has no subagents, keep the fresh-eyes principle another way: verify against externally checkable evidence (run the tests, diff against the spec) rather than re-reading your own reasoning, and do the verification pass as a separate step after building, not interleaved with it.

## 16. De-prescription — trust the model

**Applies:** S, K. The most common fix when *enhancing an existing* prompt rather than writing a new one.

**Failure prevented:** degraded output from prompts written for older, weaker models — enumerated micro-steps, ALL-CAPS MUSTs, defensive repetition. Capable models follow a briefly-stated principle better than a list of its instances, and rigid instructions actively suppress the judgment that makes them good.

How to de-prescribe:

- Collapse enumerated behavior lists into the one principle they instantiate (see patterns 5 and 6 for worked examples).
- Replace each MUST/NEVER with the reason the rule exists; keep hard absolutes only for genuine invariants (safety, data loss, legal).
- Delete instructions that restate default good behavior — if the model would do it anyway, the line is noise.
- Keep exact templates only where the output is machine-parsed or contract-bound.

Yellow flag while rewriting: if you find yourself writing ALWAYS or NEVER in caps, reframe as a reason.

## 17. Don't ask the model to echo its reasoning

**Applies:** S, K, A.

**Failure prevented:** instructions like "explain your chain of thought", "show your thinking step by step in the response", or "transcribe your reasoning" — on reasoning models these are redundant at best (reasoning already happens internally) and on some models can trigger refusals or degraded output.

Fix: delete show-your-thinking instructions. If the application needs reasoning visibility, read the model's structured thinking output from the API where available, or ask for a *decision rationale* as part of the deliverable ("state the key trade-off behind your recommendation") — a product artifact, not a reasoning transcript.
